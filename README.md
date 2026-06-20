# Planificador Portable

Aplicacion web estatica para planificar sesiones, unidades, actividades, evaluaciones y festivos de un curso academico.

La entrada principal del proyecto es `index.html`.

## Como usarlo

1. Abre `index.html` en el navegador.
2. Conecta con GitHub para cargar las carpetas de profesorado del curso activo.
3. Copia una plantilla integrada o importa un JSON de `data/` si necesitas partir de un modelo.
4. Exporta tus datos desde la propia aplicacion para conservar copias de seguridad.

La aplicacion no necesita instalacion, servidor, base de datos ni dependencias externas para abrirse. Guarda una copia local del trabajo en el navegador y sincroniza los datos del curso activo mediante GitHub.

## Estructura

```text
.
|-- index.html
|-- data/
|   |-- plans/
|   |   `-- 2026-2027/
|-- docs/
|-- .editorconfig
|-- .gitignore
`-- README.md
```

- `index.html`: aplicacion principal.
- `data/`: plantillas importables y datos del curso activo.
- `data/plans/2026-2027/`: usuarios, festivos globales y carpetas de profesorado que lee la aplicacion.
- `docs/`: documentacion interna del proyecto.

## Preparar sincronizacion con Git

Si el repositorio ya tiene remoto configurado:

```bash
git add .
git commit -m "Clean project structure"
git push
```

Si aun no hay remoto:

```bash
git remote add origin <url-del-repositorio>
git branch -M main
git push -u origin main
```

## Notas de mantenimiento

- Mantener `index.html` como unica version activa.
- Guardar nuevas plantillas importables en `data/`.
- Mantener los datos conectados a GitHub dentro de `data/plans/<curso>/`.
- Antes de sincronizar, comprobar que los JSON siguen siendo validos.