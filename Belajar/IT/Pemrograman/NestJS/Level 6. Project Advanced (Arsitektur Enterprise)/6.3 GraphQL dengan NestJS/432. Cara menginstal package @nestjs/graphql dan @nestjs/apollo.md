# 432. Cara menginstal package @nestjs/graphql dan @nestjs/apollo

## Definisi & Target
Package `@nestjs/graphql` menyediakan integrasi GraphQL untuk NestJS dengan decorator-based API. Package `@nestjs/apollo` adalah driver untuk Apollo Server v4 yang menjalankan GraphQL server. Kedua package ini wajib diinstall bersama `graphql` (core library) dan `apollo-server-express` (Express integration) untuk setup GraphQL di NestJS.

## Prasyarat
- Node.js v18+ dan npm/yarn/pnpm
- Project NestJS existing (`nest new project-name`)
- TypeScript configured (default di NestJS CLI)

## Komponen & Nest CLI Command
```bash
# Core packages
npm install @nestjs/graphql @nestjs/apollo graphql apollo-server-express

# Optional: GraphQL tools untuk schema manipulation
npm install graphql-tools

# Development dependencies
npm install -D @types/graphql

# Nest CLI commands
nest g module graphql
nest g resolver user
nest g service user
```

## Langkah-langkah Implementasi (Step-by-Step)
1. Install core packages: `@nestjs/graphql`, `@nestjs/apollo`, `graphql`, `apollo-server-express`
2. Install optional packages untuk development: `graphql-tools`, `@types/graphql`
3. Verify installation di package.json dependencies
4. Import GraphQLModule di AppModule
5. Konfigurasi driver ApolloDriver

## Kode Implementasi (Per File)

### package.json (dependencies section)
```json
{
  "dependencies": {
    "@nestjs/apollo": "^12.0.0",
    "@nestjs/graphql": "^12.0.0",
    "@nestjs/common": "^10.0.0",
    "@nestjs/core": "^10.0.0",
    "apollo-server-express": "^3.12.0",
    "graphql": "^16.8.0",
    "reflect-metadata": "^0.2.0",
    "rxjs": "^7.8.0"
  },
  "devDependencies": {
    "@types/graphql": "^14.5.0",
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0"
  }
}
```

### main.ts (bootstrap dengan GraphQL)
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.enableCors({
    origin: ['http://localhost:3000', 'https://studio.apollographql.com'],
    credentials: true,
  });
  await app.listen(3000);
  console.log(`🚀 Server ready at http://localhost:3000/graphql`);
}
bootstrap();
```

### app.module.ts (minimal config)
```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';
import { join } from 'path';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
      playground: process.env.NODE_ENV !== 'production',
      introspection: process.env.NODE_ENV !== 'production',
    }),
  ],
})
export class AppModule {}
```

### tsconfig.json (penting untuk GraphQL decorators)
```json
{
  "compilerOptions": {
    "target": "ES2021",
    "module": "commonjs",
    "lib": ["ES2021"],
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "baseUrl": "./src",
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## Konfigurasi & Integrasi
- **Driver**: `ApolloDriver` dari `@nestjs/apollo` (recommended untuk Apollo Server v4)
- **autoSchemaFile**: Path file schema generated (code-first) atau `true` untuk memory-only
- **playground**: Enable GraphQL Playground di development
- **introspection**: Enable schema introspection untuk tools seperti Apollo Studio
- **cors**: Konfigurasi CORS untuk playground dan client external

## Cara Pengujian & Verifikasi
1. Jalankan `npm install` setelah menambah dependencies
2. Jalankan `npm run start:dev`
3. Buka `http://localhost:3000/graphql` - harus muncul GraphQL Playground
4. Test query introspection:
```graphql
query Introspection {
  __schema {
    types {
      name
    }
  }
}
```
5. Cek file `src/schema.gql` generated otomatis

## Troubleshooting & Common Issues
1. **Cannot find module '@nestjs/apollo'**: Jalankan `npm install` ulang, pastikan package.json terupdate
2. **Decorator metadata error**: Pastikan `emitDecoratorMetadata: true` dan `experimentalDecorators: true` di tsconfig.json
3. **Apollo Server v4 breaking changes**: Gunakan `@nestjs/apollo` v12+ yang compatible dengan Apollo Server v4
4. **Schema not generating**: Pastikan `autoSchemaFile` path valid dan folder writable

## Best Practices
- Lock versions di package.json untuk konsistensi tim
- Gunakan ApolloDriver (bukan MercuryDriver yang deprecated)
- Disable playground dan introspection di production
- Gunakan environment variables untuk config yang beda per env
- Update packages secara berkala untuk security patches

## Referensi
- [NestJS GraphQL Installation](https://docs.nestjs.com/graphql/quick-start#installation)
- [Apollo Server v4 Migration](https://www.apollographql.com/docs/apollo-server/migration/)
- [@nestjs/apollo Changelog](https://github.com/nestjs/nest/tree/master/packages/apollo)