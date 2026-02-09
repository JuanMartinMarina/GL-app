# Arquitectura Detallada - App GL Argentina

## 1. DIAGRAMA DE ARQUITECTURA GENERAL

```
┌─────────────────────────────────────────────────────────────────┐
│                        CAPA DE CLIENTES                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │   Web App    │  │  Mobile App  │  │ Admin Panel  │           │
│  │              │  │              │  │              │           │
│  │   Next.js    │  │React Native  │  │   Next.js    │           │
│  │   (SSR/CSR)  │  │   + Expo     │  │              │           │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘           │
│         │                 │                  │                  │
└─────────┼─────────────────┼──────────────────┼──────────────────┘
          │                 │                  │
          └─────────────────┼──────────────────┘
                            │
┌──────────────────────────▼────────────────────────────────────────┐
│                    CAPA DE API GATEWAY                            │
├───────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐     │
│  │              Cloudflare / AWS API Gateway                │     │
│  │  • Rate Limiting                                         │     │
│  │  • DDoS Protection                                       │     │
│  │  • SSL/TLS Termination                                   │     │
│  │  • Request Routing                                       │     │
│  └─────────────────────┬────────────────────────────────────┘     │
│                        │                                          │
└────────────────────────┼──────────────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────────────┐
│                   CAPA DE APLICACIÓN (Backend)                   │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐   │
│  │                   NestJS API Server                       │   │
│  │                                                           │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐    │   │
│  │  │  Auth    │  │  Users   │  │Profession│  │ Booking │    │   │
│  │  │ Service  │  │ Service  │  │  Service │  │ Service │    │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └─────────┘    │   │
│  │                                                           │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐    │   │
│  │  │ Payment  │  │Messaging │  │  Search  │  │ Notif.  │    │   │
│  │  │ Service  │  │ Service  │  │ Service  │  │ Service │    │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └─────────┘    │   │
│  │                                                           │   │
│  └────────┬───────────────────────────────────────┬──────────┘   │
│           │                                       │              │
└───────────┼───────────────────────────────────────┼──────────────┘
            │                                       │
            │                                       │
┌───────────▼────────────────────┐      ┌───────────▼──────────────┐
│   CAPA DE DATOS                │      │   SERVICIOS EXTERNOS     │
├────────────────────────────────┤      ├──────────────────────────┤
│                                │      │                          │
│  ┌──────────────────────┐      │      │  ┌──────────────────┐    │
│  │   PostgreSQL 15+     │      │      │  │  Mercado Pago    │    │
│  │                      │      │      │  │  (Pagos)         │    │
│  │  • Users             │      │      │  └──────────────────┘    │
│  │  • Professionals     │      │      │                          │
│  │  • Services          │      │      │  ┌──────────────────┐    │
│  │  • Bookings          │      │      │  │  Firebase FCM    │    │
│  │  • Transactions      │      │      │  │  (Push Notif)    │    │
│  │  • Messages          │      │      │  └──────────────────┘    │
│  │  • Reviews           │      │      │                          │
│  └──────────────────────┘      │      │  ┌──────────────────┐    │
│                                │      │  │  Resend/SendGrid │    │
│  ┌──────────────────────┐      │      │  │  (Email)         │    │
│  │      Redis 7+        │      │      │  └──────────────────┘    │
│  │                      │      │      │                          │
│  │  • Session Cache     │      │      │  ┌──────────────────┐    │
│  │  • Rate Limiting     │      │      │  │  Twilio          │    │
│  │  • Job Queue         │      │      │  │  (SMS 2FA)       │    │
│  │  • Real-time Data    │      │      │  └──────────────────┘    │
│  └──────────────────────┘      │      │                          │
│                                │      │  ┌──────────────────┐    │
│  ┌──────────────────────┐      │      │  │  Cloudflare R2   │    │
│  │  Object Storage      │      │      │  │  (Files/Images)  │    │
│  │  (S3/R2)             │      │      │  └──────────────────┘    │
│  │                      │      │      │                          │
│  │  • Profile Images    │      │      │  ┌──────────────────┐    │
│  │  • Certificates      │      │      │  │  Sentry          │    │
│  │  • Documents         │      │      │  │  (Monitoring)    │    │
│  └──────────────────────┘      │      │  └──────────────────┘    │
│                                │      │                          │
└────────────────────────────────┘      └──────────────────────────┘
```

---

## 2. FLUJO DE AUTENTICACIÓN

```
┌──────────┐                                           ┌──────────┐
│  Client  │                                           │  Server  │
└────┬─────┘                                           └────┬─────┘
     │                                                      │
     │  1. POST /auth/register                              │
     │     { id_masonico, email, password }                 │
     ├─────────────────────────────────────────────────────>│
     │                                                      │
     │                              2. Validar ID masónico  │
     │                                 (GL Database/API)    │
     │                                                      │
     │                              3. Hash password        │
     │                                 (bcrypt)             │
     │                                                      │
     │                              4. Crear usuario en DB  │
     │                                                      │
     │                              5. Enviar email         │
     │                                 verificación         │
     │                                                      │
     │  6. { user_id, email }                               │
     │<─────────────────────────────────────────────────────┤
     │                                                      │
     │  7. Click en email de verificación                   │
     │     GET /auth/verify?token=XXX                       │
     ├─────────────────────────────────────────────────────>│
     │                                                      │
     │                              8. Verificar token      │
     │                              9. Actualizar DB        │
     │                                                      │
     │  10. { verified: true }                              │
     │<─────────────────────────────────────────────────────┤
     │                                                      │
     │  11. POST /auth/login                                │
     │      { email, password }                             │
     ├─────────────────────────────────────────────────────>│
     │                                                      │
     │                              12. Buscar usuario      │
     │                              13. Verificar password  │
     │                              14. Generar JWT         │
     │                                  - Access (15min)    │
     │                                  - Refresh (30d)     │
     │                              15. Guardar refresh     │
     │                                  token en Redis      │
     │                                                      │
     │  16. {                                               │
     │       access_token: "eyJhbGc...",                    │
     │       refresh_token: "eyJhbGc...",                   │
     │       user: { id, email, role }                      │
     │      }                                               │
     │<─────────────────────────────────────────────────────┤
     │                                                      │
     │  17. Requests subsiguientes                          │
     │      Header: Authorization: Bearer <access_token>    │
     ├─────────────────────────────────────────────────────>│
     │                                                      │
     │                              18. Validar JWT         │
     │                              19. Procesar request    │
     │                                                      │
     │  20. Response                                        │
     │<─────────────────────────────────────────────────────┤
     │                                                      │
     │  21. Access token expirado                           │
     │      POST /auth/refresh                              │
     │      { refresh_token }                               │
     ├─────────────────────────────────────────────────────>│
     │                                                      │
     │                              22. Validar refresh     │
     │                              23. Verificar Redis     │
     │                              24. Generar nuevo       │
     │                                  access token        │
     │                                                      │
     │  25. { access_token: "new..." }                      │
     │<─────────────────────────────────────────────────────┤
     │                                                      │
```

---

## 3. FLUJO DE RESERVA Y PAGO

```
┌────────┐         ┌────────┐         ┌──────────┐         ┌─────────┐
│Cliente │         │  API   │         │PostgreSQL│         │ Mercado │
│        │         │ Server │         │          │         │  Pago   │
└───┬────┘         └───┬────┘         └────┬─────┘         └────┬────┘
    │                  │                   │                    │
    │ 1. Buscar        │                   │                    │
    │    profesional   │                   │                    │
    ├─────────────────>│                   │                    │
    │                  │ 2. Query          │                    │
    │                  ├──────────────────>│                    │
    │                  │ 3. Results        │                    │
    │                  │<──────────────────┤                    │
    │ 4. Lista         │                   │                    │
    │<─────────────────┤                   │                    │
    │                  │                   │                    │
    │ 5. Solicitar     │                   │                    │
    │    servicio      │                   │                    │
    │    {prof_id,     │                   │                    │
    │     service,     │                   │                    │
    │     date}        │                   │                    │
    ├─────────────────>│                   │                    │
    │                  │ 6. Crear booking  │                    │
    │                  │    status=pending │                    │
    │                  ├──────────────────>│                    │
    │                  │ 7. booking_id     │                    │
    │                  │<──────────────────┤                    │
    │                  │                   │                    │
    │                  │ 8. Notificar      │                    │
    │                  │    profesional    │                    │
    │                  │    (Push/Email)   │                    │
    │                  │                   │                    │
    │ 9. booking_id    │                   │                    │
    │<─────────────────┤                   │                    │
    │                  │                   │                    │
    │                  │                   │                    │
    │ ──── Profesional acepta ────         │                    │
    │                  │                   │                    │
    │ 10. Notificación │                   │                    │
    │     aceptada     │                   │                    │
    │<─────────────────┤                   │                    │
    │                  │                   │                    │
    │ 11. Proceder     │                   │                    │
    │     al pago      │                   │                    │
    ├─────────────────>│                   │                    │
    │                  │ 12. Crear         │                    │
    │                  │     preferencia   │                    │
    │                  ├───────────────────────────────────────>│
    │                  │ 13. preference_id │                    │
    │                  │    + checkout_url │                    │
    │                  │<───────────────────────────────────────┤
    │                  │ 14. Update booking│                    │
    │                  │     payment_pending                    │
    │                  ├──────────────────>│                    │
    │ 15. checkout_url │                   │                    │
    │<─────────────────┤                   │                    │
    │                  │                   │                    │
    │ 16. Redirect     │                   │                    │
    │     a Mercado    │                   │                    │
    │     Pago         │                   │                    │
    ├──────────────────────────────────────────────────────────>│
    │                  │                   │                    │
    │                  │                   │         17. Cliente│
    │                  │                   │             paga   │
    │                  │                   │                    │
    │                  │ 18. Webhook       │                    │
    │                  │     payment.created                    │
    │                  │<───────────────────────────────────────┤
    │                  │ 19. Validar       │                    │
    │                  │     signature     │                    │
    │                  │                   │                    │
    │                  │ 20. GET payment   │                    │
    │                  │     details       │                    │
    │                  ├───────────────────────────────────────>│
    │                  │ 21. Payment data  │                    │
    │                  │<───────────────────────────────────────┤
    │                  │                   │                    │
    │                  │ 22. Update booking│                    │
    │                  │     status=paid   │                    │
    │                  │     payment_status│                    │
    │                  │     =approved     │                    │
    │                  ├──────────────────>│                    │
    │                  │                   │                    │
    │                  │ 23. Crear         │                    │
    │                  │     transaction   │                    │
    │                  ├──────────────────>│                    │
    │                  │                   │                    │
    │                  │ 24. Notificar     │                    │
    │                  │     ambas partes  │                    │
    │                  │                   │                    │
    │ 25. Success      │                   │                    │
    │     redirect     │                   │                    │
    │<──────────────────────────────────────────────────────────┤
    │                  │                   │                    │
    │                  │                   │                    │
    │ ──── Servicio completado ────        │                    │
    │                  │                   │                    │
    │ 26. Marcar como  │                   │                    │
    │     completado   │                   │                    │
    ├─────────────────>│                   │                    │
    │                  │ 27. Update        │                    │
    │                  │     status=       │                    │
    │                  │     completed     │                    │
    │                  ├──────────────────>│                    │
    │                  │                   │                    │
    │                  │ 28. Liberar pago  │                    │
    │                  │     al profesional│                    │
    │                  │     (transferencia                     │
    │                  │      desde escrow)│                    │
    │                  ├───────────────────────────────────────>│
    │                  │                   │                    │
    │ 29. Solicitar    │                   │                    │
    │     review       │                   │                    │
    │<─────────────────┤                   │                    │
    │                  │                   │                    │
```

---

## 4. ESQUEMA DE BASE DE DATOS

```sql
-- Tabla de usuarios
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    id_masonico VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    logia VARCHAR(255),
    role VARCHAR(20) DEFAULT 'user',
    verified BOOLEAN DEFAULT FALSE,
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de profesionales
CREATE TABLE professionals (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    category VARCHAR(100) NOT NULL,
    subcategory VARCHAR(100),
    description TEXT,
    years_experience INTEGER,
    address VARCHAR(255),
    city VARCHAR(100),
    province VARCHAR(100),
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    verified_professional BOOLEAN DEFAULT FALSE,
    avg_rating DECIMAL(3, 2) DEFAULT 0,
    total_reviews INTEGER DEFAULT 0,
    total_jobs INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de servicios
CREATE TABLE services (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    professional_id UUID REFERENCES professionals(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price_min DECIMAL(10, 2),
    price_max DECIMAL(10, 2),
    price_type VARCHAR(20), -- 'fixed', 'hourly', 'negotiable'
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de reservas/solicitudes
CREATE TABLE bookings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    client_id UUID REFERENCES users(id),
    professional_id UUID REFERENCES professionals(id),
    service_id UUID REFERENCES services(id),
    status VARCHAR(20) DEFAULT 'pending',
    -- 'pending', 'accepted', 'rejected', 'paid', 'in_progress', 'completed', 'cancelled'
    scheduled_date TIMESTAMP,
    description TEXT,
    address VARCHAR(255),
    amount DECIMAL(10, 2),
    payment_status VARCHAR(20),
    -- 'pending', 'processing', 'approved', 'rejected', 'refunded'
    payment_id VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de transacciones
CREATE TABLE transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    booking_id UUID REFERENCES bookings(id),
    payment_provider VARCHAR(50), -- 'mercadopago', 'stripe'
    payment_id VARCHAR(255) UNIQUE,
    amount DECIMAL(10, 2) NOT NULL,
    platform_fee DECIMAL(10, 2),
    professional_amount DECIMAL(10, 2),
    currency VARCHAR(3) DEFAULT 'ARS',
    status VARCHAR(20),
    payment_method VARCHAR(50),
    metadata JSONB,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de reviews
CREATE TABLE reviews (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    booking_id UUID REFERENCES bookings(id),
    reviewer_id UUID REFERENCES users(id),
    professional_id UUID REFERENCES professionals(id),
    rating INTEGER CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    response TEXT, -- respuesta del profesional
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de mensajes
CREATE TABLE messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    conversation_id UUID NOT NULL,
    sender_id UUID REFERENCES users(id),
    receiver_id UUID REFERENCES users(id),
    content TEXT NOT NULL,
    attachment_url VARCHAR(500),
    read_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de conversaciones
CREATE TABLE conversations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    booking_id UUID REFERENCES bookings(id),
    participant_1 UUID REFERENCES users(id),
    participant_2 UUID REFERENCES users(id),
    last_message_at TIMESTAMP DEFAULT NOW(),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de notificaciones
CREATE TABLE notifications (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    type VARCHAR(50), -- 'booking', 'payment', 'message', 'review'
    title VARCHAR(255),
    content TEXT,
    data JSONB,
    read_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de tokens de dispositivo (push notifications)
CREATE TABLE device_tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    token VARCHAR(500) UNIQUE NOT NULL,
    platform VARCHAR(20), -- 'ios', 'android', 'web'
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de certificados/documentos
CREATE TABLE documents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    type VARCHAR(50), -- 'id', 'certificate', 'license'
    url VARCHAR(500) NOT NULL,
    verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Índices para mejorar performance
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_id_masonico ON users(id_masonico);
CREATE INDEX idx_professionals_user_id ON professionals(user_id);
CREATE INDEX idx_professionals_category ON professionals(category);
CREATE INDEX idx_professionals_location ON professionals(latitude, longitude);
CREATE INDEX idx_bookings_client ON bookings(client_id);
CREATE INDEX idx_bookings_professional ON bookings(professional_id);
CREATE INDEX idx_bookings_status ON bookings(status);
CREATE INDEX idx_messages_conversation ON messages(conversation_id);
CREATE INDEX idx_notifications_user ON notifications(user_id);

-- Full-text search
CREATE INDEX idx_professionals_search ON professionals 
    USING gin(to_tsvector('spanish', description));
```

---

## 5. ESTRUCTURA DE MICROSERVICIOS (NestJS)

```
src/
├── main.ts                      # Entry point
├── app.module.ts                # Root module
│
├── common/                      # Shared utilities
│   ├── decorators/
│   ├── filters/
│   ├── guards/
│   ├── interceptors/
│   └── pipes/
│
├── config/                      # Configuration
│   ├── database.config.ts
│   ├── redis.config.ts
│   └── app.config.ts
│
├── modules/
│   │
│   ├── auth/                    # Autenticación
│   │   ├── auth.controller.ts
│   │   ├── auth.service.ts
│   │   ├── auth.module.ts
│   │   ├── strategies/
│   │   │   ├── jwt.strategy.ts
│   │   │   └── local.strategy.ts
│   │   └── guards/
│   │       ├── jwt-auth.guard.ts
│   │       └── roles.guard.ts
│   │
│   ├── users/                   # Gestión usuarios
│   │   ├── users.controller.ts
│   │   ├── users.service.ts
│   │   ├── users.module.ts
│   │   ├── entities/
│   │   │   └── user.entity.ts
│   │   └── dto/
│   │       ├── create-user.dto.ts
│   │       └── update-user.dto.ts
│   │
│   ├── professionals/           # Profesionales
│   │   ├── professionals.controller.ts
│   │   ├── professionals.service.ts
│   │   ├── professionals.module.ts
│   │   ├── entities/
│   │   │   └── professional.entity.ts
│   │   └── dto/
│   │
│   ├── services/                # Servicios ofrecidos
│   │   ├── services.controller.ts
│   │   ├── services.service.ts
│   │   └── services.module.ts
│   │
│   ├── bookings/                # Reservas
│   │   ├── bookings.controller.ts
│   │   ├── bookings.service.ts
│   │   ├── bookings.module.ts
│   │   └── bookings.gateway.ts  # WebSocket
│   │
│   ├── payments/                # Pagos (sin comisión plataforma)
│   │   ├── payments.controller.ts
│   │   ├── payments.service.ts
│   │   ├── payments.module.ts
│   │   ├── providers/
│   │   │   └── mercadopago.provider.ts
│   │   └── webhooks/
│   │       └── mercadopago.webhook.ts
│   │
│   ├── messaging/               # Chat
│   │   ├── messaging.controller.ts
│   │   ├── messaging.service.ts
│   │   ├── messaging.module.ts
│   │   └── messaging.gateway.ts  # WebSocket
│   │
│   ├── reviews/                 # Valoraciones
│   │   ├── reviews.controller.ts
│   │   ├── reviews.service.ts
│   │   └── reviews.module.ts
│   │
│   ├── notifications/           # Notificaciones
│   │   ├── notifications.controller.ts
│   │   ├── notifications.service.ts
│   │   ├── notifications.module.ts
│   │   └── providers/
│   │       ├── fcm.provider.ts
│   │       └── email.provider.ts
│   │
│   └── search/                  # Búsqueda
│       ├── search.controller.ts
│       ├── search.service.ts
│       └── search.module.ts
│
└── database/
    ├── migrations/
    └── seeds/
```

---

## 6. CONFIGURACIÓN DE ENTORNOS

```bash
# .env.development
NODE_ENV=development
PORT=3000
API_URL=http://localhost:3000

# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=postgres
DATABASE_PASSWORD=postgres
DATABASE_NAME=gl_app_dev

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT
JWT_SECRET=your-dev-secret-key
JWT_EXPIRES_IN=15m
JWT_REFRESH_SECRET=your-refresh-secret
JWT_REFRESH_EXPIRES_IN=30d

# Mercado Pago
MP_ACCESS_TOKEN=TEST-1234567890
MP_PUBLIC_KEY=TEST-abc123
MP_WEBHOOK_SECRET=webhook-secret

# Storage
AWS_ACCESS_KEY_ID=your-key
AWS_SECRET_ACCESS_KEY=your-secret
AWS_REGION=us-east-1
AWS_S3_BUCKET=gl-app-dev

# Email
RESEND_API_KEY=re_dev_key
EMAIL_FROM=noreply@gl-app.com

# SMS
TWILIO_ACCOUNT_SID=your-sid
TWILIO_AUTH_TOKEN=your-token
TWILIO_PHONE_NUMBER=+1234567890

# Push Notifications
FCM_SERVER_KEY=your-fcm-key

# Monitoring
SENTRY_DSN=https://xxx@sentry.io/xxx

# CORS
CORS_ORIGIN=http://localhost:3000,http://localhost:19006
```

---

## 7. CI/CD PIPELINE

```yaml
# .github/workflows/main.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  # Job 1: Lint y Tests
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint
        run: npm run lint
      
      - name: Type check
        run: npm run type-check
      
      - name: Run tests
        run: npm run test:ci
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test
          REDIS_URL: redis://localhost:6379
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3

  # Job 2: Build
  build:
    needs: test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
      
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist
          path: dist/

  # Job 3: Security Scan
  security:
    needs: test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  # Job 4: Deploy to Staging
  deploy-staging:
    needs: [build, security]
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Railway (Staging)
        run: |
          npm install -g @railway/cli
          railway up --environment staging
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN }}
      
      - name: Run migrations
        run: railway run npm run migration:run

  # Job 5: Deploy to Production
  deploy-production:
    needs: [build, security]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Railway (Production)
        run: |
          npm install -g @railway/cli
          railway up --environment production
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN_PROD }}
      
      - name: Run migrations
        run: railway run npm run migration:run
      
      - name: Notify Sentry
        run: |
          curl https://sentry.io/api/0/organizations/${{ secrets.SENTRY_ORG }}/releases/ \
            -X POST \
            -H 'Authorization: Bearer ${{ secrets.SENTRY_AUTH_TOKEN }}' \
            -H 'Content-Type: application/json' \
            -d '{"version":"${{ github.sha }}"}'

  # Job 6: E2E Tests (post-deploy)
  e2e-tests:
    needs: deploy-staging
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Playwright tests
        run: |
          npm ci
          npx playwright install
          npm run test:e2e
        env:
          BASE_URL: https://staging.gl-app.com
```

---

## 8. MONITOREO Y OBSERVABILIDAD

```typescript
// Configuración Sentry (NestJS)
import * as Sentry from '@sentry/node';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
  integrations: [
    new Sentry.Integrations.Http({ tracing: true }),
    new Sentry.Integrations.Postgres(),
  ],
});

// Health check endpoint
@Controller('health')
export class HealthController {
  @Get()
  async check() {
    return {
      status: 'ok',
      timestamp: new Date().toISOString(),
      uptime: process.uptime(),
      memory: process.memoryUsage(),
      database: await this.checkDatabase(),
      redis: await this.checkRedis(),
    };
  }
}

// Métricas personalizadas
import { Injectable } from '@nestjs/common';
import * as client from 'prom-client';

@Injectable()
export class MetricsService {
  private register = new client.Registry();
  
  private requestCounter = new client.Counter({
    name: 'http_requests_total',
    help: 'Total HTTP requests',
    labelNames: ['method', 'route', 'status'],
  });
  
  private requestDuration = new client.Histogram({
    name: 'http_request_duration_ms',
    help: 'Duration of HTTP requests in ms',
    labelNames: ['method', 'route', 'status'],
    buckets: [0.1, 5, 15, 50, 100, 500],
  });
  
  constructor() {
    this.register.registerMetric(this.requestCounter);
    this.register.registerMetric(this.requestDuration);
  }
}
```

---

## 9. BACKUP Y DISASTER RECOVERY

### Estrategia de Backups

```bash
# Backup automático PostgreSQL (cron diario)
#!/bin/bash
# backup.sh

DATE=$(date +%Y-%m-%d-%H%M%S)
BACKUP_DIR="/backups/postgres"
DB_NAME="gl_app_prod"

# Backup completo
pg_dump -U postgres -F c -b -v -f \
  "$BACKUP_DIR/backup-$DATE.dump" $DB_NAME

# Upload a S3
aws s3 cp "$BACKUP_DIR/backup-$DATE.dump" \
  "s3://gl-app-backups/postgres/backup-$DATE.dump"

# Eliminar backups locales > 7 días
find $BACKUP_DIR -type f -mtime +7 -delete

# Notificar
curl -X POST https://api.gl-app.com/admin/backup-status \
  -H "Content-Type: application/json" \
  -d "{\"status\":\"success\",\"date\":\"$DATE\"}"
```

### Recovery Time Objective (RTO) y Recovery Point Objective (RPO)

- **RPO**: 24 horas (backup diario)
- **RTO**: 2-4 horas (tiempo de restauración)

---

---

## CRÉDITOS Y AUTORÍA

**Idea original del proyecto:**
Homero Bonafert y Mario Garberoglio

**Desarrollo técnico y conceptualización de soluciones:**
Juan Martin Marina - Desarrollador Senior Full-Stack
- Arquitectura y diseño técnico completo
- Ideación de soluciones tecnológicas
- Desarrollo full-stack de la plataforma
- Trabajo ad honorem para la comunidad

**Propósito:**
Plataforma de servicios profesionales para fortalecer los lazos fraternales y la economía interna de la Gran Logia de Argentina.

---

**Última actualización**: Febrero 2026
**Versión**: 1.0 - Arquitectura Detallada Final
