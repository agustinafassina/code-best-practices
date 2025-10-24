## Best practices in Powershel
List of good practices in PowerShell so I don't forget.

### Checklist
#### 1. Variables name
- Usar nombres descriptivos y claros ($File, $Route, $Hash, $Count).
- Usar PascalCase o camelCase (consistencia).
- Evitar abreviaturas ambiguas.
- Para variables globales o configuraciones, usar nombres con prefijos como $Global, $Config, $SystemVariableName.
#### 2. Tipos y Declaraciones
- Para scripts complejos, usar [type] cuando sea conveniente.
- Cuando usemos funciones, considerar definir parámetros con tipos:
```
param (
  [string]$FileRoute,
  [int]$MaxOld = 30
)
```
#### 3. Funtions
- Usar functions para organizar tareas repetitivas.
- El nombre de las funciones en verb-name, example: Get-Hash, Remove-OldFiles, Post-File.
- Incluir comentarios y/o help comments (Get-Help sobre la funcion).
#### 4. Uso de foreach
- Preferir ForEach-Object { } en pipelines y foreach ($item in $collection) { } para iteración simple.
- Para rendimiento en muchas iteraciones, usar foreach en lugar de pipeline cuando la lógica sea compleja y no necesitas pasar objetos en pipeline.
#### 5. Access control
- Usar try { } catch { } para controlar errores críticos.
- Usar -ErrorAction SilentlyContinue o -ErrorAction Stop con comandos.
#### 6. Salidas y Logging
- Utilizar Write-Host, Write-Warning, Write-Verbose, Write-Debug para distintas salidas.
- En scripts para prod, es recomendable escribir logs en archivos con Start-Transcript o funciones.
#### 7. Comments
- Describir qué hace cada bloque o función si el script es enorme.
- La documentación en las funciones con comentarios <# ... #> o en formato Get-Help.
#### 8. Object and formats
- Usar Get-ChildItem y trabajar con objetos, no solo con cadenas.
#### 9. Security
- No ejecuter scripts con permisos innecesarios (important)
- Usar -WhatIf y -Confirm en operaciones destructivas.
- Validar rutas, permisos y entradas.
#### 10. Optimización
- Minimizar llamadas a comandos costosos en loops.
- Cachear resultados si se van a usar mas adelante.
- Usar -Recurse cuando sea necesario, pero tener en cuenta de no procesar más de lo necesario.
#### 11. Compatibily and versions
- Tener en cuenta versiones de PowerShell y compatibilidad.
- Usar PSVersionTable.PSVersion para verificar la versión (si es necesario).
#### 12. Structure y Legibilidad
- Separar lógicas en funciones.
- Agrupar código relacionado en bloques o scripts modulados.
