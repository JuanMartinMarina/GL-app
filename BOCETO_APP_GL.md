# Boceto Técnico - App de Servicios Profesionales GL Argentina

## 1. RESUMEN EJECUTIVO

Aplicación móvil y web para conectar miembros de la Gran Logia de Argentina que ofrecen servicios profesionales/oficios con otros miembros que los requieran.

### Características Principales
- Sistema de autenticación con ID masónico
- Directorio de profesionales por categoría
- Sistema de búsqueda y filtros
- Sistema de valoraciones y reviews
- Mensajería interna
- Sistema de pagos integrado
- Panel administrativo
- Notificaciones push

---

## 2. ARQUITECTURA PROPUESTA

### Arquitectura General
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Mobile    │     │     Web     │     │   Admin     │
│   (React    │────▶│  (React/    │────▶│   Panel     │
│   Native)   │     │   Next.js)  │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
       │                    │                    │
       └────────────────────┼────────────────────┘
                            │
                     ┌──────▼──────┐
                     │   API REST  │
                     │   (Node.js) │
                     └──────┬──────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
   ┌────▼─────┐      ┌──────▼──────┐    ┌──────▼──────┐
   │PostgreSQL│      │   Redis     │    │   Storage   │
   │   DB     │      │   Cache     │    │  (S3/R2)    │
   └──────────┘      └─────────────┘    └─────────────┘
```

---

## 3. STACK TECNOLÓGICO DETALLADO

### 3.1 FRONTEND

#### Web Application
- **Framework**: Next.js 14+ (React)
  - SSR/SSG para SEO
  - App Router
  - TypeScript
- **UI/UX**: 
  - Tailwind CSS
  - shadcn/ui (componentes)
  - Framer Motion (animaciones)
- **State Management**: Zustand o TanStack Query
- **Forms**: React Hook Form + Zod

#### Mobile Application
- **Framework**: React Native + Expo
  - Desarrollo iOS y Android simultáneo
  - Over-the-air updates
- **UI**: React Native Paper o NativeBase
- **Navigation**: React Navigation
- **Estado**: Zustand

#### PWA (Opcional)
- Service Workers
- Manifest
- Notificaciones web push

**Tiempo estimado desarrollo**: 400-600 horas

---

### 3.2 BACKEND

#### API REST
- **Runtime**: Node.js 20 LTS
- **Framework**: Express.js o Fastify
- **Lenguaje**: TypeScript
- **Validación**: Zod o Joi
- **Documentación**: Swagger/OpenAPI

#### Alternativa moderna (recomendado)
- **Framework**: NestJS
  - Arquitectura modular
  - Inyección de dependencias
  - Testing integrado
  - TypeScript nativo

#### Microservicios clave
1. **Auth Service**: Autenticación y autorización
2. **User Service**: Gestión de usuarios/profesionales
3. **Messaging Service**: Chat interno
4. **Payment Service**: Procesamiento de pagos
5. **Notification Service**: Push notifications
6. **Search Service**: Búsqueda y filtros

**Tiempo estimado desarrollo**: 500-800 horas

---

### 3.3 BASE DE DATOS

#### Base de Datos Principal
- **PostgreSQL 15+**
  - ACID compliance
  - Relaciones complejas
  - Full-text search
  - Extensiones (PostGIS si se necesita geolocalización)

**Esquema básico de tablas**:
```sql
- users (id_masonico, email, password_hash, role)
- professionals (user_id, category, description, verified)
- services (professional_id, name, description, price)
- bookings (client_id, professional_id, status, payment_status)
- reviews (booking_id, rating, comment)
- messages (sender_id, receiver_id, content, timestamp)
- transactions (booking_id, amount, payment_method, status)
```

#### Cache
- **Redis**
  - Sesiones de usuario
  - Rate limiting
  - Cache de búsquedas
  - Cola de trabajos (Bull/BullMQ)

#### Storage
- **AWS S3** o **Cloudflare R2**
  - Fotos de perfil
  - Documentos de verificación
  - Certificados masónicos
  - Imágenes de servicios

**Costo mensual estimado**: USD 50 - 200

---

### 3.4 AUTENTICACIÓN Y SEGURIDAD

#### Sistema de Autenticación
- **JWT (JSON Web Tokens)**
  - Access tokens (corta duración)
  - Refresh tokens (larga duración)
- **OAuth 2.0** (opcional para integraciones futuras)
- **2FA** (Two-Factor Authentication)
  - SMS o Authenticator app

#### Integración con GL
**Opción 1**: Base de datos compartida (si GL provee acceso)
- Sincronización de IDs masónicos
- Validación en tiempo real

**Opción 2**: API de validación (recomendado)
- GL expone endpoint para validar ID
- App consume endpoint
- Menos acoplamiento

**Opción 3**: Carga manual inicial
- Importación de CSV/Excel
- Verificación manual posterior
- Actualización periódica

#### Seguridad
- **Bcrypt/Argon2**: Hash de contraseñas
- **Helmet.js**: Headers de seguridad
- **CORS**: Configurado correctamente
- **Rate Limiting**: Prevención de abuso
- **Input Validation**: En todas las rutas
- **HTTPS**: Obligatorio
- **Firewall**: WAF (Web Application Firewall)

**Librerías clave**:
```json
{
  "jsonwebtoken": "^9.0.0",
  "bcrypt": "^5.1.0",
  "helmet": "^7.0.0",
  "express-rate-limit": "^7.0.0"
}
```

---

### 3.5 SISTEMA DE PAGOS

#### Procesadores Recomendados para Argentina

**Opción 1: Mercado Pago** (Recomendado)
- Líder en Argentina
- Integración simple
- QR, link de pago, checkout
- Comisión: ~3.99% + IVA
- SDKs oficiales

**Opción 2: Stripe**
- Requiere cuenta internacional
- Mejor tecnología
- Comisión: ~2.9% + USD 0.30
- Más complejo en Argentina

**Opción 3: TodoPago/Prisma**
- Local argentino
- Menos developer-friendly
- Comisión: Variable

#### Integración Mercado Pago
```javascript
// Backend
const mercadopago = require('mercadopago');
mercadopago.configure({
  access_token: process.env.MP_ACCESS_TOKEN
});

// Crear preferencia de pago
const preference = {
  items: [{
    title: 'Servicio de Plomería',
    quantity: 1,
    unit_price: 5000
  }],
  back_urls: {
    success: 'https://app.com/success',
    failure: 'https://app.com/failure',
    pending: 'https://app.com/pending'
  }
};
```

#### Flujo de Pagos
1. Cliente solicita servicio
2. Sistema genera orden
3. Integración con MP genera link de pago
4. Cliente paga
5. Webhook confirma pago
6. Sistema libera contacto/servicio
7. Opción de escrow (dinero retenido hasta confirmación)

#### Modelo de Servicio Ad Honorem
- **Sin comisión de la plataforma** - Servicio gratuito para la comunidad
- Solo se aplican las comisiones del procesador de pagos (Mercado Pago)
- El profesional recibe el pago completo menos la comisión de Mercado Pago

---

### 3.6 MENSAJERÍA Y NOTIFICACIONES

#### Chat en Tiempo Real
**Opción 1: Socket.io**
- WebSocket bidireccional
- Fallback a polling
- Rooms para conversaciones

**Opción 2: Firebase Cloud Messaging**
- Infraestructura managed
- Push notifications incluidas
- Costo: Gratuito hasta cierto límite

#### Notificaciones Push
- **iOS**: Apple Push Notification Service (APNS)
- **Android**: Firebase Cloud Messaging (FCM)
- **Web**: Web Push API + Service Workers

**Servicios de terceros**:
- **OneSignal**: Gratuito hasta 10k usuarios
- **Pusher**: USD 49/mes
- **Firebase**: Gratuito hasta 10k usuarios

#### Email Transaccional
- **Resend** (moderno): USD 20/mes - 50k emails
- **SendGrid**: USD 19.95/mes - 50k emails
- **Amazon SES**: USD 0.10 por 1000 emails

**Costo mensual estimado**: USD 20 - 100

---

### 3.7 INFRAESTRUCTURA Y HOSTING

#### Opción 1: Cloud Full-Managed (Recomendado inicio)

**Vercel** (Frontend)
- Next.js optimizado
- Edge functions
- **Costo**: USD 20/mes (Pro) o gratuito (Hobby)

**Railway** o **Render** (Backend)
- Deploy automático desde Git
- PostgreSQL incluido
- **Costo**: USD 20-50/mes

**Cloudflare R2** (Storage)
- Sin costo de egreso
- **Costo**: USD 0.015/GB almacenado

**Total**: ~USD 50-100/mes para empezar

#### Opción 2: AWS (Escalabilidad)

**Servicios AWS**:
- **EC2** (t3.small): USD 15/mes
- **RDS PostgreSQL** (db.t3.micro): USD 15/mes
- **S3**: USD 0.023/GB
- **CloudFront** (CDN): Variable
- **ELB** (Load Balancer): USD 20/mes
- **Route 53** (DNS): USD 1/mes

**Total**: ~USD 100-200/mes inicial

#### Opción 3: Digital Ocean (Balance)
- **Droplet** (2GB RAM): USD 18/mes
- **Managed PostgreSQL**: USD 15/mes
- **Spaces** (S3-compatible): USD 5/mes
- **CDN**: USD 0.01/GB

**Total**: ~USD 50-80/mes

#### CDN (Content Delivery Network)
- **Cloudflare**: Gratuito (básico) / USD 20/mes (Pro)
- Mejora velocidad global
- Protección DDoS

**Recomendación inicial**: Railway/Render + Vercel + Cloudflare
**Costo mensual**: USD 70-150

---

### 3.8 CI/CD Y DEVOPS

#### Control de Versiones
- **GitHub** o **GitLab**
- Branching strategy: GitFlow o GitHub Flow

#### Pipeline CI/CD

**GitHub Actions** (Recomendado - Gratuito para repos públicos)
```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  test:
    - run: npm test
  build:
    - run: npm run build
  deploy:
    - deploy to production
```

**Alternativas**:
- **GitLab CI**: Integrado
- **CircleCI**: USD 30/mes
- **Jenkins**: Self-hosted (gratuito)

#### Stages del Pipeline
1. **Lint**: ESLint, Prettier
2. **Test**: Jest, Vitest
3. **Build**: Compilación TypeScript
4. **Security Scan**: Snyk, Dependabot
5. **Deploy**: Automático a staging/production
6. **Smoke Tests**: Tests post-deploy

#### Monitoreo
- **Sentry**: Error tracking (Gratuito hasta 5k eventos/mes)
- **LogRocket**: Session replay (USD 99/mes)
- **Datadog** o **New Relic**: APM (USD 15/host/mes)

#### Testing
```json
{
  "jest": "Testing unitario",
  "supertest": "Testing API",
  "cypress": "E2E testing",
  "playwright": "E2E alternativo"
}
```

**Costo mensual**: USD 0-50 (usando servicios gratuitos)

---

### 3.9 DOMINIO Y SSL

- **Dominio .com.ar**: ~USD 20/año
- **SSL**: Gratuito con Let's Encrypt o Cloudflare
- **Email corporativo**: Google Workspace (USD 6/usuario/mes)

---

## 4. FUNCIONALIDADES DETALLADAS

### 4.1 Módulo de Autenticación
- [ ] Registro con ID masónico
- [ ] Login con email/password
- [ ] Recuperación de contraseña
- [ ] Verificación de email
- [ ] 2FA opcional
- [ ] Roles: Usuario, Profesional, Admin

### 4.2 Perfil de Usuario
- [ ] Datos personales
- [ ] ID masónico
- [ ] Logia de pertenencia
- [ ] Foto de perfil
- [ ] Verificación de identidad

### 4.3 Perfil de Profesional
- [ ] Categoría/oficio (plomero, gasista, abogado, médico, etc.)
- [ ] Descripción de servicios
- [ ] Portafolio/galería
- [ ] Precios/tarifas
- [ ] Disponibilidad
- [ ] Certificaciones
- [ ] Valoraciones y reviews

### 4.4 Búsqueda y Descubrimiento
- [ ] Búsqueda por categoría
- [ ] Filtros: ubicación, precio, valoración
- [ ] Geolocalización
- [ ] Favoritos
- [ ] Historial de búsqueda

### 4.5 Sistema de Reservas/Solicitudes
- [ ] Solicitar servicio
- [ ] Calendario de disponibilidad
- [ ] Confirmación del profesional
- [ ] Estados: pendiente, confirmado, en curso, completado
- [ ] Cancelación (políticas)

### 4.6 Sistema de Pagos
- [ ] Múltiples métodos de pago
- [ ] Escrow (retención)
- [ ] Facturación automática
- [ ] Historial de transacciones
- [ ] Reembolsos

### 4.7 Mensajería
- [ ] Chat 1 a 1
- [ ] Adjuntar archivos
- [ ] Notificaciones en tiempo real
- [ ] Historial de conversaciones

### 4.8 Valoraciones y Reviews
- [ ] Calificación 1-5 estrellas
- [ ] Comentarios
- [ ] Respuestas del profesional
- [ ] Reportar abuso

### 4.9 Panel Administrativo
- [ ] Dashboard con métricas
- [ ] Gestión de usuarios
- [ ] Moderación de contenido
- [ ] Reportes y estadísticas
- [ ] Configuración de comisiones
- [ ] Logs de actividad

### 4.10 Notificaciones
- [ ] Nueva solicitud
- [ ] Confirmación de servicio
- [ ] Recordatorios
- [ ] Mensajes nuevos
- [ ] Pagos recibidos

---

## 5. ROADMAP DE DESARROLLO

### Fase 1: MVP (3-4 meses)
**Semanas 1-4**: Setup y arquitectura
- Configuración de repositorios
- Setup de infraestructura
- CI/CD básico
- Diseño de base de datos

**Semanas 5-8**: Backend Core
- API de autenticación
- CRUD de usuarios
- CRUD de profesionales
- Sistema de búsqueda básico

**Semanas 9-12**: Frontend Web
- Páginas principales
- Registro/Login
- Perfiles
- Búsqueda

**Semanas 13-16**: Integraciones
- Mercado Pago
- Notificaciones
- Mensajería básica
- Testing y ajustes

### Fase 2: Mobile (2-3 meses)
- App React Native
- Funcionalidades core
- Push notifications
- Testing iOS/Android

### Fase 3: Features Avanzadas (2 meses)
- Geolocalización
- Calendario avanzado
- Analytics
- Optimizaciones

---

## 6. ESTIMACIÓN DE COSTOS REALES (Actualizado 2026)

### 6.1 Desarrollo (Ad Honorem)

#### Desarrollo MVP Completo
| Componente | Horas Estimadas |
|------------|-----------------|
| MVP Web Completo (Next.js + API) | 400-500h |
| Diseño UI/UX básico | 80-100h |
| Integración Pagos | 40-60h |
| Testing básico | 60-80h |
| **TOTAL MVP WEB** | **580-740 horas** |

**Desarrollador**: Juan Martin Marina (Senior Full-Stack)
**Costo**: $0 (ad honorem para la comunidad)

#### Mobile App (Opcional - Post-MVP)
| Item | Horas Estimadas |
|------|-----------------|
| React Native básico | 200-300h |
| O usar PWA | Incluido |

**Desarrollo**: Ad honorem

### 6.2 Costos Mensuales Recurrentes

| Servicio                    | Costo Inicial    | Costo con Escala |
|-----------------------------|------------------|------------------|
| **Hosting & Infra**         |                  |                  |
| - Vercel (Frontend)         | $0-20            | $20-150          |
| - Railway/Render (Backend)  | $20-50           | $100-300         |
| - PostgreSQL                | Incluido         | $50-200          |
| - Redis                     | $0-10            | $20-100          |
| - Storage (R2/S3)           | $5-20            | $50-200          |
| - CDN (Cloudflare)          | $0-20            | $20-200          |
| **Servicios**               |                  |                  |
| - Mercado Pago              | 3.99% + IVA      | Por transacción  |
| - Email (Resend)            | $0-20            | $80-300          |
| - SMS (2FA)                 | $0-30            | $100-500         |
| - Push Notifications        | $0               | $0-100           |
| - Sentry (Errores)          | $0               | $26-80           |
| - Monitoring (Datadog)      | $0               | $15-100          |
| **Dominio & SSL**           |                  |                  |
| - Dominio                   | $2/mes           | $2/mes           |
| - SSL                       | $0               | $0               |
| **TOTAL MENSUAL**           | **$50-150**      | **$500-2,000+**  |

### 6.3 Costos Reales Año 1 (Desarrollo Ad Honorem)

#### Único Escenario Real
- **Desarrollo MVP**: ~400 horas en 12 meses (ad honorem por Juan Martin Marina, 8h/semana)
- **Infraestructura (año 1)**: USD 300-350
- **Mantenimiento**: Ad honorem continuo
- **Marketing interno**: Voluntario
- **TOTAL AÑO 1**: **USD 300-350**

#### Años Siguientes
- **Desarrollo y mantenimiento**: Ad honorem (100-200 horas/año)
- **Infraestructura**: USD 1,500-3,600/año
- **TOTAL ANUAL**: **USD 1,500-3,600**

---

## 7. EQUIPO DE DESARROLLO REAL

### Desarrollo Completo
**Juan Martin Marina** - Desarrollador Senior Full-Stack
- Arquitectura completa del sistema
- Frontend (Next.js, React, TypeScript)
- Backend (API, base de datos, arquitectura de datos)
- DevOps (deployment, CI/CD, infraestructura cloud)
- Mobile (React Native)
- Seguridad y optimización
- Testing y QA
- Mantenimiento continuo

**Trabajo 100% ad honorem para la comunidad**

**Timeline:** 6-9 meses
**Horas totales:** ~400 horas (año 1, MVP Web)

**Post-Lanzamiento:**
- Mantenimiento ad honorem continuo (100-200h/año)
- Soporte interno GL

---

## 8. CONSIDERACIONES LEGALES

- [ ] **Términos y Condiciones**
- [ ] **Política de Privacidad** (GDPR compliance)
- [ ] **Ley de Protección de Datos Personales** (Argentina Ley 25.326)
- [ ] **Contrato de prestación de servicios**
- [ ] **Política de reembolsos**
- [ ] **Responsabilidad limitada** (plataforma vs profesionales)
- [ ] **Registro ante AFIP** (según estructura legal de la GL)
- [ ] **Facturación electrónica**

---

## 9. RIESGOS Y MITIGACIONES

| Riesgo                     | Probabilidad | Impacto  | Mitigación                                          |
|----------------------------|--------------|----------|-----------------------------------------------------|
| No acceso a BD de GL       | Alta         | Alto     | Sistema de validación manual inicial                |
| Baja adopción inicial      | Media        | Alto     | Marketing interno en GL, incentivos                 |
| Problemas con pagos        | Baja         | Alto     | Usar procesador confiable (MercadoPago)             |
| Escalabilidad              | Media        | Media    | Arquitectura cloud-native desde inicio              |
| Seguridad de datos         | Baja         | Crítico  | Implementar mejores prácticas desde día 1           |
| Sostenibilidad financiera  | Media        | Medio    | Costos optimizados, infraestructura escalable       |

---

## 10. MÉTRICAS DE ÉXITO (KPIs)

### Lanzamiento (3 meses)
- 500+ usuarios registrados
- 100+ profesionales activos
- 50+ servicios completados

### Año 1
- 5,000+ usuarios
- 500+ profesionales
- 1,000+ servicios/mes
- 80%+ satisfacción (reviews)
- Break-even operativo

---

## 11. PRÓXIMOS PASOS RECOMENDADOS

1. **Validación con GL**: Confirmar acceso a datos o método de validación
2. **MVP Definition**: Priorizar features mínimas
3. **Diseño**: Wireframes y mockups (ad honorem)
4. **Setup**: Infraestructura y repositorios
5. **Desarrollo**: Sprint 0 (4 semanas) - ad honorem por Juan Martin Marina
8. **Beta Testing**: Con grupo pequeño de GL
9. **Lanzamiento**: Soft launch interno
10. **Iteración**: Feedback y mejoras

---

## 13. CONTACTOS Y RECURSOS ÚTILES

### Procesadores de Pago Argentina
- Mercado Pago Developers: https://www.mercadopago.com.ar/developers
- Stripe Argentina: https://stripe.com/es-ar

### Hosting
- Vercel: https://vercel.com
- Railway: https://railway.app
- Render: https://render.com

### Documentación
- Next.js: https://nextjs.org/docs
- React Native: https://reactnative.dev
- NestJS: https://nestjs.com

---

## 14. PRESUPUESTO REAL (DESARROLLO AD HONOREM)

### Única Opción: Desarrollo por Ti Mismo

#### Costos Monetarios REALES
| Concepto          | Año 1            | Años 2-5 (anual)  |
|-------------------|------------------|-------------------|
| **Desarrollo**    | **$0**           | **$0**            |
| Infraestructura   | $600-1,200       | $1,500-2,600      |
| Dominio           | $24              | $24               |
| **TOTAL**         | **$624-1,224**   | **$1,524-2,624**  |

#### Inversión No Monetaria (Trabajo Ad Honorem)
| Fase | Horas | Valor Estimado de Mercado* |
|------|-------|-------------------|
| MVP Web | 400-500h | $16K-30K |
| Mobile App | 80-120h | $3K-7K |
| Mantenimiento anual | 100-150h/año | $4K-9K/año |

*A tarifas de mercado ($40-60/h), pero es **ad honorem**

### Resumen Ejecutivo para la GL

**Inversión requerida por la Gran Logia:**
- **Año 1**: USD 600-1,200 (solo infraestructura)
- **Años siguientes**: USD 1,500-2,600/año

**Ahorro vs. contratar desarrollo externo:**
- **$85,000 - $145,000** en desarrollo
- **$15,000 - $25,000/año** en mantenimiento

**Aporte a la comunidad:**
- 400-600 horas de trabajo especializado
- Conocimiento y experiencia técnica
- Soporte y mantenimiento continuo

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

**Documento creado**: Febrero 2026
**Versión**: 1.0 - Ad Honorem Final
**Proyecto**: Servicio comunitario sin fines de lucro
