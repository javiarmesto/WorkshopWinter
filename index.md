---
layout: default
title: Inicio
nav_order: 1
description: "Workshop de desarrollo AL con IA para Business Central"
permalink: /
---

# 🎓 Workshop: Desarrollo AL con IA para Business Central
{: .no_toc }

> **Aprende a construir extensiones de Business Central usando GitHub Copilot y AL Development Collection**

[![BC Version](https://img.shields.io/badge/Business%20Central-v24+-blue)](https://docs.microsoft.com/dynamics365/business-central/)
[![AL Development Collection](https://img.shields.io/badge/ALDC-v2.9+-green)](https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Tabla de Contenidos
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 🎯 ¿Qué Aprenderás?

Este workshop te guía paso a paso para construir **dos extensiones completas** de Business Central usando técnicas de desarrollo asistido por IA:

<div class="code-example" markdown="1">

### 🎫 Ejercicio 1: Gestión de Incidencias
**Complejidad:** 🟡 Media | **Tiempo:** 2-3 horas

Sistema completo de tickets con API REST para agentes externos. Incluye:
- Gestión de ciclo de vida (Nueva → En Progreso → Resuelta → Cerrada)
- Categorización y priorización
- Historial de comentarios automático
- API para integración con chatbots y sistemas externos

[Ir al Ejercicio 1 →](ejercicio-01-incidencias/){: .btn .btn-primary }

---

### 📋 Ejercicio 2: Registros de Eventos
**Complejidad:** 🟢 Media-Baja | **Tiempo:** 1-2 horas

Importación de registros desde Excel (Eventbrite) con API de consulta. Incluye:
- Importación automática desde Excel (34 columnas)
- Gestión de múltiples eventos
- API read-only para agentes de badges, check-in, comunicaciones
- Datos reales del Business Central & Agents Winter Fest

[Ir al Ejercicio 2 →](ejercicio-02-eventos/){: .btn .btn-primary }

</div>

---

## 🚀 Tecnologías Utilizadas

| Tecnología | Versión | Propósito |
|------------|---------|-----------|
| **Microsoft Dynamics 365 BC** | v23.0+ | Plataforma ERP |
| **AL Language** | Latest | Lenguaje de desarrollo |
| **GitHub Copilot** | Chat + Agents | Asistente IA |
| **AL Development Collection** | v2.9.0 | Framework AI-Native |
| **VS Code** | Latest | IDE |

---

## 📚 Competencias Desarrolladas

Al finalizar este workshop, dominarás:

✅ **Arquitectura AI-First**
- Diseñar con `al-architect` antes de codificar
- Documentar decisiones en `architecture.md`
- Usar el sistema Context & Memory (`.github/plans/`)

✅ **Desarrollo TDD Automático**
- Implementar con `al-conductor` y orchestra multi-agente
- Ciclo RED → GREEN → REFACTOR automatizado
- Tests generados antes del código de implementación

✅ **Integración con Agentes**
- Diseñar APIs REST con `al-api`
- Crear endpoints OData filtrables
- Exponer datos para consumo externo

✅ **Mejores Prácticas AL**
- Patrón event-driven (sin modificar base)
- Estructura AL-Go (App/ vs Test/)
- Auto-guidelines aplicadas automáticamente

---

## 🎯 Resultados Esperados

<div class="code-example" markdown="1">

Al completar ambos ejercicios habrás creado:

| Métrica | Ejercicio 1 | Ejercicio 2 | **Total** |
|---------|:-----------:|:-----------:|:---------:|
| **Tablas** | 3 | 2 | **5** |
| **Enums** | 3 | 0 | **3** |
| **Pages** | 6 | 5 | **11** |
| **APIs** | 3 | 2 | **5** |
| **Codeunits** | 2 | 1 | **3** |
| **Tests** | ~40 | ~25 | **~65** |
| **Líneas de código** | ~1,500 | ~800 | **~2,300** |

</div>

---

## 👥 Audiencia Objetivo

<div class="code-example" markdown="1">

### ✅ Perfecto para:
- **Desarrolladores AL** con experiencia básica-intermedia
- **Consultores funcionales** que quieren entender el desarrollo
- **Arquitectos de soluciones** Business Central
- **DevOps/Tech Leads** implementando IA en sus equipos

### 📋 Prerequisitos:
- Conocimientos básicos de AL y Business Central
- VS Code instalado con extensión AL Language
- GitHub Copilot activo (versión Chat)
- Sandbox de Business Central disponible

</div>

---

## ⚡ Inicio Rápido

### 1️⃣ Instalar AL Development Collection

```bash
# Opción A: Desde VS Code Marketplace
# Buscar "AL Development Collection" e instalar

# Opción B: Desde NPM
npm install github:javiarmesto/AL-Development-Collection-for-GitHub-Copilot
npx al-collection install
```

### 2️⃣ Verificar Instalación

```bash
# Verificar que los agentes están disponibles
ls .github/agents/

# Deberías ver:
# - al-architect.agent.md
# - al-conductor.agent.md
# - al-developer.agent.md
# - al-api.agent.md
# - ...y 7 más
```

### 3️⃣ Recargar VS Code

```
Ctrl+Shift+P → "Developer: Reload Window"
```

### 4️⃣ Empezar el Workshop

[🎫 Ir al Ejercicio 1: Gestión de Incidencias](ejercicio-01-incidencias/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[📋 Ver Recursos](recursos/){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## 📖 Navegación Rápida

<div class="code-example" markdown="1">

### Por Sección

| Sección | Descripción | Link |
|---------|-------------|------|
| 🎫 **Ejercicio 1** | Sistema de Gestión de Incidencias | [Abrir](ejercicio-01-incidencias/) |
| 📋 **Ejercicio 2** | Registros de Eventos con Importación | [Abrir](ejercicio-02-eventos/) |
| 📚 **Recursos** | Material de apoyo y documentación | [Abrir](recursos/) |
| 📋 **Cheatsheet** | Referencia rápida de comandos ALDC | [Abrir](recursos/cheatsheet-aldc) |
| 🐛 **Troubleshooting** | Solución de problemas comunes | [Abrir](recursos/troubleshooting) |
| 📝 **Plantillas** | Templates reutilizables | [Abrir](plantillas/) |

### Por Tipo de Documento

| Tipo | Ejercicio 1 | Ejercicio 2 |
|------|-------------|-------------|
| **PRD Ejecutivo** | [Ver](ejercicio-01-incidencias/PRD-ejecutivo) | [Ver](ejercicio-02-eventos/PRD-ejecutivo) |
| **PRD Técnico** | [Ver](ejercicio-01-incidencias/PRD-tecnico) | [Ver](ejercicio-02-eventos/PRD-tecnico) |
| **Guía Paso a Paso** | [Ver](ejercicio-01-incidencias/) | [Ver](ejercicio-02-eventos/) |
| **Solución** | [Ver](ejercicio-01-incidencias/solucion/) | [Ver](ejercicio-02-eventos/solucion/) |

</div>

---

## 🎓 Flujo del Workshop

### Parte 1: Introducción (30 min)
- Qué es el desarrollo AI-native
- Introducción a AL Development Collection
- Los 7 agentes principales + Sistema Orchestra
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

## 🔗 Enlaces Importantes

<div class="code-example" markdown="1">

### Documentación Oficial
- [AL Development Collection - GitHub](https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot)
- [Quick Start ALDC](https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot/blob/main/QUICK-START.md)
- [AI Native Architecture Framework](https://danielmeppiel.github.io/awesome-ai-native/)
- [Microsoft Dynamics 365 BC Docs](https://docs.microsoft.com/dynamics365/business-central/)

### Recursos Adicionales
- [Cheatsheet ALDC](recursos/cheatsheet-aldc)
- [Troubleshooting](recursos/troubleshooting)
- [Plantillas PRD](plantillas/PRD-template)

</div>

---

## 🤝 Contribuir

¿Encontraste un error? ¿Tienes sugerencias?

1. Abre un [Issue](https://github.com/javiarmesto/WorkshopWinter/issues) describiendo el problema/sugerencia
2. Si quieres contribuir código, haz un [Fork y Pull Request](https://github.com/javiarmesto/WorkshopWinter/fork)
3. Revisa las [guías de contribución](CONTRIBUTING)

---

## 📄 Licencia

Este workshop está bajo licencia MIT. Puedes usarlo, modificarlo y distribuirlo libremente.

Ver [LICENSE](LICENSE) para más detalles.

---

## 🙏 Créditos

- **AL Development Collection**: [Javier Armesto](https://github.com/javiarmesto)
- **Workshop creado por**: VS Sistemas
- **Basado en**: [AI Native-Instructions Architecture](https://danielmeppiel.github.io/awesome-ai-native/)
- **Datos de ejemplo**: Business Central & Agents Winter Fest 2026

---

## ⚠️ Disclaimer

> Este workshop utiliza GitHub Copilot e inteligencia artificial generativa. Los resultados pueden variar según el contexto, versiones del modelo y entradas del usuario. **Siempre revisa y prueba el código generado antes de usarlo en producción.**

---

<div style="text-align: center; padding: 2rem 0;">
  <h2>¡Empezamos! 🚀</h2>
  <a href="ejercicio-01-incidencias/" class="btn btn-primary btn-lg">Comenzar con Ejercicio 1</a>
  <br><br>
  <small>Tiempo estimado total: 3-5 horas | Nivel: Intermedio</small>
</div>

---

<footer style="text-align: center; padding: 2rem 0; border-top: 1px solid #eee; margin-top: 3rem;">
  <p>
    <strong>Workshop mantenido por VS Sistemas</strong><br>
    Última actualización: Enero 2025 | Versión: 1.0
  </p>
  <p>
    <a href="https://github.com/javiarmesto/WorkshopWinter">GitHub</a> •
    <a href="https://github.com/javiarmesto/WorkshopWinter/issues">Issues</a> •
    <a href="https://vssistemas.com">VS Sistemas</a>
  </p>
</footer>
