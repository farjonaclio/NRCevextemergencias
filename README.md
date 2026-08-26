# El Tablero de la Respuesta

Herramienta web de la actividad de análisis del **Taller de Resultados Preliminares** ·
Evaluación externa de la respuesta en emergencia · Proyecto COFM2308 (NRC Colombia).
Diseño de la actividad: CliO Consulting.

Una sola página, sin servidor y sin cuentas. Todo se procesa en el navegador de cada
persona; nada viaja a ningún lado salvo lo que cada quien copia y pega en el chat de la
reunión.

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
| 5–12 | Cada persona | **Voy a votar** → territorio → doce casillas → copia su código y lo pega en el chat. |
| 12–28 | Un dispositivo por mesa | **Soy una mesa** → territorio → seis cartas → **Copiar las cartas** → pega en el chat. |
| 28–40 | Quien facilita | **Consolidar** → pega todo el chat → **Leer lo pegado** → proyecta el mapa. |
| 40–50 | Quien facilita | **Abrir el sobre** y pestaña **Casillas calientes**. |
| 50–60 | Quien facilita | **Descargar CSV** para el informe. |

Se puede pegar en el chat varias veces: lo repetido no se duplica y los códigos mal
copiados se avisan.

## Estructura

Un único archivo: `index.html`. Sin dependencias, sin build, sin paquetes. La única
petición externa es la hoja de estilos de Google Fonts (Jost e IBM Plex Sans); si no
carga, la página usa las tipografías del sistema y se ve igual de bien.

Los logotipos están incrustados como `data:` URI, así que la página funciona completa
sin conexión una vez abierta.

## Datos

No se recoge ningún dato personal. Los códigos de la Ronda 1 contienen únicamente el
territorio y doce letras: no llevan nombre. Lo que cada persona escribe queda en el
`localStorage` de su propio navegador y se puede borrar cerrando la sesión del sitio.
En el informe las intervenciones se citan por rol y territorio, nunca por nombre.
