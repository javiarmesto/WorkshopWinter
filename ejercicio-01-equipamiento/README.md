# Ejercicio 1: Sistema de Control de Equipamiento 💻

## Descripción del Escenario

Este ejercicio te introduce al desarrollo de extensiones AL usando **ALDC + GitHub Copilot**. Desarrollarás un sistema simple pero completo para gestionar préstamos de equipamiento IT (laptops, proyectores, tablets) en Business Central.

**Complejidad:** 🟢 **Baja** | **Tiempo estimado:** 25 minutos | **Nivel:** Principiante

---

## Objetivos de Aprendizaje

Al completar este ejercicio habrás:

✅ Usado los agentes básicos de ALDC (`al-architect`, `al-conductor`)  
✅ Creado objetos AL fundamentales (tablas, páginas, enums, codeunits)  
✅ Generado automáticamente el código AL con GitHub Copilot  
✅ Implementado lógica de negocio simple (préstamo/devolución)  
✅ Ejecutado tests unitarios generados por ALDC  

---

## Recursos del Ejercicio

| Documento | Descripción | Cuándo Consultarlo |
|-----------|-------------|-------------------|
| [**PRD-ejecutivo.md**](PRD-ejecutivo.md) | Requisitos de negocio y funcionalidad | Al inicio (para entender el objetivo) |
| [**PRD-tecnico.md**](PRD-tecnico.md) | Especificaciones técnicas detalladas | Antes de iniciar desarrollo |
| [**Cheatsheet ALDC**](../recursos/cheatsheet-aldc.md) | Referencia rápida de comandos | Durante todo el proceso |

---

## Requisitos Previos

Antes de comenzar asegúrate de tener:

- [x] **VS Code** con la extensión **AL Language** instalada
- [x] **AL Development Collection v2.9.0+** activo
- [x] **GitHub Copilot** activo y configurado
- [x] Workspace de Business Central (nube o local v23.0+)
- [x] Rango de IDs asignado: **50000-50099**

---

## Paso a Paso

### 📋 Fase 1: Análisis y Planificación (5 min)

#### 1.1 Lee los requisitos de negocio

Abre y revisa:
- [PRD-ejecutivo.md](PRD-ejecutivo.md): Funcionalidad esperada
- [PRD-tecnico.md](PRD-tecnico.md): Estructura de objetos AL

#### 1.2 Genera la arquitectura con `al-architect`

```
@al-architect analiza el PRD-tecnico.md y genera la arquitectura completa del proyecto de gestión de equipamiento IT. 

Debes crear:
- 2 tablas (Equipment, Equipment Loan)
- 4 enums (Equipment Type, Equipment Status, Loan Status, Borrower Type)
- 5 páginas (Equipment List/Card, Loan List/Card, History ListPart)
- 1 table extension (Setup)
- 1 page extension (Setup)
- 1 codeunit (Equipment Management)

Usa el rango de IDs 50000-50099.
```

**¿Qué genera `al-architect`?**
- ✅ `architecture.md` en `.github/plans/` con estructura completa
- ✅ Diagrama de relaciones entre objetos
- ✅ Análisis de impacto y dependencias

#### 1.3 Revisa la arquitectura generada

Abre `.github/plans/architecture.md` y verifica:
- Estructura de tablas (campos, claves, FlowFields)
- Páginas y su navegación
- Enums y valores
- Codeunit y procedimientos

---

### 🏗️ Fase 2: Desarrollo con ALDC (15 min)

#### 2.1 Genera el código completo con `al-conductor`

```
@al-conductor implementa la arquitectura definida en .github/plans/architecture.md para el sistema de equipamiento IT.

Genera todos los objetos AL:
1. Tablas y table extensions
2. Enums
3. Páginas y page extensions
4. Codeunit de lógica de negocio
5. Permission sets

Incluye validaciones, triggers, y lógica para:
- Crear préstamo (cambiar estado a "On Loan")
- Registrar devolución (volver a "Available")
- Detectar préstamos vencidos
```

**¿Qué hace `al-conductor` orquestando otros agentes?**

| Agente Orquestado | Tarea | Archivo Generado |
|------------------|-------|-----------------|
| `al-object-generator` | Crear tablas | `Tab50000.VSSEquipment.al`, `Tab50001.VSSEquipmentLoan.al` |
| `al-object-generator` | Crear enums | `Enum50000-50003.al` |
| `al-object-generator` | Crear páginas | `Pag50000-50004.al` |
| `al-object-generator` | Crear codeunit | `Cod50000.VSSEquipmentManagement.al` |
| `al-permission-generator` | Permission sets | `PermSet50000-50001.al` |

> **💡 Tip**: Mientras `al-conductor` genera el código, GitHub Copilot autocompleta automáticamente campos, procedimientos y validaciones basándose en el contexto del PRD.

#### 2.2 Revisa el código generado

Verifica en `src/`:
- [x] Tablas tienen campos con tipos correctos
- [x] Enums tienen valores esperados
- [x] Páginas tienen actions (New Loan, Register Return)
- [x] Codeunit implementa `CreateLoan()` y `RegisterReturn()`

#### 2.3 Compila la extensión

```bash
Ctrl+Shift+P > AL: Package
```

Revisa los errores (si los hay) y corrígelos con:

```
@al-test-helper corrige los errores de compilación en [nombre del archivo]
```

---

### 🧪 Fase 3: Testing y Validación (5 min)

#### 3.1 Genera tests unitarios con `al-tdd`

```
@al-tdd genera tests unitarios para el codeunit VSS Equipment Management (50000).

Debe cubrir:
1. CreateLoan() - verificar que el equipo cambia a "On Loan"
2. RegisterReturn() - verificar que el equipo vuelve a "Available"
3. CheckOverdueLoans() - detectar préstamos vencidos
4. CanLoanEquipment() - validar que solo equipos disponibles se pueden prestar
```

**Archivos generados:**
- `test/Cod50050.VSSEquipmentMgmtTests.al`

#### 3.2 Ejecuta los tests

```bash
Ctrl+Shift+P > AL: Run Tests
```

#### 3.3 Valida cobertura con `al-test-analyzer`

```
@al-test-analyzer analiza la cobertura de tests y genera reporte
```

**Objetivo:** 80%+ de cobertura en lógica de negocio.

---

### 🚀 Fase 4: Despliegue y Demostración (opcional)

#### 4.1 Publica la extensión

```bash
Ctrl+Shift+P > AL: Publish
```

#### 4.2 Prueba en Business Central

1. Abre Business Central (Sandbox/Docker)
2. Busca "VSS Equipment List"
3. Crea un equipo (LAP-001, Laptop)
4. Crea un préstamo a un empleado
5. Verifica que el estado cambia a "On Loan"
6. Registra la devolución
7. Verifica que vuelve a "Available"

---

## Solución Completa

Si te atascas, consulta la [carpeta de solución](./solucion/) con el código completo implementado.

**Archivos disponibles:**
- Todos los objetos AL generados
- Tests unitarios
- Documentación de la arquitectura

---

## Puntos de Control (Checklist)

Antes de considerar el ejercicio completado:

- [ ] **Arquitectura** generada con `al-architect` en `.github/plans/architecture.md`
- [ ] **Código AL** completo generado con `al-conductor`
- [ ] **Compilación** exitosa sin errores
- [ ] **Tests** generados con `al-tdd` y ejecutados (>80% cobertura)
- [ ] **Demostración** funcional en Business Central

---

## Conceptos Clave Aprendidos

| Concepto | Agente ALDC | Qué Hace |
|----------|-------------|----------|
| **Arquitectura de software** | `al-architect` | Diseña estructura de objetos y relaciones |
| **Orquestación de código** | `al-conductor` | Coordina múltiples agentes para generar código completo |
| **Generación de objetos** | `al-object-generator` | Crea tablas, páginas, enums, codeunits |
| **Testing automatizado** | `al-tdd` | Genera tests unitarios automáticamente |
| **Análisis de cobertura** | `al-test-analyzer` | Evalúa calidad de tests |

---

## Próximos Pasos

Una vez completado este ejercicio:

1. **Ejercicio 2**: Sistema de Eventos (complejidad media 🟡)
2. **Ejercicio 3**: Sistema de Incidencias (complejidad alta 🔴)

O explora:
- [Cheatsheet ALDC](../recursos/cheatsheet-aldc.md) para workflows avanzados
- [Troubleshooting](../recursos/troubleshooting.md) para resolver problemas comunes

---

## Soporte

**¿Problemas?** Consulta:
- [Troubleshooting](../recursos/troubleshooting.md)
- [Documentación ALDC](https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot)
- [Preguntas Frecuentes](../README.md#faq)

---

**¡Buena suerte! 🚀**
