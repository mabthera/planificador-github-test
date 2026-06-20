# Estructura del proyecto

Este proyecto queda organizado como una aplicacion web estatica lista para versionarse con Git.

## Entrada principal

`index.html` es la version activa de la aplicacion.

## Datos activos

La carpeta `data/` contiene solo plantillas importables y datos del curso activo:

- `1__eso_modelo_2026_2027_planner.json`: modelo de 1 ESO para 2026-2027.
- `1__eso_modelo_detallado_2026_2027_planner.json`: modelo detallado de 1 ESO para 2026-2027.
- `plans/2026-2027/users.json`: usuarios disponibles en la pantalla de inicio.
- `plans/2026-2027/festivos_globales.json`: lista de fechas no lectivas compartidas.
- `plans/2026-2027/profesores/<usuario>/index.json`: indice de materias de cada profesor.
- `plans/2026-2027/profesores/<usuario>/festivos_usuario.json`: ajustes personales de festivos.

Todos los JSON se han comprobado como validos.

## Historico

Las versiones previas del HTML y los datos del curso 2025-2026 se han retirado del arbol activo para evitar duplicados y confusion. Si hace falta recuperar una version anterior, debe hacerse desde el historial de Git.

## Estado recomendado antes de sincronizar

- `index.html` debe abrirse correctamente en navegador.
- Los JSON de `data/` deben pasar validacion.
- El estado de Git debe revisarse antes de hacer commit.