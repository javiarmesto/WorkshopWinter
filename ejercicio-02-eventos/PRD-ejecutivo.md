# Gestión de Registros de Eventos para Business Central
## Documento de Requisitos - Visión Ejecutiva

**Versión:** 1.0  
**Fecha:** Enero 2025  
**Autor:** VS Sistemas

---

## 1. Resumen Ejecutivo

### ¿Qué es?
Una extensión para Microsoft Dynamics 365 Business Central que permite importar, visualizar y exponer vía API los registros de asistentes a eventos (exportados desde Eventbrite u otras plataformas de ticketing).

### ¿Por qué lo necesitamos?
- Centralizar la información de registros de eventos en Business Central
- Permitir que agentes de IA consulten datos de asistentes para tareas como badges, comunicaciones o logística
- Tener visibilidad del estado de registros sin acceder a plataformas externas
- Facilitar la integración con otros procesos de negocio (facturación, CRM, etc.)

### ¿Para quién es?
- Organizadores de eventos
- Equipos de marketing y comunicación
- Agentes de IA que necesitan consultar datos de asistentes
- Sistemas externos que requieren información de registros

---

## 2. Funcionalidades Principales

### 2.1 Importación de Datos
El sistema permitirá cargar ficheros Excel exportados de Eventbrite con la siguiente información:

| Categoría | Información |
|-----------|-------------|
| **Pedido** | Número de orden, fecha de compra |
| **Comprador** | Nombre, apellidos, email, teléfono |
| **Ubicación** | Ciudad, provincia, país, código postal |
| **Evento** | Nombre del evento, fecha, hora, ubicación |
| **Tickets** | Cantidad de entradas, add-ons |
| **Financiero** | Moneda, estado de pago, tipo de pago, ventas, comisiones |

### 2.2 Visualización en Business Central

**Lista de Registros:**
Vista principal con todos los registros importados, mostrando:
- ID de orden
- Nombre completo del comprador
- Email
- Ciudad
- Cantidad de tickets
- Fecha del pedido
- Estado del pago

**Ficha de Registro:**
Detalle completo de cada registro organizado en secciones:
- Información del pedido
- Datos del comprador
- Información del evento
- Datos financieros

### 2.3 Filtros y Búsquedas
- Por evento (para gestionar múltiples eventos)
- Por ciudad/país (para logística regional)
- Por fecha de registro
- Por cantidad de tickets

---

## 3. Integración con Sistemas Externos

### 3.1 API REST
El sistema expone una API que permite a sistemas externos:

| Operación | Descripción | Caso de Uso |
|-----------|-------------|-------------|
| **Consultar registros** | Obtener lista de asistentes con filtros | Generar badges, listas de asistencia |
| **Buscar por email** | Encontrar registro específico | Validar registro en check-in |
| **Obtener estadísticas** | Totales por evento | Dashboards, reporting |
| **Filtrar por evento** | Registros de un evento específico | Gestión multi-evento |

### 3.2 Casos de Uso de Integración

**Agente de Badges:**
1. Agente consulta API para obtener lista de asistentes
2. Genera badges personalizados con nombre y empresa
3. Envía a impresión o genera PDFs

**Agente de Comunicaciones:**
1. Consulta registros de un evento específico
2. Obtiene emails para campaña de recordatorio
3. Segmenta por ciudad para información logística local

**Check-in en Evento:**
1. App de check-in consulta registro por email
2. Valida asistente y marca presencia
3. Muestra información relevante al staff

**Dashboard de Evento:**
1. Sistema externo consulta estadísticas
2. Muestra registros totales, por ciudad, evolución temporal
3. Actualización en tiempo real

---

## 4. Proceso de Importación

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Exportar   │     │   Subir     │     │  Validar    │     │   Datos     │
│  Excel de   │ ──▶ │  fichero    │ ──▶ │  y cargar   │ ──▶ │ disponibles │
│  Eventbrite │     │  a BC       │     │  datos      │     │  en BC      │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

**Características:**
- Detección automática de columnas del Excel
- Validación de datos antes de importar
- Opción de actualizar registros existentes o solo añadir nuevos
- Log de importación con errores/advertencias

---

## 5. Información Capturada

### 5.1 Datos del Pedido
| Campo | Descripción |
|-------|-------------|
| Order ID | Identificador único de Eventbrite |
| Order Date | Fecha y hora de la compra |
| Payment Status | Estado del pago (Free Order, Paid, etc.) |
| Payment Type | Tipo de pago utilizado |

### 5.2 Datos del Comprador
| Campo | Descripción |
|-------|-------------|
| Nombre y Apellidos | Datos del comprador |
| Email | Correo electrónico (clave para comunicaciones) |
| Teléfono | Número de contacto |
| Ciudad, Provincia, País | Ubicación del comprador |

### 5.3 Datos del Evento
| Campo | Descripción |
|-------|-------------|
| Event Name | Nombre del evento |
| Event Date/Time | Fecha y hora de inicio |
| Event Location | Lugar del evento |
| Ticket Quantity | Número de entradas adquiridas |

### 5.4 Datos Financieros
| Campo | Descripción |
|-------|-------------|
| Currency | Moneda de la transacción |
| Gross Sales | Ventas brutas |
| Service Fees | Comisiones de la plataforma |
| Net Sales | Ventas netas |

---

## 6. Beneficios Esperados

### Para Organizadores de Eventos
- Visión centralizada de registros en su ERP
- No depender de acceso a Eventbrite para consultas
- Integración con datos de clientes existentes

### Para Marketing y Comunicación
- Acceso rápido a datos para campañas
- Segmentación por ubicación geográfica
- Histórico de asistentes a eventos

### Para Automatización
- Agentes pueden consultar datos sin intervención humana
- Generación automática de materiales (badges, listas)
- Integración con flujos de trabajo existentes

### Para el Día del Evento
- Check-in rápido con validación en tiempo real
- Listas de asistencia actualizadas
- Información de contacto disponible para staff

---

## 7. Alcance y Limitaciones

### Incluido en esta versión
✅ Importación de Excel de Eventbrite  
✅ Visualización completa en Business Central  
✅ API de consulta para sistemas externos  
✅ Filtros por evento, ciudad, fecha  
✅ Soporte multi-evento  

### Fuera de alcance (posibles mejoras futuras)
❌ Integración directa con API de Eventbrite (sin Excel)  
❌ Sincronización automática periódica  
❌ Gestión de check-in dentro de BC  
❌ Envío de comunicaciones desde BC  
❌ Generación de badges dentro de BC  
❌ Facturación automática a asistentes  

---

## 8. Requisitos Técnicos

| Requisito | Especificación |
|-----------|----------------|
| **Plataforma** | Microsoft Dynamics 365 Business Central |
| **Versión mínima** | BC 23.0 (2023 Wave 2) |
| **Despliegue** | Cloud (SaaS) o On-Premise |
| **Formato de importación** | Excel (.xlsx) exportado de Eventbrite |

---

## 9. Próximos Pasos

1. **Aprobación** - Validar requisitos
2. **Desarrollo** - Implementación (estimación: 1-2 semanas)
3. **Testing** - Pruebas con datos reales del Winter Fest
4. **Documentación** - Guía de uso y API
5. **Despliegue** - Instalación en producción

---

## Anexo: Ejemplo de Datos

Basado en el fichero del **Business Central & Agents Winter Fest**:
- **Evento:** Business Central & Agents Winter Fest
- **Fecha:** 17 de Enero 2026
- **Ubicación:** AZZ Valencia Congress Hotel & SPA
- **Registros:** 36 pedidos
- **Tipo:** Free Order (evento gratuito)

---

*Documento preparado por VS Sistemas*
