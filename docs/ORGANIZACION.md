# Criterio de organizacion del proyecto

Este proyecto se ha organizado para que funcione como un repositorio Git claro, facil de sincronizar y sencillo de mantener. La idea principal ha sido separar la aplicacion activa, los datos reutilizables, la documentacion y las versiones historicas.

## Version principal

La version final indicada del planificador es `v2`. Por eso se ha colocado como `index.html` en la raiz del proyecto.

Este nombre tiene varias ventajas:

- Es el punto de entrada habitual de una aplicacion web estatica.
- Permite abrir la aplicacion directamente en el navegador sin recordar nombres de version.
- Facilita publicarla mas adelante en servicios como GitHub Pages.
- Evita tener varias versiones activas compitiendo en la raiz del proyecto.

La raiz queda asi reservada para los archivos importantes del proyecto, no para copias antiguas o pruebas.

## Carpeta `data/`

Los archivos JSON se han movido a `data/` porque son datos de trabajo o modelos importables por la aplicacion.

Separarlos del HTML ayuda a distinguir claramente entre:

- Codigo de la aplicacion.
- Datos de ejemplo.
- Plantillas de programaciones.
- Festivos globales.

Tambien facilita localizar rapidamente que archivos se pueden importar, revisar o actualizar sin tocar la aplicacion principal.

## Carpeta `archive/legacy/`

Las versiones antiguas del planificador se han conservado en `archive/legacy/` en lugar de borrarlas.

Esto permite mantener una referencia historica por si hace falta comparar cambios, recuperar alguna parte anterior o entender la evolucion del proyecto.

No se dejan en la raiz porque eso genera confusion: cuando hay varios `planificador_v2.html`, `planificador_v3.html` o copias similares en el mismo nivel, no queda claro cual debe usarse. Al archivarlas, siguen disponibles pero ya no compiten con la version final.

## Carpeta `docs/`

La carpeta `docs/` guarda documentacion interna del proyecto.

Se ha creado para que el README no tenga que contenerlo todo. El README queda como entrada rapida para usar y sincronizar el proyecto, mientras que `docs/` puede contener explicaciones mas detalladas, decisiones de organizacion y futuras notas tecnicas.

## Archivos de configuracion

Se han añadido archivos basicos para que el repositorio sea mas estable al sincronizarse:

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

La organizacion no elimina informacion: solo separa lo activo de lo historico y deja el proyecto preparado para crecer de forma mas clara.
