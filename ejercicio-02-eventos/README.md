# 📋 Ejercicio 2: Sistema de Registros de Eventos

> **Importa datos de Eventbrite y expón APIs para consulta por agentes**

| Complejidad | Tiempo | Objetos | Tests |
|-------------|--------|---------|-------|
| 🟢 Media-Baja | 1-2 horas | ~10 | ~25 |

---

## 🎯 Objetivo

Crear una extensión de Business Central que permita:
- Importar registros de eventos desde Excel (formato Eventbrite)
- Visualizar asistentes en Business Central
- Exponer APIs de solo lectura para que agentes consulten datos
- Soportar múltiples eventos

---

## 📋 Requisitos de Negocio

Lee primero el documento de requisitos ejecutivo:

👉 [PRD Ejecutivo](./PRD-ejecutivo.md)

---

## 📊 Datos de Ejemplo

En la carpeta `datos-ejemplo/` encontrarás un Excel real exportado de Eventbrite del **Business Central & Agents Winter Fest**.

**Estructura del Excel (34 columnas):**
- Order ID, Order date
- Buyer first name, Buyer last name, Buyer email, Phone
- Purchaser city, state, country
- Event name, Event ID, Event start date/time, Event location
- Ticket quantity, Add-ons quantity
- Currency, Payment status, Payment type
- Gross sales, Service fees, Net sales
- Guest (Yes/No)

---

## 🏗️ Paso 1: Diseño con al-architect (15-20 min)

### 1.1 Abre GitHub Copilot Chat

```
Ctrl+Alt+I (Windows/Linux)
Cmd+Alt+I (Mac)
```

### 1.2 Copia y pega este prompt

```markdown
Use al-architect mode

Diseña un sistema de gestión de registros de eventos para Business Central con estos requisitos:

FUNCIONALIDAD PRINCIPAL:
- Importar registros desde Excel (formato Eventbrite con 34 columnas)
- Visualizar todos los asistentes registrados
- Filtrar por evento, ciudad, país, fecha
- Exponer APIs de SOLO LECTURA para agentes externos

DATOS A IMPORTAR (columnas principales del Excel Eventbrite):
- Order ID (identificador único)
- Order date (fecha/hora del pedido)
- Buyer first name, last name, email, phone
- Purchaser city, state, country
- Event name, Event ID, Event start date/time, Event location
- Ticket quantity, Add-ons quantity
- Currency, Payment status, Payment type
- Gross sales, Net sales
- Guest (Yes/No)

ENTIDADES:
1. Evento (maestro para soportar múltiples eventos)
2. Registro de Evento (datos del asistente/pedido)

REGLAS DE NEGOCIO:
- Order ID es único (no duplicados)
- Si el evento no existe, crearlo automáticamente al importar
- Buyer Full Name se calcula de First Name + Last Name
- Registrar fecha/hora y usuario de importación

APIS REQUERIDAS (solo lectura):
- /events - Lista de eventos
- /eventRegistrations - Registros con filtros por evento, email, ciudad

CONSIDERACIONES TÉCNICAS:
- Usar eventos (no modificar objetos base)
- Estructura AL-Go (App vs Test separados)
- Rango de objetos: 50200-50299
- Prefijo: VSS (VS Sistemas)
```

### 1.3 Espera la respuesta de al-architect

**Deberías recibir:**
- 📐 Modelo de datos (2 tablas)
- 🔄 Lógica de importación Excel
- 📄 Diseño de UI (pages)
- 🌐 Especificación de APIs (read-only)
- 🧪 Plan de testing

### 1.4 ✅ Checkpoint de Diseño

Verifica que el diseño incluye:
- [ ] Tabla VSS Event (maestro de eventos)
- [ ] Tabla VSS Event Registration (30+ campos)
- [ ] Page de importación Excel
- [ ] Codeunit de mapeo Excel → Tabla
- [ ] 2 API Pages (solo GET)
- [ ] Plan de tests

---

## 💻 Paso 2: Implementación TDD con al-conductor (60-90 min)

### 2.1 Inicia al-conductor

En el mismo chat de Copilot:

```markdown
Use al-conductor mode

Implementa el sistema de registros de eventos diseñado por al-architect.

Requisitos de implementación:
- Seguir ciclo TDD estricto: RED → GREEN → REFACTOR
- Generar tests ANTES del código de implementación
- Usar estructura AL-Go (App/ y Test/ separados)
- Documentar cada fase en .github/plans/
- Code review automático con al-review-subagent
- Rango de objetos: 50200-50299
- Prefijo para todos los objetos: VSS

NOTA IMPORTANTE sobre la importación Excel:
- Usar el objeto estándar "Excel Buffer" de BC
- Mapear las 34 columnas del formato Eventbrite
- Validar Order ID único antes de insertar
```

### 2.2 Proceso Automático

al-conductor orquestará automáticamente:

```
📊 PLANNING PHASE
   └── Analiza proyecto, identifica 6 fases

🔴 FASE 1: Tabla Event (Maestro)
   ├── RED: Test CRUD eventos
   ├── GREEN: Implementar tabla
   └── REFACTOR: Índice por External Event ID

🔴 FASE 2: Tabla Event Registration
   ├── RED: Test campos (30+)
   ├── GREEN: Implementar tabla completa
   └── REFACTOR: Índices para búsqueda

🔴 FASE 3: Codeunit Import Management
   ├── RED: Test mapeo Excel
   ├── GREEN: ImportFromExcel, MapExcelRow
   └── REFACTOR: Detección de duplicados

🔴 FASE 4: Pages (List + Card + Import Dialog)
   ├── RED: Test navegación
   ├── GREEN: Implementar pages
   └── REFACTOR: Filtros y vistas

🔴 FASE 5: APIs (Read-Only)
   ├── RED: Test endpoints GET
   ├── GREEN: Implementar 2 API pages
   └── REFACTOR: Filtros OData

🔴 FASE 6: Integración y Permisos
   ├── RED: Test end-to-end
   ├── GREEN: Permission sets
   └── REFACTOR: Documentación
```

### 2.3 ✅ Checkpoint de Implementación

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

---

## 📥 Paso 4: Probar la Importación (15 min)

### 4.1 Abrir Business Central

1. Buscar "VSS Event Registrations" en la búsqueda
2. Click en "Import from Excel" en la barra de acciones

### 4.2 Importar el Archivo de Ejemplo

1. Selecciona el archivo `datos-ejemplo/eventbrite-export.xlsx`
2. Opciones:
   - ☑️ Update Existing: No (primera importación)
   - ☑️ Skip Duplicates: Sí
3. Click "Import"

### 4.3 Verificar Resultados

- Deberían aparecer **36 registros** importados
- Un evento creado automáticamente: "Business Central & Agents Winter Fest"
- Datos de compradores con nombre, email, ciudad

### 4.4 Explorar los Datos

1. Filtra por ciudad (ej: "Valencia")
2. Filtra por cantidad de tickets > 1
3. Abre la ficha de un registro para ver todos los campos

---

## 🌐 Paso 5: Probar las APIs (10 min)

### 5.1 Endpoints Disponibles

```
Base URL: https://[tu-tenant].api.bc.dynamics.com/v2.0/[environment]/api/vssistemas/eventManagement/v1.0/
```

### 5.2 Ejemplos de Llamadas

**Listar Eventos:**
```http
GET /events
```

**Todos los Registros de un Evento:**
```http
GET /eventRegistrations?$filter=eventCode eq 'WINTERFEST2026'
```

**Buscar por Email (para check-in):**
```http
GET /eventRegistrations?$filter=buyerEmail eq 'asistente@empresa.com'
```

**Registros de Valencia:**
```http
GET /eventRegistrations?$filter=city eq 'Valencia'
```

**Solo Nombres y Emails (para badges):**
```http
GET /eventRegistrations?$select=buyerFullName,buyerEmail,city,ticketQuantity&$filter=eventCode eq 'WINTERFEST2026'
```

**Ordenar por Apellido:**
```http
GET /eventRegistrations?$orderby=buyerLastName&$filter=eventCode eq 'WINTERFEST2026'
```

---

## 📦 Estructura Final Esperada

```
src/
├── App/
│   ├── Tables/
│   │   ├── Tab50200.VSSEvent.al
│   │   └── Tab50201.VSSEventRegistration.al
│   ├── Pages/
│   │   ├── Pag50200.VSSEvents.al
│   │   ├── Pag50201.VSSEventCard.al
│   │   ├── Pag50202.VSSEventRegistrations.al
│   │   ├── Pag50203.VSSEventRegistrationCard.al
│   │   └── Pag50204.VSSImportEventRegistrations.al
│   ├── APIs/
│   │   ├── Pag50205.VSSEventRegistrationsAPI.al
│   │   └── Pag50206.VSSEventsAPI.al
│   ├── Codeunits/
│   │   └── Cod50200.VSSEventRegistrationMgmt.al
│   └── Permissions/
│       ├── PermSet50200.VSSEventRegAll.al
│       └── PermSet50201.VSSEventRegView.al
│
└── Test/
    └── VSSEventRegistrationTests.Codeunit.al (~25 tests)
```

---

## 🤖 Casos de Uso para Agentes

### Agente de Badges
```
1. GET /eventRegistrations?$select=buyerFullName,buyerEmail,city&$filter=eventCode eq 'WINTERFEST2026'
2. Para cada registro, generar badge con nombre y ciudad
3. Exportar a PDF o enviar a impresión
```

### Agente de Check-in
```
1. Usuario escanea QR o da su email
2. GET /eventRegistrations?$filter=buyerEmail eq '{email}'
3. Si existe → Mostrar datos y marcar presente
4. Si no existe → Informar que no está registrado
```

### Agente de Comunicaciones
```
1. GET /eventRegistrations?$filter=eventCode eq 'WINTERFEST2026'
2. Extraer emails únicos
3. Enviar recordatorio con información del evento
```

### Dashboard de Estadísticas
```
1. GET /eventRegistrations?$filter=eventCode eq 'WINTERFEST2026'&$count=true
2. Mostrar: Total registros, por ciudad, por país
3. Actualizar en tiempo real
```

---

## ✅ Checklist Final

### Funcionalidad
- [ ] Puedo importar el Excel de Eventbrite
- [ ] Se crean los registros correctamente (36)
- [ ] El evento se crea automáticamente
- [ ] Los filtros funcionan (ciudad, evento)
- [ ] La búsqueda por email funciona

### APIs
- [ ] GET /events devuelve la lista de eventos
- [ ] GET /eventRegistrations devuelve registros
- [ ] Los filtros OData funcionan
- [ ] La API es de solo lectura (no POST/PATCH)

### Calidad
- [ ] Todos los tests pasan
- [ ] No hay errores de compilación
- [ ] Permission sets generados

---

## 🐛 Troubleshooting

### "Error al importar: Column not found"
1. Verifica que el Excel tiene el formato correcto de Eventbrite
2. Las columnas deben estar en la fila 1 (encabezados)
3. Comprueba que no hay columnas renombradas

### "Registros duplicados"
1. El Order ID debe ser único
2. Usa la opción "Skip Duplicates" = Sí
3. O "Update Existing" = Sí para actualizar

### "API devuelve 0 registros"
1. Verifica que has importado datos
2. Comprueba el filtro (eventCode correcto)
3. Asegúrate de tener permisos de lectura

### "El evento no se crea"
1. Verifica que el Excel tiene las columnas Event name, Event ID
2. La función FindOrCreateEvent debe tener datos válidos

---

## 📚 Recursos Adicionales

- [PRD Técnico Completo](./PRD-tecnico.md)
- [Datos de Ejemplo](./datos-ejemplo/)
- [Volver al Workshop Principal](../README.md)

---

## 🎉 ¡Felicidades!

Has completado ambos ejercicios del workshop. Ahora sabes:

✅ Diseñar soluciones con al-architect  
✅ Implementar con TDD usando al-conductor  
✅ Crear APIs REST para agentes  
✅ Importar datos externos a Business Central  
✅ Aplicar mejores prácticas de desarrollo AL  

---

## ➡️ Próximos Pasos

1. **Adapta los ejercicios** a tus necesidades reales
2. **Combina funcionalidades** (ej: incidencias de asistentes a eventos)
3. **Explora más comandos** del AL Development Collection
4. **Comparte tu experiencia** con la comunidad

👉 [Volver al Workshop Principal](../README.md)
