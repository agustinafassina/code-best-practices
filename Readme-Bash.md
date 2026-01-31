## Best practices in Bash
List of good practices in Bash so I don't forget.

### Checklist
#### 1. Variables name
- Usar nombres descriptivos en minusculas, con guiones bajos: $file_name, $max_retries.
- Evitar abreviaturas ambiguas.
- Usar nombres que reflejen claramente el proposito de la variable.

#### 2. Variables name
- Siempre cerrar las expansiones de variables entre comillas dobles para evitar separacion de palabras y globing:
```
echo "$variable"
```
- Usar comillas simples para cadenas literales.
- Tener cuidado al usar variables sin comillas en comandos.

#### 3. Activar Modos Estrictos
- Habilitar modos estrictos para detectar errores rapidamente:
```
set -e  # Sale si algun comando falla
set -u  # Trata las variables no definidas como errores
set -o pipefail  # El pipeline devuelve fallo si cualquier comando falla
```

#### 4. Uso de Funciones
- Encapsular tareas reutilizables en funciones.
- Seguir la convencion de nombres (function_name()).
- Documentar funciones con comentarios.

#### 5. Manejo de Errores
- Verificar el estado de retorno de comandos ($?) o usa set -e.
- Gestionar errores explicitamente cuando sea necesario.
- Usar codigos de salida significativos (exit 0 exito, otros para errores).

#### 6. Comentarios y Documentacion
- Escribir comentarios claros explicando logica compleja.
- Usar bloques de comentarios (: << 'END' ... END), con moderacion.
- Documentar el proposito del script, entradas y salidas (shebang y descripcion).

#### 7.  Linea Shebang
- Especificar siempre el interprete al inicio:
```
#!/bin/bash
```

#### 8. Manejo de Cadenas y Comillas
- Usar comillas dobles para la expansion de variables.
- Evitar variables sin comillas en argumentos de comandos.

#### 9. Buenas Practicas en Bucles y Condicionales
- Usar for y while con condiciones claras.
- Es mejor usar [[ ]] en lugar de [ ], ya que tiene mas capacidades y mejor sintaxis.
- Aprovecha patrones glob y coincidencias de cadenas.

#### 10. Manejo Seguro de Archivos
- Evitar sobrescribir archivos sin verificar.
- Usar mktemp para archivos temporales.
- Usar trap para limpiar en caso de salida inesperada.

#### 11. Legibilidad y Formato
- Usar indentacion consistente (se recomienda espacios en lugar de tabulaciones).
- Dividir comandos largos en varias lineas usando \ o aqui-docs.
- Mantener el script compacto pero legible.

#### 12. Validacion de Entradas
- Verificar que los argumentos necesarios esten presentes.
- Validar rutas, permisos y entradas del usuario.

#### 13. Manejo de Errores y Codigos de Salida
- Devolver codigos de salida utiles y coherentes.
- Usar trap para manejar limpieza y errores globales.

#### 14. Registro y Salida
- Usar echo o printf para mensajes.
- Redirigir registros a archivos (>> d.log) si es necesario.
- Tener en cuenta las opciones -v y -x para depuracion y modo verbose.
