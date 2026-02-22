## Best practices in C#
List of good practices in C# so I don't forget.

### Checklist
#### 1. Naming conventions
- Usar PascalCase para clases, metodos, propiedades, constantes y namespaces.
- Usar camelCase para variables locales, parametros y campos privados.
- Usar nombres descriptivos y claros; evitar abreviaturas ambiguas.
- Prefijos _ para campos privados (opcional, segun convencion del equipo): _userName.

#### 2. Variables y tipos
- Declarar tipos explicitos cuando mejore la legibilidad; usar var cuando el tipo sea obvio.
- Inicializar variables al declararlas cuando sea posible.
- Preferir readonly y const cuando el valor no cambie despues de la asignacion:
```csharp
private readonly IUserService _userService;
private const int MaxRetries = 3;
```

#### 3. Clases y encapsulamiento
- Mantener clases pequeñas y con una sola responsabilidad (SRP).
- Usar propiedades en lugar de campos publicos cuando se necesite logica o validacion.
- Preferir inmutabilidad cuando sea posible (record, init-only properties).
- Usar expresiones init para objetos inmutables:
```csharp
public record User(string Name, string Email);
```

#### 4. Metodos
- Mantener metodos cortos y enfocados en una tarea.
- Limitar el numero de parametros (max. 3-4); usar objetos o records para agrupar.
- Usar expresiones de metodo (=>) para metodos de una sola linea cuando sea apropiado.
- Nombrar metodos con verbos: GetUserById, SaveData, ValidateInput.

#### 5. Manejo de errores
- Usar try-catch solo donde se pueda manejar el error de forma significativa.
- No capturar Exception generica sin re-throw; ser especifico con el tipo de excepcion.
- Usar throw; para re-lanzar preservando el stack trace (no throw ex;).
- Preferir excepciones personalizadas cuando aporten valor.
- Usar ArgumentException, ArgumentNullException para validacion de parametros.

#### 6. Null safety
- Usar nullable reference types (C# 8+): string? para valores que pueden ser null.
- Preferir el operador null-conditional (?.) y null-coalescing (??) cuando sea apropiado:
```csharp
var name = user?.Name ?? "Unknown";
```
- Evitar null cuando sea posible; considerar Option<T> o patrones similares en logica compleja.

#### 7. LINQ y colecciones
- Preferir metodos de LINQ sobre bucles cuando mejoren la legibilidad.
- Usar AsEnumerable(), ToList(), ToArray() solo cuando sea necesario materializar.
- Evitar multiples iteraciones sobre la misma coleccion; materializar una vez si se reutiliza.
- Usar colecciones inmutables (ImmutableList, ImmutableDictionary) cuando los datos no cambien.

#### 8. Async y Await
- Usar async/await para operaciones I/O; evitar async void (excepto event handlers).
- Siempre usar CancellationToken en metodos async para permitir cancelacion.
- Usar ConfigureAwait(false) en librerias cuando no se necesite el contexto de sincronizacion.
- Nombres de metodos async con sufijo Async: GetUserByIdAsync, SaveDataAsync.

#### 9. Dependency Injection
- Inyectar dependencias por constructor; evitar new en clases que deban ser testeables.
- Registrar servicios en el contenedor de DI (Program.cs o Startup).
- Preferir interfaces sobre implementaciones concretas para desacoplar.
- Usar scoped para DbContext, transient para servicios stateless, singleton con cuidado.

#### 10. Comentarios y documentacion
- Escribir XML comments (///) en APIs publicas y metodos complejos.
- Evitar comentarios que describan lo obvio; el codigo debe ser autoexplicativo.
- Documentar parametros, retornos y excepciones lanzadas.
- Usar TODO o FIXME con contexto claro cuando sea necesario.

#### 11. Seguridad
- Nunca hardcodear secretos, credenciales o API keys; usar User Secrets, Variables de entorno o Key Vault.
- Validar y sanitizar todas las entradas de usuario.
- Usar parametros preparados en consultas SQL para evitar inyeccion.
- Aplicar principio de menor privilegio en permisos.

#### 12. Performance
- Evitar boxing innecesario; usar tipos genericos cuando sea posible.
- Usar StringBuilder para concatenacion de cadenas en bucles.
- Considerar Span<T> y Memory<T> para manipulacion de buffers sin copias.
- Usar object pooling para objetos que se crean y destruyen frecuentemente.

#### 13. Organizacion de archivos y proyectos
- Un archivo por clase; el nombre del archivo debe coincidir con el de la clase.
- Estructurar namespaces segun la estructura de carpetas del proyecto.
- Separar concerns: Controllers, Services, Repositories, Models, DTOs.
- DTO: Data transfer object, es para datos que se manejan entre varias capas.
- Entities: referencias a las tablas de las base de datos.
- Usar proyectos separados para capas (API, Domain, Infrastructure, Application).

#### 14. Testing
- Escribir tests unitarios para logica de negocio critica.
- Usar nombres descriptivos: MethodName_Scenario_ExpectedBehavior.
- Preferir AAA (Arrange, Act, Assert) para estructura clara.
- Mockear dependencias; evitar tests que dependan de I/O o red real.

#### 15. Estructura y legibilidad
- Usar regiones con moderacion; preferir archivos mas pequeños.
- Agrupar miembros relacionados (campos, propiedades, metodos).
- Mantener indentacion y formato consistente (usar .editorconfig o formateador).
- Respetar el limite de lineas por archivo (ej. 200-300) para facilitar navegacion.
