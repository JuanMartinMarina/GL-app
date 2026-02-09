# Stack Tecnológico - Resumen Ejecutivo

## STACK RECOMENDADO (2026)

### Frontend
```
Web: Next.js 14 + TypeScript + Tailwind CSS
Mobile: React Native + Expo
UI Components: shadcn/ui
State: Zustand / TanStack Query
```

### Backend
```
Runtime: Node.js 20 LTS
Framework: NestJS (TypeScript)
API: REST + WebSockets (Socket.io)
Validation: Zod
Docs: Swagger/OpenAPI
```

### Base de Datos
```
Principal: PostgreSQL 15+
Cache: Redis 7+
Storage: Cloudflare R2 / AWS S3
Search: PostgreSQL Full-Text Search
```

### Autenticación
```
Strategy: JWT (Access + Refresh tokens)
Password: Bcrypt / Argon2
2FA: SMS / Authenticator App
OAuth: Opcional (futuro)
```

### Pagos
```
Procesador: Mercado Pago (Argentina)
Alternativa: Stripe (internacional)
Comisión: ~3.99% + IVA
Escrow: Retención hasta confirmación
```

### Infraestructura
```
Frontend Hosting: Vercel
Backend Hosting: Railway / Render
Database: Railway / Render Managed
CDN: Cloudflare
DNS: Cloudflare
Monitoring: Sentry
```

### CI/CD
```
Version Control: GitHub
Pipeline: GitHub Actions
Testing: Jest + Playwright
Deployment: Automático (main branch)
Environments: dev, staging, production
```

### Comunicación
```
Push Notifications: Firebase Cloud Messaging
Email: Resend / SendGrid
SMS: Twilio / MessageBird
Real-time Chat: Socket.io
```

---

## COSTOS ESTIMADOS

### Desarrollo
**Todo el desarrollo es realizado ad honorem por Juan Martin Marina** (Senior Developer)
- Tiempo estimado: ~400 horas (año 1, MVP Web, 8h/semana)
- Costo real: **$0** (servicio a la comunidad)

### Operación Mensual (Costos Reales)
| Servicio            | Mes 1-6        | Mes 6-12       | Escala          |
|---------------------|----------------|----------------|-----------------|
| Hosting (Frontend)  | $0-20          | $20            | $20-150         |
| Hosting (Backend)   | $20-50         | $50            | $100-300        |
| Base de Datos       | Incluido       | $50            | $50-200         |
| Storage             | $5-20          | $20            | $50-200         |
| CDN                 | $0             | $0-20          | $20-200         |
| Email               | $0-20          | $20            | $80-300         |
| Monitoring          | $0             | $26            | $26-100         |
| Push Notif.         | $0             | $0             | $0-100          |
| **TOTAL**           | **$50-150**    | **$200-400**   | **$500-2,000+** |

### Pagos por Transacción
- Mercado Pago: 3.99% + IVA (21%)
  - Esta comisión es absorbida por el profesional
  - **La plataforma NO cobra comisión adicional**

---

## DEPENDENCIAS PRINCIPALES (package.json)

### Backend (NestJS)
```json
{
  "dependencies": {
    "@nestjs/common": "^10.0.0",
    "@nestjs/core": "^10.0.0",
    "@nestjs/jwt": "^10.0.0",
    "@nestjs/passport": "^10.0.0",
    "@nestjs/typeorm": "^10.0.0",
    "@nestjs/websockets": "^10.0.0",
    "bcrypt": "^5.1.1",
    "class-validator": "^0.14.0",
    "jsonwebtoken": "^9.0.2",
    "mercadopago": "^2.0.0",
    "pg": "^8.11.0",
    "redis": "^4.6.0",
    "socket.io": "^4.6.0",
    "typeorm": "^0.3.17",
    "zod": "^3.22.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.3.0",
    "jest": "^29.7.0",
    "@nestjs/testing": "^10.0.0"
  }
}
```

### Frontend Web (Next.js)
```json
{
  "dependencies": {
    "next": "^14.1.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "typescript": "^5.3.0",
    "tailwindcss": "^3.4.0",
    "@radix-ui/react-*": "latest",
    "zustand": "^4.5.0",
    "@tanstack/react-query": "^5.17.0",
    "react-hook-form": "^7.49.0",
    "zod": "^3.22.0",
    "axios": "^1.6.0",
    "socket.io-client": "^4.6.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "eslint": "^8.56.0",
    "prettier": "^3.1.0",
    "playwright": "^1.40.0"
  }
}
```

### Mobile (React Native)
```json
{
  "dependencies": {
    "react": "18.2.0",
    "react-native": "0.73.0",
    "expo": "~50.0.0",
    "@react-navigation/native": "^6.1.0",
    "@react-navigation/stack": "^6.3.0",
    "axios": "^1.6.0",
    "react-native-paper": "^5.11.0",
    "zustand": "^4.5.0",
    "socket.io-client": "^4.6.0",
    "expo-notifications": "~0.27.0",
    "expo-image-picker": "~14.7.0"
  }
}
```

---

## ESTRUCTURA DE PROYECTO

```
proyecto-gl/
├── apps/
│   ├── web/                 # Next.js web app
│   ├── mobile/              # React Native app
│   └── admin/               # Admin panel
├── packages/
│   ├── api/                 # Backend NestJS
│   ├── shared/              # Código compartido
│   └── database/            # TypeORM entities
├── infrastructure/
│   ├── docker/
│   ├── kubernetes/          # Opcional futuro
│   └── terraform/           # IaC
├── .github/
│   └── workflows/           # CI/CD
└── docs/
    ├── api/                 # Swagger docs
    └── architecture/        # Diagramas
```

---

## ROADMAP TÉCNICO

### Sprint 0 (Semana 1-2)
- [x] Setup repositorio monorepo (Turborepo/Nx)
- [x] Configurar ESLint + Prettier
- [x] Setup CI/CD básico
- [x] Configurar ambientes (dev, staging, prod)
- [x] Diseñar schema de base de datos

### Sprint 1-2 (Semana 3-6): Backend Foundation
- [x] API de autenticación (registro, login, JWT)
- [x] CRUD usuarios y profesionales
- [x] Integración con PostgreSQL
- [x] Setup Redis para cache
- [x] Middleware de autorización
- [x] Tests unitarios

### Sprint 3-4 (Semana 7-10): Core Features
- [x] Sistema de búsqueda y filtros
- [x] Perfiles de profesionales
- [x] Sistema de categorías
- [x] Upload de imágenes a S3/R2
- [x] API de reviews y ratings
- [x] Tests de integración

### Sprint 5-6 (Semana 11-14): Frontend Web
- [x] Páginas principales (Home, Search, Profile)
- [x] Formularios de registro/login
- [x] Dashboard de usuario
- [x] Dashboard de profesional
- [x] Responsive design
- [x] Integración con API

### Sprint 7-8 (Semana 15-18): Pagos y Mensajería
- [x] Integración Mercado Pago
- [x] Flujo de reserva y pago
- [x] Chat en tiempo real (Socket.io)
- [x] Sistema de notificaciones
- [x] Webhooks para pagos

### Sprint 9-10 (Semana 19-22): Mobile App
- [x] Setup React Native + Expo
- [x] Pantallas principales
- [x] Navegación
- [x] Push notifications
- [x] Testing iOS y Android

### Sprint 11-12 (Semana 23-26): Admin Panel
- [x] Dashboard con métricas
- [x] Gestión de usuarios
- [x] Moderación de contenido
- [x] Reportes y analytics
- [x] Configuración de plataforma

### Sprint 13+ (Post-MVP)
- [ ] Geolocalización avanzada
- [ ] Sistema de calendario
- [ ] Reportes avanzados
- [ ] Optimizaciones de performance
- [ ] Features adicionales según feedback

---

## SEGURIDAD - CHECKLIST

### Autenticación
- [x] Passwords hasheados (bcrypt/argon2)
- [x] JWT con expiración corta
- [x] Refresh tokens
- [x] 2FA opcional
- [x] Rate limiting en login

### API
- [x] HTTPS obligatorio
- [x] CORS configurado correctamente
- [x] Input validation (Zod)
- [x] SQL injection prevention (ORM)
- [x] XSS protection (sanitización)
- [x] CSRF tokens
- [x] Rate limiting global

### Datos
- [x] Encriptación en tránsito (TLS 1.3)
- [x] Encriptación en reposo (database)
- [x] Backups automáticos diarios
- [x] Logs de auditoría
- [x] GDPR compliance
- [x] Ley 25.326 compliance (Argentina)

### Infraestructura
- [x] Firewall (WAF)
- [x] DDoS protection (Cloudflare)
- [x] Secrets en variables de entorno
- [x] Monitoreo de vulnerabilidades (Dependabot)
- [x] Security headers (Helmet.js)

---

## MÉTRICAS A TRACKEAR

### Técnicas
- Uptime (objetivo: 99.9%)
- Response time API (< 200ms p95)
- Error rate (< 0.1%)
- Database query time
- CDN cache hit rate

### Negocio
- Usuarios registrados (total y activos)
- Profesionales activos
- Servicios solicitados
- Servicios completados
- GMV (Gross Merchandise Value)
- Tasa de conversión
- Retención (D1, D7, D30)

### Herramientas
- **Sentry**: Errores
- **Datadog/New Relic**: APM
- **Google Analytics**: Web analytics
- **Mixpanel**: Product analytics
- **LogRocket**: Session replay

---

## ALTERNATIVAS CONSIDERADAS

### Backend
| Opción | Pros | Contras | Elegido |
|--------|------|---------|---------|
| NestJS | Estructura, TypeScript, escalable | Curva aprendizaje | SI |
| Express | Simple, flexible | Sin estructura | NO |
| Fastify | Muy rápido | Menos maduro | NO |

### Frontend
| Opción | Pros | Contras | Elegido |
|--------|------|---------|---------|
| Next.js | SSR, SEO, Vercel | Vendor lock-in leve | SI |
| Vite + React | Muy rápido | Sin SSR fácil | NO |
| Remix | Moderno | Menos maduro | NO |

### Mobile
| Opción | Pros | Contras | Elegido |
|--------|------|---------|---------|
| React Native | Code sharing, comunidad | Performance | SI |
| Flutter | Performance | Dart, menos sharing | NO |
| Nativo | Mejor performance | 2x desarrollo | NO |

### Base de Datos
| Opción | Pros | Contras | Elegido |
|--------|------|---------|---------|
| PostgreSQL | ACID, relaciones, maduro | Escalar vertical | SI |
| MongoDB | Flexible, escala horizontal | No relaciones | NO |
| MySQL | Popular | Menos features | NO |

---

## CONTACTOS ÚTILES

### Proveedores Argentina
- **Mercado Pago**: developers@mercadopago.com
- **AFIP**: 0810-999-2347
- **Hosting Argentina**: [proveedores locales]

### Comunidades
- **NestJS Discord**: discord.gg/nestjs
- **Next.js Discord**: discord.gg/nextjs
- **React Native**: discord.gg/react-native

---

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
Plataforma de servicios profesionales para fortalecer los lazos fraternales y la economía interna de la Gran Logia de Argentina.

---

**Última actualización**: Febrero 2026
**Versión**: 1.0 - Stack Tecnológico Final
