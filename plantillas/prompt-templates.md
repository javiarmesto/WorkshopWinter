# 📝 Plantillas de Prompts

> **Prompts reutilizables para desarrollo AL con ALDC**

---

## 🏗️ Plantilla: Diseño con al-architect

```markdown
Use al-architect mode

Diseña [NOMBRE DEL SISTEMA] para Business Central con estos requisitos:

FUNCIONALIDAD PRINCIPAL:
- [Funcionalidad 1: descripción clara]
- [Funcionalidad 2: descripción clara]
- [Funcionalidad 3: descripción clara]

ENTIDADES (tablas principales):
1. [Entidad 1]: [descripción y campos principales]
2. [Entidad 2]: [descripción y campos principales]
3. [Entidad 3]: [descripción y campos principales]

REGLAS DE NEGOCIO:
- [Regla 1: condición y resultado]
- [Regla 2: condición y resultado]
- [Regla 3: condición y resultado]

INTEGRACIONES (si aplica):
- [Sistema externo 1]: [tipo de integración]
- [Sistema externo 2]: [tipo de integración]

APIS REQUERIDAS:
- /[endpoint1] - [operaciones: GET/POST/PATCH/DELETE]
- /[endpoint2] - [operaciones]

CONSIDERACIONES TÉCNICAS:
- Usar eventos (no modificar objetos base BC)
- Estructura AL-Go (App vs Test separados)
- Cobertura de tests 100%
- Rango de objetos: [XXXXX-XXXXX]
- Prefijo: [XXX]
```

---

## 💻 Plantilla: Implementación con al-conductor

```markdown
Use al-conductor mode

Implementa [NOMBRE DEL SISTEMA] diseñado por al-architect.

Requisitos de implementación:
- Seguir ciclo TDD estricto: RED → GREEN → REFACTOR
- Generar tests ANTES del código de implementación
- Usar estructura AL-Go (App/ y Test/ separados)
- Documentar cada fase en .github/plans/
- Code review automático con al-review-subagent
- Rango de objetos: [XXXXX-XXXXX]
- Prefijo para todos los objetos: [XXX]

NOTAS ADICIONALES (si aplica):
- [Nota 1 sobre implementación específica]
- [Nota 2 sobre dependencias]
```

---

## 🌐 Plantilla: Diseño de API con al-api

```markdown
Use al-api mode

Diseña APIs REST para [NOMBRE DEL SISTEMA] con estos requisitos:

ENDPOINTS REQUERIDOS:
1. /[recurso1]
   - GET: Listar todos / filtrar por [campos]
   - GET /{id}: Obtener uno
   - POST: Crear nuevo
   - PATCH: Actualizar
   - DELETE: Eliminar (si aplica)

2. /[recurso2]
   - [operaciones requeridas]

FILTROS ODATA NECESARIOS:
- $filter por [campo1, campo2, ...]
- $orderby por [campo1, campo2, ...]
- $select para [campos específicos]
- $expand para [relaciones]

AUTENTICACIÓN:
- OAuth2 / Basic / API Key

CASOS DE USO:
1. [Agente/Sistema 1]: [qué necesita consultar]
2. [Agente/Sistema 2]: [qué necesita consultar]

CONSIDERACIONES:
- Rango de objetos: [XXXXX-XXXXX]
- APIPublisher: '[publisher]'
- APIGroup: '[group]'
- APIVersion: 'v1.0'
```

---

## 🐛 Plantilla: Debug con al-debugger

```markdown
Use al-debugger mode

Tengo un problema en [COMPONENTE/ARCHIVO]:

ERROR:
[Copiar mensaje de error completo]

CONTEXTO:
- Qué estaba haciendo: [acción]
- Qué esperaba: [resultado esperado]
- Qué ocurrió: [resultado real]

CÓDIGO RELEVANTE:
```al
[Pegar código problemático]
```

INTENTOS DE SOLUCIÓN:
- [Qué ya probaste 1]
- [Qué ya probaste 2]
```

---

## ✅ Plantilla: Testing con al-tester

```markdown
Use al-tester mode

Necesito una estrategia de tests para [COMPONENTE]:

FUNCIONALIDADES A TESTEAR:
1. [Funcionalidad 1]: [casos positivos y negativos]
2. [Funcionalidad 2]: [casos positivos y negativos]
3. [Funcionalidad 3]: [casos positivos y negativos]

ESCENARIOS CRÍTICOS:
- [Escenario edge case 1]
- [Escenario edge case 2]

DATOS DE PRUEBA NECESARIOS:
- [Entidad 1]: [datos requeridos]
- [Entidad 2]: [datos requeridos]

MOCKS/STUBS NECESARIOS:
- [Dependencia a mockear]

COBERTURA OBJETIVO: [100% / 80% / etc.]
```

---

## 📦 Plantilla: Sistema CRUD Básico

```markdown
Use al-architect mode

Diseña un sistema CRUD para gestionar [ENTIDAD] con:

CAMPOS:
- [Campo 1] ([Tipo]): [descripción]
- [Campo 2] ([Tipo]): [descripción]
- [Campo 3] ([Tipo]): [descripción]
- ...

RELACIONES:
- [Relación con tabla X]
- [Relación con tabla Y]

VALIDACIONES:
- [Campo X]: [validación requerida]
- [Campo Y]: [validación requerida]

UI:
- Lista con filtros por [campos]
- Card con [grupos/FastTabs]
- FactBox de [información relacionada]

API:
- CRUD completo vía OData

Rango: [XXXXX-XXXXX]
Prefijo: [XXX]
```

---

## 🔄 Plantilla: Importación de Datos

```markdown
Use al-architect mode

Diseña un sistema de importación de datos desde [ORIGEN] a Business Central:

FORMATO DE ENTRADA:
- Tipo: [Excel / CSV / JSON / XML]
- Columnas/campos:
  - [columna 1]: [tipo y descripción]
  - [columna 2]: [tipo y descripción]
  - ...

TABLA DESTINO:
- [Nombre tabla BC]
- Campos a mapear: [lista]

REGLAS DE IMPORTACIÓN:
- Duplicados: [Skip / Update / Error]
- Validaciones: [lista]
- Transformaciones: [si aplica]

UI DE IMPORTACIÓN:
- Diálogo con selección de archivo
- Opciones de importación
- Log de resultados

Rango: [XXXXX-XXXXX]
Prefijo: [XXX]
```

---

## 🔗 Plantilla: Event Subscriber

```markdown
Use al-architect mode

Diseña event subscribers para [FUNCIONALIDAD]:

EVENTOS A SUSCRIBIR:
1. [Tabla/Codeunit].[Evento]
   - Trigger: [cuándo se dispara]
   - Acción: [qué debe hacer]
   
2. [Tabla/Codeunit].[Evento]
   - Trigger: [cuándo se dispara]
   - Acción: [qué debe hacer]

DATOS NECESARIOS:
- Acceso a: [tablas/campos]
- Parámetros del evento: [lista]

CONSIDERACIONES:
- No bloquear transacción principal
- Manejo de errores: [estrategia]
- Logging: [si aplica]

Rango: [XXXXX-XXXXX]
Prefijo: [XXX]
```

---

## 💡 Tips para Mejores Resultados

1. **Sé específico**: Más detalle = mejores resultados
2. **Incluye ejemplos**: Ayudan a entender el contexto
3. **Define límites**: Rango de IDs, prefijos, versiones
4. **Menciona dependencias**: Tablas existentes, APIs, etc.
5. **Especifica restricciones**: Qué NO debe hacer

---

*Última actualización: Enero 2025*
