# Code Best Practices 📚
Coleccion de buenas practicas de programacion en distintos lenguajes y tecnologias. Lista de checklists para no olvidarme lo importante cuando escribo codigo.

## Content 📋
| Tech | Description |
|------------|-------------|
| [Bash](Readme-Bash.md) | Scripts Bash: variables, modos estrictos, funciones, manejo de errores, shebang, bucles y condicionales |
| [C#](Readme-Csharp.md) | C#: naming, tipos, async/await, LINQ, Dependency Injection, null safety, SOLID, testing |
| [JavaScript, React y Next.js](Readme-Javascript.md) | JS/React/Next.js: hooks, componentes, App Router, data fetching, API Routes, TypeScript |
| [PowerShell](Readme-Powershell.md) | PowerShell: verb-noun, ForEach, try/catch, Write-* cmdlets, seguridad y optimizacion |
| [Python](Readme-Python.md) | Python: PEP 8, type hints, virtualenv, async, list comprehensions, context managers |
| [Terraform](Readme-Terraform.md) | Terraform: modulos, estado remoto, variables, plan/apply, seguridad, workspaces |
| [Azure](Readme-Azure.md) | Azure: naming, RBAC, VNet, Key Vault, App Service/AKS, costos, DR, governance |
| [AWS](Readme-Aws.md) | AWS: naming, IAM, VPC, Secrets Manager, Lambda/EKS, costos, DR, governance |
| [Security](Readme-Security.md) | Seguridad transversal: auth, OWASP, secretos, APIs, validacion, uploads, webhooks, contenedores, criptografia |

## Uso 💻
Cada archivo sigue una estructura de **Checklist** con puntos numerados. Usa los readmes como referencia rapida antes de escribir codigo o durante code reviews.

## Cursor rules (IA) 🤖
Plantillas en [templates/cursor-rules/](templates/cursor-rules/) para copiar a `.cursor/rules/` de cada proyecto. La IA las aplica automaticamente al editar archivos del stack (ej. `**/*.cs`).

| Template | Checklist |
|----------|-----------|
| [csharp.mdc](templates/cursor-rules/csharp.mdc) | [Readme-Csharp.md](Readme-Csharp.md) |


## Orden de secciones 🧭
Los readmes comparten el mismo orden de titulos donde aplica. Las secciones especificas de cada stack van en el medio; el contenido de cada una sigue las convenciones de esa tecnologia.

| Orden | Seccion | Aplica a |
|-------|---------|----------|
| 1 | Naming conventions 🏷️ | Todos |
| 2 | Variables y tipos 📐 | Lenguajes, Cloud (tags), Terraform (variables) |
| 3 | Imports / modulos 📥 | Python, Terraform |
| 4 | Funciones ⚙️ | Lenguajes de programacion |
| — | *Secciones especificas del stack* | React, Next.js, LINQ, modulos, red cloud, etc. |
| n-6 | Manejo de errores ❌ | Lenguajes |
| n-5 | Comentarios y documentacion 📝 | Todos |
| n-4 | Seguridad 🔒 | Todos |
| n-3 | Testing 🧪 | Lenguajes |
| n-2 | Performance ⚡ | Lenguajes, Cloud (costos) |
| n-1 | Estructura y organizacion 📂 | Todos |
| n | Legibilidad / Formato y herramientas ✨ 🛠️ | Todos |

## Folder structure 🗂️
```
code-best-practices/
├── Readme.md           # This file
├── Readme-Bash.md
├── Readme-Csharp.md
├── Readme-Javascript.md
├── Readme-Powershell.md
├── Readme-Python.md
├── Readme-Terraform.md
├── Readme-Azure.md
├── Readme-Aws.md
├── Readme-Security.md
└── templates/
    └── cursor-rules/
        ├── Readme.md
        └── csharp.mdc
```

### Licence ⚖️
Creted by **Agustina Fassina** (este repositorio es para referencia y uso propio).