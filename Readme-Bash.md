## Best practices in Bash
List of good practices in Bash so I don't forget.

### Checklist
#### 1. Variables name
- Usar nombres descriptivos en minúsculas, con guiones bajos: $file_name, $max_retries.
- Evitar abreviaturas ambiguas.
- Usar nombres que reflejen claramente el propósito de la variable.

#### 2. Variables name
- Siempre cerrar las expansiones de variables entre comillas dobles para evitar separación de palabras y globing:
```
echo "$variable"
```
- Usar comillas simples para cadenas literales.
- Tener cuidado al usar variables sin comillas en comandos.

#### 3. Activar Modos Estrictos
- Habilitar modos estrictos para detectar errores rápidamente:
```
set -e  # Sale si algún comando falla
set -u  # Trata las variables no definidas como errores
set -o pipefail  # El pipeline devuelve fallo si cualquier comando falla
```

#### 4. Uso de Funciones
- Encapsular tareas reutilizables en funciones.
- Seguir la convención de nombres (function_name()).
- Documentar funciones con comentarios.

#### 5. Manejo de Errores
- Verificar el estado de retorno de comandos ($?) o usa set -e.
- Gestionar errores explícitamente cuando sea necesario.
- Usar códigos de salida significativos (exit 0 éxito, otros para errores).

#### 6. Comentarios y Documentación
- Escribir comentarios claros explicando lógica compleja.
- Usar bloques de comentarios (: << 'END' ... END), con moderación.
- Documentar el propósito del script, entradas y salidas (shebang y descripción).

#### 7.  Línea Shebang
- Especificar siempre el intérprete al inicio:
```
#!/bin/bash
```

#### 8. Manejo de Cadenas y Comillas
- Usar comillas dobles para la expansión de variables.
- Evitar variables sin comillas en argumentos de comandos.

#### 9. Buenas Prácticas en Bucles y Condicionales
- Usar for y while con condiciones claras.
- Es mejor usar [[ ]] en lugar de [ ], ya que tiene más capacidades y mejor sintaxis.
- Aprovecha patrones glob y coincidencias de cadenas.

#### 10. Manejo Seguro de Archivos
- Evitar sobrescribir archivos sin verificar.
- Usar mktemp para archivos temporales.
- Usar trap para limpiar en caso de salida inesperada.

#### 11. Legibilidad y Formato
- Usar indentación consistente (se recomienda espacios en lugar de tabulaciones).
- Dividir comandos largos en varias líneas usando \ o aquí-docs.
- Mantener el script compacto pero legible.

#### 12. Validación de Entradas
- Verificar que los argumentos necesarios estén presentes.
- Validar rutas, permisos y entradas del usuario.

#### 13. Manejo de Errores y Códigos de Salida
- Devolver códigos de salida útiles y coherentes.
- Usar trap para manejar limpieza y errores globales.

#### 14. Registro y Salida
- Usar echo o printf para mensajes.
- Redirigir registros a archivos (>> d.log) si es necesario.
- Tener en cuenta las opciones -v y -x para depuración y modo verbose.
