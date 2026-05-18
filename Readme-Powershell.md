## Best practices in PowerShell 💙
List of good practices in PowerShell so I don't forget.

### Checklist 📋
#### 1. Naming conventions 🏷️
- Usar nombres descriptivos y claros ($File, $Route, $Hash, $Count).
- Usar PascalCase o camelCase (consistencia).
- Evitar abreviaturas ambiguas.
- Para variables globales o configuraciones, usar nombres con prefijos como $Global, $Config, $SystemVariableName.
- Funciones en formato Verb-Noun: Get-Hash, Remove-OldFiles, Post-File.

#### 2. Variables y tipos 📐
- Para scripts complejos, usar [type] cuando sea conveniente.
- Cuando usemos funciones, considerar definir parametros con tipos:
```
param (
  [string]$FileRoute,
  [int]$MaxOld = 30
)
```

#### 3. Funciones ⚙️
- Usar functions para organizar tareas repetitivas.
- Incluir comentarios y/o help comments (Get-Help sobre la funcion).

#### 4. Bucles e iteracion 🔁
- Preferir ForEach-Object { } en pipelines y foreach ($item in $collection) { } para iteracion simple.
- Para rendimiento en muchas iteraciones, usar foreach en lugar de pipeline cuando la logica sea compleja y no necesitas pasar objetos en pipeline.

#### 5. Manejo de errores ❌
- Usar try { } catch { } para controlar errores criticos.
- Usar -ErrorAction SilentlyContinue o -ErrorAction Stop con comandos.

#### 6. Objetos y formatos 📦
- Usar Get-ChildItem y trabajar con objetos, no solo con cadenas.

#### 7. Logging y salida 📤
- Utilizar Write-Host, Write-Warning, Write-Verbose, Write-Debug para distintas salidas.
- En scripts para prod, es recomendable escribir logs en archivos con Start-Transcript o funciones.

#### 8. Comentarios y documentacion 📝
- Describir que hace cada bloque o funcion si el script es enorme.
- Documentar funciones con comentarios <# ... #> o en formato Get-Help.

#### 9. Seguridad 🔒
- No ejecutar scripts con permisos innecesarios (importante).
- Usar -WhatIf y -Confirm en operaciones destructivas.
- Validar rutas, permisos y entradas.

#### 10. Performance ⚡
- Minimizar llamadas a comandos costosos en loops.
- Cachear resultados si se van a usar mas adelante.
- Usar -Recurse cuando sea necesario, pero tener en cuenta de no procesar mas de lo necesario.

#### 11. Compatibilidad y versiones 🔢
- Tener en cuenta versiones de PowerShell y compatibilidad.
- Usar $PSVersionTable.PSVersion para verificar la version (si es necesario).

#### 12. Estructura y organizacion 📂
- Separar logicas en funciones.
- Agrupar codigo relacionado en bloques o scripts modulados.

#### 13. Legibilidad y formato ✨
- Mantener indentacion y formato consistente.
- Dividir scripts largos en modulos o funciones reutilizables.
