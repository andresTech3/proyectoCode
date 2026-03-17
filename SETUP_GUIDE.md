# 🚀 SETUP_GUIDE.md — Guía de Instalación del Stack Cognitivo en WSL

Guía paso a paso para configurar el entorno de desarrollo multi-agente.

---

## Prerrequisitos

- WSL2 con Ubuntu instalado
- Git configurado con tu cuenta de GitHub
- Python 3.10+ instalado en WSL
- Node.js 18+ instalado en WSL (para OpenCode y herramientas)

---

## 1. OpenCode (Orquestador Agéntico)

OpenCode es la alternativa open-source a Claude Code, multi-proveedor.

```bash
# Opción A: Instalación via ecosistema Gentle AI
curl -fsSL https://gentlemanprogramming.com/install | bash

# Opción B: Instalación directa con npm
npm install -g opencode

# Verificar instalación
opencode --version
```

### Configurar API Keys

Edita `~/.opencode/config.json`:

```json
{
  "providers": {
    "openai": { "apiKey": "sk-..." },
    "anthropic": { "apiKey": "sk-ant-..." },
    "gemini": { "apiKey": "AIza..." },
    "ollama": { "baseUrl": "http://localhost:11434" }
  }
}
```

> **Nota:** Solo configura los proveedores que tengas. OpenCode detectará automáticamente los modelos disponibles.

---

## 2. Engram (Memoria Persistente)

Evita la "amnesia" entre sesiones con una base de datos SQLite.

```bash
# Clonar Engram
git clone https://github.com/Gentleman-Programming/engram.git ~/.engram

# Instalación automática (detecta SO y hace symlink)
cd ~/.engram && ./setup.sh

# Verificar
engram --version
```

### ¿Qué almacena Engram?
- Decisiones arquitectónicas
- Correcciones de bugs
- Contexto aprendido entre sesiones

---

## 3. Servidores MCP (Model Context Protocol)

Permiten que OpenCode interactúe con servicios externos.

### Configurar en `~/.opencode/mcp.json`:

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "ghp_..." }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/ruta/a/tu/proyecto"]
    }
  }
}
```

📚 **Directorio oficial de servidores MCP:** https://modelcontextprotocol.io

---

## 4. Skills (Habilidades Modulares)

Se cargan bajo demanda (lazy loading), no saturan el contexto.

```bash
# Instalar skills desde el repositorio oficial
# Visita https://skills.sh para ver el catálogo

# Ejemplo: instalar skill de Python
opencode skills install python-patterns
```

---

## 5. Git Worktrees (Aislamiento Multi-Agente)

Evita que agentes paralelos destruyan código entre sí.

```bash
# Desde la raíz del repo, crear worktrees para agentes
git worktree add ../agent-feature-A feature/feature-A
git worktree add ../agent-feature-B feature/feature-B

# Cada agente trabaja en su carpeta, vinculada a su rama
# Al terminar, limpiar:
git worktree remove ../agent-feature-A
```

### Flujo:
1. Agente 1 trabaja en `../agent-feature-A/` → rama `feature/feature-A`
2. Agente 2 trabaja en `../agent-feature-B/` → rama `feature/feature-B`
3. Cada uno abre su PR cuando termina
4. Tú revisas y apruebas

---

## 6. Agent Teams Lite (Orquestación)

Para tareas muy complejas, divide el trabajo entre agentes especializados.

```bash
# Clonar repositorio
git clone https://github.com/Gentleman-Programming/agent-teams-lite.git ~/.agent-teams

# Configurar para OpenCode y agentes locales automáticamente
cd ~/.agent-teams && ./scripts/setup.sh --all
```

### Roles de los agentes:
| Agente       | Función                     |
|--------------|-----------------------------|
| Arquitecto   | Propone la arquitectura     |
| Tester       | Escribe los tests           |
| Implementador| Escribe el código           |

📚 **Repo:** https://github.com/Gentleman-Programming/agent-teams-lite

---

## 7. Protección de Rama `main` en GitHub

### Configuración manual:

1. Ve a tu repo → **Settings** → **Rules** → **Rulesets**
2. Crea una nueva regla para la rama `main`
3. Activa:
   - ✅ **Require a pull request before merging**
   - ✅ **Require status checks to pass** (selecciona el job `lint-and-test`)
   - ✅ **Block force pushes**
4. Guarda la regla

> ⚠️ **NUNCA** se hace push directo a `main`. Solo vía Pull Request.

---

## 8. Release Please (Versionado Automático)

Ya está configurado en `.github/workflows/release.yml`.

### ¿Cómo funciona?
1. Haces merge de un PR a `main`
2. Release Please lee los commits (Conventional Commits)
3. Genera automáticamente:
   - 📋 Release Notes
   - 🏷️ Tag semántico (`v1.0.0`, `v1.1.0`, etc.)
   - 📦 PR de release con changelog

---

## Flujo de Trabajo Diario

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 1. ESPECIFICAR│───→│ 2. DELEGAR   │───→│ 3. AUDITAR   │───→│ 4. DESPLEGAR │
│ (Modo Plan)  │    │ (OpenCode)   │    │ (Human Loop) │    │ (Auto-merge) │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
     SDD Text          Git Worktree       PR + CI Tests      Release Please
```

---

## Enlaces Útiles

| Recurso | URL |
|---------|-----|
| OpenCode | https://opencode.ai |
| Engram | https://github.com/Gentleman-Programming/engram |
| MCP Servers | https://modelcontextprotocol.io |
| Skills | https://skills.sh |
| Agent Teams | https://github.com/Gentleman-Programming/agent-teams-lite |
| Release Please | https://github.com/googleapis/release-please |
| Agents.md (plantillas) | https://agents.md |
| NotebookLM (RAG) | https://notebooklm.google.com |
