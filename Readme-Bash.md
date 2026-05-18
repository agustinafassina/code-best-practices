## Best practices in Bash 🐚
List of good practices in Bash so I don't forget.

### Checklist 📋
#### 1. Naming conventions 🏷️
- Usar nombres descriptivos en minusculas, con guiones bajos: $file_name, $max_retries.
- Evitar abreviaturas ambiguas.
- Usar nombres que reflejen claramente el proposito de la variable.
- Seguir la convencion de nombres en funciones (function_name).

#### 2. Variables y tipos 💬
- Siempre cerrar las expansiones de variables entre comillas dobles para evitar separacion de palabras y globing:
```
echo "$variable"
```
- Usar comillas simples para cadenas literales.
- Tener cuidado al usar variables sin comillas en argumentos de comandos.
- Usar comillas dobles para la expansion de variables; evitar variables sin comillas en argumentos de comandos.

#### 3. Shebang y configuracion del script 📄
- Especificar siempre el interprete al inicio:
```
#!/bin/bash
```
- Habilitar modos estrictos para detectar errores rapidamente:
```
set -e  # Sale si algun comando falla
set -u  # Trata las variables no definidas como errores
set -o pipefail  # El pipeline devuelve fallo si cualquier comando falla
```

#### 4. Funciones ⚙️
- Encapsular tareas reutilizables en funciones.
- Documentar funciones con comentarios.

#### 5. Bucles y condicionales 🔁
- Usar for y while con condiciones claras.
- Es mejor usar [[ ]] en lugar de [ ], ya que tiene mas capacidades y mejor sintaxis.
- Aprovechar patrones glob y coincidencias de cadenas.

#### 6. Manejo de errores ❌
- Verificar el estado de retorno de comandos ($?) o usar set -e.
- Gestionar errores explicitamente cuando sea necesario.
- Usar codigos de salida significativos (exit 0 exito, otros para errores).
- Devolver codigos de salida utiles y coherentes.
- Usar trap para manejar limpieza y errores globales.

#### 7. Validacion de entradas ✅
- Verificar que los argumentos necesarios esten presentes.
- Validar rutas, permisos y entradas del usuario.

#### 8. Manejo de archivos 📁
- Evitar sobrescribir archivos sin verificar.
- Usar mktemp para archivos temporales.
- Usar trap para limpiar en caso de salida inesperada.

#### 9. Logging y salida 📤
- Usar echo o printf para mensajes.
- Redirigir registros a archivos (>> logfile) si es necesario.
- Tener en cuenta las opciones -v y -x para depuracion y modo verbose.

#### 10. Comentarios y documentacion 📝
- Escribir comentarios claros explicando logica compleja.
- Usar bloques de comentarios (: << 'END' ... END), con moderacion.
- Documentar el proposito del script, entradas y salidas.

#### 11. Legibilidad y formato ✨
- Usar indentacion consistente (se recomienda espacios en lugar de tabulaciones).
- Dividir comandos largos en varias lineas usando \ o here-docs.
- Mantener el script compacto pero legible.
