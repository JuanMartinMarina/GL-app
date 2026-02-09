# Plan de Acción y Presupuesto Detallado - App GL Argentina

## RESUMEN EJECUTIVO

**Proyecto**: Plataforma de conexión de profesionales masones
**Objetivo**: MVP Web en 12 meses (año 1) | Mobile App en año 2
**Desarrollo**: Ad honorem por Juan Martin Marina (~400 horas año 1, 8h/semana)
**Costos operativos**: USD 100 - 400/mes inicialmente

### Nota sobre Referencias de Equipo
Las menciones de "Equipo" en este documento son **referencias de mercado** que indican el personal que se requeriría si este proyecto se desarrollara contratando una empresa externa. Estos datos sirven para:
- Comparar el valor del trabajo ad honorem
- Estimar costos si en el futuro se necesitara contratar personal adicional
- Entender la magnitud del esfuerzo de desarrollo

**En la realidad:** Todo el desarrollo es realizado ad honorem por **Juan Martin Marina** (Desarrollador Senior Full-Stack).

---

## FASE 0: PLANIFICACIÓN Y VALIDACIÓN (Semanas 1-2)

### Tareas Clave
- [ ] Reunión con autoridades de GL para validar ID masónico
- [ ] Definir método de verificación de usuarios
- [ ] Encuesta a potenciales usuarios (20-30 masones)
- [ ] Validar disposición de pago
- [ ] Definir categorías de profesionales
- [ ] Establecer presupuesto final
- [ ] Definir plan de trabajo detallado

### Entregables
- Documento de requerimientos funcionales
- Wireframes básicos (Figma)
- Definición de MVP features
- Acuerdo con GL sobre datos
- Presupuesto aprobado

### Tiempo estimado
- 30 horas (3-4 semanas)

---

## FASE 1: MVP DESARROLLO (Semanas 3-18)

### Semanas 3-6: Setup & Backend Core

#### Tareas
- [ ] Setup repositorio GitHub
- [ ] Configurar entornos (dev/staging/prod)
- [ ] Setup CI/CD con GitHub Actions
- [ ] Diseño final de base de datos
- [ ] API de autenticación (registro, login, JWT)
- [ ] CRUD usuarios y profesionales
- [ ] Sistema de roles y permisos
- [ ] Tests unitarios backend

#### Stack
```
- NestJS 10
- PostgreSQL 15
- TypeORM
- JWT + Bcrypt
- Jest (testing)
```

#### Equipo (Referencia de mercado si se contratara)
- 1 Tech Lead (full-time) - *Referencia comparativa*

#### Tiempo estimado: 80 horas (10 semanas)

---

### Semanas 7-10: Backend Features + Pagos

#### Tareas
- [ ] API de búsqueda y filtros
- [ ] Sistema de categorías
- [ ] Upload de imágenes (Cloudflare R2)
- [ ] **Integración Mercado Pago** (20h)
  - SDK setup
  - Crear preferencias de pago
  - Webhooks de confirmación
  - Testing de flujo completo
- [ ] Sistema de escrow básico
- [ ] API de reviews
- [ ] **Integración Email (Resend)** (8h)
  - Templates transaccionales
  - Confirmaciones y notificaciones
- [ ] Tests de integración

#### Entregables
- API REST completa y documentada (Swagger)
- Postman collection
- Integración de pagos funcional

#### Equipo (Referencia de mercado si se contratara)
- 1 Tech Lead + 1 Backend Developer - *Referencia comparativa*

#### Tiempo estimado: 90 horas (11 semanas)
*Incluye 28h de integraciones específicas*

---

### Semanas 11-14: Frontend Web (Next.js)

#### Tareas
- [ ] Setup Next.js 14 + TypeScript
- [ ] Diseño UI/UX implementado
- [ ] Páginas principales:
  - Landing page
  - Registro/Login
  - Home (dashboard)
  - Búsqueda de profesionales
  - Perfil de usuario
  - Perfil de profesional
  - Detalle de servicio
  - Checkout y pago
- [ ] Responsive design
- [ ] Integración con API
- [ ] Testing E2E (Playwright)

#### Diseño
- **shadcn/ui** + Tailwind CSS
- Paleta de colores corporativa GL
- Componentes reutilizables

#### Equipo (Referencia de mercado si se contratara)
- 1 Frontend Developer (full-time) - *Referencia comparativa*

#### Tiempo estimado: 120 horas (15 semanas)

---

### Semanas 15-18: Features Finales MVP

#### Tareas
- [ ] **Integración Chat (Socket.io)** (15h)
  - Setup WebSocket server
  - Rooms y mensajería 1-1
  - Persistencia de mensajes
- [ ] **Sistema de notificaciones** (10h)
  - Push notifications web
  - Email notifications
- [ ] Panel de administración básico
- [ ] **Integración Auth (Better-auth/Clerk)** (12h)
  - Setup y configuración
  - Flujos de registro/login
  - Verificación email
- [ ] Testing completo
- [ ] Bug fixing
- [ ] Documentación

#### Entregables
- Web app completa y funcional
- Admin panel básico
- Documentación técnica
- Manual de usuario

#### Equipo (Referencia de mercado si se contratara)
- 1 Tech Lead + 1 Backend Developer + 1 Frontend Developer - *Referencia comparativa*

#### Tiempo estimado: 60 horas (7-8 semanas)
*Incluye 37h de integraciones específicas*

---

## TESTING Y DESPLIEGUE FINAL (Semanas 47-52)

### Tareas Finales
- [ ] Testing de carga y performance
- [ ] Security audit
- [ ] Deploy a producción
- [ ] Configuración de monitoring (Sentry)
- [ ] Documentación final
- [ ] Capacitación admin GL

#### Tiempo estimado: 20 horas (5-6 semanas)

---

**TOTAL AÑO 1 (MVP WEB): ~400 horas en 52 semanas**

---

## FASE 2: MOBILE APP (AÑO 2 - POST-MVP)

### Desarrollo React Native

#### Tareas
- [ ] Setup React Native + Expo
- [ ] Navegación (React Navigation)
- [ ] Pantallas principales (10-15)
- [ ] Integración con API
- [ ] Push notifications (FCM)
- [ ] Testing en iOS y Android
- [ ] Publicación en stores

#### Pantallas Core
1. Splash / Onboarding
2. Login / Registro
3. Home / Dashboard
4. Búsqueda
5. Lista de profesionales
6. Perfil de profesional
7. Detalle de servicio
8. Chat
9. Mis reservas
10. Mi perfil
11. Configuración
12. Notificaciones

#### Equipo (Referencia de mercado si se contratara)
- 1 Mobile Developer (full-time, 8 semanas) - *Referencia comparativa*

#### Tiempo estimado: 300-400 horas (Año 2)
*Se desarrolla DESPUÉS del MVP web*

---

## DISEÑO UI/UX

### Alcance
- Wireframes (todas las pantallas)
- Mockups high-fidelity
- Prototipo interactivo (Figma)
- Design system
- Guía de estilos
- Assets (iconos, ilustraciones)

### Entregables
- 20-30 pantallas diseñadas
- Design system en Figma
- Assets exportados
- Guía de marca

### Equipo
- Diseño ad honorem

### Tiempo estimado: 200-250 horas

---

## TESTING & QA

### Tipos de Testing
- **Unitario**: Jest (backend + frontend)
- **Integración**: Supertest (API)
- **E2E**: Playwright (web)
- **Manual**: Casos de uso críticos
- **Performance**: Lighthouse, k6

### Casos de Prueba
- Registro y autenticación
- Búsqueda de profesionales
- Solicitud de servicio
- Flujo de pago completo
- Chat
- Notificaciones
- Responsive en múltiples dispositivos

### Equipo
- Testing ad honorem

### Tiempo estimado: 120-160 horas

---

## PRESUPUESTO DETALLADO POR OPCIONES

### OPCIÓN A: FULL CUSTOM (Recomendado para largo plazo)

#### Desarrollo (One-time)
| Item                      | Horas | Tarifa/h  | Costo USD              |
|---------------------------|-------|-----------|------------------------|
| Tech Lead / Arquitecto    | 320h  | $60-80    | $19,200 - $25,600      |
| Backend Developer (x2)    | 640h  | $40-60    | $25,600 - $38,400      |
| Frontend Developer        | 320h  | $40-60    | $12,800 - $19,200      |
| Mobile Developer          | 320h  | $45-65    | $14,400 - $20,800      |
| UI/UX Designer            | 160h  | $35-50    | $5,600 - $8,000        |
| QA Tester                 | 120h  | $30-40    | $3,600 - $4,800        |
| DevOps/Infrastructure     | 40h   | $50-70    | $2,000 - $2,800        |
| Project Manager           | 160h  | $40-60    | $6,400 - $9,600        |
| **SUBTOTAL DESARROLLO**   |       |           | **$89,600 - $129,200** |
| **Contingencia (10%)**    |       |           | **$8,960 - $12,920**   |
| **TOTAL**                 |       |           | **$98,560 - $142,120** |

#### Infraestructura Año 1
| Servicio          | Mensual  | Anual      |
|-------------------|----------|------------|
| Vercel Pro        | $20      | $240       |
| Railway/Render    | $50      | $600       |
| Cloudflare R2     | $15      | $180       |
| PostgreSQL        | $50      | $600       |
| Redis             | $15      | $180       |
| Resend (Email)    | $20      | $240       |
| Sentry            | $26      | $312       |
| Dominio           | $2       | $24        |
| **TOTAL INFRA**   | **$198** | **$2,376** |

#### Servicios Transaccionales
- Mercado Pago: 3.99% + IVA por transacción
- SMS (2FA): ~$0.05/SMS
- Push Notifications: Gratis (Firebase)

#### **TOTAL AÑO 1**: 1,680-2,250 horas (desarrollo ad honorem) + USD 1,200-2,400 (infraestructura)

---

### OPCIÓN B: OPTIMIZADA (Mejor costo-beneficio)

#### Desarrollo (One-time)
|     Item              |   Ajuste   | Costo USD |
|-----------------------|------------|-----------------------|
|   Equipo más compacto | ---------- | --------------------- |
| Tech Lead (part-time) | 50% menos  | $9,600 - $12,800      |
| Backend Dev (1.5x)    |            | $19,200 - $28,800     |
|          Frontend Dev | Same       | $12,800 - $19,200     |
|            Mobile Dev | Same       | $14,400 - $20,800     |
|        UI/UX Designer | Same       | $5,600 - $8,000       |
|    QA (reducido)      | 50% menos  | $1,800 - $2,400       |
|        No PM dedicado | -100%      | $0                    |
|             **TOTAL** |            | **$63,400 - $92,000** |

#### Infraestructura Año 1
- Usar planes gratuitos donde sea posible
- **Total**: USD 1,200 - 2,000

#### **TOTAL AÑO 1**: 1,400-1,800 horas (desarrollo ad honorem) + USD 1,200-2,000 (infraestructura)

---

### OPCIÓN C: MVP LEAN (Mínimo viable)

#### Alcance Reducido
- Sin mobile app inicial (PWA solamente)
- Chat básico (sin tiempo real)
- Admin panel limitado
- Core features funcionales

#### Desarrollo (One-time)
|           Item          |       Costo USD       |
|-------------------------|-----------------------|
| Full-stack Developer x2 | $25,600 - $38,400     |
| UI/UX Designer          | $4,000 - $6,000       |
| QA (manual)             | $1,000 - $2,000       |
| **TOTAL**               | **$30,600 - $46,400** |

#### Infraestructura
- Vercel Hobby: Gratis
- Railway Hobby: $5/mes
- **Total**: USD 300 - 600/año

#### **TOTAL AÑO 1**: 900-1,200 horas (desarrollo ad honorem) + USD 300-600 (infraestructura)

---

## MODELO DE DESARROLLO DEL PROYECTO

### Desarrollo Ad Honorem por Juan Martin Marina

| Criterio          | Desarrollo Ad Honorem  |
|-------------------|------------------------|
| **Costo**         | $0 (ad honorem)        |
| **Control**       | Total                  |
| **Velocidad**     | Alta                   |
| **Riesgo**        | Bajo                   |
| **Compromiso**    | Máximo                 |

### Ventajas de Este Modelo

**Costo $0** en desarrollo
**Control total** del proyecto
**Conocimiento profundo** del sistema
**Compromiso a largo plazo** con la comunidad
**Flexibilidad** para iterar según necesidades
**Ahorro significativo** vs. contratar externos ($85K-145K)

---

## PROCESADORES DE PAGO ARGENTINA - DETALLE

### Mercado Pago (Recomendado #1)

#### Comisiones
- **Tarjeta de crédito**: 3.99% + IVA (4.83% total)
- **Tarjeta de débito**: 2.99% + IVA (3.62% total)
- **QR / Link de pago**: 3.99% + IVA
- **Cuenta de Mercado Pago**: 0% (entre usuarios MP)

#### Tiempos de Acreditación
- Inmediato: +3.49% adicional
- 14 días: Tarifa estándar
- 30 días: -1% de descuento

#### Integración
- SDK oficial (JavaScript, PHP, Java, Python, etc.)
- Documentación en español
- Sandbox para testing
- Webhooks confiables

#### Ventajas
- #1 en Argentina
- Usuarios ya tienen cuenta
- Múltiples métodos de pago
- Excelente soporte

#### Desarrollo de la Integración
- Sin costos de setup por parte de Mercado Pago
- Sin mensualidad

---

### TodoPago/Prisma (Alternativa)

#### Comisiones
- Variable según volumen: 2.5% - 4.5%
- Requiere negociación
- Mínimo mensual: USD 50-100

#### Ventajas
- Banco local
- Menos comisiones (para alto volumen)

#### Desventajas
- Integración más compleja
- Menos developer-friendly
- Proceso de onboarding largo

---

### Decidir (First Data)

#### Comisiones
- 2.8% - 4.2% según volumen
- Requiere cuenta bancaria

#### Proceso
- Aplicación formal
- Verificación 2-4 semanas

---

## CRONOGRAMA DETALLADO

### Fase 0: Pre-desarrollo (2 semanas)
```
Semana 1:
  Lunes-Martes:    Reuniones con GL
  Miércoles-Viernes: Wireframes y requerimientos

Semana 2:
  Lunes-Miércoles: Setup inicial ad honorem
  Jueves-Viernes:  Comenzar desarrollo
```

### Fase 1: Backend (6 semanas)
```
Sprint 1 (Sem 3-4):
  - Setup infraestructura
  - API Auth
  - CRUD básico
  
Sprint 2 (Sem 5-6):
  - Features core
  - Búsqueda
  - Upload archivos
  
Sprint 3 (Sem 7-8):
  - Integración pagos
  - Webhooks
  - Tests
```

### Fase 2: Frontend Web (6 semanas)
```
Sprint 4 (Sem 9-10):
  - Setup + Páginas principales
  - Auth UI
  
Sprint 5 (Sem 11-12):
  - Perfiles y búsqueda
  - Dashboard
  
Sprint 6 (Sem 13-14):
  - Checkout
  - Admin panel
  - Testing
```

### Fase 3: Features Finales (2 semanas)
```
Sprint 7 (Sem 15-16):
  - Chat
  - Notificaciones
  - Bug fixing
```

### Fase 4: Beta Testing (2 semanas)
```
Semana 17:
  - Deploy a staging
  - Testing con usuarios reales (20-30)
  - Recolectar feedback
  
Semana 18:
  - Ajustes finales
  - Preparar lanzamiento
```

### Fase 5: Lanzamiento (1 semana)
```
Semana 19:
  - Deploy a producción
  - Comunicación a GL
  - Monitoreo intensivo
```

### Fase 6: Mobile (Opcional, 8 semanas)
```
Semanas 20-27:
  - Desarrollo React Native
  - Testing
  - Publicación stores
```

---

## PROYECCIÓN DE USO Y COSTOS

### Supuestos de Adopción
- 5,000 masones activos en GL
- 10% adopción inicial (500 usuarios)
- 10% son profesionales (50)
- Promedio 2 servicios/mes por profesional
- Ticket promedio: ARS 50,000 (USD 50)

### Proyección de Actividad (Año 1)

| Mes             | Usuarios | Prof. | Servicios | GMV (USD)  | Costo Infra |
|-----------------|----------|-------|-----------|------------|-------------|
| 1-2             | 100      | 10    | 10        | $500       | $50         |
| 3-4             | 250      | 25    | 30        | $1,500     | $100        |
| 5-6             | 400      | 40    | 60        | $3,000     | $150        |
| 7-8             | 500      | 50    | 100       | $5,000     | $200        |
| 9-10            | 700      | 70    | 140       | $7,000     | $250        |
| 11-12           | 1,000    | 100   | 200       | $10,000    | $300        |
| **TOTAL AÑO 1** |          |       | **640**   | **$27,000**| **$2,400**  |

### Costos Operativos Requeridos (Año 1)

| Concepto                              | Mensual     | Anual      |
|---------------------------------------|-------------|------------|
| Infraestructura (hosting, DB, storage)| $200        | $2,400     |
| Mantenimiento técnico (part-time dev) | $1,000      | $12,000    |
| Soporte comunitario (voluntario)      | $0          | $0         |
| Marketing interno                     | $300        | $3,600     |
| Servicios externos (email, SMS)       | $100        | $1,200     |
| **TOTAL OPERATIVO**                   | **$1,600**  | **$19,200**|

### Modelo de Financiamiento Ad Honorem

**Año 1:**
- Desarrollo: ~400 horas ad honorem por Juan Martin Marina (año 1)
- Costos operativos infraestructura: USD 1,200-2,400
- **Total Año 1**: USD 1,200-2,400 (solo infraestructura)
- **Modelo**: Servicio gratuito para la comunidad

**Año 2 en adelante:**
- Solo costos operativos: USD 19,200/año
- Mantenimiento y mejoras: USD 12,000/año
- **Total anual recurrente**: USD 31,200

### Sostenibilidad
Este es un **proyecto de servicio comunitario** sin fines de lucro. Los costos operativos pueden ser:
- Financiados por la Gran Logia como servicio a la membresía
- Cubiertos mediante aportes voluntarios de usuarios
- Subsidiados como parte del valor agregado de la membresía

---

## MÉTRICAS DE ÉXITO (KPIs)

### Mes 1-3 (Beta)
- 100+ usuarios registrados
- 10+ profesionales activos
- 10+ servicios completados
- 0 errores críticos
- < 2s tiempo de carga

### Mes 4-6 (Lanzamiento)
- 500+ usuarios
- 50+ profesionales
- 100+ servicios completados
- 4+ rating promedio
- 70%+ satisfacción

### Mes 7-12 (Crecimiento)
- 1,000+ usuarios
- 100+ profesionales
- 50+ servicios/mes
- 4.5+ rating promedio
- 80%+ satisfacción
- < 5% churn rate

---

## CONSIDERACIONES LEGALES ARGENTINA

### Registros Necesarios
1. **AFIP**
   - Alta como responsable inscripto o monotributista
   - CUIT
   - Facturación electrónica

2. **ARCA** (ARBA si es Buenos Aires)
   - Ingresos brutos
   - Alícuota según actividad

3. **IGJ** (si es SRL/SA)
   - Registro de sociedad
   - Estatuto

### Contratos Requeridos
- Términos y condiciones
- Política de privacidad (Ley 25.326)
- Contrato de prestación de servicios
- Política de reembolsos

### Facturación
- Sistema de facturación electrónica AFIP
- Retenciones a profesionales
- Liquidación de comisiones

### Costo Legal Inicial
- Constitución de sociedad: USD 500 - 1,500
- Contratos y T&C: USD 1,000 - 2,000
- Asesoría contable (año 1): USD 3,000 - 6,000

---

## CHECKLIST FINAL PRE-LANZAMIENTO

### Técnico
- [ ] Backend API funcionando 100%
- [ ] Frontend sin errores críticos
- [ ] Integración de pagos testeada
- [ ] SSL certificado instalado
- [ ] Backups automáticos configurados
- [ ] Monitoring (Sentry) activo
- [ ] Email transaccional funcionando
- [ ] Testing de carga realizado

### Legal
- [ ] Términos y condiciones aprobados
- [ ] Política de privacidad publicada
- [ ] Alta en AFIP
- [ ] Contratos de servicio listos

### Negocio
- [ ] Acuerdo con GL firmado
- [ ] Validación de usuarios definida
- [ ] Estructura de comisiones aprobada
- [ ] Plan de marketing definido
- [ ] Soporte al cliente planificado

### Contenido
- [ ] Textos revisados
- [ ] Imágenes optimizadas
- [ ] FAQs escritas
- [ ] Tutoriales/ayuda creados

---

## PRÓXIMOS PASOS INMEDIATOS

1. **Esta semana**:
   - Reunión con autoridades GL
   - Definir presupuesto disponible
   - Validar acceso a base de datos

2. **Próximas 2 semanas**:
   - Iniciar diseño UI/UX (ad honorem)
   - Setup infraestructura
   - Comenzar desarrollo ad honorem

3. **Mes 1**:
   - Sprint 1 completado (backend base)
   - Primeros mockups aprobados

---

## RECOMENDACIÓN FINAL

Para una organización como la Gran Logia, con presupuesto limitado pero necesidad de calidad, recomiendo:

### OPCIÓN REAL: Desarrollo Ad Honorem

**Desarrollo**:
- **Desarrollador**: Juan Martin Marina (Senior Full-Stack)
- **Tiempo**: ~400 horas ad honorem (año 1)
- **Timeline**: 4-5 meses
- **Incluye**: Web + Admin Panel + Integración pagos

**Post-MVP**:
- Mantenimiento ad honorem continuo (100-200h/año)
- Agregar features según feedback
- Desarrollar mobile app cuando haya tracción

**Infraestructura**:
- Usar Railway/Render inicialmente (USD 50-100/mes)
- Escalar según crezca

**Total Año 1**: ~400 horas (ad honorem) + USD 1,200-2,400 (infraestructura)

### Justificación de la opción
Balance costo-calidad
Menor riesgo
Equipo con experiencia
Soporte post-lanzamiento
Escalable

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
**Versión**: 1.0 - Presupuesto Real Ad Honorem
**Proyecto**: Plataforma de Servicios Profesionales GL
