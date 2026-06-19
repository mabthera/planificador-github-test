# Planificador Portable

Aplicacion web estatica para planificar sesiones, unidades, actividades, evaluaciones y festivos de un curso academico.

La version final indicada para el proyecto es `v2`, conservada como `index.html` para que sea la entrada principal del repositorio.

## Como usarlo

1. Abre `index.html` en el navegador.
2. Importa los modelos JSON desde la carpeta `data/` cuando quieras cargar ejemplos o plantillas.
3. Exporta tus datos desde la propia aplicacion para conservar copias de seguridad.

La aplicacion no necesita instalacion, servidor, base de datos ni dependencias externas. Guarda el trabajo en el almacenamiento local del navegador con la clave `planificador_data_v2`.

## Estructura

```text
.
|-- index.html
|-- data/
|-- docs/
|-- archive/
|   `-- legacy/
|-- .editorconfig
|-- .gitignore
`-- README.md
```

- `index.html`: aplicacion principal, basada en `planificador_v2.html`.
- `data/`: modelos JSON y festivos de ejemplo.
- `docs/`: documentacion interna del proyecto.
- `archive/legacy/`: versiones anteriores conservadas como referencia.

## Preparar sincronizacion con Git

Si el repositorio ya tiene remoto configurado:

```bash
git add .
git commit -m "Organize project structure"
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
- Guardar nuevas plantillas o ejemplos en `data/`.
- Evitar editar archivos en `archive/legacy/` salvo que sea para documentar el historico.
- Antes de sincronizar, comprobar que los JSON siguen siendo validos.

## Planes de evolucion

- `docs/PLAN_MULTIUSUARIO_GITHUB.md`: propuesta sencilla para convertir la aplicacion en multiusuario usando cuentas de GitHub y guardado directo de planificaciones JSON en el repositorio.
