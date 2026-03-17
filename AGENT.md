# AGENT.md — Reglas del Agente IA

> Este archivo define las instrucciones que cualquier agente IA (OpenCode, Copilot, etc.)
> debe seguir al trabajar en este proyecto. **Máximo 500 líneas.**

---

## 🌐 Idioma

- Responde **siempre en español**.
- Los nombres de variables, funciones y archivos se escriben en **inglés**.
- Los comentarios en el código van en **español**.

---

## 🛠️ Stack Tecnológico

| Componente       | Tecnología                  |
|------------------|-----------------------------|
| Lenguaje         | Python 3.10+                |
| Linter           | flake8 / ruff               |
| Formatter        | black                       |
| Testing          | pytest                      |
| Gestor de paquetes | pip + requirements.txt    |
| Control de versiones | Git                     |
| CI/CD            | GitHub Actions              |
| Versionado       | Release Please (Google)     |

---

## 🚀 Comandos de Arranque

```bash
# Instalar dependencias
pip install -r requirements.txt

# Ejecutar la aplicación
python main.py

# Ejecutar tests
pytest

# Ejecutar linter
ruff check .

# Formatear código
black .
```

---

## 📐 Convenciones de Código

### Principios
- **Clean Code**: nombres descriptivos, funciones cortas (<20 líneas), sin código muerto.
- **Modularización**: cada archivo debe tener una sola responsabilidad.
- **DRY**: no repetir lógica. Extraer funciones reutilizables a módulos en `utils/`.
- **Type Hints**: usar anotaciones de tipos en todas las funciones.

### Estructura de Archivos
```
proyecto/
├── AGENT.md
├── SETUP_GUIDE.md
├── requirements.txt
├── main.py
├── src/
│   ├── __init__.py
│   ├── core/          # Lógica principal
│   ├── utils/         # Utilidades compartidas
│   └── models/        # Modelos de datos
├── tests/
│   ├── __init__.py
│   └── test_*.py
└── .github/
    └── workflows/
        ├── ci.yml
        └── release.yml
```

### Estilo
- Máximo **88 caracteres** por línea (estándar Black).
- Usar **docstrings** en todas las funciones públicas.
- Imports ordenados: stdlib → terceros → locales (usar `isort`).
- Usar `pathlib.Path` en lugar de `os.path`.

---

## 🚫 Prohibiciones Estrictas

1. **NUNCA** hacer push directo a `main`. Solo vía Pull Request.
2. **NUNCA** hardcodear contraseñas, API keys o secretos en el código.
3. **NUNCA** ignorar errores silenciosamente (`except: pass` está prohibido).
4. **NUNCA** crear archivos >300 líneas. Modularizar si se excede.
5. **NUNCA** instalar dependencias sin agregarlas a `requirements.txt`.
6. **NUNCA** commitear sin mensaje descriptivo (Conventional Commits).

---

## 📝 Conventional Commits

Todos los commits deben seguir el estándar:

```
<tipo>(<alcance>): <descripción>

feat(auth): agregar sistema de autenticación JWT
fix(parser): corregir error al parsear archivos CSV vacíos
docs(readme): actualizar instrucciones de instalación
test(core): agregar tests unitarios para el módulo de cálculo
chore(deps): actualizar dependencias de desarrollo
refactor(utils): simplificar función de validación
```

---

## 🔄 Flujo de Trabajo (SDD)

1. **Especificar**: Definir requerimientos en texto plano (modo Plan de OpenCode).
2. **Delegar**: OpenCode ejecuta, crea ramas y escribe código en su Git Worktree.
3. **Auditar**: Revisar el PR manualmente + GitHub Actions ejecuta tests.
4. **Desplegar**: Aprobar PR → Release Please actualiza versión automáticamente.

---

## 🧠 Contexto para el Agente

- Este proyecto utiliza **Engram** como sistema de memoria persistente.
- Los servidores **MCP** están configurados para acceder a GitHub y bases de datos.
- Las **Skills** se cargan bajo demanda — no saturar el contexto.
- Siempre verificar que los tests pasan antes de proponer un PR.
- Ante la duda, **preguntar** al desarrollador en lugar de asumir.
