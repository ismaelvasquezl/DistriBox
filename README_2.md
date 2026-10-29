# DistriBox

**Distribución y uso de box clínicos · APS Chile**

Herramienta web de archivo único para gestionar la distribución diaria de box clínicos en
establecimientos de la red asistencial chilena —CESFAM, CECOSF, COSAM, PSR, SAR, SAPU y otros— y
para medir la capacidad instalada que se pierde cuando un box queda vacío y nadie lo ocupa.

No requiere instalación, servidor ni base de datos. Se abre con doble clic o desde GitHub Pages, y
todos los datos quedan en el dispositivo de quien la usa.

---

## Contenido del repositorio

| Archivo | Qué es |
|---|---|
| `index.html` | **La aplicación.** Arranca vacía, con asistente de configuración y un botón de datos de ejemplo |
| `guia-de-uso.html` | Manual completo: configuración, ciclo diario, módulos, preguntas frecuentes y glosario |
| `presentacion.html` | Presentación de 13 láminas para mostrar a un equipo o una jefatura. Se exporta a PDF |
| `carrusel-linkedin.html` | Láminas 4:5 para publicar DistriBox como documento en redes |
| `.nojekyll` | Necesario para que GitHub Pages sirva el sitio tal cual |

Todos los archivos son autocontenidos: se abren con doble clic y no dependen unos de otros.

---

## El problema que resuelve

Una planilla de distribución dice quién debería estar en cada box. No dice qué pasó hoy.

Cuando un profesional se ausenta, su box queda libre y alguien tiene que decidir en minutos quién lo
usa. Esa decisión casi nunca se registra, así que al final del mes no hay forma de responder cuántas
horas de box se perdieron, por qué motivo, en qué franja horaria ni qué tan seguido se logró
recuperarlas.

DistriBox convierte ese flujo en un registro: cada ausencia libera automáticamente los box de esa
persona, cada liberación entra en una bandeja que exige una decisión —reasignar o descartar con
motivo— y cada decisión alimenta un indicador de **capacidad perdida**.

---

## Módulos

**Operación diaria**

| | Módulo | Para qué |
|---|---|---|
| 00 | Panel del día | Qué se programó, qué se cayó, qué se recuperó y cuánta capacidad se está perdiendo ahora |
| 01 | Jornada y novedades | Abrir la jornada y registrar ausencias, box fuera de servicio, reservas, liberaciones y presencias extra |
| 02 | Reasignación de box | La bandeja de box liberados, con candidatos sugeridos y su fundamento |
| 03 | Pizarra de box | Vista de mesón: todos los box en un bloque horario con su estado real |
| 04 | Vista semanal | Matriz box × bloque de la semana tipo |

**Configuración**

| | Módulo | Para qué |
|---|---|---|
| 05 | Establecimiento | Identificación, días, rejilla horaria y catálogos de estamentos y programas |
| 06 | Box clínicos | Catastro de espacios, con servicio afín y bloques en que opera cada uno |
| 07 | Recurso humano | Dotación y estamentos |
| 08 | Programación base | La semana tipo, editable celda por celda |

**Análisis**

| | Módulo | Para qué |
|---|---|---|
| 09 | Uso y ocupación | Estadística estructural de la programación |
| 10 | Capacidad perdida | Horas caídas, recuperadas y perdidas, por motivo, estamento, box y franja |
| 11 | Informe técnico | Reporte trazable del período, imprimible o guardable como PDF |

---

## Cómo se usa un día normal

1. **Abrir la jornada** al llegar (módulo 01).
2. **Registrar las ausencias** conocidas. Una ausencia libera de una sola vez todos los box de esa
   persona en el alcance indicado: jornada completa, sólo mañana, sólo tarde o bloques sueltos.
3. **Resolver la bandeja** (módulo 02). Para cada box liberado, DistriBox propone candidatos
   ordenados por disponibilidad real, afinidad del box con el estamento y el servicio, y uso previo
   del espacio. Si no hay a quién asignarlo, se descarta con un motivo: eso es lo que después
   explica la pérdida.
4. **Registrar lo que vaya pasando**: box que se cae por una falla, un reemplazo que llega, una
   reserva para un operativo.
5. **Cerrar la jornada** al terminar. Si quedan liberaciones sin decidir, la herramienta avisa.

Los días cerrados alimentan los módulos 10 y 11.

---

## Modelo de datos

Tres capas separadas, que cambian a ritmos distintos:

- **Configuración** — establecimiento, rejilla horaria, box, dotación, catálogos. Se toca rara vez.
- **Programación base** — la semana tipo, con fecha de vigencia. Se actualiza cuando cambia la
  dotación o la agenda.
- **Jornada** — un registro por fecha. No guarda un estado calculado: guarda la **lista ordenada de
  eventos** del día y recalcula el estado desde la programación base cada vez. Por eso deshacer es
  simplemente quitar un evento, y el historial nunca queda desincronizado de lo que se ve.

### Cómo se cuentan las horas

- **Capacidad programada**: suma de la duración de los bloques-box que tenían a alguien asignado.
  Los bloques institucionales y los bloques en que un box no opera no cuentan.
- **Caída**: todos los asignados a ese bloque-box están ausentes, o el box se liberó manualmente.
- **Recuperada**: se registró una reasignación sobre ese mismo bloque-box.
- **Perdida**: la liberación no se reasignó, se haya descartado con motivo o quede sin decidir.
- **Box fuera de servicio**: se informa aparte. Es pérdida de infraestructura, no de dotación.

---

## Persistencia

El archivo **`.json` del proyecto es el respaldo real**: contiene configuración, programación y todas
las jornadas. Se descarga y se carga desde la barra lateral.

La herramienta además autoguarda en el navegador (`localStorage`) para que no se pierda el día si
alguien recarga la página, pero ese autoguardado es por navegador y por equipo: se pierde al borrar
los datos del sitio o al cambiar de computador. Para trabajar entre varios equipos, el `.json` es el
que viaja.

---

## Empezar desde cero

El botón **«Vaciar y empezar de cero»** (módulo *Proyecto y datos*) permite elegir qué borrar:
programación, jornadas, catastro de box, dotación, identificación del establecimiento o catálogos.
Tras vaciar, la herramienta arranca en un asistente de cinco pasos.

Para armar un establecimiento nuevo sin datos reales existen **nombres simbólicos**: crear box en
lote (`Box 1`… `Box N`), funcionarios con nombre genérico y una rejilla horaria de ejemplo de
08:00 a 17:00 con pausa y colación. También hay un botón de **datos de ejemplo** que carga un CESFAM
ficticio con 8 box y 10 funcionarios para probar la herramienta.

---

## Cargar una planilla Excel

DistriBox lee planillas de distribución que no son tablas, sino varias mini-matrices «hora × día».
Localiza cada grilla por su celda **`Hora`** y toma el nombre del box de la celda inmediatamente
superior, de modo que tolera que las grillas estén en distintas posiciones, que se agreguen o quiten
box, y que cada box tenga su propia columna de horas.

Al cargar una planilla nueva se conservan los estamentos ya registrados, emparejando por nombre
normalizado.

El módulo *Proyecto y datos* incluye un **control de calidad** que recalcula en cada carga:
conflictos de simultaneidad, box con rejillas distintas, funcionarios sin estamento, entidades no
nominales, celdas con separadores ambiguos y box sin programación.

---

## Privacidad

Todo el procesamiento ocurre en el navegador. No hay servidor, no hay telemetría y ningún dato sale
del dispositivo.

Si va a publicar DistriBox en un sitio público, el módulo *Proyecto y datos* ofrece **«Descargar
index.html sin datos»**: genera una copia de la aplicación sin ninguna nómina incorporada, para que
quien la visite la configure desde cero y usted cargue su proyecto `.json` localmente.

El informe técnico y las exportaciones admiten anonimizar los nombres, conservando estamento,
servicio, box, horario y métricas.

DistriBox no contiene ni procesa información clínica ni de pacientes.

---

## Publicar en GitHub Pages

```bash
git add index.html .nojekyll README.md
git commit -m "DistriBox"
git push
```

Luego: **Settings → Pages → Source: Deploy from a branch → `main` / root**.

Queda en `https://<usuario>.github.io/<repositorio>/`.

También funciona abriendo `index.html` con doble clic, sin servidor.

---

## Dependencias

- [SheetJS](https://sheetjs.com) por CDN, sólo para leer archivos Excel. Si no carga, la aplicación
  avisa y sigue funcionando con normalidad; sólo queda inhabilitada la importación.
- Los gráficos son HTML y SVG nativos, sin librería.

---

## Tecnología

Un solo archivo `index.html` con HTML5, CSS3 y JavaScript vanilla. Sin framework, sin build, sin
servidor. Modo claro y oscuro, y diseño utilizable en computador, tablet y teléfono.
