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

---

# Sesión de trabajo — secciones, propiedades, admin y contacto

> Todo lo de abajo se agregó en una sesión larga. **Importante:** muchos cambios todavía requieren **deploy** para verse (ver "Deploy y datos en vivo").

## Deploy y datos en vivo ⚠️ (clave para no confundirse)
- Las propiedades se leen **EN VIVO desde la GitHub API** (`Maticornara/CODIGO_VS`, rama `main`, carpeta `content/propiedades`). Por eso **un cambio local en un `.md` NO se ve hasta hacer push**. Si algo nuevo (ej. características/chips) "no aparece" en local, es porque los datos viven en GitHub.
- **Sitio público → Vercel** (push a `main`). **Admin → Netlify** (builds PAUSADOS; hay que hacer **deploy manual** para ver cambios de `admin/`).

## Routing de fichas (importante)
- La ficha individual ahora se abre con **query param**: `/propiedades/?p=slug`. Funciona en **cualquier server** (localhost incluido), sin depender del rewrite de Vercel. Se mantiene compatibilidad con `/propiedades/slug` (path) por las dudas. Las cards (home y listado) linkean a `?p=`.

## Sección "Lo que nos define" (Valores)
- Animaciones al hacer scroll con **IntersectionObserver** (NO scroll listeners): líneas divisorias verticales que crecen, y columnas con fade+slide escalonado. Threshold 0.7 + pausa de 0.5s al entrar (si no, no se llega a ver).
- La **barra de navegación se vuelve transparente** sobre esta sección (clase `over-dark`), como en el hero. Y la sección tiene el **mismo degradé animado (blobs) del hero** (capa `.hero-pattern` reutilizada, recortada por una capa interna para no cortar dropdowns).

## Sección "Sobre mí"
- Foto real: `fotos/foto_antonella.jpeg` (3/4, sin recuadro naranja desfasado).
- Secuencia con IntersectionObserver: espera ~1s al entrar → foto + eyebrow fade → typewriter del título "Hola, soy Antonella Gutiérrez" → bio → badges escalonados.
- **Typewriter SIN layout shift:** todo el texto ya está renderizado (cada carácter en un `<span class="ch">` transparente) y se "revela" cambiando color, no insertando texto. El bio mantiene `<strong>` (negrita) por carácter. El typing total dura ~3s (revelado por `requestAnimationFrame`, duración exacta).

## FAQ
- Se abre con **click/tap** (antes era hover), clase `.open`. Soporta teclado.
- Hover sobre la pregunta: **reborde naranja de 3px** que aparece/desaparece por **opacidad** (fade 0.5s), solo en items cerrados (`:not(.open)`).

## Navegación
- Orden: **Inicio · Propiedades▾ · Servicios · Nuestra esencia · Sobre mí · FAQ · Contacto▾ · Consultar**.
- **Propiedades** va destacado (naranja + bold, clase `nav-destacado`).
- **Contacto▾** es una bandeja desplegable con WhatsApp / Email / Instagram (por ahora solo en el nav del home).
- Favicon: `fotos/LOGO.PNG`.

## Sistema tipográfico (desktop, consistente en las 3 páginas)
- Referencias "buenas" del usuario: hero-sub (~16px) y bio de Sobre mí (15px).
- **Cuerpo / desarrollo = 16px** (descripciones, FAQ, bio, datos de ficha). **Labels mayúscula = 11px** (eyebrows, ZONA, etc.). Subtítulos/cards 17px, pregunta FAQ 20px. Precios/m² grandes sin cambios. Se subió el contraste de varios textos apagados.

## Propiedades — listado y ficha
- **Buscadores (home + listado) iguales (estilo barra blanca)** y filtran igual. El header del listado quedó en **claro/crema** (no verde).
- **Precio y m² son inputs libres** (no dropdowns). El valor que pone el usuario es una **referencia ±30%** (aparecen propiedades alrededor, no un tope). Placeholder "Sin especificar" en itálica. Label "Precio U$S".
- **Búsqueda sin tildes ni mayúsculas:** función `normTxt()` (minúsculas + saca acentos). Se usa en ubicación y características.
- **Listado:** las **destacadas van al FINAL** (abajo) y muestran badge **"★ Destacada"** (naranja, arriba-der).
- **Ficha (layout desktop):** contenedor **1280px**. Arriba el **título**. Después **galería a la izquierda + card de precio/contacto a la derecha** (`.prop-top`, la card NO se estira: su alto se acomoda al contenido). Debajo (`.prop-body`, max-width 820px para que la descripción tenga aire): datos (Superficie grande arriba, **Zona / Localidad** separadas en un renglón — si solo hay `ubicacion` viejo, se separa por la coma), descripción, reel. Mapa abajo.
- **Galería:** foto principal **16:9** con relleno **verde** si la foto es vertical. **Flechas naranjas** (SVG, con hover scale) + **teclado ← →**. **Fade** al cambiar de foto. Miniaturas con hover scale. Lightbox al click.
- **Mapa:** acepta coordenadas DMS o decimal; `dmsToDecimal()` las convierte para el embed de Google Maps.
- **Sin "Moneda"** en los datos (ya está implícita en el precio).

## Características (hashtags) — feature completo
- Campo nuevo en el admin: **"Características"** = lista de **texto libre** (Antonella agrega: Cochera, Pileta, Vista al lago…).
- Se muestran como **#hashtags**: en la **card verde de la ficha** (clickeables → llevan al listado filtrado, `?tag=`) y en las **cards** (no clickeables porque la card ya es un link).
- **Búsqueda por características** en home y listado: una **fila de chips** que se arma **sola** con las etiquetas existentes. Filtro **OR** (muestra las que tengan **alguna**). Decidido: texto libre (no lista cerrada). La normalización (`normTxt`) agrupa por minúsculas/sin acentos, pero sinónimos distintos ("Cochera" vs "Garage") quedan como chips distintos.

## Contacto
- **Formulario → WhatsApp:** al enviar, abre WhatsApp con los datos (nombre, tel, email, consulta). (Se probó mailto y no es confiable en desktop sin cliente de correo; por eso WhatsApp.)
- Además, visibles como otros medios: **Email** y **Instagram** (`@solar_propiedades` → https://www.instagram.com/solar_propiedades/).
- **Email temporal de prueba: `matyy.cornara@gmail.com`** (reemplazar por el de la inmobiliaria; aparece en el form del home, en `var DEST`, y en la ficha 2 veces).
- Los mensajes (WhatsApp y mail) dicen explícito **"desde la Página web de Solar Propiedades"**. En la ficha, el mensaje sigue el ejemplo de Antonella: *"¡Hola! Estoy interesado en la propiedad que vi en su web: (TÍTULO) - (CÓDIGO). ¿Me podrían brindar más información? Aquí está el enlace del inmueble: (LINK)"*.

## Admin (Decap CMS)
- **Verde de marca `#36512B`** en header y acentos (antes #1F3216).
- Botón corregido a **"Nueva propiedad"** (era "Nuevo Propiedad").
- **Logo** en login/header: `logo_url: /fotos/LOGO.PNG`.
- **Vista previa en vivo** (`CMS.registerPreviewTemplate` + `registerPreviewStyle`): muestra la propiedad con el estilo real del sitio (foto 16:9 verde, badges, título Archivo Black, superficie, zona/localidad, precio, #hashtags, descripción) mientras Antonella edita.
- Límite honesto: Decap es la "cáscara" de React; reestilizar componentes internos profundos es frágil (clases ofuscadas). Lo robusto: branding (logo/colores/fuente) + preview custom.

## Pendientes (cuando se retome)
- **Mobile:** menú hamburguesa (hoy en celular NO hay menú) + repaso mobile completo (home, listado, ficha).
- **Servicios** (sección nunca revisada), footer.
- Reemplazar email de prueba por el real.
- Open Graph / SEO para compartir lindo por WhatsApp/redes.
- Opcional: características estructuradas (dormitorios, baños) como datos, no como tags.

---

# Sesión — Rediseño ficha (Remax) + GRAN FILTRO + campos nuevos del admin

## Modelo de datos / Admin (campos nuevos en `admin/config.yml`)
Además de los ya existentes (codigo, titulo, tipo, precio, moneda, zona, localidad, coordenadas, descripcion, fotos, link_reel, destacada), se agregaron:
- **tipo:** se sumó **Departamento** (opciones: Lote, Casa, Departamento, Alquiler permanente, Alquiler turístico).
- **metros_cuadrados** (= "Superficie total"), **metros_cubiertos**, **metros_semicubiertos**, **metros_terreno**.
- **ambientes** (número) + **ambientes_detalle** (lista de nombres: Living, Cocina…; texto libre con sugerencias en el `hint`).
- **dormitorios**, **banos**, **antiguedad** (años).
- **servicios** (lista texto libre, sugerencias en `hint`: Luz, Gas, Agua, Cloacas…).
- **caracteristicas** (lista texto libre, ya existía; ahora SIN # en las cards).
- ⚠️ Decap no tiene autocompletado real en listas → las "sugerencias" son el `hint`. Para sugerencias clickeables habría que usar checkboxes fijos.

## Ficha de propiedad (`propiedades/index.html`) — estructura tipo Remax (vertical, todo apilado)
Orden: **1)** Hero (breadcrumb + badge + título + ubicación 📍) **2)** Galería **3)** Card de datos (precio + íconos) **4)** Descripción **5)** Detalles **6)** Reel **7)** Mapa **8)** Similares. Todo en `.prop-page` (max-width **1200px**).
- **Galería:** foto **16:9** (siempre) a la izquierda + **carrusel vertical de thumbnails a la derecha** (columna 240px) con **scroll local sin barra**. Degradés arriba/abajo que aparecen/desaparecen por opacity según haya más fotos (clases `show-top`/`show-bottom` en `.gallery-thumbs`, vía `updateThumbFade()`). En mobile el carrusel pasa a fila horizontal. El hero usa `.prop-hero { min-height: calc(100vh - nav) }` para que la card de precio no asome en la 1ª pantalla.
- **Robustez de imágenes:** `keepValidImages()` precarga y descarta fotos rotas (nunca se ve un recuadro roto); en cards, `imgFallback()` reemplaza por placeholder.
- **Card de datos** (`.prop-datacard`, borde naranja): código + precio + grid de stats con íconos SVG (m² totales/cubiertos/terreno, ambientes, dormitorios, baños, antigüedad). Contacto (WhatsApp/Email) debajo.
- **Sección "Detalles de la propiedad"** (recuadro solo-borde, igual que Descripción): subsecciones **Superficies y datos** (label: valor), **Servicios** (✓), **Ambientes** (✓), **Características** (✓, clickeables → `?tag=`). Cada lista aparece solo si hay datos.
- **Propiedades similares:** `renderSimilares(p)` trae todas, puntúa (mismo **tipo** pesa fuerte + precio parecido misma moneda + características compartidas), deduplica por código, muestra top 3 (más parecida a la izquierda). Aparece vacía si solo hay una propiedad.

## GRAN FILTRO — en home (`index.html`) y listado (`propiedades/index.html`)
**Misma barra en las dos pages** (ancho **1280px = --max-w**, margen lateral 56px; en el listado el hero/cards quedan en 1100 pero la barra es 1280).
- **Filtros principales (barra):** Tipo (dropdown) · Ubicación (input con **autocompletado** que sugiere localidades/zonas existentes; igual permite texto libre) · **Precio** (dropdown con selector **U$S / $** + inputs **Desde/Hasta**) · **Superficie m²** (dropdown con **Desde/Hasta**).
- **"Más filtros"** (botón que despliega `.more-filters`): Ambientes/Dormitorios/Baños (mín., selects), m² cubiertos/terreno (mín.), Antigüedad (máx.), y **Servicios** + **Características** como **botones colapsables** (`.mf-chips-toggle`; no aparecen de una).
- **Reglas clave (decisiones del usuario):**
  - **Nada es obligatorio:** input vacío = NO filtra. Las etiquetas de Precio/m² quedan **vacías** (no dicen "Cualquiera").
  - **Moneda sin toggle global:** el selector U$S/$ vive dentro del filtro de Precio; solo define en qué moneda se interpreta el rango. Sin cotización/conversión (filtra por `moneda` que coincida).
  - **El filtrado se ejecuta SOLO al tocar "Buscar"** (o Enter), NO en vivo. Los filtros **no se borran** al buscar (solo con "Limpiar filtros").
- **Listado:** filtra sobre `allProps` y **lee parámetros de la URL** (`tipo, ubic, cur, pmin, pmax, mmin, mmax, amb, dorm, banos, cub, ter, ant, tags, serv`). Si vienen chips por URL, abre "Más filtros" y el grupo correspondiente.
- **Home:** la barra **no filtra ahí** (las destacadas son vidriera). "Buscar" (`irABusqueda`) **junta todo y redirige a `/propiedades?...`**. Las destacadas siguen con `mostrarDestacadas()`.
- Se quitó el slider de dos perillas (el usuario lo encontró limitante) → inputs Desde/Hasta.

## Cards (home + listado)
- Muestran las **características principales como píldoras** (`60 m²` · `3 amb.` · `2 dorm.` · `1 baño`), **sin #** (`cardStats(p)` / `statsStr`). Ubicación arriba en texto plano.

## Home (`index.html`) — ajustes varios
- **Nav blanca/sólida sobre la sección de esencia** (antes se transparentaba): `navDarkSections = []`.
- Bloques 1/2/3 de "Lo que nos define" **subidos** (menos margen del título y padding de los items).
- Botón **"Ver todas"** de destacadas **destacado** (relleno naranja, `.btn-ver-todas-dest`).

## Deploy de esta sesión
- Push a `main` → Vercel (sitio) ✅. Para el **admin** (campos nuevos) hace falta **deploy de Netlify** (estaba pausado; el usuario lo habilitó para publicar).

---

# Sesión — Admin simplificado, favicon, listas como texto, y presupuesto de deploys

## ⚠️ PRESUPUESTO DE DEPLOYS DE NETLIFY (importante)
- Cuenta **nueva** de Netlify con **POCOS deploys disponibles** (quedan ~2). **Batchear** todos los cambios de `admin/` y avisar al usuario para que haga UN solo "Trigger deploy".
- **Las ediciones de propiedades de Antonella NO gastan deploys de Netlify** (commitean a GitHub → publica Vercel). Netlify solo se gasta cuando cambia código de `admin/` (config o index) y se hace deploy manual. Netlify queda **pausado** a propósito.

## Favicon
- `LOGO.PNG` como favicon en `index.html`, `propiedades/index.html` y `admin/index.html` (ruta absoluta `/fotos/LOGO.PNG`).

## Listas (características / servicios / ambientes) → TEXTO LIBRE
- Problema: el widget `list` de Decap cortaba en "tags" (confuso) y dejaba ítems vacíos (`- ""` → en el sitio salía `# ""`).
- Solución: en `admin/config.yml` esos 3 campos son **`widget: text`** (cuadro de texto; se escribe con coma y espacios, ej. "Gas natural, Luz").
- En el sitio se parsea con **`toList(v)`** (en `propiedades/index.html` e `index.html`): acepta string ("a, b") o lista YAML vieja, separa por coma/salto de línea y **descarta vacíos**. Está aplicado en TODOS los consumidores (ficha Detalles, filtros, chips, similares, home).
- **`parseYAML`** (ambos archivos): ignora ítems de lista vacíos y saca comillas → no más `# ""`.
- Datos migrados a texto en `content/propiedades/lo-615.md` (CA-001) y `lo-615-1.md` (ALP-001).
- ⚠️ Tras deployar el nuevo `config.yml`, abrir cada propiedad una vez para que esos campos queden en formato texto (si quedó alguno en lista vieja, se ve raro hasta re-guardar).

## Fotos en el admin
- `fotos` → **`widget: image, multiple: true`** (se eligen/arrastran varias). Sigue guardando lista de paths; el sitio lee `p.fotos` igual.
- `admin/index.html` fuerza `multiple` en los `input[type=file]` (para elegir varias en el explorador).
- ⚠️ Las fotos recién subidas tardan **1-2 min en verse en el sitio** porque el archivo lo sirve **Vercel** (rebuild). El `.md` se lee en vivo pero la imagen necesita el build. `keepValidImages()` oculta las que aún no están listas.

## Admin (`admin/index.html`) — branding/UX por JS (Decap no se reestiliza fácil)
- Oculta pestaña **"Medios"** y botón **"Añadir rápido"** (matcheo ESPECÍFICO `añadir rápido`/`quick add` para NO ocultar otros botones con "Añadir").
- Botón **"Nueva Propiedad"** (corrige "Nuevo"→"Nueva") y **"Subir nuevo"** en **naranja**. "Confirmar selección" y el resto, sin tocar.
- **Preview en vivo** reescrito: ubicación (zona · localidad), stats (m² total/cubiertos/terreno, ambientes, dorm, baños, antigüedad), y grupos Servicios/Ambientes/Características. Helper `toArr()` parsea string o lista Immutable.

## Ficha
- Las **características** dentro de la propiedad ya **NO son clickeables al filtro** (se descartó esa idea por ahora); quedan como texto con ✓.

## Barra de búsqueda
- Mismo ancho en home y listado: ambas `max-width: var(--max-w)` (1280px).

---

# Sesión — Traducción ES/EN (toggle propio, SIN Google Translate)

## Por qué NO Google Translate
- Google Translate rompía las animaciones de tipografía (typewriter del **hero** y de **Sobre mí**) porque reemplaza nodos de texto en vivo. Se descartó. Se hizo un **toggle propio ES/EN**.

## Cómo funciona el i18n (en `index.html` y `propiedades/index.html`)
- **Estado:** `window.SOLAR_LANG` se define en el `<head>` (antes de cualquier animación) leyendo `localStorage['solar-lang']` (default `'es'`). También setea `document.documentElement.lang`.
- **Helper para JS:** `window.T(es, en)` devuelve el string según el idioma. Se usa en TODO el texto generado por JS (cards, stats de la ficha, labels de detalle, status de resultados, navBackLabel, etc.).
- **Texto estático en HTML:** atributo **`data-en`** (se swapea el `innerHTML`) y **`data-en-ph`** (se swapea el `placeholder` de inputs). `solarApplyLang()` guarda el original en `data-es`/`data-es-ph` la primera vez y luego intercambia según idioma.
- **Toggle:** botón `#langToggle` (clase `.nav-lang`, dice "EN" o "ES") → `solarToggleLang()` guarda el próximo idioma en localStorage y hace **`location.reload()`**. **Recargar es a propósito:** así el hero y Sobre mí se re-inicializan limpios en el idioma elegido y no se ven cortes/saltos de las animaciones.
- **Texto animado:** el hero usa `window.__setHeroLang(lang)` que reasigna `BASE`/`WORDS` del typewriter; el título de Sobre mí lee el atributo `data-text-en` (además de `data-text`). La bio/badges usan `data-en` (se swapean antes de que corra la animación de reveal).
- **Fallback seguro:** lo que no tenga `data-en`/`T()` queda en español (no rompe nada; permite traducir de a poco).

## Qué se traduce y qué NO
- **Se traduce toda la UI:** nav, hero, buscador + "Más filtros", secciones (Servicios, esencia, Sobre mí, FAQ, Contacto, footer), y en `propiedades/`: hero del listado, barra de búsqueda, labels de la ficha (Descripción, Detalles, Ubicación, Similares), botones de contacto, breadcrumb, stats, etc.
- **Los DATOS de cada propiedad SÍ se traducen automáticamente** (ver abajo). Antonella los carga **solo en español**; no carga nada dos veces.

## Traducción AUTOMÁTICA de los datos de las propiedades (MyMemory)
- **Por qué automática:** se descartó pedirle a Antonella cargar todo dos veces (ES/EN). En su lugar, los datos se traducen al vuelo SOLO cuando el visitante pone la página en inglés.
- **Motor:** **MyMemory** (`api.mymemory.translated.net`), **gratis y sin API key**, con **CORS abierto** → se llama directo desde el navegador (no hace falta función serverless). Límite ~5.000 palabras/día por IP (suficiente con el caché). Si algún día la calidad no alcanza, se migra a Google Cloud Translation cambiando solo la función `tr()`.
- **Módulo `window.solarTr`** (definido en el `<head>` de `index.html` y `propiedades/index.html`, idéntico):
  - `tr(text)` → Promise con la traducción (o el original si falla / si idioma ≠ en).
  - `el(node)` → traduce el `textContent` de un elemento (campos cortos: título, ubicación).
  - `html(container)` → traduce los **nodos de texto** dentro de un contenedor **preservando el HTML** (para la descripción con `<strong>`/`<ul>` y las listas con ✓).
  - `cards(scope)` → traduce `.prop-card-titulo` y `.prop-card-ubic` de las cards dentro de `scope`.
  - `window.solarTrTipo(t)` → **mapa fijo** de tipos (Lote→Lot, Casa→House, Departamento→Apartment, etc.). No se manda a MyMemory porque "Lote" lo traduce mal.
- **Solo on-demand + caché:** solo traduce cuando `SOLAR_LANG==='en'` y SOLO lo que el visitante está mirando (la propiedad/cards en pantalla, no todo el catálogo). Cada traducción se **cachea en `localStorage['solar-tr-cache']`** → la 2ª vez es instantánea y no gasta cuota. Se ve el español un instante y se reemplaza (sin saltos).
- **Dónde se engancha:** cards del listado y similares (`solarTr.cards`), y en la ficha: `propTitulo` (`el`), `propTipo` (mapa), `propUbicTxt` / `propDesc` / listas de Servicios/Ambientes/Características (`html`). En el home: cards de destacadas (`solarTr.cards`) + tipo (mapa). Badge "★ Destacada"→"★ Featured" con `T()`.

## Deploy
- Todo es del sitio público → **push a `main` → Vercel**. NO toca el admin, así que **no gasta deploys de Netlify**.
