# Cursor rules templates

Plantillas `.mdc` para copiar en proyectos y que Cursor aplique las best practices automaticamente.

## Uso en un proyecto C#

1. Crear la carpeta en tu repo:
   ```
   mi-proyecto/
   └── .cursor/rules/
   ```

2. Copiar el template:
   ```
   templates/cursor-rules/csharp.mdc  →  mi-proyecto/.cursor/rules/csharp.mdc
   ```

3. (Opcional) Copiar o enlazar la checklist completa:
   ```
   Readme-Csharp.md  →  mi-proyecto/docs/best-practices-csharp.md
   ```

4. Reiniciar o abrir un archivo `.cs` — la regla aplica cuando trabajas en archivos C#.

## Que hace cada archivo

| Archivo | Aplica cuando |
|---------|----------------|
| `csharp.mdc` | Abres o editas `**/*.cs` |

## Referencia

- Checklist humana: [Readme-Csharp.md](../../Readme-Csharp.md)
- Reglas Cursor: [documentacion](https://docs.cursor.com/context/rules)
