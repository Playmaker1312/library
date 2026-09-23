# 72 - Email Service — Nodemailer & Resend SDK

## Penjelasan
Queue untuk email sudah siap. Sekarang kita perlu **email service** yang benar-benar mengirim email. Ada 2 pendekatan populer: **Nodemailer** (kirim via SMTP — self-hosted atau Gmail/SendGrid) dan **Resend** (API modern, lebih simpel). Untuk development, kita pakai **Mailhog** — SMTP server palsu yang menangkap email, jadi tidak perlu kirim ke email beneran. Ibarat gedung, Mailhog adalah **keranjang "contoh surat"** — surat yang kita kirim untuk tes tidak benar-benar sampai ke tujuan, cuma dicek formatnya saja.

## Fungsi
- **Nodemailer** — library SMTP untuk kirim email (support Gmail, SendGrid, Mailgun)
- **Resend SDK** — API-first email service (modern, reliable)
- **Mailhog** — SMTP server palsu untuk development/testing
- **MailerModule** atau custom **EmailService** sebagai abstraction
- **HTML Template** dengan Handlebars atau EJS

## Cara Pengimplementasian

### 1. Setup Mailhog via Docker

```yml
# docker-compose.yml (tambah service baru)
services:
  redis:
    image: redis:7-alpine
    # ...

  mailhog:
    image: mailhog/mailhog
    container_name: nest-mailhog
    ports:
      - "1025:1025"   # SMTP port
      - "8025:8025"   # Web UI
    environment:
      MH_STORAGE: maildir
```

Jalankan:

```bash
docker compose up -d
```

Buka `http://localhost:8025` untuk lihat email yang masuk.

### 2. Install Package

```bash
npm install nodemailer @types/nodemailer
npm install resend   # untuk Resend SDK
```

### 3A. Custom EmailService dengan Nodemailer + Mailhog

```typescript
// email.service.ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import * as nodemailer from 'nodemailer';
import type { Transporter } from 'nodemailer';

@Injectable()
export class EmailService {
  private transporter: Transporter;

  constructor(private config: ConfigService) {
    this.transporter = nodemailer.createTransport({
      host: this.config.get('SMTP_HOST', 'localhost'),
      port: this.config.get('SMTP_PORT', 1025),
      secure: false, // true untuk 465, false untuk lainnya
      auth: {
        user: this.config.get('SMTP_USER', ''),
        pass: this.config.get('SMTP_PASS', ''),
      },
      // Untuk Mailhog, auth bisa kosong
    });
  }

  async sendMail(options: {
    to: string | string[];
    subject: string;
    html: string;
    from?: string;
  }) {
    const result = await this.transporter.sendMail({
      from: options.from || this.config.get('EMAIL_FROM', 'noreply@example.com'),
      to: options.to,
      subject: options.subject,
      html: options.html,
    });

    console.log(`Email sent: ${result.messageId}`);
    return result;
  }
}
```

**`.env` untuk Mailhog:**

```
SMTP_HOST=localhost
SMTP_PORT=1025
SMTP_USER=
SMTP_PASS=
EMAIL_FROM=noreply@myapp.com
```

### 3B. EmailService dengan Resend SDK

```typescript
// email-resend.service.ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { Resend } from 'resend';

@Injectable()
export class ResendEmailService {
  private resend: Resend;

  constructor(private config: ConfigService) {
    this.resend = new Resend(config.get('RESEND_API_KEY'));
  }

  async sendMail(options: {
    to: string | string[];
    subject: string;
    html: string;
    from?: string;
  }) {
    const { data, error } = await this.resend.emails.send({
      from: options.from || this.config.get('EMAIL_FROM', 'noreply@myapp.com'),
      to: Array.isArray(options.to) ? options.to : [options.to],
      subject: options.subject,
      html: options.html,
    });

    if (error) throw new Error(`Resend error: ${error.message}`);
    return data;
  }
}
```

### 4. Test Kirim Email

```typescript
// email-test.controller.ts
import { Controller, Post, Body } from '@nestjs/common';

@Controller('email-test')
export class EmailTestController {
  constructor(private readonly emailService: EmailService) {}

  @Post('send')
  async sendTest(@Body() dto: { to: string }) {
    await this.emailService.sendMail({
      to: dto.to,
      subject: 'Test Email dari NestJS',
      html: '<h1>Halo!</h1><p>Ini email test.</p>',
    });

    return { message: 'Email terkirim (cek Mailhog)' };
  }
}
```

### 5. Kirim via Queue (Integrasi)

```typescript
// email.processor.ts
@Processor('email')
export class EmailProcessor extends WorkerHost {
  constructor(private emailService: EmailService) {
    super();
  }

  async process(job: Job): Promise<any> {
    const { to, subject, html } = job.data;
    await this.emailService.sendMail({ to, subject, html });
    return { sent: true };
  }
}
```

### 6. HTML Template Sederhana

```typescript
async sendWelcomeEmail(to: string, name: string) {
  const html = `
    <div style="font-family: Arial; max-width: 600px; margin: 0 auto;">
      <h1 style="color: #333;">Selamat Datang, ${name}!</h1>
      <p>Terima kasih telah mendaftar di aplikasi kami.</p>
      <a href="https://myapp.com/verify" 
         style="display: inline-block; padding: 12px 24px; background: #4F46E5; color: white; text-decoration: none; border-radius: 6px;">
        Verifikasi Email
      </a>
    </div>
  `;

  await this.emailQueue.add('send', { to, subject: 'Selamat Datang!', html });
}
```

## Analogi
- **Nodemailer** = **Kurir pribadi** — Anda tulis surat, bungkus, dan serahkan ke kurir yang mengantar lewat jalur SMTP.
- **Resend SDK** = **Jasa ekspedisi modern** — Anda tinggal klik "kirim", sisanya diurus API.
- **Mailhog** = **Keranjang "contoh" di meja** — surat tes tidak benar-benar dikirim, cuma dicek formatnya.
- **SMTP** = **Jalan pos** — infrastruktur tradisional untuk kirim surat antar kantor.

## Dipakai untuk Apa
- Development: testing email dengan Mailhog
- Production: Nodemailer (Gmail/SendGrid/Mailgun) atau Resend
- Kirim email transaksional (welcome, order, invoice)
- Kirim email marketing/newsletter

## Kesalahan Umum
- **SMTP credentials salah** — email gagal dikirim, timeout
- **Mailhog port bentrok** — port 1025/8025 sudah dipakai
- **Tidak handle error** — email gagal silent tanpa notifikasi
- **HTML template tidak responsif** — rusak di mobile email client
- **Lupa set `secure`** — true untuk port 465, false untuk 587/1025

## Soal Latihan

**Soal:**
Setup Mailhog via Docker, buat EmailService dengan Nodemailer yang terkoneksi ke Mailhog, dan kirim test email. Pastikan bisa dilihat di web UI Mailhog port 8025.

**Jawaban:**

**1. `docker-compose.yml`:**
```yml
services:
  mailhog:
    image: mailhog/mailhog
    ports:
      - "1025:1025"
      - "8025:8025"
```

**2. `.env`:**
```
SMTP_HOST=localhost
SMTP_PORT=1025
EMAIL_FROM=test@myapp.com
```

**3. `email.service.ts`:**
```typescript
@Injectable()
export class EmailService {
  private transporter: Transporter;

  constructor() {
    this.transporter = nodemailer.createTransport({
      host: 'localhost',
      port: 1025,
      secure: false,
    });
  }

  async sendTest() {
    const info = await this.transporter.sendMail({
      from: 'test@myapp.com',
      to: 'user@example.com',
      subject: 'Test Mailhog',
      html: '<h1>Halo dari NestJS!</h1>',
    });
    console.log('Message ID:', info.messageId);
  }
}
```

**4. Panggil dari controller atau test:**
```bash
# Buka Mailhog UI
http://localhost:8025
# Lihat email masuk
```
