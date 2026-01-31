## Best practices in JavaScript, React and Next.js
List of good practices in JavaScript, React and Next.js so I don't forget.

### Checklist
#### 1. Naming conventions (JavaScript)
- Usar camelCase para variables y funciones: userName, getUserById.
- Usar PascalCase para componentes React y clases: UserCard, UserService.
- Usar UPPER_SNAKE_CASE para constantes: API_URL, MAX_RETRIES.
- Nombres descriptivos; evitar abreviaturas ambiguas (evitar x, fn, tmp).

#### 2. Variables y declaraciones
- Preferir const sobre let; usar let solo cuando el valor cambie.
- Evitar var; usar const/let por scope de bloque.
- Declarar variables lo mas cerca posible de su uso.
- Usar destructuring para objetos y arrays cuando mejore la legibilidad:
```javascript
const { name, email } = user;
const [first, ...rest] = items;
```

#### 3. Funciones
- Usar arrow functions para callbacks y funciones cortas.
- Preferir function declarations para funciones que necesiten hoisting.
- Evitar funciones anidadas profundas; extraer a funciones nombradas.
- Usar parametros por defecto en lugar de || o ternarios para defaults:
```javascript
function greet(name = "Guest") { ... }
```

#### 4. React - Componentes
- Mantener componentes pequeños y enfocados en una tarea.
- Preferir componentes funcionales con hooks sobre class components.
- Un componente por archivo; nombre del archivo igual al del componente (PascalCase).
- Extraer logica reutilizable a custom hooks; extraer UI repetitiva a subcomponentes.

#### 5. React - Hooks
- Llamar hooks solo en el nivel superior; nunca dentro de bucles o condicionales.
- Usar useState para estado local; useContext + useReducer para estado global o complejo.
- Usar useEffect con array de dependencias correcto; evitar dependencias faltantes.
- Usar useCallback para funciones pasadas a hijos que se re-renderizan frecuentemente.
- Usar useMemo para calculos costosos; no abusar, medir antes de optimizar.
- Custom hooks: nombrar con prefijo use (useAuth, useFetch, useLocalStorage).

#### 6. React - Props y estado
- Props: evitar pasar mas de 3-4 props; usar objeto o children cuando crezca.
- Evitar prop drilling; usar Context o state management (Zustand, Redux) cuando sea necesario.
- No mutar estado directamente; usar setState con funcion para estado derivado:
```javascript
setCount(prev => prev + 1);
```
- Validar props con PropTypes o TypeScript cuando no uses TS.

#### 7. React - Rendering y performance
- Evitar re-renders innecesarios; usar React.memo para componentes puros.
- Mover estado lo mas abajo posible en el arbol de componentes.
- Usar key estable y unica en listas (evitar index como key cuando los items cambien).
- Lazy load con React.lazy() y Suspense para rutas o componentes pesados.

#### 8. Next.js - App Router (Next.js 13+)
- Usar app/ directory con layout.tsx, page.tsx, loading.tsx, error.tsx.
- Servers Components por defecto; usar "use client" solo cuando haga falta interactividad.
- Colocar "use client" lo mas abajo posible; mantener padres como Server Components.
- Usar Route Groups (carpetas con parentesis) para organizar sin afectar la URL.

#### 9. Next.js - Data fetching
- Usar fetch con cache de Next.js en Server Components; no useEffect para datos iniciales.
- Usar async/await directamente en page.tsx o layout.tsx para datos del servidor.
- Implementar loading.tsx para estados de carga; error.tsx para manejo de errores.
- Para datos en cliente: usar SWR, React Query (TanStack Query) o fetch en useEffect.

#### 10. Next.js - Routing y navegacion
- Usar Link de next/link en lugar de <a> para navegacion interna (prefetch).
- Usar useRouter para navegacion programatica.
- Dynamic routes: [id], [...slug]; usar generateStaticParams para SSG.
- Metadata: exportar metadata o generateMetadata en layout/page para SEO.

#### 11. Next.js - API Routes
- API Routes en app/api/ o pages/api/; un archivo por endpoint.
- Retornar Response con status correcto; usar NextResponse.json().
- Validar inputs; sanitizar datos antes de procesar.
- Para APIs complejas, considerar Route Handlers en app/api/[route]/route.ts.

#### 12. Async y Promises
- Usar async/await sobre .then() para legibilidad.
- Manejar errores con try/catch en async; no dejar promises sin catch.
- Usar Promise.all() o Promise.allSettled() para operaciones en paralelo.
- Evitar async sin await; verificar que la funcion realmente espere algo.

#### 13. Manejo de errores
- Usar Error Boundaries en React para capturar errores de rendering.
- Mostrar mensajes amigables al usuario; no exponer stack traces en produccion.
- Loggear errores (Sentry, etc.) para monitoreo.
- Validar datos en frontera; no confiar en input del usuario.

#### 14. Seguridad
- Nunca exponer API keys en cliente; usar variables de entorno (NEXT_PUBLIC_ solo para publico).
- Sanitizar HTML si se usa dangerouslySetInnerHTML; considerar librerias como DOMPurify.
- Validar y sanitizar inputs; evitar XSS e inyeccion.
- Usar HTTPS; configurar headers de seguridad (CSP, etc.).

#### 15. Estructura y organizacion
- Estructura por features o por tipo (components/, hooks/, utils/, lib/).
- Co-locar archivos relacionados: Component.tsx, Component.test.tsx, Component.module.css.
- Separar logica de negocio de componentes UI; custom hooks para logica.
- Usar barrel exports (index.ts) con moderacion; evitar ciclos de import.

#### 16. Testing
- Usar Vitest o Jest; React Testing Library para componentes.
- Testear comportamiento, no implementacion.
- Mockear fetch y APIs externas; usar MSW para mocks mas realistas.
- Testear hooks con renderHook (React Testing Library).

#### 17. TypeScript (recomendado)
- Habilitar strict mode en tsconfig.
- Tipar props, estado y retornos de funciones; evitar any.
- Usar interfaces para objetos; type para unions e intersecciones.
- Aprovechar tipos de React: React.FC, React.PropsWithChildren, etc.

#### 18. Formato y herramientas
- Usar ESLint y Prettier; configurar format on save.
- Configurar path aliases (@/components) en jsconfig/tsconfig.
- Usar convenciones de commits (Conventional Commits) para changelogs.
- Revisar bundle size; usar next/bundle-analyzer o similar.
