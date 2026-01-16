# 🎓 Workshop: Desarrollo AL con IA para Business Central

> **Aprende a construir extensiones de Business Central usando GitHub Copilot y AL Development Collection**

[![BC Version](https://img.shields.io/badge/Business%20Central-v24+-blue)](https://docs.microsoft.com/dynamics365/business-central/)
[![AL Development Collection](https://img.shields.io/badge/ALDC-v2.9+-green)](https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> [!TIP]
> 🌐 **Sitio web del workshop**: [https://javiarmesto.github.io/WorkshopWinter](https://javiarmesto.github.io/WorkshopWinter)

---

## 📋 Descripción

Este workshop te guía paso a paso para construir **tres extensiones completas** de Business Central usando técnicas de desarrollo asistido por IA:

| Ejercicio | Descripción | Complejidad | Tiempo con IA |
|-----------|-------------|-------------|---------------|
| **💼 Control de Equipamiento** | Préstamo de laptops, proyectores (demo rápida) | 🟢 Baja | 25 min ⚡ |
| **📋 Registros de Eventos** | Importación de Eventbrite con API de consulta | 🟢 Media-Baja | 35 min ⚡ |
| **🎫 Gestión de Incidencias** | Sistema de tickets con API para agentes | 🟡 Media | 50 min ⚡ |

> ⚡ **Tiempos con ALDC + GitHub Copilot**: La IA genera todo el código. Tú diseñas y pruebas.

**Lo que aprenderás:**
- ✅ Diseñar con `al-architect` - La IA crea la arquitectura
- ✅ Implementar con `al-conductor` - TDD automático, 0 líneas manuales
- ✅ Crear APIs para agentes - Generadas con best practices
- ✅ Importar datos desde Excel - Código de mapeo automático
- ✅ Aplicar mejores prácticas AL - Auto-guidelines en segundo plano

> 💡 **Lo que antes tomaba 2 días, ahora toma 1 hora**

---

## 🎯 Objetivos del Workshop

Al finalizar serás capaz de:

1. **Diseñar soluciones** usando al-architect antes de escribir código
2. **Implementar con TDD** usando el ciclo RED → GREEN → REFACTOR
3. **Crear APIs REST** consumibles por agentes de IA y sistemas externos
4. **Importar datos externos** (Excel) a Business Central
5. **Aplicar patrones** event-driven sin modificar objetos base

---

## 👥 Audiencia

- Desarrolladores AL con experiencia básica-intermedia
- Consultores funcionales que quieren entender el desarrollo
- Arquitectos de soluciones Business Central
- Cualquier persona interesada en desarrollo asistido por IA

**Prerequisitos técnicos:**
- Conocimientos básicos de AL y Business Central
- VS Code instalado con extensión AL Language
- GitHub Copilot activo (versión Chat)
- Sandbox de Business Central disponible

---

## 🛠️ Preparación del Entorno

### 1. Instalar AL Development Collection

```bash
# Opción A: Desde VS Code Marketplace
# Buscar "AL Development Collection" e instalar

# Opción B: Desde NPM
npm install github:javiarmesto/AL-Development-Collection-for-GitHub-Copilot
npx al-collection install
```

### 2. Verificar Instalación

```bash
# Verificar que los agentes están disponibles
ls .github/agents/

# Deberías ver:
# - al-architect.agent.md
# - al-conductor.agent.md
# - al-developer.agent.md
# - al-api.agent.md
# - ...
```

### 3. Recargar VS Code

```
Ctrl+Shift+P → "Developer: Reload Window"
```

---

## 📚 Estructura del Workshop

```
workshop-bc-ai-development/
├── README.md                          # Este archivo
├── LICENSE
│
├── ejercicio-01-equipamiento/         # 💼 Control de Equipamiento (WARM-UP)
│   ├── README.md                      # Guía del ejercicio
│   ├── PRD-ejecutivo.md              # Requisitos de negocio
│   ├── PRD-tecnico.md                # Especificaciones técnicas
│   └── solucion/                     # Código de referencia
│       └── ...
│
├── ejercicio-02-eventos/              # 📋 Registros de Eventos
│   ├── README.md                      # Guía del ejercicio
│   ├── PRD-ejecutivo.md              # Requisitos de negocio
│   ├── PRD-tecnico.md                # Especificaciones técnicas
│   ├── datos-ejemplo/                # Excel de ejemplo
│   │   └── eventbrite-export.xlsx
│   └── solucion/                     # Código de referencia
│       └── ...
│
├── ejercicio-03-incidencias/          # 🎫 Sistema de Incidencias (AVANZADO)
│   ├── README.md                      # Guía del ejercicio
│   ├── PRD-ejecutivo.md              # Requisitos de negocio
│   ├── PRD-tecnico.md                # Especificaciones técnicas
│   └── solucion/                     # Código de referencia
│       └── ...
│
├── recursos/                          # Material adicional
│   ├── cheatsheet-aldc.md            # Referencia rápida comandos
│   ├── troubleshooting.md            # Solución de problemas
│   └── slides/                       # Presentación (opcional)
│
└── plantillas/                        # Templates reutilizables
    ├── PRD-template.md
    └── prompt-templates.md
```

---

## 🚀 Flujo del Workshop

### 🎯 Modalidad Express (1 hora) - Recomendado para demos

```
00:00-00:10 → Intro: "La IA genera el código, tú diseñas"
00:10-00:35 → Ejercicio 1: Control de Equipamiento (completo)
00:35-00:55 → Ejercicio 2: Registros de Eventos (completo)  
00:55-01:00 → Conclusiones y próximos pasos

RESULTADO: ✅ DOS sistemas funcionando, ~50 objetos AL generados
```

### 📚 Modalidad Completa (2-3 horas) - Workshop profundo

**Parte 1: Introducción (15 min)**
- Desarrollo AI-native vs tradicional
- AL Development Collection: 7 agentes + Orchestra
- Demo: al-architect → al-conductor (en vivo)

**Parte 2: Ejercicio 1 - Equipamiento (25 min)**
1. Leer PRD ejecutivo (5 min)
2. `al-architect` genera diseño (6 min)
3. `al-conductor` implementa con TDD (12 min)
4. Probar funcionalidad (2 min)

**Parte 3: Ejercicio 2 - Eventos (35 min)**
1. Análisis del PRD y datos ejemplo (3 min)
2. `al-architect` diseña importación (6 min)
3. `al-conductor` implementa completo (18 min)
4. Importar Excel + probar API (8 min)

**Parte 4: Ejercicio 3 - Incidencias (50 min)**
1. PRD ejecutivo - sistema complejo (3 min)
2. `al-architect` arquitectura completa (8 min)
3. `al-conductor` orquesta 8 fases (30 min)
   - Ver TDD en acción: RED → GREEN → REFACTOR
   - Observar review automático por al-review-subagent
4. Probar APIs REST (5 min)
5. Revisar documentación en `.github/plans/` (4 min)

**Parte 5: Cierre (20 min)**
- Comparativa: Manual vs ALDC (8h → 2h)
- Mejores prácticas aplicadas automáticamente
- Recursos y próximos pasos

---

## ⚡ Inicio Rápido

### Si quieres empezar ya:

**Ejercicio 1 - Equipamiento:**
```bash
cd ejercicio-01-equipamiento
# Lee el README.md y sigue los pasos
```

**Ejercicio 2 - Eventos:**
```bash
cd ejercicio-02-eventos
# Lee el README.md y sigue los pasos
```

**Ejercicio 3 - Incidencias:**
```bash
cd ejercicio-03-incidencias
# Lee el README.md y sigue los pasos
```

---

## 📖 Comandos ALDC Utilizados

| Comando | Uso en Workshop |
|---------|-----------------|
| `Use al-architect mode` | Diseñar arquitectura de la solución |
| `Use al-conductor mode` | Implementar con TDD automático |
| `Use al-api mode` | Diseñar APIs REST |
| `@workspace use al-build` | Compilar y desplegar |
| `@workspace use al-permissions` | Generar permisos |
| `@workspace use al-diagnose` | Debugging si hay errores |

---

## 🎯 Resultados Esperados

Al completar los tres ejercicios habrás creado:

| Métrica | Ej. 1 (Equip.) | Ej. 2 (Eventos) | Ej. 3 (Incid.) | **Total** |
|---------|:--------------:|:---------------:|:--------------:|:---------:|
| **Tablas** | 2 | 2 | 3 | **7** |
| **Enums** | 1 | 0 | 3 | **4** |
| **Pages** | 4 | 5 | 6 | **15** |
| **APIs** | 0 | 2 | 3 | **5** |
| **Codeunits** | 1 | 1 | 2 | **4** |
| **Tests** | ~20 | ~25 | ~40 | **~85** |
| **Tiempo con IA** | 25 min ⚡ | 35 min ⚡ | 50 min ⚡ | **110 min** |
| **Tiempo manual** | 3-4h 🐌 | 4-6h 🐌 | 8-10h 🐌 | **15-20h** |

> 🚀 **Productividad**: ~10x más rápido con ALDC + GitHub Copilot

---

## 🤝 Contribuir

¿Encontraste un error? ¿Tienes sugerencias?

1. Abre un Issue describiendo el problema/sugerencia
2. Si quieres contribuir código, haz un Fork y Pull Request
3. Revisa las [guías de contribución](CONTRIBUTING.md)

---

## 📄 Licencia

Este workshop está bajo licencia MIT. Puedes usarlo, modificarlo y distribuirlo libremente.

---

## 🙏 Créditos

- **AL Development Collection**: [Javier Armesto](https://github.com/javiarmesto)
- **Workshop creado por**: VS Sistemas
- **Basado en**: [AI Native-Instructions Architecture](https://danielmeppiel.github.io/awesome-ai-native/)

---

## 📞 Contacto

- **GitHub Issues**: Para preguntas técnicas sobre el workshop
- **VS Sistemas**: [vssistemas.com](https://vssistemas.com)

---

> **⚠️ Nota**: Este workshop utiliza GitHub Copilot e inteligencia artificial generativa. Los resultados pueden variar. Siempre revisa y prueba el código generado antes de usarlo en producción.

---

**¡Empezamos! 🚀**

👉 [Ir al Ejercicio 1: Control de Equipamiento](./ejercicio-01-equipamiento/README.md)
