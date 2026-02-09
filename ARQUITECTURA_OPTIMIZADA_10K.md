# Arquitectura Optimizada - 10K Usuarios con Planes FREE

## CONSIDERACIONES CLAVE

- **Escala objetivo**: 10,000 usuarios activos
- **Presupuesto**: Maximizar uso de planes gratuitos
- **Stack**: Herramientas modernas optimizadas
- **Escalabilidad**: Preparado para crecer sin refactoring mayor

---

## STACK TECNOLÓGICO MODERNO (2026)

### Frontend Web
```typescript
Framework: Next.js 15 (App Router)
Lenguaje: TypeScript (strict mode)
Styling: Tailwind CSS v4
Componentes: shadcn/ui
Estado: Zustand + TanStack Query
Forms: React Hook Form + Zod
Testing: Vitest + Playwright
```

**Justificación del stack:**
- Excelente soporte para TypeScript
- shadcn/ui se integra perfectamente (componentes copiables)
- Zod para validación type-safe
- Stack muy documentado y con gran ecosistema

### Backend
```typescript
Framework: Next.js API Routes + Server Actions (todo en uno)
Alternative: Hono + Cloudflare Workers (100% free hasta 100K req/día)
ORM: Drizzle ORM (type-safe y SQL-like)
Validación: Zod (compartido con frontend)
Auth: Better-auth o Clerk (Auth.js también)
```

**Justificación:**
- Drizzle ORM: SQL-like, type-safe, excelente developer experience
- Next.js Server Actions: Menos código, más productivo
- Zod: Reutilizable frontend/backend
- Hono: Super ligero, sintaxis moderna, excelente DX

### Base de Datos para 10K Usuarios

**Opción 1: Supabase (Recomendado)**
```
Plan FREE incluye:
- PostgreSQL 500 MB (suficiente para inicio)
- 2GB de transferencia/mes
- Realtime subscriptions
- Auth built-in
- Storage 1GB
- 50K usuarios activos mensuales

Escala: $25/mes (8GB database, más recursos)
```

**Opción 2: Neon (PostgreSQL Serverless)**
```
Plan FREE incluye:
- PostgreSQL 0.5GB
- Branching (git para BD)
- Auto-scaling
- Suspend automático (ahorra recursos)

Escala: $19/mes (3GB)
```

**Opción 3: Turso (SQLite edge)**
```
Plan FREE incluye:
- 9GB de storage
- 500 databases
- Unlimited reads
- Edge replication

Escala: $29/mes
```

**Recomendación para 10K usuarios**: **Neon** ($19/mes) o **Supabase** ($25/mes)

### Cache / Redis

**Opción 1: Upstash Redis (Recomendado)**
```
Plan FREE incluye:
- 10K comandos/día
- Max 256 MB
- Global edge locations
- Durable storage

Escala: Pay-as-you-go ($0.20 por 100K comandos)
```

**Opción 2: Vercel KV (powered by Upstash)**
```
Hobby: 30K comandos/mes free
Pro: 500K comandos incluidos
```

### Storage (Archivos/Imágenes)

**Opción 1: Cloudflare R2**
```
Plan FREE incluye:
- 10GB storage
- 1 millón de operaciones clase A/mes
- 10 millones operaciones clase B/mes
- SIN costo de egreso (bandwidth)

Costo escala: $0.015/GB/mes
```

**Opción 2: Supabase Storage (si usas Supabase)**
```
FREE: 1GB
Pro: 100GB
```

### Hosting

**Frontend: Vercel (FREE tier es generoso)**
```
Hobby FREE incluye:
- 100GB bandwidth/mes
- Deployments ilimitados
- Preview deploys
- Edge Functions
- Serverless Functions (100GB-hrs)
- Dominios custom
- SSL automático

Limitación: 1 developer, proyectos personales
```

**Backend API: Render.com**
```
FREE tier incluye:
- Web services (sleep después 15 min inactividad)
- 750 horas/mes (suficiente para 1 servicio 24/7)
- HTTPS automático
- Auto-deploy desde Git
- PostgreSQL 90 días free, luego $7/mes

Escala: $7/mes (siempre activo)
```

**Backend Alternativo: Cloudflare Workers**
```
FREE tier incluye:
- 100,000 requests/día
- 10ms CPU time por request
- Workers KV
- Cron triggers

Escala: $5/mes (10 millones requests)
```

**Recomendación 10K usuarios**:
- Frontend: Vercel Hobby (gratis)
- Backend: Render $7/mes o Cloudflare Workers $5/mes

---

## ARQUITECTURA PROPUESTA (10K Usuarios - Servicio Comunitario)

```
┌────────────────────────────────────────────────────┐
│            USUARIOS (10K) - Sin Comisiones         │
│                                                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐          │
│  │   Web    │  │  Mobile  │  │  Admin   │          │
│  │ (Next.js)│  │  (Expo)  │  │ (Next.js)│          │
│  └──────────┘  └──────────┘  └──────────┘          │
└─────────┬──────────────┬──────────────┬────────────┘
          │              │              │
          └──────────────┼──────────────┘
                         │
┌────────────────────────▼─────────────────────────┐
│           Cloudflare (CDN + DDoS)                │
│              Cache, SSL, Firewall                │
└────────────────────────┬─────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────┐
│              Vercel (Frontend Host)              │
│                                                  │
│  • Next.js App (SSR + SSG)                       │
│  • Edge Functions                                │
│  • Serverless Functions                          │
│  • Automatic scaling                             │
└────────────────────────┬─────────────────────────┘
                         │
                         │ API Calls
                         │
┌────────────────────────▼─────────────────────────┐
│    Backend API (Render $7/mes o CF Workers)      │
│                                                  │
│  Opción A: Next.js API Routes (mismo repo)       │
│  Opción B: Hono + Cloudflare Workers             │
│  Opción C: Node.js + Express en Render           │
│                                                  │
│  Módulos:                                        │
│  • Auth (Better-auth)                            │
│  • Users CRUD                                    │
│  • Professionals                                 │
│  • Bookings (pago directo profesional)           │
│  • Payments (Mercado Pago - sin comisión app)    │
│  • Messaging                                     │
│  • Notifications                                 │
└────────┬─────────────────┬─────────────┬─────────┘
         │                 │             │
         │                 │             │
┌────────▼──────┐  ┌──────▼──────┐  ┌──▼────────┐
│  Neon/Supabase│  │   Upstash   │  │Cloudflare │
│  PostgreSQL   │  │    Redis    │  │    R2     │
│               │  │             │  │           │
│  • Users      │  │  • Sessions │  │ • Images  │
│  • Bookings   │  │  • Cache    │  │ • Docs    │
│  • Messages   │  │  • Queue    │  │           │
│  • Reviews    │  │             │  │           │
└───────────────┘  └─────────────┘  └───────────┘

┌──────────────────────────────────────────────────┐
│              Servicios Externos                  │
├──────────────────────────────────────────────────┤
│  • Mercado Pago (Payments - comisión solo MP)    │
│  • Resend (Email) - 3K/mes free                  │
│  • Firebase FCM (Push) - Free                    │
│  • Sentry (Errors) - 5K events/mes free          │
└──────────────────────────────────────────────────┘
```

---

## COSTOS MENSUALES REALES (10K Usuarios)

### Fase Inicial (0-1K usuarios)
| Servicio            | Plan              | Costo          |
|---------------------|-------------------|----------------|
| Vercel              | Hobby             | **$0**         |
| Render (Backend)    | Free              | **$0**         |
| Neon PostgreSQL     | Free              | **$0**         |
| Upstash Redis       | Free              | **$0**         |
| Cloudflare R2       | Free              | **$0**         |
| Cloudflare CDN      | Free              | **$0**         |
| Resend (Email)      | Free (3K/mes)     | **$0**         |
| Firebase FCM        | Free              | **$0**         |
| Sentry              | Free (5K/mes)     | **$0**         |
| Dominio .com.ar     |                   | **$2**         |
| **TOTAL FASE 1**    |                   | **$2/mes**     |

### Fase Crecimiento (1K-5K usuarios)
| Servicio            | Plan                | Costo            |
|---------------------|---------------------|------------------|
| Vercel              | Hobby               | **$0**           |
| Render (Backend)    | Starter (always on) | **$7**           |
| Neon PostgreSQL     | Scale               | **$19**          |
| Upstash Redis       | Pay-as-you-go       | **$5-10**        |
| Cloudflare R2       | Pay-as-you-go       | **$2-5**         |
| Cloudflare CDN      | Free                | **$0**           |
| Resend (Email)      | Grow                | **$20**          |
| Firebase FCM        | Free                | **$0**           |
| Sentry              | Team                | **$26**          |
| Dominio             |                     | **$2**           |
| **TOTAL FASE 2**    |                     | **$81-89/mes**   |

### Fase Escala (5K-10K usuarios)
| Servicio            | Plan       | Costo              |
|---------------------|------------|--------------------|
| Vercel              | Pro        | **$20**            |
| Render (Backend)    | Standard   | **$25**            |
| Neon PostgreSQL     | Scale      | **$19-40**         |
| Upstash Redis       | Scale      | **$15-25**         |
| Cloudflare R2       |            | **$5-10**          |
| Cloudflare CDN      | Pro        | **$20**            |
| Resend (Email)      |            | **$40**            |
| Firebase FCM        |            | **$0-10**          |
| Sentry              | Team       | **$26**            |
| Dominio             |            | **$2**             |
| **TOTAL FASE 3**    |            | **$172-218/mes**   |

### Fase Consolidación (10K+ usuarios)
| Servicio            | Plan       | Costo              |
|---------------------|------------|--------------------|
| Vercel              | Pro        | **$20**            |
| Render              | Pro        | **$85**            |
| Neon/Supabase       | Pro        | **$50-100**        |
| Upstash Redis       |            | **$30-50**         |
| Cloudflare R2       |            | **$15-30**         |
| Cloudflare CDN      | Pro        | **$20**            |
| Resend              |            | **$80-150**        |
| Firebase FCM        |            | **$10-20**         |
| Sentry              | Business   | **$80**            |
| **TOTAL 10K**       |            | **$390-565/mes**   |

---

## STACK TÉCNICO DETALLADO

### 1. Monorepo Structure (Turborepo)

```
gl-app/
├── apps/
│   ├── web/                    # Next.js app principal
│   │   ├── app/                # App Router
│   │   │   ├── (auth)/
│   │   │   │   ├── login/
│   │   │   │   └── register/
│   │   │   ├── (dashboard)/
│   │   │   │   ├── search/
│   │   │   │   ├── profile/
│   │   │   │   └── bookings/
│   │   │   ├── api/            # API Routes
│   │   │   │   ├── auth/
│   │   │   │   ├── users/
│   │   │   │   └── webhooks/
│   │   │   └── layout.tsx
│   │   ├── components/
│   │   │   ├── ui/            # shadcn/ui
│   │   │   └── features/
│   │   └── lib/
│   │       ├── db/            # Drizzle
│   │       ├── auth/          # Better-auth
│   │       └── utils/
│   │
│   ├── mobile/                # Expo app
│   │   ├── app/               # Expo Router
│   │   ├── components/
│   │   └── lib/
│   │
│   └── admin/                 # Admin panel
│       └── (similar structure)
│
├── packages/
│   ├── db/                    # Drizzle schema compartido
│   │   ├── schema/
│   │   │   ├── users.ts
│   │   │   ├── professionals.ts
│   │   │   ├── bookings.ts
│   │   │   └── index.ts
│   │   └── index.ts
│   │
│   ├── validators/            # Zod schemas compartidos
│   │   ├── auth.ts
│   │   ├── user.ts
│   │   └── booking.ts
│   │
│   ├── ui/                    # Componentes compartidos
│   │   └── components/
│   │
│   └── config/                # Config compartido
│       ├── tailwind/
│       └── typescript/
│
├── turbo.json
├── package.json
└── pnpm-workspace.yaml
```

### 2. Drizzle ORM (Type-safe y SQL-like)

```typescript
// packages/db/schema/users.ts
import { pgTable, uuid, varchar, timestamp, boolean } from 'drizzle-orm/pg-core';

export const users = pgTable('users', {
  id: uuid('id').defaultRandom().primaryKey(),
  idMasonico: varchar('id_masonico', { length: 50 }).unique().notNull(),
  email: varchar('email', { length: 255 }).unique().notNull(),
  passwordHash: varchar('password_hash', { length: 255 }).notNull(),
  firstName: varchar('first_name', { length: 100 }).notNull(),
  lastName: varchar('last_name', { length: 100 }).notNull(),
  phone: varchar('phone', { length: 20 }),
  logia: varchar('logia', { length: 255 }),
  role: varchar('role', { length: 20 }).default('user'),
  verified: boolean('verified').default(false),
  active: boolean('active').default(true),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});

export type User = typeof users.$inferSelect;
export type NewUser = typeof users.$inferInsert;
```

**Justificación de Drizzle:**
- Type-safe al 100%
- SQL-like (más intuitivo)
- Excelente autocompletado TypeScript
- Migrations en TypeScript
- Más ligero que Prisma
- Mejor performance

### 3. Zod Validators (Compartidos)

```typescript
// packages/validators/auth.ts
import { z } from 'zod';

export const registerSchema = z.object({
  idMasonico: z.string().min(3).max(50),
  email: z.string().email(),
  password: z.string().min(8).max(100),
  firstName: z.string().min(2).max(100),
  lastName: z.string().min(2).max(100),
  phone: z.string().optional(),
  logia: z.string().min(2).max(255),
});

export type RegisterInput = z.infer<typeof registerSchema>;

export const loginSchema = z.object({
  email: z.string().email(),
  password: z.string().min(1),
});

export type LoginInput = z.infer<typeof loginSchema>;
```

Usar en frontend:
```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { registerSchema, type RegisterInput } from '@repo/validators/auth';

const form = useForm<RegisterInput>({
  resolver: zodResolver(registerSchema),
});
```

Usar en backend:
```typescript
import { registerSchema } from '@repo/validators/auth';

export async function POST(req: Request) {
  const body = await req.json();
  const data = registerSchema.parse(body); // Auto-validated!
  // ...
}
```

### 4. Better-auth (Auth Simple y Moderno)

```typescript
// apps/web/lib/auth/auth.ts
import { betterAuth } from 'better-auth';
import { drizzleAdapter } from 'better-auth/adapters/drizzle';
import { db } from '@repo/db';

export const auth = betterAuth({
  database: drizzleAdapter(db, {
    provider: 'pg',
  }),
  emailAndPassword: {
    enabled: true,
    requireEmailVerification: true,
  },
  session: {
    expiresIn: 60 * 60 * 24 * 7, // 7 days
    updateAge: 60 * 60 * 24, // 1 day
  },
});

export type Session = typeof auth.$Infer.Session;
```

**Justificación de Better-auth:**
- Más moderno que NextAuth.js
- Type-safe
- Funciona con cualquier framework
- Soporta múltiples DBs
- Plugins extensibles

### 5. shadcn/ui (Componentes reutilizables)

```bash
# Inicializar
npx shadcn@latest init

# Agregar componentes según requerimientos
npx shadcn@latest add button
npx shadcn@latest add form
npx shadcn@latest add card
npx shadcn@latest add dialog
npx shadcn@latest add dropdown-menu
npx shadcn@latest add avatar
npx shadcn@latest add badge
npx shadcn@latest add input
npx shadcn@latest add label
npx shadcn@latest add textarea
npx shadcn@latest add select
npx shadcn@latest add tabs
npx shadcn@latest add toast
```

**Justificación:**
- Sin dependencia externa (componentes integrados al proyecto)
- Totalmente customizable
- Tailwind CSS
- Accessible (Radix UI)
- Estructura clara y type-safe

### 6. TanStack Query (Data Fetching)

```typescript
// apps/web/lib/api/client.ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 60 * 1000, // 1 min
      retry: 1,
    },
  },
});

// Hook ejemplo
export function useProfessionals(category?: string) {
  return useQuery({
    queryKey: ['professionals', category],
    queryFn: () => fetchProfessionals(category),
  });
}
```

---

## AUTENTICACIÓN COMPLETA CON BETTER-AUTH

### Setup

```typescript
// apps/web/app/api/auth/[...all]/route.ts
import { auth } from '@/lib/auth';

export const { GET, POST } = auth.handler;
```

### Server Component

```typescript
// apps/web/app/(dashboard)/profile/page.tsx
import { auth } from '@/lib/auth';
import { headers } from 'next/headers';

export default async function ProfilePage() {
  const session = await auth.api.getSession({
    headers: await headers(),
  });

  if (!session) {
    redirect('/login');
  }

  return (
    <div>
      <h1>Bienvenido, {session.user.firstName}</h1>
    </div>
  );
}
```

### Client Component

```typescript
// apps/web/components/auth/login-form.tsx
'use client';

import { useAuth } from '@/lib/auth/client';

export function LoginForm() {
  const { signIn, isPending } = useAuth();

  const onSubmit = async (data: LoginInput) => {
    await signIn.email({
      email: data.email,
      password: data.password,
      callbackURL: '/dashboard',
    });
  };

  return <form onSubmit={handleSubmit(onSubmit)}>...</form>;
}
```

---

## INTEGRACIÓN MERCADO PAGO (Simplificada)

```typescript
// apps/web/app/api/payments/create-preference/route.ts
import { MercadoPagoConfig, Preference } from 'mercadopago';

const client = new MercadoPagoConfig({
  accessToken: process.env.MP_ACCESS_TOKEN!,
});

export async function POST(req: Request) {
  const { bookingId, amount, description } = await req.json();

  const preference = new Preference(client);
  
  const result = await preference.create({
    body: {
      items: [
        {
          id: bookingId,
          title: description,
          quantity: 1,
          unit_price: amount,
        },
      ],
      back_urls: {
        success: `${process.env.NEXT_PUBLIC_APP_URL}/bookings/${bookingId}/success`,
        failure: `${process.env.NEXT_PUBLIC_APP_URL}/bookings/${bookingId}/failure`,
        pending: `${process.env.NEXT_PUBLIC_APP_URL}/bookings/${bookingId}/pending`,
      },
      notification_url: `${process.env.NEXT_PUBLIC_APP_URL}/api/webhooks/mercadopago`,
      auto_return: 'approved',
    },
  });

  return Response.json({
    checkoutUrl: result.init_point,
    preferenceId: result.id,
  });
}
```

### Webhook Handler

```typescript
// apps/web/app/api/webhooks/mercadopago/route.ts
import { db } from '@repo/db';
import { bookings, transactions } from '@repo/db/schema';
import { eq } from 'drizzle-orm';

export async function POST(req: Request) {
  const body = await req.json();

  // Validar firma
  const signature = req.headers.get('x-signature');
  if (!validateMPSignature(body, signature)) {
    return Response.json({ error: 'Invalid signature' }, { status: 401 });
  }

  const { type, data } = body;

  if (type === 'payment') {
    const paymentId = data.id;
    
    // Obtener detalles del pago
    const payment = await fetchPaymentDetails(paymentId);
    
    // Actualizar booking
    await db
      .update(bookings)
      .set({
        paymentStatus: payment.status,
        updatedAt: new Date(),
      })
      .where(eq(bookings.id, payment.external_reference));

    // Crear transacción
    await db.insert(transactions).values({
      bookingId: payment.external_reference,
      paymentProvider: 'mercadopago',
      paymentId: paymentId,
      amount: payment.transaction_amount,
      status: payment.status,
      metadata: payment,
    });
  }

  return Response.json({ received: true });
}
```

---

## MOBILE APP (Expo)

### Setup Expo Router

```typescript
// apps/mobile/app/_layout.tsx
import { Stack } from 'expo-router';
import { QueryClientProvider } from '@tanstack/react-query';
import { queryClient } from '@/lib/query-client';

export default function RootLayout() {
  return (
    <QueryClientProvider client={queryClient}>
      <Stack>
        <Stack.Screen name="(auth)" options={{ headerShown: false }} />
        <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      </Stack>
    </QueryClientProvider>
  );
}
```

### Reutilizar Validators

```typescript
// apps/mobile/app/(auth)/register.tsx
import { registerSchema, type RegisterInput } from '@repo/validators/auth';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';

export default function RegisterScreen() {
  const form = useForm<RegisterInput>({
    resolver: zodResolver(registerSchema),
  });

  // Mismo schema que web!
}
```

---

## CI/CD CON GITHUB ACTIONS (100% FREE)

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - uses: pnpm/action-setup@v3
        with:
          version: 9
      
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
      
      - run: pnpm install --frozen-lockfile
      
      - run: pnpm lint
      
      - run: pnpm type-check
      
      - run: pnpm test
      
      - name: Upload coverage
        uses: codecov/codecov-action@v4

  deploy-web:
    needs: lint-and-test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      # Vercel auto-deploys desde Git
      # No hace falta hacer nada aquí!
      
  deploy-api:
    needs: lint-and-test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      # Render auto-deploys desde Git
      # Tampoco hace falta hacer nada!
```

**Todo es automático!**
- Push a `main` → deploy automático
- Pull requests → preview deployments (Vercel)
- Tests corriendo en cada PR
- 100% gratis (GitHub Actions free para repos públicos)

---

## ESTIMACIÓN CAPACIDAD REAL (10K Usuarios)

### Límites del Stack FREE

| Recurso               | Free Tier    | 10K Usuarios           | ¿Alcanza?      |
|-----------------------|--------------|------------------------|----------------|
| **Vercel Bandwidth**  | 100GB/mes    | ~50GB/mes estimado     | SÍ             |
| **Render Compute**    | 750h/mes     | 720h/mes (24/7)        | SÍ             |
| **Neon Storage**      | 0.5GB        | 2-5GB necesario        | NO → $19/mes   |
| **Neon Compute**      | 0.25 vCPU    | 0.5-1 vCPU necesario   | NO → $19/mes   |
| **Upstash Redis**     | 10K cmds/día | 50-100K/día            | NO → $10/mes   |
| **Cloudflare R2**     | 10GB storage | 5-10GB                 | SÍ (border)    |
| **Resend Email**      | 3K/mes       | 5-10K/mes              | NO → $20/mes   |

### Costo Real Mínimo (10K usuarios activos)

**Mes 1-3 (0-1K usuarios)**: $2/mes (solo dominio)
**Mes 4-6 (1K-3K usuarios)**: $50-80/mes
**Mes 7-12 (3K-10K usuarios)**: $120-180/mes

**Promedio año 1**: ~$100/mes

---

## ROADMAP DE DESARROLLO

### Fase 0: Setup (Semana 1)

```bash
# Crear proyecto
pnpm create next-app@latest gl-app --typescript --tailwind --app
cd gl-app

# Instalar Turborepo
pnpm add -D turbo
pnpm dlx @turbo/gen workspace apps/mobile
pnpm dlx @turbo/gen workspace packages/db
pnpm dlx @turbo/gen workspace packages/validators

# Setup Drizzle
cd packages/db
pnpm add drizzle-orm postgres
pnpm add -D drizzle-kit

# Setup shadcn
cd ../../apps/web
pnpm dlx shadcn@latest init

# Setup auth
pnpm add better-auth

# Setup validación
cd ../../packages/validators
pnpm add zod

# Abrir el proyecto y comenzar!
```

### Fase 1: Auth (Semana 2-3)

**Pasos**:
1. Crear schema users en Drizzle
2. Setup Better-auth
3. Crear validators para register/login
4. Crear UI con shadcn (login, register)
5. Implementar server actions
6. Testing

**Sistema de autenticación completo**:
```
Complete authentication system with:
- Drizzle schema for users table
- Better-auth configuration
- Zod validators for registration and login
- shadcn/ui forms for login and register pages
- Email verification flow
- Session management
```

### Fase 2: Profesionales (Semana 4-5)

**Sistema de perfiles profesionales**:
```
Professional profiles system:
- Drizzle schema for professionals, services, categories
- API endpoints for CRUD operations
- Professional profile page with service listings
- Search and filter functionality
- shadcn/ui components for professional cards
- Image upload to Cloudflare R2
```

### Fase 3: Bookings (Semana 6-7)

```
Create booking system:
- Drizzle schema for bookings table
- Booking flow: request → accept → pay
- Mercado Pago integration for payments
- Status management (pending, paid, completed)
- Email notifications with Resend
- Booking history page
```

### Fase 4: Reviews y Chat (Semana 8-9)

```
Create reviews and messaging:
- Reviews system with ratings 1-5
- Real-time chat with Pusher or Supabase Realtime
- Notification system
- Review moderation
```

### Fase 5: Admin Panel (Semana 10)

```
Create admin dashboard:
- User management
- Professional verification
- Booking oversight
- Analytics and metrics
- Content moderation
```

**Total: 10 semanas con herramientas modernas** (vs 16-20 semanas manual)

---

## HERRAMIENTAS DE DESARROLLO ESENCIALES

### Extensions recomendadas
- Tailwind CSS IntelliSense
- Prisma (funciona con Drizzle también)
- ESLint
- Prettier

---

## package.json Principal

```json
{
  "name": "gl-app",
  "private": true,
  "scripts": {
    "dev": "turbo dev",
    "build": "turbo build",
    "lint": "turbo lint",
    "type-check": "turbo type-check",
    "test": "turbo test",
    "db:generate": "turbo db:generate",
    "db:migrate": "turbo db:migrate",
    "db:studio": "turbo db:studio"
  },
  "devDependencies": {
    "@types/node": "^20.11.0",
    "turbo": "^2.0.0",
    "typescript": "^5.3.3"
  },
  "packageManager": "pnpm@9.0.0"
}
```

---

## CHECKLIST FINAL

### Infraestructura FREE
- [ ] Vercel conectado a GitHub
- [ ] Render conectado a GitHub
- [ ] Neon database creada
- [ ] Upstash Redis configurado
- [ ] Cloudflare R2 bucket creado
- [ ] Dominio configurado
- [ ] SSL activo (automático)

### Servicios Externos
- [ ] Cuenta Mercado Pago (vendedor)
- [ ] API keys de Mercado Pago (test y prod)
- [ ] Cuenta Resend con dominio verificado
- [ ] Firebase project para FCM
- [ ] Sentry project creado

### Desarrollo
- [ ] Monorepo con Turborepo configurado
- [ ] Drizzle ORM con schema inicial
- [ ] Better-auth configurado
- [ ] shadcn/ui inicializado
- [ ] GitHub Actions workflow activo
- [ ] Reglas de proyecto configuradas

---

## CONCLUSIÓN

Con este stack moderno y aprovechando planes FREE:

**Costo inicial**: $0-50/mes (primeros 6 meses)
**Costo con 10K usuarios**: $120-180/mes
**Desarrollo eficiente** con herramientas modernas
**Type-safety 100%** (TypeScript + Drizzle + Zod)
**Escalable** hasta 50K+ usuarios sin cambios mayores
**Moderno** (stack 2026)
**DX excelente** (optimizado para productividad)
**Sin comisiones propias** - Servicio ad honorem para la comunidad

**Ahorro vs stack anterior**: 
- Infraestructura: 60-70% menos
- Desarrollo: 40% más rápido
- Mantenimiento: Más simple
- **Modelo**: Servicio comunitario sin fines de lucro

---

## CRÉDITOS DEL PROYECTO

**Idea original del proyecto:**
Homero Bonafert y Mario Garberoglio

**Desarrollo técnico y conceptualización de soluciones:**
Juan Martin Marina - Desarrollador Senior Full-Stack
- Arquitectura y diseño técnico completo
- Ideación de soluciones tecnológicas
- Desarrollo full-stack de la plataforma
- Trabajo ad honorem para la comunidad

**Propósito:**
Plataforma de servicios profesionales sin fines de lucro para fortalecer la comunidad masónica argentina.

---

**Versión**: 1.0 - Stack Optimizado Ad Honorem
**Fecha**: Febrero 2026

---

**Próximo paso**: Iniciar el setup del proyecto según la Fase 0 del roadmap.
