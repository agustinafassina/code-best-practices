## Best practices in Python 🐍
List of good practices in Python so I don't forget.

### Checklist 📋
#### 1. Naming conventions (PEP 8) 🏷️
- Usar snake_case para variables, funciones y modulos: user_name, get_user_by_id.
- Usar PascalCase para clases: UserService, DataProcessor.
- Usar MAYUSCULAS_CON_GUIONES para constantes: MAX_RETRIES, API_URL.
- Prefijo _ para "privado" (convencion): _internal_method, _private_var.
- Evitar abreviaturas ambiguas; usar nombres descriptivos.

#### 2. Variables y tipos 📐
- Usar type hints cuando mejore la legibilidad (Python 3.5+):
```python
def get_user(user_id: int) -> User | None:
    ...
```
- Preferir None explicito sobre null; usar Optional[T] o T | None para valores opcionales.
- Usar dataclasses o NamedTuple para estructuras de datos claras:
```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    email: str
```

#### 3. Imports 📥
- Agrupar imports: stdlib, third-party, local; separar cada grupo con linea en blanco.
- Evitar import *; importar solo lo necesario.
- Ordenar imports alfabeticamente dentro de cada grupo.
- Usar import absolutos sobre relativos cuando sea posible.

#### 4. Funciones ⚙️
- Mantener funciones cortas y con una sola responsabilidad.
- Limitar parametros (aax. 3-4); usar *args/**kwargs con cuidado.
- Nombrar funciones con verbos: get_user, save_data, validate_input.
- Usar docstrings (Google o NumPy style) para documentar:
```python
def get_user(user_id: int) -> User | None:
    """Obtiene un usuario por ID.

    Args:
        user_id: El ID del usuario.

    Returns:
        El usuario si existe, None en caso contrario.
    """
```

#### 5. Manejo de errores ❌
- Ser especifico con excepciones; no capturar Exception generica sin re-raise.
- Usar try/except/finally solo donde se pueda manejar el error.
- Preferir EAFP (Easier to Ask Forgiveness than Permission) sobre LBYL cuando sea apropiado.
- Usar raise desde except para re-lanzar preservando el stack trace:
```python
try:
    process_data()
except ValueError as e:
    log_error(e)
    raise
```

#### 6. Context managers y recursos 🗃️
- Usar with para archivos y recursos que requieran limpieza:
```python
with open("file.txt", "r") as f:
    content = f.read()
```
- Crear context managers con @contextmanager o __enter__/__exit__ cuando sea necesario.
- Usar contextlib.suppress para ignorar excepciones especificas de forma explicita.

#### 7. List comprehensions y generadores 🧮
- Preferir list/dict/set comprehensions sobre bucles cuando mejoren la legibilidad.
- Usar generadores ( ) para secuencias grandes; ahorran memoria:
```python
squares = (x**2 for x in range(1000000))
```
- Evitar comprehensions anidados complejos; extraer a funciones si es necesario.

#### 8. Virtual environments 🌐
- Siempre usar un entorno virtual (venv, poetry, conda) por proyecto.
- Crear requirements.txt o pyproject.toml para dependencias.
- Fijar versiones de dependencias para reproducibilidad.

#### 9. Async (asyncio) ⏳
- Usar async/await para operaciones I/O concurrentes.
- Usar asyncio.gather() para ejecutar tareas en paralelo cuando sea posible.
- Evitar bloqueos en codigo async; usar asyncio.sleep en lugar de time.sleep.
- Nombres de funciones async con prefijo o sufijo que indique async: fetch_user, get_data_async.

#### 10. Comentarios y documentacion 📝
- Escribir docstrings en modulos, clases y funciones publicas.
- Evitar comentarios que describan lo obvio; el codigo debe ser legible.
- Usar # para comentarios inline; mantenerlos cortos.
- Documentar decisiones no obvias o trampas conocidas.

#### 11. Seguridad 🛡️
- Nunca hardcodear secretos; usar variables de entorno o .env (python-dotenv).
- Validar y sanitizar entradas; usar bibliotecas como pydantic para validacion.
- Evitar eval() y exec() con input de usuario; riesgo de inyeccion.
- Usar parametros preparados en consultas SQL (cursor.execute con placeholders).

#### 12. Testing 🧪
- Usar pytest o unittest; preferir pytest por sintaxis mas limpia.
- Nombres descriptivos: test_get_user_returns_none_when_not_found.
- Usar fixtures para setup reutilizable.
- Mockear I/O y dependencias externas; usar unittest.mock o pytest-mock.
- Mantener tests rapidos; separar tests de integracion de unitarios.

#### 13. Estructura de proyectos 📂
- Seguir estructura estandar: src/, tests/, docs/, etc.
- Un modulo por archivo; nombre del archivo en snake_case.
- Usar __init__.py para paquetes; exponer API publica de forma controlada.
- Considerar estructuras como: app/, models/, services/, utils/, tests/.

#### 14. Performance ⚡
- Usar generadores para secuencias grandes en lugar de listas.
- Evitar concatenacion de strings en bucles; usar join() o f-strings.
- Considerar cProfile para profiling cuando haya problemas de rendimiento.
- Usar list/dict comprehensions; suelen ser mas rapidos que bucles equivalentes.

#### 15. Formato y herramientas 🛠️
- Seguir PEP 8; usar black o ruff para formateo automatico.
- Usar isort para ordenar imports.
- Configurar pre-commit o CI para linting (ruff, pylint, mypy).
- Limite de 88-100 caracteres por linea (black usa 88).
