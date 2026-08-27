# El Tablero de la Respuesta

Herramienta web de la actividad de análisis del **Taller de Resultados Preliminares** ·
Evaluación externa de la respuesta en emergencia · Proyecto COFM2308 (NRC Colombia).
Diseño de la actividad: CliO Consulting.

Una sola página, sin instalación y sin cuentas. Cada dispositivo procesa lo suyo en el
navegador y lo sincroniza con una sala compartida; si no hay conexión, la actividad sigue
funcionando con códigos que se pegan en el chat de la reunión.

## La sala compartida

La constante `DB` en `index.html` apunta a una base Realtime Database de Firebase
(`https://nrcevexem-default-rtdb.firebaseio.com`) y `SALA_POR_DEFECTO` fija la sala
`cofm2308-k7rq`. Todo cuelga de `/salas/<sala>`:

| Ruta | Qué guarda |
|---|---|
| `/salas/<sala>/votos/<código>` | `{t: territorio, v: doce letras}` — un voto individual, sin nombre |
| `/salas/<sala>/cartas/<P·N·C>` | las seis cartas de la mesa de ese territorio |

Se llega por REST puro (`fetch` para escribir, `EventSource` para escuchar en vivo): sin
SDK, sin dependencias y sin build. Si la base no responde, la app marca «sin conexión» y
las pestañas de códigos vuelven a aparecer.

Para usar otra sala —una prueba, o un segundo taller— basta agregar el parámetro:
`…/NRCevextemergencias/?sala=prueba-2026`. Las salas no se ven entre sí.

> Los datos de la sala quedan en la base hasta que se borren a mano desde la consola de
> Firebase. No hay nombres ni datos personales: los códigos llevan territorio y doce
> letras. Conviene borrar el nodo `/salas/<sala>` cuando el informe esté cerrado.

---

## Publicarla en GitHub Pages

```bash
cd tablero-app
git init
git add .
git commit -m "Tablero de la Respuesta · actividad COFM2308"
git branch -M main
git remote add origin https://github.com/<usuario>/<repositorio>.git
git push -u origin main
```

Después, en el repositorio: **Settings → Pages → Source: Deploy from a branch →
Branch: `main` / `(root)` → Save**. En uno o dos minutos queda en

```
https://<usuario>.github.io/<repositorio>/
```

Ese es el enlace que se comparte por el chat al empezar el taller.

> **Repositorio público o privado.** Con GitHub Free, Pages solo funciona en repositorios
> públicos. Eso significa que cualquiera con el enlace puede abrir la página, y que el
> código es visible. No hay datos personales ni información del proyecto dentro: la página
> está vacía hasta que alguien la usa. La única pieza sensible es el sobre cerrado — ver
> abajo.

## El sobre cerrado

La lectura provisional del equipo evaluador **no viene incluida** en este repositorio,
justamente para que no se pueda leer antes del taller. La constante `SOBRE_B64` en
`index.html` está vacía y, mientras lo esté, la pestaña «El sobre» pide pegarlo.

El día de la sesión, quien facilita abre la pestaña **El sobre**, pega el bloque que le
pasó el equipo evaluador y presiona **Cargar el sobre**. Queda guardado en ese navegador.

Si prefiere dejarlo incrustado —solo tiene sentido en un repositorio privado o en un
despliegue que no se comparta—, pegue el JSON codificado en base64 dentro de las comillas
de `SOBRE_B64`. El formato es:

```json
{ "Pertinencia|Putumayo": { "c": "A", "r": "La comunidad valida ex post una oferta ya definida." } }
```

Con `c` ∈ `V` · `A` · `R` · `SD`, y una entrada por cada criterio × territorio.

## Cómo se usa el día del taller

| Momento | Quién | Qué hace |
|---|---|---|
| 0–5 | Todos | Abren el enlace. Quien facilita anuncia las mesas y sus territorios. |
| 5–12 | Cada persona | **Voy a votar** → territorio → doce casillas → **Enviar**. Llega solo. |
| 12–28 | Un dispositivo por mesa | **Soy una mesa** → territorio → los votos de su gente aparecen ya cargados → seis cartas. Se guardan solas. |
| 28–40 | Quien facilita | **Consolidar**: el mapa se arma en vivo, sin pegar nada. |
| 40–50 | Quien facilita | **Abrir el sobre** y pestaña **Casillas calientes**. |
| 50–60 | Quien facilita | **Descargar CSV** para el informe. |

Si la conexión falla, reaparecen los códigos: cada persona copia el suyo al chat, la mesa
copia su bloque y quien facilita lo pega todo en **Leer lo pegado**. Se puede pegar varias
veces: lo repetido no se duplica y los códigos mal copiados se avisan.

## Estructura

Un único archivo: `index.html`. Sin dependencias, sin build, sin paquetes. Las únicas
peticiones externas son la hoja de estilos de Google Fonts (Jost e IBM Plex Sans) —si no
carga, la página usa las tipografías del sistema y se ve igual de bien— y la base de la
sala.

Los logotipos están incrustados como `data:` URI, así que la página funciona completa
sin conexión una vez abierta.

## Datos

No se recoge ningún dato personal. Los códigos de la Ronda 1 contienen únicamente el
territorio y doce letras: no llevan nombre. Lo que cada persona escribe queda en el
`localStorage` de su propio navegador y, si hay conexión, también en la sala compartida
—siempre sin identificar a quien lo escribió—. En el informe las intervenciones se citan
por rol y territorio, nunca por nombre. Cerrado el informe, conviene borrar el nodo
`/salas/<sala>` desde la consola de Firebase.
