# Estructura del proyecto

Este proyecto queda organizado como una aplicacion web estatica lista para versionarse con Git.

## Entrada principal

`index.html` es la version activa. Procede de `planificador_v2.html`, indicada como version final del proyecto.

## Datos

La carpeta `data/` contiene ficheros JSON importables desde la aplicacion:

- `festivos_globales.json`: lista de fechas no lectivas.
- `1__eso_planner.json`: ejemplo breve de 1 ESO.
- `1__eso_modelo_2025_2026_planner.json`: modelo de 1 ESO para 2025-2026.
- `1__eso_modelo_detallado_2025_2026_planner.json`: modelo detallado de 1 ESO para 2025-2026.
- `2__bach_ccss_planner.json`: ejemplo de 2 Bachillerato CCSS.
- `fpb_cc_aa__planner.json`: ejemplo de FPB CC AA.

Todos los JSON se han comprobado como validos.

## Historico

La carpeta `archive/legacy/` conserva versiones previas del HTML. No son la version activa, pero ayudan a recuperar cambios o comparar evolucion.

## Estado recomendado antes de sincronizar

- `index.html` debe abrirse correctamente en navegador.
- Los JSON de `data/` deben pasar validacion.
- Las versiones antiguas deben quedarse en `archive/legacy/`.
- El estado de Git debe revisarse antes de hacer commit.
