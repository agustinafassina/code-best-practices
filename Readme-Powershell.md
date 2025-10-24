# Best practices in Powershel
List of good practices in PowerShell so I don't forget.

### Checklist
1.Nombres de Variables
- Usa nombres descriptivos y claros ($Archivo, $Ruta, $Hash, $Contador).
- Usa PascalCase o camelCase (consistencia).
- Evita abreviaturas ambiguas.
- Para variables globales o configuraciones, usa nombres con prefijos como $Global, $Config.
2. Tipos y Declaraciones
- PowerShell es dinámico, no es necesario declarar tipos, pero para scripts complejos, usa [type] cuando sea conveniente.
- Cuando crees funciones, considera definir parámetros con tipos:
powershell
```
param (
  [string]$RutaArchivo,
  [int]$EdadMaxima = 30
)
```
3. Funciones
- Usa funciones para organizar tareas repetitivas.
- El nombre de funciones en verbo-nombre, ejemplo: Get-Hash, Remove-OldFiles.
- Incluye comentarios y/o help comments (Get-Help sobre la función).
4. Uso de foreach
- Preferir ForEach-Object { } en pipelines y foreach ($item in $collection) { } para iteración simple.
- Para rendimiento en muchas iteraciones, usa foreach en lugar de pipeline cuando la lógica sea compleja y no necesitas pasar objetos en pipeline.
5. Control de errores
- Usa try { } catch { } para controlar errores críticos.
- Usa -ErrorAction SilentlyContinue o -ErrorAction Stop con comandos.
- Siempre que puedas, captura errores y registra o controla.
6. Salidas y Logging
- Utiliza Write-Host, Write-Warning, Write-Verbose, Write-Debug para distintas salidas.
- En scripts para producción, es recomendable escribir logs en archivos con Start-Transcript o funciones personalizadas.
7. Comentarios
- Describe qué hace cada bloque o función.
- La documentación en las funciones con comentarios <# ... #> o en formato Get-Help.
8. Uso de Pipeline Correctamente
- Encapsula lógica en funciones y pásalas por pipeline.
- Evita usar Write-Output fuera de funciones, o los Write-* en scripts complejos.
- Prefiere devolver objetos, no cadenas o salidas no estructuradas.
9. Objetos y Formatos
- Usa Get-ChildItem y trabaja con objetos, no solo con cadenas.
- Aprovecha objetos de PowerShell en pipeline para filtrar, ordenar y seleccionar.
10. Seguridad
- No ejecutes scripts con permisos innecesarios.
- Usa -WhatIf y -Confirm en operaciones destructivas.
- Validar rutas, permisos y entradas.
11. Optimización
- Minimiza llamadas a comandos costosos en loops.
- Cachea resultados si los vas a usar varias veces.
- Usa -Recurse cuando sea necesario, pero con cuidado para no procesar más de lo necesario.
12. Compatibilidad y Versiones
- Programar considerando versiones de PowerShell y compatibilidad.
- Usa PSVersionTable.PSVersion para verificar la versión si es necesario.
13. Estructura y Legibilidad
- Indenta con tabs o espacios de manera consistente.
- Separar lógicas en funciones.
- Agrupar código relacionado en bloques o scripts modulados.
