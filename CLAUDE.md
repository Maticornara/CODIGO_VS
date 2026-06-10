# Solar Propiedades — Contexto del proyecto

Sitio web inmobiliario de **Solar Propiedades** (Antonella Gutiérrez · Neuquén, Patagonia Argentina).
La idea es entregárselo terminado a Antonella, que NO es técnica: ella administra propiedades desde un panel sin tocar código.

## Idioma
Hablame y comentá el código en **español** (argentino).

## Stack
- **HTML / CSS / JS puro**, sin frameworks ni build step. Todo vive en archivos `.html`.
- **CMS:** Decap CMS (ex Netlify CMS) con **Netlify Identity + Git Gateway**. Antonella crea/edita propiedades desde el panel; cada cambio se guarda como `.md` en el repo de GitHub.
- Las propiedades son archivos YAML en `content/propiedades/*.md`.

## URLs
- **Sitio público:** codigo-vs.vercel.app (Vercel, sin límite de deploys)
- **Admin / CMS:** sunny-blancmange-47869e.netlify.app/admin/ (Netlify)
- **Repo:** github.com/Maticornara/CODIGO_VS (rama de trabajo y producción: `main`)

## Deploy ⚠️ IMPORTANTE
- **Producción se despliega en VERCEL al hacer push a `main`.** Para publicar: commit + push a `main`.
- **Los builds de Netlify están PAUSADOS** a propósito (para no gastar créditos). Netlify se usa SOLO como Identity provider + Git Gateway del CMS. NO reactivar los builds de Netlify.
- Hacer commit/push solo cuando el usuario lo pida.

## Archivos clave
- `index.html` — sitio principal (incluye todo el hero animado).
- `propiedades/index.html` — hace doble función: listado (`/propiedades`) con búsqueda/filtros, y página individual (`/propiedades/[slug]`). Detecta el modo por la URL.
- `admin/index.html` + `admin/config.yml` — panel Decap CMS.
- `content/propiedades/*.md` — propiedades en YAML.
- `fotos/` — logos e imágenes (`LOGO.PNG` = pin; `ISOLOGO_VECTOR.svg` = wordmark "SOLAR PROPIEDADES").

## Datos
- **WhatsApp:** 5492994029148
- **Paleta:** verde `#36512B` / verde oscuro `#1e2e17` / fondo hero `#1a2a10` · acento naranja `#EE7A13` · crema `#FFFCEB`.
- **Tipografías:** Archivo (cuerpo) + Archivo Black (display).

## Hero animado (index.html) — cómo funciona
Todo está en un IIFE dentro de `index.html`. Hay 4 ilustraciones line-art que rotan sincronizadas con un typewriter ("El valor justo de tu Casa./Departamento./Terreno./Propiedad.").

- Cada ilustración es un `<svg class="illus-svg" id="illus-0..3">` con grupos:
  - `.ill-stroke` → contornos que se **dibujan** (animación de trazo, Web Animations API).
  - `.ill-fill` → caras que se **rellenan** con el gradiente `#faceGrad` (crema→naranja sutil), vía `fill-opacity`.
  - `.ill-grow` → elementos que **crecen/brotan** (scaleY desde la base, con rebote): árboles y carteles.
- Trazo: usa `pathLen()` (getTotalLength × escala del CTM, por `non-scaling-stroke`) + dasharray con gap grande + overshoot (evita "puntitos" al desdibujar). NO usar un DASH fijo.
- **Escenas:**
  - **Casa (0):** chimenea + humo (`.smoke`) + caminito + árboles que crecen.
  - **Edificio (1):** ventanas que se encienden (`.ill-lights`, crema) + destello/glint en las ventanas + auto y canteros (árboles) abajo.
  - **Terreno (2):** árboles + cartel (plop) + pájaros (`.bird`) que cruzan.
  - **Llave (3):** levita + sombra + destello + una **mano** que entra desde la derecha y la agarra (con recoil).
- Cada ilustración tiene su `viewBox` y, si necesita más lugar, una caja propia (`#illus-N { max-width/height }`) y/o un `transform` de posición. Posicionar suele requerir iteración con capturas del usuario.

## Ilustraciones — flujo de trabajo
- Las ilustraciones las **dibuja y exporta el usuario en Illustrator** (line-art), con grupos `llenos` (rellenos) y `strokes` (contornos). NO dibujarlas a mano en código (desentonan con las pro).
- Los SVG fuente quedan en `ILUSTRACIONES/` (carpeta de trabajo, **ignorada por git** — el sitio no la usa porque los SVG van inline en el HTML).
- `ILUSTRACIONES/_extract.py` extrae los vectores del export de Illustrator, calcula viewBox y genera snippets para pegar en `index.html`.

## Cómo correr en local
Es HTML puro, sin build. Servidor local: `python -m http.server 3000` (o `npx serve .`).

## Preferencias del usuario (Mati) y cómo le gusta trabajar
- **Itera con capturas.** Trabaja muy visual: pide cambios, manda screenshots y se afina de a poco. Cuando algo de posición/escala no se puede "ver", asumir que va a necesitar varias vueltas con capturas. No dar por cerrado sin su OK visual.
- **Idioma español argentino**, tono cercano. Usa expresiones como "mandale", "dale", "está re lindo", "joya", "hermoso".
- **Nada se superpone.** Regla fuerte: ningún elemento debe pisarse con otro (ni árbol con árbol, ni con el edificio, ni el auto). Lo repitió varias veces.
- **Le importa el detalle y la "magia".** Pidió explícitamente "darle más magia", "que destaque", "que se vea un poco más lindo", "agregarle capas y poderes". Valora micro-animaciones y vida en las ilustraciones.

## Decisiones e intenciones de diseño que marcó (su "voz")
- **Tipografía hero más grande** y que cada frase respete **2 líneas** (nunca 3). Palabras del typewriter con **mayúscula inicial** y en **naranja de acento** (Casa./Departamento./Terreno./Propiedad.).
- **Wordmark "SOLAR PROPIEDADES" nunca negro ni blanco puro:** sobre fondo oscuro → crema; sobre crema → verde. (Le molesta el negro y el blanco puro.)
- **Splash:** logo + wordmark en horizontal (igual que el nav), con un "truco" de degradé: glow naranja detrás para que el verde del pin contraste. No le gustaba el verde plano.
- **Relleno de las ilustraciones:** crema con un **degradé sutil hacia naranja** para dar profundidad (no plano). Lo aprobó fuerte ("quedó hermoso").
- **Desdibujado parejo, sin "puntitos".** Le molestaban los restos de trazo al desdibujar; quedó resuelto con gap grande + overshoot. El timing tiene que ser fluido y coordinado con el typewriter.
- **Árboles y cartel: efecto "plop"** — que **crezcan/broten desde la base** con un rebotecito (no que se dibujen). Y que **crezcan, pero NO se "descrezcan"**: al salir se **desdibujan como todo** (no se encogen). En el terreno, el plop del cartel va **antes** del fill.
- **Humo de la casa:** que se mueva **siempre** y se vaya en **fade-out** (no que se congele). Estilo "bocanadas que suben".
- **Pájaros:** silueta en **"V" con perspectiva** (ala izquierda más larga), **pico pronunciado**, **aleteo lento** (~1 por segundo, no achatado/pivote), **vuelo corto** que entra y sale con transparencia. Solo en el **terreno**.
- **Llave:** **levita** suave, con **sombra desenfocada** debajo, **destello/glint** bien **blanco** y **giro sutil**. La **mano entra desde la derecha y la agarra**, con el **brazo cortándose por el viewport** (no se ve el codo). Al agarrar, un **recoil de movimiento** (se va un toque a la izquierda y vuelve, "como mostrando la llave"), NO un scale. La mano al salir **se desdibuja** (no se va).
- **Edificio:** **ventanas que se encienden** en crema (no le gustaba el amarillo cálido), y **destello en las ventanas** que ocurre **después** de que prenden las luces (las ventanas son crema FFFCEB para que el glint blanco contraste). Auto + canteros (árboles en línea, tipo vereda) abajo; los canteros también hacen "plop".

## Notas de memoria
- Este `CLAUDE.md` es la "memoria" del proyecto: se lee automáticamente en cada chat del agente en esta carpeta. Mantenerlo actualizado con decisiones y preferencias nuevas.
