# 📋 Cheatsheet - AL Development Collection

> **Referencia rápida de comandos y modos**

---

## 🎭 Modos Principales (Agents)

| Modo | Uso | Cuándo Usarlo |
|------|-----|---------------|
| `Use al-architect mode` | Diseño de arquitectura | Al iniciar una nueva funcionalidad |
| `Use al-conductor mode` | Implementación TDD | Después del diseño, para codificar |
| `Use al-developer mode` | Desarrollo directo | Cambios simples (1-2 objetos) |
| `Use al-api mode` | Diseño de APIs | Cuando necesitas APIs REST/OData |
| `Use al-debugger mode` | Diagnóstico profundo | Cuando hay bugs complejos |
| `Use al-tester mode` | Estrategia de testing | Para planificar tests |
| `Use al-copilot mode` | Features de Copilot | Para AI capabilities en BC |

---

## ⚡ Comandos de Workspace

### Setup y Build
```bash
@workspace use al-initialize    # Inicializa proyecto AL
@workspace use al-build         # Compila y despliega
@workspace use al-permissions   # Genera permission sets
```

### Desarrollo
```bash
@workspace use al-events        # Genera event subscribers
@workspace use al-pages         # Genera pages
@workspace use al-api           # Genera API pages
```

### Análisis y Debug
```bash
@workspace use al-diagnose      # Diagnóstico completo
@workspace use al-performance   # Análisis de rendimiento
@workspace use al-migrate       # Ayuda en migraciones
```

### Copilot Features
```bash
@workspace use al-copilot-capability     # Crea capability
@workspace use al-copilot-promptdialog   # Crea prompt dialog
@workspace use al-copilot-test           # Tests de Copilot
```

---

## 🎯 Flujo por Complejidad

### 🟢 Simple (1-2 objetos)
```
1. Describe lo que necesitas a Copilot
2. @workspace use al-build
```

### 🟡 Media (3-6 objetos)
```
1. Use al-architect mode → Diseña
2. Use al-conductor mode → Implementa TDD
3. @workspace use al-permissions
4. @workspace use al-build
```

### 🔴 Compleja (7+ objetos)
```
1. Use al-architect mode → Arquitectura completa
2. Use al-api mode (si hay APIs)
3. Use al-conductor mode → Implementación multi-fase
4. @workspace use al-performance
5. @workspace use al-permissions
6. @workspace use al-build
```

---

## 📝 Prompts Útiles

### Diseño con al-architect
```markdown
Use al-architect mode

Diseña [descripción del sistema] con estos requisitos:

FUNCIONALIDAD:
- [requisito 1]
- [requisito 2]

REGLAS DE NEGOCIO:
- [regla 1]
- [regla 2]

CONSIDERACIONES TÉCNICAS:
- Usar eventos (no modificar objetos base)
- Estructura AL-Go
- Rango de objetos: [XXXXX-XXXXX]
```

### Implementación con al-conductor
```markdown
Use al-conductor mode

Implementa [nombre del sistema] diseñado por al-architect.

Requisitos:
- Ciclo TDD estricto: RED → GREEN → REFACTOR
- Estructura AL-Go (App/ y Test/)
- Rango de objetos: [XXXXX-XXXXX]
- Prefijo: [XXX]
```

### Debug con al-debugger
```markdown
Use al-debugger mode

Tengo este error en [archivo]:
[descripción del error]

Contexto:
- [qué estaba haciendo]
- [qué esperaba]
```

---

## 🔧 Auto-Guidelines (Automáticas)

Estas reglas se aplican automáticamente mientras codificas:

| Guideline | Qué Hace |
|-----------|----------|
| `al-code-style` | Formato y estructura de código |
| `al-naming-conventions` | Nombres PascalCase, prefijos |
| `al-performance` | SetLoadFields, filtrado temprano |
| `al-error-handling` | TryFunctions, error labels |
| `al-events` | Patrón event-driven |
| `al-testing` | Estructura de tests AL-Go |

---

## 📁 Estructura de Proyecto Recomendada

```
proyecto/
├── app.json
├── .vscode/
│   ├── launch.json
│   └── settings.json
├── .github/
│   ├── agents/           # Agentes ALDC
│   ├── instructions/     # Guidelines
│   ├── prompts/          # Workflows
│   └── plans/            # Documentación generada
├── src/
│   ├── App/
│   │   ├── Tables/
│   │   ├── Pages/
│   │   ├── Codeunits/
│   │   ├── APIs/
│   │   └── Permissions/
│   └── Test/
└── res/
```

---

## 🐛 Comandos de Troubleshooting

```bash
# Verificar instalación
ls .github/agents/

# Reinstalar ALDC
npm install github:javiarmesto/AL-Development-Collection-for-GitHub-Copilot --force
npx al-collection install

# Recargar VS Code
Ctrl+Shift+P → "Developer: Reload Window"

# Validar configuración
npm run validate
```

---

## 🔗 Enlaces Útiles

- **GitHub ALDC**: https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot
- **Quick Start**: https://github.com/javiarmesto/AL-Development-Collection-for-GitHub-Copilot/blob/main/QUICK-START.md
- **AI Native Architecture**: https://danielmeppiel.github.io/awesome-ai-native/

---

## 💡 Tips

1. **Siempre empieza con al-architect** para funcionalidades medias/complejas
2. **Proporciona contexto rico** en tus prompts
3. **Confía en las auto-guidelines** - trabajan en segundo plano
4. **Usa @workspace** para comandos que necesitan contexto del proyecto
5. **Revisa los tests generados** - son excelentes ejemplos de uso

---

*Versión: 2.8.0 | Última actualización: Enero 2025*
