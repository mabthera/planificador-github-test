# Planificador de materias

Aplicacion web del Departamento de Matematicas del IES de Teis para organizar la programacion de materias durante el curso.

Permite repartir unidades o actividades en un calendario real, teniendo en cuenta:

- Fechas de inicio y fin de curso.
- Sesiones semanales de cada materia.
- Fechas de evaluacion.
- Festivos generales del centro.
- Festivos o dias no lectivos personalizados de cada docente.
- Copias, importaciones y exportaciones de materias.

La aplicacion principal es `index.html`.

## Para que sirve

La app ayuda a comprobar si una programacion cabe en el calendario disponible.

Cada materia muestra:

- Las sesiones totales disponibles.
- Las sesiones ya programadas.
- Las sesiones libres o pendientes.
- El reparto por evaluaciones.
- Un calendario visual con las unidades o actividades colocadas dia a dia.

La finalidad no es solo guardar una lista de unidades, sino ver si el tiempo real del curso alcanza para impartirlas.

## Como entrar

1. Abre `index.html` en el navegador.
2. Introduce tu usuario de GitHub y tu token de acceso.
3. Pulsa **Conectar con GitHub**.
4. Selecciona tu carpeta de docente.

Cada docente trabaja en su propia carpeta. Si entras en la carpeta de otra persona, normalmente la veras en modo lectura.

## Token de GitHub

El token permite que la app cargue y guarde tus materias en GitHub.

Cada docente debe crear su propio token en su cuenta de GitHub:

1. Entra en GitHub.
2. Abre la pagina de tokens personales.
3. Crea un token clasico.
4. Marca el permiso `public_repo`.
5. Copia el token y pegalo en la pantalla inicial de la app.

Importante: el token funciona como una contrasena. No debe compartirse.

## Pantalla principal

Despues de entrar aparece el panel de materias.

Desde ahi puedes:

- Abrir una materia propia.
- Crear una materia nueva.
- Duplicar una materia.
- Exportar una materia como archivo JSON.
- Importar una materia desde un JSON.
- Copiar un ejemplo general a tus materias.
- Consultar carpetas del departamento.
- Ver el calendario combinado.

Los ejemplos generales son modelos comunes. Para editarlos, primero hay que copiarlos a tus materias.

## Editar una materia

Al abrir una materia aparecen dos zonas:

- A la izquierda, los datos editables.
- A la derecha, el resumen y el calendario.

En la configuracion general puedes cambiar:

- Nombre de la materia.
- Color.
- Fecha de inicio.
- Fecha de fin.
- Modo de planificacion.
- Fechas de evaluacion.
- Fechas senaladas.
- Sesiones semanales.

Los cambios se guardan automaticamente.

## Modos de planificacion

Hay dos formas de planificar una materia.

### Por unidades

Es el modo mas simple.

Cada unidad tiene:

- Nombre.
- Sesiones base.
- Sesiones extra.

La app coloca las unidades en orden en el calendario.

### Por actividades

Es un modo mas detallado.

Cada unidad puede contener varias actividades, y cada actividad tiene sus propias sesiones.

Este modo sirve cuando se quiere controlar mejor que partes de una unidad ocupan cada tramo del calendario.

## Sesiones base y sesiones extra

Las sesiones base representan la duracion principal prevista.

Las sesiones extra permiten ajustar:

- Refuerzos.
- Repasos.
- Perdidas de tiempo.
- Recortes de sesiones.

Un valor positivo suma sesiones.

Un valor negativo resta sesiones. Si una unidad queda por debajo de cero, la app la cuenta como cero sesiones, porque no se pueden programar sesiones negativas.

## Evaluaciones

Las fechas de evaluacion dividen el curso en tramos.

El resumen por evaluaciones muestra:

- `Tot`: sesiones disponibles en esa evaluacion.
- `Prog`: sesiones colocadas en esa evaluacion.
- `Pend`: sesiones libres o pendientes en esa evaluacion.

Si `Prog` coincide con `Tot`, esa evaluacion esta completa.

## Festivos

La app distingue entre dos tipos de festivos.

### Festivos globales

Son dias no lectivos compartidos por el departamento.

Se guardan en:

```text
data/plans/2026-2027/festivos_globales.json
```

### Festivos personales

Son ajustes propios de cada docente.

Sirven para marcar dias en los que una materia concreta no debe avanzar para ese profesor o profesora.

Se gestionan desde los botones:

- **Gestion Festivos**
- **Exportar Mis Festivos**
- **Importar Mis Festivos**

Al activar **Gestion Festivos**, se pueden marcar o desmarcar dias en el calendario.

## Fechas senaladas

Las fechas senaladas sirven para mostrar avisos en el calendario, por ejemplo:

- Recuperaciones.
- Pruebas.
- Entregas.
- Actividades especiales.

No eliminan sesiones por si solas. Solo anaden una marca visual.

Si un dia debe dejar de contar como lectivo, debe marcarse como festivo.

## Como leer el resumen

En cada materia aparece un resumen estadistico.

### Sesiones totales

Cantidad de sesiones disponibles en el calendario entre la fecha de inicio y fin, descontando festivos.

### Programadas

Sesiones que la app ha colocado en el calendario.

### Pendientes / libres

Diferencia entre las sesiones disponibles y las sesiones programadas.

Si el numero es positivo, quedan huecos libres.

Si el numero es negativo, la programacion no cabe en el calendario.

## Calendario combinado

El calendario combinado permite ver varias materias juntas.

Es util para revisar una vision global del curso o comparar cargas entre materias.

## Guardado y sincronizacion

La app guarda una copia local en el navegador y tambien sincroniza con GitHub cuando tienes permiso de escritura.

En la practica:

- Si editas tu carpeta, los cambios se guardan automaticamente.
- Si estas en una carpeta ajena, la app la muestra en modo lectura.
- Si GitHub no esta disponible, puede usarse una copia local temporal.

Para asegurarte de que ves las ultimas novedades, entra conectado a GitHub y carga de nuevo la carpeta del docente.

## Exportar e importar

### Exportar materia

Descarga una materia como JSON.

Sirve como copia de seguridad o para compartir una programacion concreta.

### Importar materia

Permite cargar una materia desde un archivo JSON.

La materia importada pasa a tu carpeta si tienes permiso de escritura.

### Exportar o importar festivos

Permite guardar y recuperar tus festivos personales.

## Recomendaciones de uso

- Revisa primero las fechas de inicio, fin y evaluaciones.
- Ajusta las sesiones semanales antes de editar muchas unidades.
- Usa festivos para dias que no deben contar como clase.
- Usa fechas senaladas solo como marcas informativas.
- Comprueba el resumen por evaluaciones despues de hacer cambios grandes.
- Si una materia no cuadra, revisa sesiones extra negativas o festivos personalizados.

## Problemas frecuentes

### No aparecen mis materias nuevas

Puede que la copia local no este sincronizada con GitHub.

Solucion habitual: volver a conectar con GitHub y abrir de nuevo tu carpeta.

### Veo una carpeta en modo lectura

Probablemente no es tu carpeta o tu usuario de GitHub no tiene permiso de escritura sobre ella.

### El contador no coincide con lo esperado

Revisa:

- Fechas de inicio y fin.
- Sesiones semanales.
- Festivos globales.
- Festivos personales.
- Sesiones extra negativas.
- Fechas de evaluacion.

### Una fecha senalada no descuenta sesiones

Es normal. Las fechas senaladas solo son marcas visuales.

Para descontar sesiones hay que marcar ese dia como festivo.

## Archivos principales

```text
index.html
data/
docs/
README.md
```

- `index.html`: aplicacion principal.
- `data/`: materias, modelos, usuarios y festivos.
- `docs/`: documentacion interna del proyecto.
- `README.md`: esta guia de uso.

## Nota para mantenimiento

Este proyecto es una aplicacion web estatica. No necesita instalacion, servidor ni base de datos para abrirse.

La sincronizacion de datos se realiza mediante GitHub.

Para detalles de organizacion interna, consulta:

- `docs/ESTRUCTURA.md`
- `docs/ORGANIZACION.md`