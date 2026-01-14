# 🐛 Guía de Troubleshooting

> **Solución a problemas comunes durante el workshop**

---

## 🔧 Problemas de Instalación

### "Los agentes no aparecen en Copilot"

**Síntoma**: Al escribir `Use al-architect mode`, Copilot no reconoce el modo.

**Solución**:
```bash
# 1. Verificar que existen los archivos
ls .github/agents/

# 2. Si no existen, reinstalar
npm install github:javiarmesto/AL-Development-Collection-for-GitHub-Copilot --force
npx al-collection install

# 3. Recargar VS Code
Ctrl+Shift+P → "Developer: Reload Window"

# 4. Verificar extensión de archivos (deben ser .agent.md)
ls -la .github/agents/
```

### "npm command not found"

**Síntoma**: El comando npm no se reconoce.

**Solución**:
1. Instala Node.js desde https://nodejs.org
2. Reinicia VS Code
3. Verifica: `node --version` y `npm --version`

### "Copilot no está activo"

**Síntoma**: No hay sugerencias de Copilot.

**Solución**:
1. Verifica tu suscripción a GitHub Copilot
2. En VS Code: Extensions → GitHub Copilot → Enabled
3. Reinicia VS Code
4. Abre un archivo `.al` para activar

---

## 🏗️ Problemas con al-architect

### "al-architect no responde"

**Síntoma**: Después de enviar el prompt, no hay respuesta.

**Solución**:
1. Espera hasta 3 minutos (análisis complejo)
2. Si no hay respuesta, cancela y reintenta
3. Simplifica el prompt (menos requisitos)
4. Verifica conexión a internet

### "Diseño incompleto"

**Síntoma**: al-architect no incluye todos los componentes esperados.

**Solución**:
```markdown
Por favor completa el diseño incluyendo:
- [componente faltante 1]
- [componente faltante 2]
```

---

## 💻 Problemas con al-conductor

### "al-conductor se detiene a mitad"

**Síntoma**: La implementación se para sin completar.

**Solución**:
```markdown
Use al-conductor mode

Continúa la implementación desde la fase [número de fase].
El estado actual es:
- Fases completadas: [lista]
- Última fase en progreso: [nombre]
```

### "Tests fallan constantemente"

**Síntoma**: El ciclo RED → GREEN no avanza.

**Solución**:
```markdown
Use al-debugger mode

Los tests de [componente] fallan con este error:
[mensaje de error]

Contexto:
- Fase actual: [fase]
- Test específico: [nombre del test]
```

### "al-conductor genera código incorrecto"

**Síntoma**: El código generado no compila o tiene errores lógicos.

**Solución**:
1. Usa `@workspace use al-diagnose` para análisis
2. Proporciona más contexto sobre el error
3. Pide corrección específica:
```markdown
El código generado tiene este problema:
[descripción del problema]

Por favor corrige [archivo] para [solución esperada]
```

---

## 🔨 Problemas de Compilación

### "Table not found"

**Síntoma**: Error de compilación indicando que una tabla no existe.

**Solución**:
```bash
# 1. Verifica que la tabla está en el proyecto
ls src/App/Tables/

# 2. Recompila el proyecto
@workspace use al-build

# 3. Si persiste, verifica app.json (idRanges)
```

### "Symbol not found"

**Síntoma**: Símbolos de BC no se encuentran.

**Solución**:
```bash
# 1. Descarga símbolos
Ctrl+Shift+P → "AL: Download Symbols"

# 2. Verifica launch.json
# - server, serverInstance correctos
# - authentication configurada

# 3. Reinicia
@workspace use al-initialize
```

### "Object ID conflict"

**Síntoma**: Dos objetos tienen el mismo ID.

**Solución**:
1. Verifica el rango en app.json
2. Busca duplicados: `grep -r "50100" src/`
3. Renumera objetos conflictivos
4. Actualiza referencias

---

## 🌐 Problemas de APIs

### "API devuelve 401 Unauthorized"

**Síntoma**: Llamadas a la API fallan con error de autenticación.

**Solución**:
1. Verifica configuración OAuth2
2. Comprueba que el token no ha expirado
3. Verifica permisos del usuario:
   - El usuario debe tener el Permission Set asignado
   - El usuario debe tener acceso a la API

### "API devuelve 404 Not Found"

**Síntoma**: El endpoint no se encuentra.

**Solución**:
1. Verifica la URL (publisher, group, version correctos)
2. Comprueba que la extensión está publicada
3. Verifica que la API page existe y está activa
```
/api/[publisher]/[group]/v[version]/[entitySetName]
```

### "Filtros OData no funcionan"

**Síntoma**: `$filter` no devuelve resultados esperados.

**Solución**:
1. Verifica sintaxis OData:
   - Strings entre comillas simples: `$filter=status eq 'New'`
   - Campos en camelCase: `$filter=customerNo eq 'C001'`
2. Combina con `and`/`or`: `$filter=status eq 'New' and priority eq 'High'`
3. Usa encoding URL para caracteres especiales

---

## 📥 Problemas de Importación Excel

### "Column not found"

**Síntoma**: Error al mapear columnas del Excel.

**Solución**:
1. Verifica que los encabezados están en la fila 1
2. Comprueba nombres exactos de columnas (case-sensitive)
3. No renombres columnas del export original
4. Elimina filas vacías al inicio

### "Registros duplicados"

**Síntoma**: Se crean registros duplicados.

**Solución**:
1. Usa opción "Skip Duplicates = Sí"
2. O "Update Existing = Sí" para actualizar
3. Verifica que Order ID es único en el Excel

### "Datos incorrectos"

**Síntoma**: Los datos no se mapean correctamente.

**Solución**:
1. Verifica formato de fechas (YYYY-MM-DD)
2. Verifica formato de números (punto decimal)
3. Comprueba encoding del archivo (UTF-8)

---

## 🔄 Problemas de Business Central

### "Permission denied"

**Síntoma**: Error de permisos al acceder a datos.

**Solución**:
1. Verifica que el Permission Set está generado
2. Asigna el Permission Set al usuario:
   - Users → [usuario] → Permission Sets → Add
3. Refresca la sesión (logout/login)

### "Page not found"

**Síntoma**: La página no aparece en búsqueda.

**Solución**:
1. Verifica `UsageCategory` en la page:
```al
UsageCategory = Lists;  // o Administration, Tasks, etc.
ApplicationArea = All;
```
2. Recompila y republica
3. Refresca BC (Ctrl+F5)

### "No. Series error"

**Síntoma**: Error al generar número automático.

**Solución**:
1. Ve a Setup → configura No. Series
2. Verifica que hay números disponibles
3. Comprueba que el campo No. Series está configurado en la tabla

---

## 💡 Tips Generales

### Para mejor rendimiento de Copilot:
1. **Mantén archivos abiertos** relevantes al contexto
2. **Proporciona ejemplos** cuando sea posible
3. **Sé específico** en los prompts
4. **Usa @workspace** para contexto del proyecto

### Para evitar problemas:
1. **Guarda frecuentemente**
2. **Compila después de cada fase**
3. **Revisa el código generado**
4. **Haz commits incrementales**

### Si todo falla:
1. **Reinicia VS Code**
2. **Limpia la carpeta .alpackages**
3. **Descarga símbolos de nuevo**
4. **Contacta soporte** con logs detallados

---

## 📞 Obtener Ayuda

### En el Workshop
- Levanta la mano para asistencia
- Comparte pantalla si es necesario

### Después del Workshop
- Abre un Issue en el repositorio
- Incluye:
  - Descripción del problema
  - Pasos para reproducir
  - Mensajes de error completos
  - Versiones (VS Code, AL, BC)

---

*Última actualización: Enero 2025*
