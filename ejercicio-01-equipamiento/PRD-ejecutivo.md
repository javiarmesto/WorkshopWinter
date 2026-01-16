# PRD Ejecutivo: Sistema de Control de Equipamiento IT

## 1. Resumen Ejecutivo

### ¿Qué es?
Un módulo sencillo para **Business Central** que permite registrar y controlar préstamos de equipamiento IT (laptops, proyectores, cámaras) a empleados y usuarios externos.

### ¿Para quién?
- **Departamento IT**: Control de inventario tecnológico
- **Administración**: Gestión de préstamos
- **Empleados**: Transparencia sobre qué tienen asignado

### ¿Por qué?
Actualmente muchas empresas gestionan préstamos de equipamiento con:
- ❌ Excel (sin trazabilidad, propenso a errores)
- ❌ Email (difícil de auditar)
- ❌ Sin sistema (pérdida de equipos)

**Con este módulo**:
- ✅ Registro centralizado en Business Central
- ✅ Historial completo de préstamos
- ✅ Estados en tiempo real (Disponible/Prestado/Reparación/Retirado)

---

## 2. Funcionalidad Principal

### 2.1 Catálogo de Equipamiento

**Tabla maestra** que almacena:
- Código único del equipo (ej: `LAP-001`, `PROJ-005`)
- Descripción (marca, modelo, características)
- Tipo de equipo (Laptop, Proyector, Tablet, Cámara, Monitor, Teclado/Ratón, Otro)
- Estado actual
- Ubicación física cuando disponible
- Número de serie
- Fecha y costo de compra
- Notas adicionales

### 2.2 Gestión de Préstamos

**Registro de transacciones** con:
- Número de préstamo (autonumérico)
- Equipo prestado (con descripción denormalizada)
- Datos del prestatario:
  - Tipo (Empleado o Externo)
  - Código/Número
  - Nombre
  - Email de contacto
- Fechas:
  - Préstamo (auto-rellena con hoy)
  - Devolución esperada
  - Devolución real (vacío hasta que devuelve)
- Estado (Activo, Devuelto, Vencido)
- Notas (finalidad, observaciones)

### 2.3 Estados del Equipamiento

| Estado | Descripción | Color UI |
|--------|-------------|----------|
| **Disponible** | Listo para prestar | 🟢 Verde |
| **Prestado** | En poder de usuario | 🟡 Amarillo |
| **En Reparación** | No disponible temporalmente | 🔴 Rojo |
| **Retirado** | Dado de baja | ⚫ Gris |

---

## 3. Interfaz de Usuario

### 3.1 Páginas Principales

#### **Lista de Equipamiento** (VSS Equipment List)
- Vista general de todo el inventario
- Filtros rápidos:
  - Disponibles
  - Prestados
  - Todos
- Columnas: Código, Descripción, Tipo, Estado, Ubicación, Préstamo Activo
- Acciones:
  - Nuevo Préstamo
  - Ver Préstamo Activo (si aplica)
  - Historial de Préstamos

#### **Ficha de Equipo** (VSS Equipment Card)
- Pestaña General: Código, Descripción, Tipo, Estado, Ubicación
- Pestaña Detalles: Número de serie, Fecha compra, Costo, Notas
- FactBox lateral: Historial de préstamos (últimos 10)
- Acciones:
  - Crear Préstamo
  - Ver Préstamo Activo

#### **Lista de Préstamos** (VSS Equipment Loans)
- Todos los préstamos registrados
- Filtros:
  - Préstamos Activos
  - Préstamos Vencidos
  - Todos los Préstamos
- Columnas: Nº Préstamo, Equipo, Prestatario, Fecha Préstamo, Devolución Esperada, Estado
- Acciones:
  - Registrar Devolución
  - Ver Ficha de Equipo

#### **Ficha de Préstamo** (VSS Equipment Loan Card)
- Pestaña General: Nº Préstamo, Equipo (con lookup)
- Pestaña Prestatario: Tipo, Código, Nombre, Email
- Pestaña Fechas: Préstamo, Devolución Esperada, Devolución Real
- Pestaña Notas: Finalidad del préstamo
- Acciones:
  - Registrar Devolución

---

## 4. Casos de Uso

### Caso 1: Préstamo de Laptop
**Actor**: Administrador IT

1. Usuario solicita laptop para evento
2. Admin busca "VSS Equipment List"
3. Filtra por "Disponibles" y tipo "Laptop"
4. Selecciona `LAP-003` → Acción "Nuevo Préstamo"
5. Rellena:
   - Prestatario: `Juan Pérez` (juan.perez@empresa.com)
   - Devolución esperada: `20/01/2025`
   - Notas: `Presentación congreso Madrid`
6. Confirma → Estado cambia a "Prestado"

### Caso 2: Devolución de Proyector
**Actor**: Administrador IT

1. Usuario devuelve proyector
2. Admin busca "VSS Equipment Loans"
3. Filtra por "Activos"
4. Selecciona préstamo `LOAN-00123` → Acción "Registrar Devolución"
5. Sistema auto-rellena fecha devolución con hoy
6. Estado del proyector vuelve a "Disponible"

### Caso 3: Detección de Vencidos
**Actor**: Sistema (automático)

1. Cada día ejecuta verificación
2. Encuentra préstamos con:
   - Estado = "Activo"
   - Devolución esperada < Hoy
3. Cambia estado a "Vencido"
4. Aparece en filtro "Préstamos Vencidos"

---

## 5. Beneficios

| Beneficio | Métrica Esperada |
|-----------|-----------------|
| Reducción de equipos perdidos | -70% anual |
| Tiempo de gestión de préstamos | -80% (de 15 min a 3 min) |
| Trazabilidad completa | 100% de operaciones auditables |
| Visibilidad de disponibilidad | Tiempo real vs. 24h de retraso |

---

## 6. Alcance

### ✅ Incluido (MVP)
- Catálogo de equipamiento
- Registro de préstamos/devoluciones
- Cambio automático de estados
- Historial de préstamos por equipo
- Detección de vencidos

### ❌ Excluido (Futuro)
- Escaneo de códigos de barras/QR
- Reservas anticipadas
- Notificaciones por email
- Integración con APIs externas
- App móvil

---

## 7. Requisitos No Funcionales

### Plataforma
- **Business Central**: Versión 23.0+ (Cloud/On-Premise)
- **Lenguaje**: AL (Application Language)
- **Arquitectura**: Extension (no modifica objetos base)

### Performance
- Lista de equipamiento: <2 segundos para 1000 registros
- Creación de préstamo: <1 segundo
- Consulta de historial: <3 segundos

### Seguridad
- Permission Set "VSS Equipment - All" (acceso completo)
- Permission Set "VSS Equipment - View" (solo lectura)

---

## 8. Próximos Pasos

1. **Validación**: Revisión con stakeholders (IT Manager)
2. **Prototipo**: Mockups de páginas principales
3. **Desarrollo**: Implementación con ALDC + GitHub Copilot
4. **Testing**: Pruebas unitarias + UAT
5. **Despliegue**: Sandbox → Producción

---

**Estimación de Desarrollo**: 25 minutos (con ALDC + GitHub Copilot)  
**Fecha de Entrega Esperada**: Enero 2025  
**Owner**: Departamento IT  
**Stakeholders**: Administración, Empleados

---

*Documento preparado para el Workshop Winter - Ejercicio 1*
