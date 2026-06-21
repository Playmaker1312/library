# 73 - Email Templates & EmailService yang Reusable

## Penjelasan
Email service dasar sudah bisa kirim teks. Tapi aplikasi nyata butuh berbagai jenis email dengan HTML yang cantik dan konsisten: welcome, verifikasi email, reset password, order confirmation. Daripada bikin HTML setiap kali, kita buat **EmailService reusable** dengan method spesifik per jenis email plus template engine (Handlebars/EJS). Ibarat gedung, ini seperti **cetak brosur dengan template standar**: ada template "Selamat Datang", "Tagihan", "Peringatan" — tinggal ganti nama dan tanggal, tanpa bikin dari nol.

## Fungsi
- Template reusable untuk berbagai jenis email
- EmailService dengan method spesifik: `sendWelcome()`, `sendVerification()`, `sendResetPassword()`, `sendOrderConfirmation()`
- HTML responsif yang rapi di berbagai email client
- Integrasi template engine (Handlebars / EJS)

## Cara Pengimplementasian

### 1. Install Template Engine

```bash
npm install handlebars
# atau npm install ejs
```

### 2. Struktur Folder Template

```
templates/
  email/
    welcome.hbs
    verification.hbs
    reset-password.hbs
    order-confirmation.hbs
```

### 3. Template Email (Handlebars)

**templates/email/welcome.hbs:**
```html
<!DOCTYPE html>
<html>
<head><meta charset="utf-8"/></head>
<body style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; padding: 20px;">
  <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 40px; text-align: center; border-radius: 12px 12px 0 0;">
    <h1 style="color: white; margin: 0;">Selamat Datang, {{name}}!</h1>
  </div>
  <div style="background: #ffffff; padding: 30px; border: 1px solid #e0e0e0; border-radius: 0 0 12px 12px;">
    <p>Halo <strong>{{name}}</strong>,</p>
    <p>{{message}}</p>
    <a href="{{actionUrl}}" 
       style="display: inline-block; padding: 12px 24px; background: #4F46E5; color: white; text-decoration: none; border-radius: 6px; margin: 20px 0;">
      {{actionText}}
    </a>
    <p style="color: #666; font-size: 12px; margin-top: 30px;">
      &copy; 2026 {{appName}}. All rights reserved.
    </p>
  </div>
</body>
</html>
```

**templates/email/order-confirmation.hbs:**
```html
<div style="font-family: Arial; padding: 20px;">
  <h2 style="color: #333;">Pesanan #{{orderId}} Dikonfirmasi</h2>
  <p>Halo {{name}}, pesanan Anda telah dikonfirmasi.</p>
  <table style="width: 100%; border-collapse: collapse; margin: 20px 0;">
    <tr style="background: #f3f4f6;">
      <th style="padding: 10px; text-align: left;">Produk</th>
      <th style="padding: 10px; text-align: right;">Harga</th>
      <th style="padding: 10px; text-align: right;">Qty</th>
      <th style="padding: 10px; text-align: right;">Total</th>
    </tr>
    {{#each items}}
    <tr style="border-bottom: 1px solid #e0e0e0;">
      <td style="padding: 10px;">{{this.name}}</td>
      <td style="padding: 10px; text-align: right;">Rp {{this.price}}</td>
      <td style="padding: 10px; text-align: right;">{{this.qty}}</td>
      <td style="padding: 10px; text-align: right;">Rp {{this.subtotal}}</td>
    </tr>
    {{/each}}
  </table>
  <h3 style="text-align: right;">Total: Rp {{total}}</h3>
</div>
```

### 4. EmailService Reusable

```typescript
// email.service.ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import * as nodemailer from 'nodemailer';
import * as handlebars from 'handlebars';
import * as fs from 'fs';
import * as path from 'path';

@Injectable()
export class EmailService {
  private transporter: nodemailer.Transporter;
  private templates: Map<string, HandlebarsTemplateFunction> = new Map();

  constructor(private config: ConfigService) {
    this.transporter = nodemailer.createTransport({
      host: this.config.get('SMTP_HOST'),
      port: this.config.get('SMTP_PORT'),
      secure: false,
    });

    this.loadTemplates();
  }

  private loadTemplates() {
    const templateDir = path.join(__dirname, '../../templates/email');
    const files = fs.readdirSync(templateDir);
    for (const file of files) {
      const content = fs.readFileSync(path.join(templateDir, file), 'utf-8');
      const name = path.basename(file, '.hbs');
      this.templates.set(name, handlebars.compile(content));
    }
  }

  private render(templateName: string, context: any): string {
    const template = this.templates.get(templateName);
    if (!template) throw new Error(`Template ${templateName} tidak ditemukan`);
    return template(context);
  }

  async sendWelcome(to: string, name: string) {
    const html = this.render('welcome', {
      name,
      message: 'Terima kasih telah mendaftar. Klik tombol di bawah untuk memulai.',
      actionUrl: `${this.config.get('APP_URL')}/dashboard`,
      actionText: 'Mulai Sekarang',
      appName: 'MyApp',
    });

    await this.transporter.sendMail({
      from: this.config.get('EMAIL_FROM'),
      to,
      subject: 'Selamat Datang di MyApp!',
      html,
    });
  }

  async sendVerification(to: string, name: string, token: string) {
    const html = this.render('welcome', {
      name,
      message: 'Verifikasi email Anda untuk mengaktifkan akun.',
      actionUrl: `${this.config.get('APP_URL')}/verify?token=${token}`,
      actionText: 'Verifikasi Email',
      appName: 'MyApp',
    });

    await this.transporter.sendMail({
      from: this.config.get('EMAIL_FROM'),
      to,
      subject: 'Verifikasi Email Anda',
      html,
    });
  }

  async sendResetPassword(to: string, name: string, token: string) {
    // Template berbeda untuk reset password
    const html = this.render('reset-password', {
      name,
      actionUrl: `${this.config.get('APP_URL')}/reset-password?token=${token}`,
      appName: 'MyApp',
    });

    await this.transporter.sendMail({
      from: this.config.get('EMAIL_FROM'),
      to,
      subject: 'Reset Password',
      html,
    });
  }

  async sendOrderConfirmation(to: string, name: string, order: any) {
    const html = this.render('order-confirmation', {
      name,
      orderId: order.id,
      items: order.items,
      total: order.total,
    });

    await this.transporter.sendMail({
      from: this.config.get('EMAIL_FROM'),
      to,
      subject: `Pesanan #${order.id} Dikonfirmasi`,
      html,
    });
  }
}
```

### 5. Integrasi dengan Queue

```typescript
// email.processor.ts
@Processor('email')
export class EmailProcessor extends WorkerHost {
  constructor(private emailService: EmailService) {
    super();
  }

  async process(job: Job): Promise<any> {
    const { type, data } = job.data;

    switch (type) {
      case 'welcome':
        await this.emailService.sendWelcome(data.to, data.name);
        break;
      case 'verification':
        await this.emailService.sendVerification(data.to, data.name, data.token);
        break;
      case 'reset-password':
        await this.emailService.sendResetPassword(data.to, data.name, data.token);
        break;
      case 'order-confirmation':
        await this.emailService.sendOrderConfirmation(data.to, data.name, data.order);
        break;
      default:
        throw new Error(`Unknown email type: ${type}`);
    }
  }
}

// auth.service.ts — contoh pemanggilan
await this.emailQueue.add('send-email', {
  type: 'welcome',
  data: { to: user.email, name: user.name },
});
```

## Analogi
Email template adalah **cetakan brosur** di percetakan gedung:
- **Template welcome** = Cetakan "Brosur Selamat Datang" — tinggal isi nama tamu baru
- **Template verifikasi** = Cetakan "Kartu Akses Sementara" — tinggal isi kode PIN
- **Template reset password** = Cetakan "Formulir Lupa Kunci Lemari" — tinggal isi nomor lemari
- **EmailService reusable** = **Mesin cetak otomatis** — tinggal pilih template, isi data, klik cetak

## Dipakai untuk Apa
- Email transaksional (welcome, verifikasi, reset password)
- Email order/invoice
- Email notifikasi sistem
- Semua jenis email yang butuh format HTML konsisten

## Kesalahan Umum
- **Path template salah** — error `Template not found` saat runtime
- **Tidak compile template handlebars** — variable `{{name}}` muncul mentah di email
- **HTML tidak responsif** — rusak di Gmail/Outlook mobile
- **Inline CSS tidak lengkap** — banyak email client hapus `<style>` tag
- **Tidak handle error rendering** — data tidak lengkap bikin template crash

## Soal Latihan

**Soal:**
Buat 3 template email (welcome, verification, reset-password) menggunakan Handlebars. Implementasikan EmailService dengan method spesifik untuk masing-masing template. Gunakan queue untuk mengirim.

**Jawaban:**

**Template `welcome.hbs`:**
```html
<h1>Halo {{name}}!</h1>
<p>Selamat datang di {{appName}}. Akun Anda siap digunakan.</p>
<a href="{{dashboardUrl}}">Masuk Dashboard</a>
```

**Template `verification.hbs`:**
```html
<h1>Verifikasi Email</h1>
<p>Klik link di bawah untuk verifikasi:</p>
<a href="{{verifyUrl}}">Verifikasi Sekarang</a>
<p>Link berlaku 24 jam.</p>
```

**Template `reset-password.hbs`:**
```html
<h1>Reset Password</h1>
<p>Klik link untuk reset password:</p>
<a href="{{resetUrl}}">Reset Password</a>
<p>Jika tidak meminta reset, abaikan email ini.</p>
```

**EmailService:**
```typescript
@Injectable()
export class EmailService {
  private transporter: Transporter;

  constructor() {
    this.transporter = nodemailer.createTransport({ host: 'localhost', port: 1025 });
    this.registerPartials();
  }

  private render(tpl: string, ctx: any) {
    const source = fs.readFileSync(`templates/email/${tpl}.hbs`, 'utf-8');
    return handlebars.compile(source)(ctx);
  }

  async sendWelcome(to: string, name: string) {
    const html = this.render('welcome', { name, appName: 'MyApp', dashboardUrl: 'http://localhost:3000/dashboard' });
    await this.transporter.sendMail({ from: 'noreply@myapp.com', to, subject: 'Welcome!', html });
  }

  async sendVerification(to: string, token: string) {
    const html = this.render('verification', { verifyUrl: `http://localhost:3000/verify?token=${token}` });
    await this.transporter.sendMail({ from: 'noreply@myapp.com', to, subject: 'Verifikasi Email', html });
  }

  async sendResetPassword(to: string, token: string) {
    const html = this.render('reset-password', { resetUrl: `http://localhost:3000/reset?token=${token}` });
    await this.transporter.sendMail({ from: 'noreply@myapp.com', to, subject: 'Reset Password', html });
  }
}
```
