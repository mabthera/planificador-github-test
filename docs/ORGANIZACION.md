# Criterio de organizacion del proyecto

Este proyecto se ha organizado para que funcione como un repositorio Git claro, facil de sincronizar y sencillo de mantener. La idea principal es separar la aplicacion activa, los datos reutilizables y la documentacion, dejando fuera del arbol de trabajo las versiones historicas que ya no se usan.

## Version principal

La aplicacion activa esta en `index.html`, que queda como punto de entrada habitual de una aplicacion web estatica.

Este nombre tiene varias ventajas:

- Permite abrir la aplicacion directamente en el navegador sin recordar nombres de version.
- Facilita publicarla mas adelante en servicios como GitHub Pages.
- Evita tener varias versiones activas compitiendo en la raiz del proyecto.

La raiz queda reservada para los archivos importantes del proyecto, no para copias antiguas o pruebas.

## Carpeta `data/`

Los archivos JSON de `data/` son datos de trabajo, modelos importables y datos conectados al curso activo.

Separarlos del HTML ayuda a distinguir claramente entre:

- Codigo de la aplicacion.
- Plantillas de programaciones.
- Festivos globales del curso activo.
- Carpetas de profesorado usadas por el guardado en GitHub.

Tambien facilita localizar rapidamente que archivos se pueden importar, revisar o actualizar sin tocar la aplicacion principal. El curso activo queda en `data/plans/2026-2027/`.

## Historico

Las versiones antiguas del planificador y los datos del curso 2025-2026 se han retirado de la estructura activa.

El historial de Git queda como referencia para comparar cambios o recuperar alguna parte anterior si hace falta.

No se dejan copias antiguas en carpetas locales porque generan confusion: cuando hay varios `planificador_v2.html`, `planificador_v3.html` o copias similares, no queda claro cual debe usarse.

## Carpeta `docs/`

La carpeta `docs/` guarda documentacion interna del proyecto.

Se ha creado para que el README no tenga que contenerlo todo. El README queda como entrada rapida para usar y sincronizar el proyecto, mientras que `docs/` puede contener explicaciones mas detalladas, decisiones de organizacion y futuras notas tecnicas.

## Archivos de configuracion

Se han anadido archivos basicos para que el repositorio sea mas estable al sincronizarse:

- `.gitignore`: evita subir archivos temporales, copias locales, carpetas de editor y salidas generadas.
- `.editorconfig`: define reglas comunes de formato para editores compatibles.
- `.gitattributes`: normaliza los finales de linea para reducir cambios falsos entre Windows y otros sistemas.

Estos archivos no cambian el funcionamiento de la aplicacion, pero ayudan a que el historial de Git sea limpio.

## Rama `main`

El repositorio se ha dejado en la rama `main`, que es la convencion habitual para proyectos nuevos.

Esto facilita la sincronizacion con plataformas actuales y evita tener que renombrar la rama mas adelante.

## Resultado

La estructura final queda pensada para tres usos:

- Abrir y usar la aplicacion desde `index.html`.
- Mantener los modelos JSON ordenados en `data/`.
- Sincronizar el proyecto con Git sin mezclar archivos activos, documentacion e historico.

La organizacion mantiene en el arbol de trabajo solo lo que la aplicacion necesita hoy y deja el proyecto preparado para crecer de forma mas clara.