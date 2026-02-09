# Plan de Desarrollo Individual - Ad Honorem

## CONTEXTO

**Modalidad de desarrollo**: desarrollo individual (single contributor)
**Modalidad contractual**: Ad honorem, en tiempo libre
**Objetivo**: Construcción completa de la plataforma en fases incrementales
**Enfoque**: Estimación de horas de trabajo realistas y costos de infraestructura tecnológica

---

## ESTIMACIÓN DE HORAS

### Régimen de Trabajo Part-Time

**Disponibilidad estimada**:
- 2-3 horas por día de semana (lunes a viernes)
- 4-6 horas los fines de semana
- **Total**: ~20-25 horas/semana

**Con ajustes por compromisos externos** (trabajo, familia, imprevistos):
- Semanas de alta productividad: 20-25 horas
- Semanas regulares: 10-15 horas
- Semanas de baja disponibilidad: 5-10 horas
- **Promedio estimado**: ~15 horas/semana

---

## ROADMAP POR FASES

### Fase 0: Setup e Infraestructura Inicial (20-30 horas)
**Tiempo estimado**: 2-3 semanas

**Tareas:**
- [ ] Crear proyecto Next.js + TypeScript
- [ ] Setup Turborepo (monorepo)
- [ ] Configurar Drizzle ORM
- [ ] Setup shadcn/ui
- [ ] Configurar servicios cloud (Vercel, Neon, etc.)
- [ ] Primera conexión a base de datos
- [ ] Git + GitHub

#### Complejidad: Baja-Media
- Documentación amplia disponible
- Las herramientas modernas simplifican la configuración inicial
- Se utilizan componentes reutilizables y configuraciones estándar

#### Costos de tecnología:
- **$0/mes** (todos los servicios en tier gratuito en esta fase)

---

### Fase 1: Autenticación (40-60 horas)
**Tiempo estimado**: 3-4 semanas

**Tareas:**
- [ ] Schema de usuarios en Drizzle
- [ ] Setup Better-auth o Clerk
- [ ] Página de registro con validación
- [ ] Página de login
- [ ] Recuperación de contraseña
- [ ] Verificación de email
- [ ] Protección de rutas
- [ ] Testing básico

#### Desglose por tarea:
| Tarea              | Horas   | Dificultad |
|--------------------|---------|------------|
| Schema DB          | 3-5h    |        |
| Auth setup         | 5-8h    |      |
| UI Login/Register  | 8-12h   |        |
| Email verification | 5-8h    |      |
| Protección rutas   | 3-5h    |        |
| Testing            | 5-8h    |        |
| Debugging          | 10-15h  | Variable   |

#### Complejidad: Media
- La configuración inicial puede requerir tiempo adicional
- Existen múltiples ejemplos y documentación de referencia
- Se aprovechan componentes reutilizables para reducir código boilerplate

#### Costos de tecnología:
- **$0-2/mes** (Neon free, Resend free hasta 3K emails)

#### Nota técnica:
Se recomienda abordar el desarrollo de forma incremental: esquema de base de datos primero, luego autenticación básica (login/registro), y finalmente verificación de email. No se recomienda implementar todas las funcionalidades en paralelo.

---

### Fase 2: Perfiles de Usuario (30-40 horas)
**Tiempo estimado**: 2-3 semanas

**Tareas:**
- [ ] Schema de perfiles
- [ ] Página "Mi Perfil"
- [ ] Editar datos personales
- [ ] Upload de foto de perfil (Cloudflare R2)
- [ ] Validación de ID masónico (manual en esta etapa)

#### Desglose:
| Tarea             | Horas  | Dificultad |
|-------------------|--------|------------|
| Schema perfiles   | 2-3h   |         |
| UI perfil         | 8-12h  |       |
| Edición perfil    | 5-8h   |       |
| Upload de imagen  | 8-12h  |     |
| Validación        | 5-8h   |       |

#### Complejidad: Baja-Media
- Operaciones CRUD estándar
- La funcionalidad de upload de archivos requiere atención especial en la primera implementación

#### Costos de tecnología:
- **$0-5/mes** (Cloudflare R2 free hasta 10GB)

---

### Fase 3: Profesionales y Servicios (50-70 horas)
**Tiempo estimado**: 4-5 semanas

**Tareas:**
- [ ] Schema de profesionales y servicios
- [ ] Registrarse como profesional
- [ ] Crear/editar servicios ofrecidos
- [ ] Categorías de servicios
- [ ] Página de perfil público de profesional
- [ ] Galería de trabajos (opcional)

#### Desglose:
| Tarea             | Horas   | Dificultad |
|-------------------|---------|------------|
| Schema DB         | 4-6h    |       |
| Form profesional  | 8-12h   |       |
| CRUD servicios    | 10-15h  |       |
| Categorías        | 5-8h    |       |
| Perfil público    | 12-18h  |     |
| Galería           | 8-12h   |       |

#### Complejidad: Media
- Desarrollo significativo de interfaces
- La lógica de negocio no presenta alta complejidad

#### Costos de tecnología:
- **$0-10/mes** (storage de imágenes)

---

### Fase 4: Búsqueda y Filtros (40-60 horas)
**Tiempo estimado**: 3-4 semanas

**Tareas:**
- [ ] Página de búsqueda/exploración
- [ ] Filtros por categoría
- [ ] Filtros por ubicación
- [ ] Filtros por rating (cuando existan datos)
- [ ] Búsqueda por texto
- [ ] Paginación o infinite scroll
- [ ] Cards de profesionales

#### Desglose:
| Tarea               | Horas   | Dificultad |
|---------------------|---------|------------|
| UI búsqueda         | 10-15h  |       |
| Query con filtros   | 8-12h   |     |
| Full-text search    | 5-8h    |       |
| Paginación          | 5-8h    |       |
| Cards profesionales | 8-12h   |       |
| Performance         | 4-6h    |     |

#### Complejidad: Media
- Las queries combinadas pueden alcanzar complejidad significativa
- La optimización de rendimiento es crítica en esta fase

#### Costos de tecnología:
- **$0-20/mes** (si el volumen de queries lo requiere, se escala el plan de Neon)

---

### Fase 5: Sistema de Reservas (50-70 horas)
**Tiempo estimado**: 4-5 semanas

**Tareas:**
- [ ] Schema de bookings
- [ ] Solicitar servicio a profesional
- [ ] Profesional acepta/rechaza
- [ ] Estados de reserva
- [ ] Historial de reservas
- [ ] Notificaciones email

#### Desglose:
| Tarea                 | Horas   | Dificultad |
|-----------------------|---------|------------|
| Schema bookings       | 4-6h    |       |
| Form solicitud        | 8-12h   |       |
| Lógica estados        | 10-15h  |     |
| UI historial          | 8-12h   |       |
| Notificaciones email  | 8-12h   |     |
| Testing flujo         | 10-15h  |     |

#### Complejidad: Media-Alta
- La máquina de estados de reservas presenta complejidad significativa
- Existen múltiples casos borde a contemplar

#### Costos de tecnología:
- **$20-30/mes** (Resend para mayor volumen de emails)

---

### Fase 6: Pagos con Mercado Pago (60-80 horas)
**Tiempo estimado**: 5-6 semanas

**Tareas:**
- [ ] Integrar SDK Mercado Pago
- [ ] Crear preferencia de pago
- [ ] Checkout redirect
- [ ] Webhook para confirmación
- [ ] Actualizar estado de reserva
- [ ] Sistema de escrow (retención de pago)
- [ ] Liberar pago al completar servicio
- [ ] Manejo de errores y reembolsos

#### Desglose:
| Tarea              | Horas   | Dificultad |
|--------------------|---------|------------|
| Setup MP           | 4-6h    |       |
| Crear preferencia  | 6-8h    |       |
| Checkout flow      | 10-15h  |     |
| Webhooks           | 12-18h  |   |
| Escrow logic       | 8-12h   |   |
| Testing pagos      | 15-20h  |   |
| Edge cases         | 10-15h  |   |

#### Complejidad: Alta
- La implementación de webhooks requiere atención especial
- El testing requiere cuenta sandbox de Mercado Pago
- Existe un número elevado de casos especiales a contemplar

#### Nota técnica:
Esta fase concentra la mayor complejidad del proyecto. Se recomienda:
- Desarrollar íntegramente en modo sandbox (pruebas) antes de pasar a producción
- Realizar testing exhaustivo de todos los flujos de pago
- Documentar cada tipo de webhook recibido y su manejo correspondiente

#### Costos de tecnología:
- **$0** en desarrollo (sandbox gratis)
- **3.99% + IVA** en producción por transacción

---

### Fase 7: Reviews y Ratings (30-40 horas)
**Tiempo estimado**: 2-3 semanas

**Tareas:**
- [ ] Schema de reviews
- [ ] Dejar review después de servicio completado
- [ ] Rating 1-5 estrellas
- [ ] Comentario opcional
- [ ] Mostrar reviews en perfil
- [ ] Calcular rating promedio
- [ ] Profesional puede responder

#### Desglose:
| Tarea             | Horas  | Dificultad |
|-------------------|--------|------------|
| Schema reviews    | 2-3h   |         |
| Form review       | 6-8h   |       |
| Rating component  | 4-6h   |       |
| Listado reviews   | 6-8h   |       |
| Rating promedio   | 4-6h   |       |
| Respuestas        | 6-8h   |       |

#### Complejidad: Baja-Media
- Implementación directa
- El componente de interfaz de usuario es el que demanda mayor tiempo de desarrollo

---

### Fase 8: Chat/Mensajería (50-70 horas)
**Tiempo estimado**: 4-5 semanas

**Tareas:**
- [ ] Schema de mensajes
- [ ] UI de chat
- [ ] Enviar/recibir mensajes
- [ ] Listado de conversaciones
- [ ] Real-time (Pusher o Supabase Realtime)
- [ ] Notificaciones de nuevos mensajes
- [ ] Mark as read

#### Desglose:
| Tarea            | Horas   | Dificultad |
|------------------|---------|------------|
| Schema mensajes  | 3-5h    |       |
| UI chat          | 15-20h  |     |
| Send/receive     | 8-12h   |       |
| Real-time setup  | 12-18h  |   |
| Notificaciones   | 8-12h   |     |
| Polish UI        | 6-10h   |       |

#### Complejidad: Alta
- La comunicación en tiempo real presenta complejidad de implementación significativa
- La interfaz de chat requiere desarrollo extenso

#### Costos de tecnología:
- **Pusher**: $0 (100 conexiones) o $5/mes
- **Supabase Realtime**: $0 (incluido en plan free)

#### Alternativa: implementación mediante polling
- Chat basado en polling (refresh cada 5-10 segundos)
- Complejidad de implementación considerablemente menor
- Suficiente para la fase inicial de la plataforma
- Migración a tiempo real en etapas posteriores si el uso lo justifica

---

### Fase 9: Admin Panel Básico (40-60 horas)
**Tiempo estimado**: 3-4 semanas

**Tareas:**
- [ ] Dashboard con métricas
- [ ] Listado de usuarios
- [ ] Verificar profesionales manualmente
- [ ] Moderar reviews
- [ ] Ver todas las reservas
- [ ] Estadísticas básicas

#### Desglose:
| Tarea         | Horas   | Dificultad |
|---------------|---------|------------|
| Auth admin    | 3-5h    |       |
| Dashboard UI  | 10-15h  |       |
| Listados      | 8-12h   |       |
| Verificación  | 6-8h    |       |
| Moderación    | 6-8h    |       |
| Stats         | 8-12h   |     |

#### Complejidad: Baja-Media
- Operaciones CRUD estándar
- El módulo de estadísticas puede presentar complejidad adicional

---

### Fase 10: Mobile App (80-120 horas)
**Tiempo estimado**: 6-8 semanas
**Planificación**: prevista para el Año 2 del proyecto

**Tareas:**
- [ ] Setup Expo
- [ ] Replicar pantallas principales
- [ ] Autenticación
- [ ] Navegación
- [ ] Push notifications
- [ ] Testing iOS y Android

#### Desglose:
| Tarea           | Horas   | Dificultad |
|-----------------|---------|------------|
| Setup Expo      | 4-6h    |       |
| Navegación      | 8-12h   |       |
| Auth mobile     | 10-15h  |     |
| Pantallas core  | 30-50h  |     |
| Push notifs     | 12-18h  |   |
| Testing         | 15-25h  |     |

#### Complejidad: Alta
- React Native presenta diferencias significativas respecto al desarrollo web
- Se requiere manejo de diferencias entre plataformas iOS y Android

#### Nota técnica:
La aplicación mobile no es requisito para el MVP. La versión web responsive y/o PWA (Progressive Web App) proporciona cobertura móvil adecuada para la fase inicial.

---

## RESUMEN DE HORAS TOTALES

### MVP Web Completo (Sin Mobile)
| Fase           | Horas          | Semanas (15h/sem) |
|----------------|----------------|-------------------|
| Setup          | 20-30h         | 2-3               |
| Auth           | 40-60h         | 3-4               |
| Perfiles       | 30-40h         | 2-3               |
| Profesionales  | 50-70h         | 4-5               |
| Búsqueda       | 40-60h         | 3-4               |
| Reservas       | 50-70h         | 4-5               |
| Pagos          | 60-80h         | 5-6               |
| Reviews        | 30-40h         | 2-3               |
| Chat           | 50-70h         | 4-5               |
| Admin          | 40-60h         | 3-4               |
| **TOTAL**      | **410-580h**   | **28-39 semanas** |

### Timeline Estimado

**Dedicación regular (15h/semana)**:
- **7-9 meses** para MVP completo

**Dedicación intensiva (25h/semana)**:
- **4-6 meses** para MVP completo

**Con aplicación mobile (Año 2)**:
- **+2-3 meses** adicionales

---

## COSTOS DE TECNOLOGÍA (Servicios de Infraestructura)

### Año 1 - Por Fase de Usuarios

#### Desarrollo (0-100 usuarios)
| Servicio         | Costo/mes             |
|------------------|-----------------------|
| Dominio          | $2                    |
| Vercel           | $0 (Hobby)            |
| Neon DB          | $0 (Free)             |
| Upstash Redis    | $0 (Free)             |
| Cloudflare R2    | $0 (Free)             |
| Resend           | $0 (3K emails/mes)    |
| Sentry           | $0 (5K eventos/mes)   |
| **TOTAL**        | **$2/mes**         |

#### Beta (100-500 usuarios)
| Servicio       | Costo/mes        |
|----------------|------------------|
| Dominio        | $2               |
| Vercel         | $0               |
| Neon DB        | $19 (Scale)      |
| Upstash Redis  | $0-5             |
| Cloudflare R2  | $2-5             |
| Resend         | $20 (Grow)       |
| Sentry         | $0-26            |
| **TOTAL**      | **$43-77/mes**   |

#### Producción (500-2,000 usuarios)
| Servicio        | Costo/mes               |
|-----------------|-------------------------|
| Dominio         | $2                      |
| Vercel          | $0-20 (Pro si hace falta)|
| Neon DB         | $19-40                  |
| Upstash Redis   | $10-20                  |
| Cloudflare R2   | $5-10                   |
| Cloudflare CDN  | $0-20                   |
| Resend          | $40-80                  |
| Sentry          | $26                     |
| Pusher (chat)   | $5-10                   |
| **TOTAL**       | **$107-228/mes**        |

#### Escala (2K-10K usuarios)
| Servicio          | Costo/mes        |
|-------------------|------------------|
| Dominio           | $2               |
| Vercel Pro        | $20              |
| Neon DB Pro       | $40-100          |
| Upstash Redis     | $20-40           |
| Cloudflare R2     | $10-30           |
| Cloudflare Pro    | $20              |
| Resend            | $80-150          |
| Sentry Business   | $80              |
| Pusher            | $10-20           |
| **TOTAL**         | **$282-462/mes** |

### Costos Transaccionales (Procesador de pagos)
- **Mercado Pago**: 3.99% + IVA = ~4.8% por transacción
  - Esta comisión es absorbida por el profesional
  - **La plataforma no cobra comisión adicional** (servicio ad honorem)
- **Ejemplo**: Servicio $50,000 ARS
  - MP cobra: $2,400
  - Plataforma: $0 (sin comisión)
  - **Profesional recibe: $47,600**

### TOTAL PRIMER AÑO (Inversión en Tecnología)

**Meses 1-3** (desarrollo): $2/mes = **$6**
**Meses 4-6** (beta): $50/mes = **$150**
**Meses 7-12** (producción): $150/mes = **$900**

**Total año 1**: ~**$1,056 USD**

---

## ESTRATEGIA DE DESARROLLO

### 1. Priorización MoSCoW

#### MUST (Imprescindible para lanzamiento)
- Autenticación (registro, login)
- Perfiles básicos
- Profesionales y servicios
- Búsqueda simple
- Solicitar servicio (sin pago)
- Reviews básicos

**Horas**: ~250-350h | **4-6 meses** estimados

#### SHOULD (Importante pero no crítico para lanzamiento)
- Pagos online
- Chat en tiempo real
- Admin panel
- Notificaciones email

**Horas**: +150-200h | **2-3 meses**

#### COULD (Funcionalidades deseables)
- Mobile app
- Geolocalización
- Calendario de disponibilidad
- Analytics avanzado

**Horas**: +150-300h | **3-6 meses**

#### WON'T (Fuera de alcance en esta etapa)
- Video llamadas
- Sistema de recomendaciones automáticas
- Gamification

### 2. Enfoque Iterativo

**Sprint 0** (1 mes): Setup + Autenticación
- Resultado: sistema de registro y login funcional

**Sprint 1** (1 mes): Perfiles + Profesionales
- Resultado: profesionales registrados y listados en la plataforma

**Sprint 2** (1 mes): Búsqueda + Reviews
- Resultado: funcionalidad de búsqueda y sistema de reviews operativo

**Sprint 3** (1 mes): Reservas simples
- Resultado: solicitud de servicios funcional (sin integración de pagos)

**LANZAMIENTO BETA** (4 meses acumulados)
- Prueba con 10-20 usuarios reales
- Recopilación de feedback

**Sprint 4** (1.5 meses): Pagos
- Integración de Mercado Pago

**Sprint 5** (1 mes): Chat básico
- Mensajería simple entre usuarios

**LANZAMIENTO PÚBLICO** (6.5 meses acumulados)

### 3. Metodología de Trabajo

#### Desarrollo incremental
Se recomienda dividir cada funcionalidad en tareas de 1-2 horas máximo.

Ejemplo para el sistema de reservas:
- Tarea 1 (1h): Schema de bookings
- Tarea 2 (2h): Formulario de solicitud
- Tarea 3 (1.5h): Listado de reservas
- Tarea 4 (2h): Funcionalidad de aceptar/rechazar

#### Control de versiones
Se recomienda realizar commits frecuentes (al menos una vez por sesión de trabajo) para mantener trazabilidad y facilitar la reversión de cambios.

### 4. Herramientas de Desarrollo

| Herramienta | Función |
|-------------|---------|
| shadcn/ui | Biblioteca de componentes reutilizables pre-estilizados y accesibles |
| Drizzle Studio | Interfaz visual para administración de base de datos |
| v0.dev (opcional) | Prototipado rápido de componentes UI |

---

## STACK TECNOLÓGICO

### Frontend
```
Next.js 15 (App Router)
TypeScript
Tailwind CSS
shadcn/ui
```
**Justificación**: Proyecto unificado que simplifica el desarrollo y el despliegue.

### Backend
```
Next.js Server Actions (mismo proyecto)
```
**Justificación**: Se elimina la necesidad de un backend separado en la fase inicial, reduciendo complejidad operativa.

### Base de Datos
```
Neon PostgreSQL (serverless)
Drizzle ORM
```
**Justificación**: Free tier generoso. Drizzle proporciona type-safety y una API concisa.

### Autenticación
```
Better-auth o Clerk
```
**Justificación**:
- Better-auth: gratuito, flexible, buena experiencia de desarrollo
- Clerk: interfaz pre-construida, 10K usuarios en tier gratuito

### Storage
```
Cloudflare R2
```
**Justificación**: 10GB en tier gratuito, sin costo de egreso de datos.

### Email
```
Resend
```
**Justificación**: 3K envíos/mes en tier gratuito, buena experiencia de desarrollo, compatible con React Email.

### Pagos
```
Mercado Pago
```
**Justificación**: Líder en el mercado argentino, integración bien documentada.

### Chat (fase posterior)
```
Supabase Realtime (tier gratuito)
O implementación mediante polling (menor complejidad)
```

### Monitoring
```
Sentry (free 5K eventos/mes)
Vercel Analytics (incluido)
```

---

## TABLA RESUMEN

| Métrica                                 | Valor          |
|-----------------------------------------|----------------|
| **Horas MVP Web**                       | 410-580h       |
| **Timeline regular (15h/sem)**          | 7-9 meses      |
| **Timeline intensivo (25h/sem)**        | 4-6 meses      |
| **Costo tecnología/mes (desarrollo)**   | $2/mes         |
| **Costo tecnología/mes (500 usuarios)** | $50-80/mes     |
| **Costo tecnología/mes (2K usuarios)**  | $110-150/mes   |
| **Costo tecnología/mes (10K usuarios)** | $280-460/mes   |
| **Total año 1 (solo tecnología)**       | ~$1,000 USD    |
| **Tiempo de desarrollo (ad honorem)**   | 400-600 horas  |
| **Valor estimado de mercado**           | $16K-36K USD*  |

*Referencia de valor de mercado. El desarrollo se realiza ad honorem.

---

## CONCLUSIÓN

El proyecto es viable bajo la modalidad de desarrollo individual, considerando los siguientes parámetros:

**Requisitos de viabilidad:**
- Disponibilidad de 15-25 horas semanales
- Constancia sostenida durante 6-9 meses
- Conocimientos de desarrollo web con el stack definido
- Uso de herramientas modernas para optimizar tiempos de desarrollo

**Inversión requerida:**
- Infraestructura tecnológica: ~$1,000 USD/año
- Tiempo de desarrollo: 400-600 horas (estimado 9 meses en régimen regular)

**Resultado esperado:**
- Plataforma web funcional con capacidad para 10K usuarios
- PWA con funcionamiento en dispositivos móviles
- Pagos integrados (sin comisión de plataforma)
- Panel de administración completo
- Servicio comunitario ad honorem que genera valor para la Gran Logia de Argentina

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

**Versión**: 1.0 - Plan Desarrollo Solo Final
**Fecha**: Febrero 2026
