# Gestión de Incidencias para Business Central
## Documento de Requisitos - Visión Ejecutiva

**Versión:** 1.0  
**Fecha:** Enero 2025  
**Autor:** VS Sistemas

---

## 1. Resumen Ejecutivo

### ¿Qué es?
Una extensión para Microsoft Dynamics 365 Business Central que permite gestionar incidencias de clientes de forma centralizada, con capacidad de integración con sistemas externos y agentes de IA.

### ¿Por qué lo necesitamos?
- Centralizar el seguimiento de incidencias en el mismo sistema donde está la información del cliente
- Permitir que agentes de IA y sistemas externos puedan crear y consultar incidencias automáticamente
- Tener visibilidad del estado de las incidencias y su resolución

### ¿Para quién es?
- Equipos de soporte y atención al cliente
- Responsables de calidad
- Agentes de IA y chatbots que reciben incidencias
- Sistemas externos que necesitan reportar problemas

---

## 2. Funcionalidades Principales

### 2.1 Registro de Incidencias
El sistema permitirá registrar incidencias con la siguiente información:

| Información | Descripción |
|-------------|-------------|
| **Identificación** | Número único automático, descripción corta y detallada |
| **Clasificación** | Categoría y prioridad (Baja, Media, Alta, Crítica) |
| **Origen** | Canal de entrada y referencia externa |
| **Cliente** | Datos del cliente afectado (opcional) |
| **Contacto** | Nombre, email y teléfono del reportador |
| **Asignación** | Usuario responsable de la resolución |
| **Fechas** | Creación, fecha límite y resolución |

### 2.2 Ciclo de Vida de la Incidencia

```
┌─────────┐     ┌─────────────┐     ┌──────────┐     ┌─────────┐
│  Nueva  │ ──▶ │ En Progreso │ ──▶ │ Resuelta │ ──▶ │ Cerrada │
└─────────┘     └─────────────┘     └──────────┘     └─────────┘
                      │                   │
                      ▼                   │
               ┌─────────────┐            │
               │  Pendiente  │ ───────────┘
               │  (Cliente/  │
               │  Interno)   │
               └─────────────┘
```

**Estados disponibles:**
- **Nueva** - Recién creada, sin asignar
- **En Progreso** - Siendo trabajada activamente
- **Pendiente Cliente** - Esperando información del cliente
- **Pendiente Interno** - Esperando recursos internos
- **Resuelta** - Solucionada, pendiente de cierre
- **Cerrada** - Finalizada
- **Cancelada** - Descartada

### 2.3 Comentarios e Historial
Cada incidencia mantiene un registro de actividad que incluye:
- Notas añadidas por los usuarios
- Cambios de estado automáticos
- Cambios de asignación
- Notas de resolución

### 2.4 Categorías Configurables
Sistema de categorías personalizables para clasificar incidencias según las necesidades del negocio (ej: Soporte Técnico, Facturación, Logística, etc.).

---

## 3. Interfaces de Usuario

### 3.1 Lista de Incidencias
Vista principal que muestra todas las incidencias con filtros predefinidos:
- **Mis incidencias abiertas** - Las asignadas al usuario actual
- **Todas las abiertas** - Pendientes de resolución
- **Críticas** - Prioridad máxima sin resolver

### 3.2 Ficha de Incidencia
Pantalla detallada para ver y editar una incidencia individual, organizada en secciones:
- Información general
- Descripción detallada
- Datos de contacto
- Información de origen
- Asignación
- Resolución

### 3.3 Configuración
Apartado en la configuración de Business Central para definir la numeración automática de incidencias.

---

## 4. Integración con Sistemas Externos

### 4.1 API REST
El sistema expone una API estándar que permite a sistemas externos:

| Operación | Descripción |
|-----------|-------------|
| **Crear incidencia** | Registrar una nueva incidencia desde un chatbot, formulario web, etc. |
| **Consultar incidencias** | Obtener lista de incidencias con filtros (por cliente, estado, fecha...) |
| **Actualizar estado** | Cambiar el estado de una incidencia |
| **Añadir comentarios** | Agregar notas a una incidencia existente |
| **Consultar categorías** | Obtener las categorías disponibles |

### 4.2 Casos de Uso de Integración

**Chatbot / Agente IA:**
1. Cliente reporta problema por chat
2. Agente crea incidencia vía API con los datos recogidos
3. Agente puede consultar estado de incidencias existentes del cliente

**Portal de Clientes:**
1. Cliente crea incidencia desde portal web
2. Sistema envía datos a Business Central vía API
3. Cliente puede consultar estado de sus incidencias

**Sistema de Monitorización:**
1. Sistema detecta anomalía
2. Crea incidencia automáticamente con referencia al evento
3. Equipo técnico ve la incidencia en Business Central

---

## 5. Permisos y Seguridad

### 5.1 Perfiles de Acceso

| Perfil | Permisos |
|--------|----------|
| **Administrador** | Acceso completo: crear, modificar, eliminar, configurar |
| **Usuario** | Crear, modificar incidencias asignadas, añadir comentarios |
| **Consulta** | Solo lectura para reporting |
| **API** | Acceso programático según configuración |

---

## 6. Beneficios Esperados

### Para el Equipo de Soporte
- Visión centralizada de todas las incidencias
- Información del cliente disponible en el mismo sistema
- Historial completo de cada caso

### Para la Dirección
- Visibilidad del volumen y estado de incidencias
- Métricas de tiempo de resolución
- Identificación de problemas recurrentes

### Para el Cliente
- Seguimiento de sus incidencias
- Comunicación más fluida
- Resolución más rápida al tener contexto completo

### Para la Automatización
- Integración nativa con agentes de IA
- Creación automática de incidencias
- Consulta de estado sin intervención humana

---

## 7. Alcance y Limitaciones

### Incluido en esta versión
✅ Gestión completa del ciclo de vida de incidencias  
✅ Categorización y priorización  
✅ Asignación a usuarios  
✅ Historial de comentarios  
✅ API para integración externa  
✅ Relación opcional con clientes de BC  

### Fuera de alcance (posibles mejoras futuras)
❌ Adjuntos y documentos  
❌ Notificaciones por email  
❌ Acuerdos de nivel de servicio (SLA)  
❌ Flujos de aprobación  
❌ Portal de cliente integrado  
❌ Informes y dashboards avanzados  

---

## 8. Requisitos Técnicos

| Requisito | Especificación |
|-----------|----------------|
| **Plataforma** | Microsoft Dynamics 365 Business Central |
| **Versión mínima** | BC 23.0 (2023 Wave 2) |
| **Despliegue** | Cloud (SaaS) o On-Premise |
| **Licencias** | Usuarios con licencia Essential o Premium |

---

## 9. Próximos Pasos

1. **Aprobación** - Validar requisitos con stakeholders
2. **Desarrollo** - Implementación de la extensión (estimación: 2-3 semanas)
3. **Testing** - Pruebas funcionales y de integración
4. **Documentación** - Manual de usuario y guía de API
5. **Despliegue** - Instalación en entorno de producción
6. **Formación** - Capacitación al equipo de soporte

---

## Anexo: Glosario

| Término | Definición |
|---------|------------|
| **Incidencia** | Registro de un problema, consulta o solicitud reportada |
| **API** | Interfaz de programación que permite a otros sistemas comunicarse con Business Central |
| **Agente IA** | Sistema automatizado que puede interactuar con usuarios y sistemas |
| **Business Central** | Sistema ERP de Microsoft donde se implementará esta solución |

---

*Documento preparado por VS Sistemas*
