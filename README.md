# 🎓 Workshop: Desarrollo AL con IA para Business Central

> **Aprende a construir extensiones de Business Central usando GitHub Copilot y AL Development Collection**

[![BC Version](https://img.shields.io/badge/Business%20Central-v24+-blue)](https://docs.microsoft.com/dynamics365/business-central/)
[![AL Development Collection](https://img.shields.io/badge/ALDC-v2.8+-green)](https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📋 Descripción

Este workshop te guía paso a paso para construir **dos extensiones completas** de Business Central usando técnicas de desarrollo asistido por IA:

| Ejercicio | Descripción | Complejidad | Tiempo |
|-----------|-------------|-------------|--------|
| **🎫 Gestión de Incidencias** | Sistema de tickets con API para agentes | 🟡 Media | 2-3 horas |
| **📋 Registros de Eventos** | Importación de Eventbrite con API de consulta | 🟢 Media-Baja | 1-2 horas |

**Lo que aprenderás:**
- ✅ Diseño arquitectónico con `al-architect`
- ✅ Implementación TDD con `al-conductor`
- ✅ Creación de APIs para agentes externos
- ✅ Importación de datos desde Excel
- ✅ Mejores prácticas de desarrollo AL

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
├── ejercicio-01-incidencias/          # 🎫 Sistema de Incidencias
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

### Parte 1: Introducción (30 min)
- Qué es el desarrollo AI-native
- Introducción a AL Development Collection
- Los 3 niveles de complejidad
- Demo rápida de al-architect + al-conductor

### Parte 2: Ejercicio 1 - Incidencias (2-3 horas)
1. Análisis del PRD ejecutivo
2. Diseño con `al-architect`
3. Implementación con `al-conductor`
4. Pruebas de la API
5. Revisión y Q&A

### Parte 3: Ejercicio 2 - Eventos (1-2 horas)
1. Análisis del PRD y datos de ejemplo
2. Diseño con `al-architect`
3. Implementación con `al-conductor`
4. Pruebas de importación y API
5. Revisión y Q&A

### Parte 4: Cierre (30 min)
- Mejores prácticas aprendidas
- Recursos adicionales
- Próximos pasos

---

## ⚡ Inicio Rápido

### Si quieres empezar ya:

**Ejercicio 1 - Incidencias:**
```bash
cd ejercicio-01-incidencias
# Lee el README.md y sigue los pasos
```

**Ejercicio 2 - Eventos:**
```bash
cd ejercicio-02-eventos
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

Al completar ambos ejercicios habrás creado:

| Métrica | Ejercicio 1 | Ejercicio 2 | Total |
|---------|-------------|-------------|-------|
| **Tablas** | 3 | 2 | 5 |
| **Pages** | 6 | 5 | 11 |
| **APIs** | 3 | 2 | 5 |
| **Codeunits** | 2 | 1 | 3 |
| **Tests** | ~40 | ~25 | ~65 |
| **Tiempo** | 2-3h | 1-2h | 3-5h |

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

👉 [Ir al Ejercicio 1: Gestión de Incidencias](./ejercicio-01-incidencias/README.md)
