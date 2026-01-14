# 🎫 Ejercicio 1: Sistema de Gestión de Incidencias

> **Construye un sistema completo de tickets con API para agentes externos**

| Complejidad | Tiempo | Objetos | Tests |
|-------------|--------|---------|-------|
| 🟡 Media | 2-3 horas | ~15 | ~40 |

---

## 🎯 Objetivo

Crear una extensión de Business Central que permita:
- Registrar y gestionar incidencias de clientes
- Seguir el ciclo de vida completo (Nueva → En Progreso → Resuelta → Cerrada)
- Exponer APIs REST para que agentes de IA puedan crear y consultar incidencias
- Mantener historial de comentarios y cambios

---

## 📋 Requisitos de Negocio

Lee primero el documento de requisitos ejecutivo para entender **qué** vamos a construir:

👉 [PRD Ejecutivo](./PRD-ejecutivo.md)

---

## 🏗️ Paso 1: Diseño con al-architect (20-30 min)

### 1.1 Abre GitHub Copilot Chat

```
Ctrl+Alt+I (Windows/Linux)
Cmd+Alt+I (Mac)
```

### 1.2 Copia y pega este prompt

```markdown
Use al-architect mode

Diseña un sistema de gestión de incidencias para Business Central con estos requisitos:

FUNCIONALIDAD PRINCIPAL:
- Registrar incidencias con: descripción, categoría, prioridad, cliente, contacto
- Gestionar ciclo de vida: Nueva → En Progreso → Pendiente → Resuelta → Cerrada
- Asignar incidencias a usuarios
- Registrar comentarios e historial de cambios
- Exponer APIs REST para agentes externos

ENTIDADES:
1. Categoría de Incidencia (maestro de categorías)
2. Incidencia (entidad principal con todos los datos)
3. Comentario de Incidencia (historial y notas)

REGLAS DE NEGOCIO:
- Número de incidencia automático (No. Series)
- Cambio de estado genera comentario automático
- Cambio de asignación genera comentario automático
- Estado Resuelto requiere fecha de resolución
- Email de contacto debe validarse si se proporciona

APIS REQUERIDAS:
- /incidents - CRUD completo de incidencias
- /incidentCategories - Gestión de categorías
- /incidentComments - Añadir y consultar comentarios

CONSIDERACIONES TÉCNICAS:
- Usar eventos (no modificar objetos base)
- Estructura AL-Go (App vs Test separados)
- Cobertura de tests 100%
- Rango de objetos: 50100-50199
- Prefijo: VSS (VS Sistemas)
```

### 1.3 Espera la respuesta de al-architect

**Deberías recibir:**
- 📐 Modelo de datos completo (3 tablas + 1 extensión)
- 🔗 Puntos de integración
- 📄 Diseño de UI (pages)
- 🌐 Especificación de APIs
- 🧪 Plan de testing

### 1.4 ✅ Checkpoint de Diseño

Verifica que el diseño incluye:
- [ ] Tabla VSS Incident Category
- [ ] Tabla VSS Incident (con todos los campos)
- [ ] Tabla VSS Incident Comment
- [ ] Enum VSS Incident Status
- [ ] Enum VSS Incident Priority
- [ ] Codeunit VSS Incident Management
- [ ] 3 API Pages
- [ ] Plan de tests

---

## 💻 Paso 2: Implementación TDD con al-conductor (90-120 min)

### 2.1 Inicia al-conductor

En el mismo chat de Copilot:

```markdown
Use al-conductor mode

Implementa el sistema de gestión de incidencias diseñado por al-architect.

Requisitos de implementación:
- Seguir ciclo TDD estricto: RED → GREEN → REFACTOR
- Generar tests ANTES del código de implementación
- Usar estructura AL-Go (App/ y Test/ separados)
- Documentar cada fase en .github/plans/
- Code review automático con al-review-subagent
- Rango de objetos: 50100-50199
- Prefijo para todos los objetos: VSS
```

### 2.2 Proceso Automático

al-conductor orquestará automáticamente:

```
📊 PLANNING PHASE
   └── Analiza proyecto, identifica 8 fases

🔴 FASE 1: Enums (Status, Priority, Comment Type)
   ├── RED: Test de valores enum
   ├── GREEN: Implementar enums
   └── REFACTOR: Review y documentación

🔴 FASE 2: Tabla Incident Category
   ├── RED: Test CRUD categorías
   ├── GREEN: Implementar tabla
   └── REFACTOR: Validaciones

🔴 FASE 3: Tabla Incident
   ├── RED: Test campos y relaciones
   ├── GREEN: Implementar tabla completa
   └── REFACTOR: Índices y FlowFields

🔴 FASE 4: Tabla Incident Comment
   ├── RED: Test comentarios
   ├── GREEN: Implementar tabla
   └── REFACTOR: Auto-increment LineNo

🔴 FASE 5: Codeunit Management
   ├── RED: Test lógica de negocio
   ├── GREEN: CreateIncident, UpdateStatus, AddComment
   └── REFACTOR: Comentarios automáticos

🔴 FASE 6: Pages (List + Card)
   ├── RED: Test navegación
   ├── GREEN: Implementar pages
   └── REFACTOR: Actions y FactBoxes

🔴 FASE 7: APIs
   ├── RED: Test endpoints
   ├── GREEN: Implementar 3 API pages
   └── REFACTOR: Filtros OData

🔴 FASE 8: Setup Extension
   ├── RED: Test No. Series
   ├── GREEN: Table/Page extension
   └── REFACTOR: Integración
```

### 2.3 Durante la Implementación

**Interactúa cuando sea necesario:**
- Si al-conductor pregunta algo, responde con contexto
- Si hay errores de compilación, usa `@workspace use al-diagnose`
- Puedes pedir pausas entre fases para revisar

### 2.4 ✅ Checkpoint de Implementación

Después de cada fase, verifica:
- [ ] Tests pasan (GREEN)
- [ ] Código compila sin errores
- [ ] Review de al-review-subagent OK

---

## 🔧 Paso 3: Permisos y Build (10 min)

### 3.1 Generar Permission Sets

```bash
@workspace use al-permissions
```

### 3.2 Compilar y Desplegar

```bash
@workspace use al-build
```

### 3.3 Verificar en Business Central

1. Abrir Business Central
2. Buscar "VSS Incidents" en la búsqueda
3. Crear una categoría de prueba
4. Crear una incidencia
5. Cambiar estado y verificar comentario automático

---

## 🌐 Paso 4: Probar las APIs (15 min)

### 4.1 Endpoints Disponibles

```
Base URL: https://[tu-tenant].api.bc.dynamics.com/v2.0/[environment]/api/vssistemas/incidentManagement/v1.0/
```

### 4.2 Ejemplos de Llamadas

**Crear Incidencia:**
```http
POST /incidents
Content-Type: application/json

{
  "description": "Cliente no puede acceder al portal",
  "categoryCode": "SOPORTE",
  "priority": "High",
  "customerNo": "C00010",
  "contactName": "Juan García",
  "contactEmail": "juan@cliente.com",
  "sourceChannel": "ChatBot"
}
```

**Consultar Incidencias Abiertas:**
```http
GET /incidents?$filter=status ne 'Closed' and status ne 'Cancelled'&$orderby=createdDateTime desc
```

**Añadir Comentario:**
```http
POST /incidentComments
Content-Type: application/json

{
  "incidentNo": "INC-00001",
  "comment": "Contactado cliente, esperando información adicional",
  "commentType": "Note"
}
```

### 4.3 Probar con Postman o REST Client

1. Configura autenticación OAuth2
2. Prueba GET /incidents (debería devolver lista vacía o con datos de prueba)
3. Prueba POST /incidents para crear una incidencia
4. Prueba GET /incidents/{id} para verificar

---

## 📦 Estructura Final Esperada

```
src/
├── App/
│   ├── Enums/
│   │   ├── Enum50100.VSSIncidentStatus.al
│   │   ├── Enum50101.VSSIncidentPriority.al
│   │   └── Enum50102.VSSIncidentCommentType.al
│   ├── Tables/
│   │   ├── Tab50100.VSSIncidentCategory.al
│   │   ├── Tab50101.VSSIncident.al
│   │   └── Tab50102.VSSIncidentComment.al
│   ├── TableExtensions/
│   │   └── TabExt50103.VSSIncidentSetup.al
│   ├── Pages/
│   │   ├── Pag50100.VSSIncidentCategories.al
│   │   ├── Pag50101.VSSIncidentCategoryCard.al
│   │   ├── Pag50102.VSSIncidents.al
│   │   ├── Pag50103.VSSIncidentCard.al
│   │   ├── Pag50104.VSSIncidentComments.al
│   │   └── Pag50105.VSSIncidentCommentList.al
│   ├── PageExtensions/
│   │   └── PagExt50106.VSSIncidentSetupExt.al
│   ├── APIs/
│   │   ├── Pag50107.VSSIncidentsAPI.al
│   │   ├── Pag50108.VSSIncidentCategoriesAPI.al
│   │   └── Pag50109.VSSIncidentCommentsAPI.al
│   ├── Codeunits/
│   │   ├── Cod50100.VSSIncidentManagement.al
│   │   └── Cod50101.VSSIncidentAPIActions.al
│   └── Permissions/
│       ├── PermSet50100.VSSIncidentMgmtAll.al
│       └── PermSet50101.VSSIncidentMgmtView.al
│
└── Test/
    └── VSSIncidentTests.Codeunit.al (~40 tests)
```

---

## ✅ Checklist Final

### Funcionalidad
- [ ] Puedo crear categorías de incidencias
- [ ] Puedo crear incidencias con todos los campos
- [ ] Los cambios de estado generan comentarios automáticos
- [ ] Puedo asignar incidencias a usuarios
- [ ] El historial de comentarios funciona

### APIs
- [ ] GET /incidents devuelve lista de incidencias
- [ ] POST /incidents crea nueva incidencia
- [ ] Los filtros OData funcionan ($filter, $orderby)
- [ ] POST /incidentComments añade comentarios

### Calidad
- [ ] Todos los tests pasan
- [ ] No hay errores de compilación
- [ ] Permission sets generados
- [ ] Documentación en .github/plans/

---

## 🐛 Troubleshooting

### "No se genera el número de incidencia"
1. Ve a Setup → Sales & Receivables Setup
2. Configura el campo "Incident Nos." con una No. Series válida

### "API devuelve 401 Unauthorized"
1. Verifica que tienes los permisos correctos
2. Comprueba la configuración OAuth2
3. Asegúrate de que el usuario tiene el Permission Set asignado

### "El test falla con 'Table not found'"
```bash
@workspace use al-build
# Recompila para generar los símbolos
```

### "al-conductor se detiene"
```markdown
Use al-conductor mode

Continúa la implementación desde la fase [X]
```

---

## 📚 Recursos Adicionales

- [PRD Técnico Completo](./PRD-tecnico.md)
- [Ejemplos de API](./ejemplos-api.md)
- [Volver al Workshop Principal](../README.md)

---

## ➡️ Siguiente Ejercicio

Una vez completado, continúa con:

👉 [Ejercicio 2: Registros de Eventos](../ejercicio-02-eventos/README.md)
