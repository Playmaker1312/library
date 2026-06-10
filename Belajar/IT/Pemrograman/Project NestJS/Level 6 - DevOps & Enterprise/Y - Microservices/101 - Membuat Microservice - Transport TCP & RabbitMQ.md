# 101 - Membuat Microservice - Transport TCP & RabbitMQ

## Penjelasan
Setelah memahami konsep microservices (materi 100), kita implementasi yang paling konkret: memisahkan **Email Service** dari monolith. Email service adalah kandidat ideal — tidak butuh response real-time, I/O heavy, bisa pakai message broker. NestJS menyediakan `@MessagePattern()` untuk request-response dan `@EventPattern()` untuk event async.

## Fungsi
- **NestFactory.createMicroservice()**: Membuat aplikasi microservice (bukan HTTP server).
- **@MessagePattern()**: Request-response pattern — client kirim, tunggu balasan.
- **@EventPattern()**: Event-driven — client kirim event, tidak tunggu balasan.
- **ClientProxy**: Service pengirim pesan ke microservice.
- **RabbitMQ**: Message broker — menjamin pesan tidak hilang meskipun consumer sedang sibuk.

## Cara Pengimplementasian

### 1. Struktur Folder
```
project/
├── apps/
│   ├── api-gateway/          # HTTP server utama
│   │   ├── src/
│   │   │   ├── email-client/
│   │   │   │   ├── email-client.module.ts
│   │   │   │   └── email-client.service.ts
│   │   │   └── main.ts
│   │   └── package.json
│   └── email-service/        # Microservice
│       ├── src/
│       │   ├── email/
│       │   │   ├── email.module.ts
│       │   │   ├── email.controller.ts  # @MessagePattern / @EventPattern
│       │   │   └── email.service.ts
│       │   └── main.ts
│       └── package.json
```

### 2. Email Service (Microservice — RabbitMQ)

**`apps/email-service/src/main.ts`**
```typescript
import { NestFactory } from '@nestjs/core';
import { Transport, MicroserviceOptions } from '@nestjs/microservices';
import { EmailModule } from './email/email.module';

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    EmailModule,
    {
      transport: Transport.RMQ,
      options: {
        urls: ['amqp://localhost:5672'],
        queue: 'email_queue',
        queueOptions: { durable: true },
        prefetchCount: 1,
      },
    },
  );

  await app.listen();
  console.log('Email Service is listening on RabbitMQ');
}
bootstrap();
```

**`apps/email-service/src/email/email.controller.ts`**
```typescript
import { Controller } from '@nestjs/common';
import { MessagePattern, EventPattern, Payload } from '@nestjs/microservices';
import { EmailService } from './email.service';

@Controller()
export class EmailController {
  constructor(private readonly emailService: EmailService) {}

  // Request-response pattern — menunggu balasan
  @MessagePattern({ cmd: 'send_email' })
  async sendEmail(@Payload() data: { to: string; subject: string; body: string }) {
    console.log(`[EmailService] Received send_email request for ${data.to}`);
    return this.emailService.send(data);
  }

  // Event pattern — fire and forget
  @EventPattern('user_registered')
  async handleUserRegistered(@Payload() data: { email: string; name: string }) {
    console.log(`[EmailService] User registered: ${data.email}`);
    await this.emailService.sendWelcomeEmail(data.email, data.name);
  }

  @EventPattern('order_confirmed')
  async handleOrderConfirmed(@Payload() data: { email: string; orderId: number }) {
    await this.emailService.sendOrderConfirmation(data.email, data.orderId);
  }
}
```

**`apps/email-service/src/email/email.service.ts`**
```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class EmailService {
  async send(data: { to: string; subject: string; body: string }) {
    // Simulasi kirim email (ganti dengan nodemailer/SES di production)
    console.log(`[Email] To: ${data.to}, Subject: ${data.subject}`);
    await new Promise(resolve => setTimeout(resolve, 500)); // simulasi delay SMTP
    return { status: 'sent', to: data.to };
  }

  async sendWelcomeEmail(email: string, name: string) {
    await this.send({
      to: email,
      subject: 'Welcome to our platform!',
      body: `Hi ${name}, welcome!`,
    });
  }

  async sendOrderConfirmation(email: string, orderId: number) {
    await this.send({
      to: email,
      subject: `Order #${orderId} Confirmed`,
      body: `Your order #${orderId} has been confirmed.`,
    });
  }
}
```

### 3. API Gateway (Client — mengirim pesan ke microservice)

**`apps/api-gateway/src/email-client/email-client.module.ts`**
```typescript
import { Module } from '@nestjs/common';
import { ClientsModule, Transport } from '@nestjs/microservices';
import { EmailClientService } from './email-client.service';

@Module({
  imports: [
    ClientsModule.register([
      {
        name: 'EMAIL_SERVICE',
        transport: Transport.RMQ,
        options: {
          urls: ['amqp://localhost:5672'],
          queue: 'email_queue',
          queueOptions: { durable: true },
        },
      },
    ]),
  ],
  providers: [EmailClientService],
  exports: [EmailClientService],
})
export class EmailClientModule {}
```

**`apps/api-gateway/src/email-client/email-client.service.ts`**
```typescript
import { Injectable, Inject } from '@nestjs/common';
import { ClientProxy } from '@nestjs/microservices';
import { lastValueFrom } from 'rxjs';

@Injectable()
export class EmailClientService {
  constructor(
    @Inject('EMAIL_SERVICE') private readonly emailClient: ClientProxy,
  ) {}

  // Request-response — menunggu hasil
  async sendEmail(to: string, subject: string, body: string) {
    return lastValueFrom(
      this.emailClient.send({ cmd: 'send_email' }, { to, subject, body }),
    );
  }

  // Event — fire and forget
  async userRegistered(email: string, name: string) {
    this.emailClient.emit('user_registered', { email, name });
  }

  async orderConfirmed(email: string, orderId: number) {
    this.emailClient.emit('order_confirmed', { email, orderId });
  }
}
```

**`apps/api-gateway/src/users/users.service.ts`**
```typescript
import { Injectable } from '@nestjs/common';
import { EmailClientService } from '../email-client/email-client.service';

@Injectable()
export class UsersService {
  constructor(private emailClient: EmailClientService) {}

  async register(dto: CreateUserDto) {
    const user = await this.prisma.user.create({ data: dto });

    // Kirim event — tidak perlu menunggu email terkirim
    await this.emailClient.userRegistered(user.email, user.name);

    return user;
  }
}
```

### 4. Docker Compose — RabbitMQ
```yaml
version: '3.8'
services:
  rabbitmq:
    image: rabbitmq:3-management-alpine
    container_name: rabbitmq
    ports:
      - "5672:5672"      # AMQP
      - "15672:15672"    # Management UI
    environment:
      RABBITMQ_DEFAULT_USER: guest
      RABBITMQ_DEFAULT_PASS: guest
```

### 5. Menjalankan
```bash
# Terminal 1 — RabbitMQ
docker compose up -d rabbitmq

# Terminal 2 — Email Service
npx nest start email-service

# Terminal 3 — API Gateway
npx nest start api-gateway

# Test
curl -X POST http://localhost:3000/users/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","name":"Test"}'
```

## Analogi
Gedung pusat (API Gateway) menerima tamu. Saat tamu registrasi, petugas tidak mengantar sendiri surat ke kantor pos — dia memasukkan surat ke **lubang pos** (RabbitMQ queue). Petugas pos (Email Service) mengambil surat dari lubang, mengirimkannya, lalu menempel stempel "TERKIRIM" (response) ke lubang balasan.

- **@MessagePattern()**: Seperti petugas yang menelepon balik ke pengirim — "surat sudah terkirim" (sinkron).
- **@EventPattern()**: Seperti membuang surat ke lubang dan pergi — tidak peduli kapan dibaca (async).
- **RabbitMQ**: Lubang pos yang aman — jika petugas pos sedang istirahat (service down), surat tetap aman di lubang (queue durable).

## Dipakai untuk apa
- Memisahkan service I/O heavy (email, SMS, push notification).
- Background job processing (resize image, export data).
- Event-driven architecture — user registered → kirim email + update CRM + log analytics.
- Komunikasi antar-service tanpa saling mengenal direktori satu sama lain.

## Kesalahan Umum
| Kesalahan | Akibat | Solusi |
|-----------|--------|--------|
| Pakai `send()` untuk operasi yang tidak butuh response | Client menunggu, resource terblokir | Gunakan `emit()` untuk event async |
| Tidak handle koneksi RabbitMQ terputus | Service mati saat RabbitMQ restart | Aktifkan automatic reconnection (bawaan @nestjs/microservices) |
| Queue tidak durable | Pesan hilang saat RabbitMQ restart | Set `queueOptions: { durable: true }` |
| PrefetchCount tidak diatur | Satu consumer kebanjiran pesan | Set `prefetchCount: 1` untuk fair dispatch |
| Tidak ada retry mechanism | Email gagal terkirim, tidak dicoba lagi | Implementasi dead letter queue + retry |

## Soal Latihan

**Soal 1:** Pisahkan modul Email dari monolith menjadi microservice menggunakan RabbitMQ. Buat event `user_registered` dan `order_confirmed`.

**Jawaban 1:**
1. Buat `apps/email-service` dengan `NestFactory.createMicroservice` transport RMQ
2. Di email controller: `@EventPattern('user_registered')` dan `@EventPattern('order_confirmed')`
3. Di gateway: `ClientsModule.register([{ name: 'EMAIL_SERVICE', transport: Transport.RMQ }])`
4. Inject `ClientProxy` dan panggil `this.emailClient.emit('user_registered', data)`
5. Test dengan menjalankan RabbitMQ, email service, dan gateway

**Soal 2:** Apa beda `send()` vs `emit()` di ClientProxy? Kapan pakai yang mana?

**Jawaban 2:** 
- `send()` → Request-response. Client mengirim pesan dan **menunggu balasan**. Cocok untuk operasi yang butuh hasil: validasi data, query, get status.
- `emit()` → Event-driven. Client mengirim pesan dan **lanjut tanpa menunggu**. Cocok untuk notifikasi, log, email, event yang tidak butuh respons langsung.

Gunakan `emit()` untuk email karena:
1. User tidak perlu menunggu email terkirim untuk dapat response
2. Jika SMTP lambat, user tidak terpengaruh
3. Jika email service down, pesan tetap di queue sampai service kembali
