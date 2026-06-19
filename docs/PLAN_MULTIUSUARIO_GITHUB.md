# Plan sencillo: usuarios de GitHub y JSON guardados directamente en el repositorio

## 1. Enfoque revisado

La aplicacion debe seguir siendo lo mas simple posible:

- Un `index.html` principal.
- Sin base de datos externa.
- Sin Supabase, Firebase, Clerk, Auth0 ni servicios similares.
- Sin backend propio si se acepta que el navegador hable directamente con la API de GitHub.
- Cada usuario entra con su cuenta de GitHub o con un token de GitHub.
- Las materias se cargan desde JSON del repositorio.
- Al guardar, la aplicacion actualiza los JSON dentro del mismo repositorio.
- El jefe de departamento puede ver los JSON de todos porque tiene acceso al repositorio completo.

Este enfoque es viable, pero hay que aceptar una consecuencia: si el navegador escribe directamente en GitHub, el usuario debe tener permisos reales sobre el repositorio. La aplicacion no podra ocultar ni limitar completamente esos permisos, porque no hay servidor intermedio que actue como filtro.

## 2. Arquitectura propuesta

```text
index.html publicado en GitHub Pages
  |
  | autenticacion GitHub
  v
token de GitHub en el navegador
  |
  | GitHub REST API
  v
repositorio GitHub
  |
  | JSON por usuario y materia
  v
data/plans/
```

La aplicacion deja de usar `localStorage` como fuente principal. `localStorage` solo se usaria para:

- recordar preferencias;
- mantener una copia temporal;
- recuperar cambios si falla la conexion;
- guardar un token solo si el usuario lo decide explicitamente.

## 3. Dos formas de identificarse con GitHub

### Opcion A: OAuth Device Flow

Es la opcion mas parecida a "Entrar con GitHub" sin backend.

Funcionamiento:

1. El usuario pulsa "Entrar con GitHub".
2. La app pide a GitHub un codigo temporal.
3. La app muestra un codigo tipo `ABCD-1234`.
4. El usuario abre `https://github.com/login/device`.
5. Introduce el codigo y autoriza la aplicacion.
6. La app recibe un token y ya puede leer/escribir en GitHub.

Ventajas:

- No hay que pedir al usuario que copie un token manualmente.
- No hace falta guardar un `client_secret` en el HTML.
- Cada usuario autoriza con su propia cuenta de GitHub.

Limitaciones:

- Hay que registrar una OAuth App en GitHub y activar Device Flow.
- La experiencia es menos directa que usuario/contrasena.
- El token queda en el navegador durante la sesion.
- El usuario debe tener permisos de escritura en el repositorio.

Esta seria la opcion recomendada si se quiere una experiencia limpia y aun asi evitar servicios externos.

### Opcion B: token personal fino de GitHub

Es la opcion mas simple de implementar.

Funcionamiento:

1. Cada profesor crea en GitHub un Fine-grained personal access token.
2. El token se limita al repositorio del planificador.
3. El token tiene permiso `Contents: read and write`.
4. El profesor pega el token en la pantalla inicial de la aplicacion.
5. La app usa ese token para cargar y guardar JSON.

Ventajas:

- Muy facil de desarrollar.
- No requiere OAuth App.
- No requiere backend.
- Permite empezar rapido.

Limitaciones:

- Es menos comodo para el profesorado.
- El token es sensible y debe tratarse como una contrasena.
- Si se guarda en `localStorage`, cualquier script cargado por la pagina podria leerlo.
- Si no se guarda, el usuario debe pegarlo en cada sesion.

Esta opcion es buena para un prototipo interno, pero para uso real conviene pasar a Device Flow.

## 4. Repositorio y permisos

Hay que decidir si el repositorio sera de una organizacion o de una cuenta personal.

Recomendacion:

- Crear una organizacion de GitHub para el departamento o centro.
- Crear un repositorio privado para el planificador.
- Anadir a todos los profesores como colaboradores.
- Dar permisos de escritura solo si deben guardar sus propias materias.
- Dar al jefe de departamento permisos de mantenimiento o administracion.

Limitacion importante:

GitHub no permite, de forma sencilla, decir "este usuario solo puede escribir en su carpeta" dentro del mismo repositorio si todos tienen permiso de escritura. Sin backend, la separacion por usuario sera una regla de la aplicacion y una norma de uso, no una barrera de seguridad fuerte.

Si se necesita seguridad estricta por carpetas, haria falta:

- un repositorio por profesor; o
- ramas por profesor con protecciones; o
- un backend intermedio; o
- revisar cambios mediante pull requests.

Para el caso descrito, la solucion sencilla puede asumir confianza entre usuarios del departamento.

## 5. Estructura de carpetas

La estructura actual `data/` puede evolucionar asi:

```text
data/
  templates/
    1__eso_modelo_2025_2026_planner.json
    1__eso_modelo_detallado_2025_2026_planner.json
    2__bach_ccss_planner.json
    fpb_cc_aa__planner.json
    festivos_globales.json

  plans/
    2025-2026/
      users.json
      festivos_globales.json
      profesores/
        ana-garcia/
          index.json
          matematicas_1_eso.json
          matematicas_2_eso.json
        luis-perez/
          index.json
          tecnologia_3_eso.json
```

`users.json` sirve para que la app sepa que usuarios existen y que rol tiene cada uno:

```json
[
  {
    "githubLogin": "ana-garcia",
    "displayName": "Ana Garcia",
    "role": "teacher",
    "folder": "ana-garcia"
  },
  {
    "githubLogin": "jefe-departamento",
    "displayName": "Jefe de Departamento",
    "role": "department_head",
    "folder": "jefe-departamento"
  }
]
```

`index.json` de cada profesor lista sus materias:

```json
{
  "githubLogin": "ana-garcia",
  "displayName": "Ana Garcia",
  "academicYear": "2025-2026",
  "subjects": [
    {
      "id": "mjjx9fbt7x9vabc76gg",
      "name": "Matematicas 1 ESO",
      "file": "matematicas_1_eso.json",
      "updatedAt": "2026-06-18T18:00:00Z"
    }
  ]
}
```

Cada materia mantiene practicamente el formato actual:

```json
{
  "schemaVersion": 2,
  "ownerGithubLogin": "ana-garcia",
  "academicYear": "2025-2026",
  "id": "mjjx9fbt7x9vabc76gg",
  "name": "Matematicas 1 ESO",
  "color": "#d033db",
  "startDate": "2025-09-08",
  "endDate": "2026-06-30",
  "schedule": {
    "1": 1,
    "2": 1,
    "3": 1,
    "4": 1,
    "5": 0
  },
  "planningMode": "unit",
  "units": [],
  "localHolidays": [],
  "customDates": [],
  "evaluations": {},
  "updatedAt": "2026-06-18T18:00:00Z"
}
```

## 6. Flujo de uso

### Profesor

1. Abre `index.html`.
2. Pulsa "Entrar con GitHub" o pega su token.
3. La app consulta `https://api.github.com/user` para saber su `login`.
4. La app lee `data/plans/2025-2026/users.json`.
5. Busca el usuario por `githubLogin`.
6. Si tiene rol `teacher`, carga solo `data/plans/2025-2026/profesores/<folder>/index.json`.
7. Muestra sus materias.
8. Al abrir una materia, carga el JSON correspondiente.
9. Al guardar, actualiza ese JSON en GitHub.
10. Tambien actualiza el `index.json` del profesor con `updatedAt`.

### Jefe de departamento

1. Abre `index.html`.
2. Se identifica con GitHub.
3. La app consulta `users.json`.
4. Si su rol es `department_head`, muestra un panel global.
5. El panel lista todos los profesores y todas sus materias.
6. Puede abrir cualquier materia en modo consulta.
7. Si se decide permitirlo, tambien puede editar materias ajenas.

## 7. Como se guarda un JSON en GitHub

La app debe usar GitHub REST API, endpoint de contenidos:

```text
GET /repos/{owner}/{repo}/contents/{path}
PUT /repos/{owner}/{repo}/contents/{path}
```

Para actualizar un archivo existente:

1. Hacer `GET` del archivo.
2. Guardar su `sha`.
3. Convertir el JSON nuevo a Base64.
4. Hacer `PUT` enviando:
   - `message`: mensaje del commit;
   - `content`: JSON en Base64;
   - `sha`: SHA actual del archivo;
   - `branch`: normalmente `main`.

Ejemplo conceptual:

```json
{
  "message": "Actualizar Matematicas 1 ESO",
  "content": "eyJpZCI6ICJtan...",
  "sha": "3d21ec53a331a6f037a91c368710b99387d012c1",
  "branch": "main"
}
```

Para crear un archivo nuevo no se envia `sha`.

## 8. Cambios concretos en la app actual

### 8.1. Sustituir `localStorage` como fuente principal

Ahora el codigo usa:

- `app.loadFromStorage()`
- `app.saveToStorage()`
- `app.saveToStorageRaw()`
- `app.saveCurrentSubject()`

Hay que crear una capa:

```text
GitHubStorage
  login()
  logout()
  getCurrentUser()
  loadUsers()
  listMySubjects()
  listAllSubjects()
  loadSubject(path)
  saveSubject(path, subject)
  deleteSubject(path)
```

La app debe trabajar asi:

```text
pantalla
  -> modifica AppState
  -> llama a DataService
  -> DataService usa GitHubStorage
  -> GitHubStorage actualiza JSON en GitHub
```

### 8.2. Pantalla inicial

Anadir una vista antes del dashboard:

- Boton "Entrar con GitHub".
- Alternativa "Usar token personal".
- Mensaje de estado.
- Boton "Cerrar sesion".

### 8.3. Estado de sincronizacion

La app debe mostrar:

- "Sin identificar".
- "Cargando desde GitHub".
- "Guardando".
- "Guardado en GitHub".
- "Error de guardado".
- "Hay una version mas reciente en GitHub".

### 8.4. Guardado con retardo

No conviene hacer un commit por cada tecla pulsada.

Propuesta:

- Guardar cambios locales en memoria al instante.
- Esperar 1,5 o 2 segundos sin cambios.
- Enviar el JSON a GitHub.
- Si el usuario pulsa "Guardar ahora", enviar inmediatamente.

## 9. Conflictos

GitHub exige el `sha` actual para actualizar un archivo. Esto ayuda a detectar si otro dispositivo ha cambiado el JSON antes.

Regla:

- Si el `sha` local coincide, se guarda.
- Si GitHub devuelve conflicto, la app no sobrescribe.
- La app muestra:
  - "Este archivo ha cambiado en GitHub".
  - "Recargar version de GitHub".
  - "Guardar como copia".

Para una primera version, no hace falta mezclar cambios automaticamente.

## 10. Seguridad realista

Este modelo es sencillo, pero no debe venderse como seguridad fuerte por usuario.

Lo que si protege:

- Solo usuarios con cuenta GitHub y permiso al repositorio pueden leer/escribir.
- GitHub registra quien hizo cada commit.
- El jefe puede ver el historial.
- Los tokens pueden revocarse.

Lo que no protege completamente:

- Un profesor con permiso de escritura podria modificar archivos de otro profesor fuera de la app.
- Un token guardado en el navegador puede ser robado si se introduce codigo externo inseguro.
- No hay reglas de servidor que impidan rutas incorrectas.

Medidas prudentes:

- Repositorio privado.
- No cargar scripts externos.
- Usar Fine-grained tokens si se usa token manual.
- No guardar tokens en `localStorage` por defecto.
- Mostrar una opcion "recordar en este dispositivo" solo bajo responsabilidad del usuario.
- Revisar periodicamente el historial de commits.

## 11. Fases de desarrollo

### Fase 1: GitHubStorage con token manual

Objetivo: validar la escritura en GitHub cuanto antes.

Tareas:

- Crear pantalla para pegar token.
- Consultar `https://api.github.com/user`.
- Leer `users.json`.
- Cargar materias del usuario.
- Guardar una materia con `PUT /contents`.
- Mostrar errores de permisos y conflictos.

Resultado:

- La app ya funciona desde varios dispositivos si se usa el mismo usuario/token.

### Fase 2: reorganizar `data/`

Objetivo: separar plantillas de planificaciones reales.

Tareas:

- Mover ejemplos actuales a `data/templates/`.
- Crear `data/plans/2025-2026/users.json`.
- Crear carpetas por profesor.
- Crear `index.json` por profesor.
- Adaptar importacion de plantillas.

Resultado:

- La app distingue entre modelos generales y materias reales.

### Fase 3: panel del jefe de departamento

Objetivo: vista global.

Tareas:

- Detectar rol `department_head`.
- Leer todos los `index.json`.
- Mostrar profesores y materias.
- Abrir materias en modo consulta.
- Decidir si el jefe puede editar y guardar materias de otros.

Resultado:

- El jefe ve todas las planificaciones.

### Fase 4: OAuth Device Flow

Objetivo: evitar que el usuario tenga que crear y pegar tokens manuales.

Tareas:

- Registrar OAuth App en GitHub.
- Activar Device Flow.
- Implementar solicitud de `device_code`.
- Mostrar `user_code`.
- Hacer polling hasta recibir `access_token`.
- Sustituir el formulario de token por "Entrar con GitHub".

Resultado:

- Experiencia mas sencilla para el profesorado.

### Fase 5: pulido

Objetivo: hacer la herramienta comoda para uso diario.

Tareas:

- Guardado automatico con retardo.
- Boton "Guardar ahora".
- Indicador de ultimo guardado.
- Cache local de emergencia.
- Deteccion de conflictos.
- Exportacion manual como copia de seguridad.
- Validacion del JSON antes de guardar.

## 12. Recomendacion practica

Para este proyecto, el camino mas pragmatico es:

1. Mantener `index.html`.
2. Implementar primero login por token manual fino.
3. Guardar JSON directamente con GitHub REST API.
4. Reorganizar los datos en `data/templates/` y `data/plans/`.
5. Crear vista de profesor y vista de jefe.
6. Cuando el flujo este probado, sustituir el token manual por OAuth Device Flow.

Asi se evita depender de aplicaciones externas y se comprueba rapido si GitHub como almacen principal encaja con la forma de trabajar del departamento.

## 13. Referencias oficiales utiles

- GitHub REST API Contents: `https://docs.github.com/en/rest/repos/contents`
- GitHub OAuth Apps y Device Flow: `https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps`
- GitHub Personal Access Tokens: `https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens`
