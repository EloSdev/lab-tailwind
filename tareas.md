# Tareas: Tueste — landing del tostadero

> Fuente: brief.md, enunciado O1–O9 (`proyecto/enunciado.md`) y
> `verify-task/references/criterios-lab.md`. Se construyen en orden y cada una
> se verifica con `verify-task` antes de empezar la siguiente (comprobando
> también que no rompe las ya ✅).
> Estado: ⬜ pendiente · 🔄 en curso · ✅ verificada · ❌ falla

## T1 · Base y tokens — ✅
**Qué:** copiar `_plantilla-html/` (sin `node_modules`) a `maquetacion_tueste/`,
`name` y `<title>` propios, `npm install`, fuentes con `@fontsource-variable`,
`@theme` con la paleta «Pegatina pop», sombra dura y fuentes; esqueleto
`header` / `main` (con un `h1` provisional) / `footer`.
**Depende de:** —
**Criterios:**
- C1.1 `package.json` con `name: "maquetacion-tueste"` y `<title>` con sentido (contiene «Tueste») — leer ficheros + `check-estatico` — O1
- C1.2 `@theme` tiene `--color-espresso-50/500/900` (≥ 3 tonos), `--color-tomate-*` (acento), `--color-crema` y `--color-menta` — leer `src/style.css` — O2, brief §Dirección visual
- C1.3 `@fontsource-variable/bricolage-grotesque` y `@fontsource-variable/dm-sans` en `package.json`, importadas en `src/main.js` antes del CSS; `--font-display` y `--font-sans` en `@theme` — leer ficheros — O2
- C1.4 Token de sombra dura sin difuminado en `@theme` (p. ej. `--shadow-pegatina: 4px 4px 0 …`) — leer `src/style.css` — brief §Dirección visual
- C1.5 `getComputedStyle(h1).fontFamily` empieza por «Bricolage Grotesque» y el `body` por «DM Sans»; fondo del `body` = crema — `browser_evaluate` — O2
- C1.6 Existen `header`, `main` y `footer` como hijos de `body`; no hay `tailwind.config.js` — `check-estatico` — O1
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, 4 avisos (sin favicon, sin `meta description` → SEO 90, DM Sans aún sin texto que la use, `dist/` previo) · Lighthouse a11y 100
  · 2026-10-11 · ✅ PASA tras corregir avisos (meta description, favicon SVG, `@source not` bitácora) · Lighthouse a11y 100 / BP 100 / SEO 100 · queda: DM Sans sin texto aún (T2)

## T2 · Cabecera con menú hamburguesa sin JS — ✅
**Qué:** logo `☕ tueste`, menú (Cafés, Planes, FAQ) y botón «Suscríbete» con
el efecto «se aplasta». En móvil, menú en `<details>`/`<summary>` (D1); desde
`md`, en línea.
**Depende de:** T1
**Criterios:**
- C2.1 `header` es flex con logo a la izquierda y menú a la derecha a 1440 (`left` del logo < `left` del `nav`, `top` parecido) — `browser_evaluate` + captura — O3
- C2.2 Enlaces del menú en `nav ul > li > a` (≥ 3) apuntando a `#cafes`, `#planes`, `#faq` — `browser_evaluate` — O1
- C2.3 Enlaces con `hover:` y `focus-visible:` en las clases, y foco visible en los 3 — `check-estatico` + `sonda-foco` — O3, O8
- C2.4 A 360 el menú está cerrado; con Tab hasta el `summary` + Enter se abre, los enlaces reciben foco y otro Enter lo cierra — `browser_press_key` + `sonda-foco` + captura — D1, brief §Móvil
- C2.5 A ≥ 768 el `summary` no se ve y los enlaces del menú sí, en fila — `browser_evaluate` (`display`, `top`) — brief §Móvil
- C2.6 Botón «Suscríbete» tomate con texto *espresso*, `border-2` y la sombra dura; `motion-safe:` en su `hover:`/`active:` `translate-*` — `check-estatico` + `sonda-pagina` (contraste) — brief §Detalle memorable
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: 3 «>12 utilidades» (summary, ul, botón), `<ul>` fuera del `<details>` (el summary anuncia «expandido» sin contenido dentro), Safari/VoiceOver sin probar · botón 5,37:1, espresso/menta 10,46:1 · Lighthouse 100/100/100 · DM Sans ya carga sola

## T2b · Utilidad `btn-pegatina` (D4) — ✅
**Qué:** extraer la forma y el efecto «se aplasta» del botón a una utilidad
propia con `@utility btn-pegatina` en `src/style.css`, para reutilizarla en
hero, tarjetas y planes. El color de fondo se queda en el HTML (el botón
secundario «Ver cafés» tendrá otro). Quitar `ontouchstart=""` del `<body>`
(decisión del alumno: no lo sabría explicar).
**Depende de:** T1, T2
**Criterios:**
- C2b.1 `src/style.css` tiene `@utility btn-pegatina` con borde de 2 px espresso, `--shadow-pegatina`, transición, hover (sombra `-lg` y subir 2 px), active (sin sombra y bajar 4 px) y foco visible; los desplazamientos solo dentro de `prefers-reduced-motion: no-preference`; sin colores a pelo (tokens) — leer `src/style.css` — D4, brief §Detalle memorable
- C2b.2 El botón «Suscríbete» de la cabecera usa `btn-pegatina` y ya no da el aviso de más de 12 utilidades — leer `index.html` + `check-estatico` — D4, skill `tailwind-clases-limpias`
- C2b.3 Mismo resultado que en T2 (sin regresión visual): fondo tomate, texto espresso (≥ 4,5:1), sombra `4px 4px 0`; en hover `translate -2px -2px` y sombra de 6 px; en active `translate 4px 4px` y sin sombra; con reduced-motion, sin `translate` — `browser_hover` / ratón + `browser_evaluate` + `sonda-pagina` — T2 C2.6
- C2b.4 El `<body>` no tiene `ontouchstart` — leer `index.html` — decisión del alumno
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: 2 «>12 utilidades» esperados (summary, ul), `:active` en iOS sin `ontouchstart` sin probar · botón de 19 a 5 clases, mismo resultado que T2 (5,37:1, hover −2 px / active +4 px, nada con reduced-motion) · Lighthouse 100/100/100 · T1 y T2 sin regresiones

## T3 · Hero con sello giratorio — ✅
**Qué:** `h1`, subtítulo, botón principal «Suscríbete» y secundario «Ver
cafés», foto, sello SVG «Recién tostado ·» girando sobre la esquina de la foto
y entrada en cascada.
**Depende de:** T1, T2, T2b (botones con `btn-pegatina`)
**Criterios:**
- C3.1 Un solo `h1` en la página, dentro del hero; 2 enlaces/botones y 1 `img` con `alt` no vacío — `sonda-pagina` (encabezados, imágenes) — O1, O4
- C3.2 A 360 texto e imagen apilados (`top` distinto); a 1440 lado a lado (`left` distinto, `top` parecido) — `browser_evaluate` + capturas — O4
- C3.3 Párrafo con `max-w-prose` o `max-w-xl` (ancho ≤ ~75ch a 1440) — `check-estatico` + `browser_evaluate` — O4
- C3.4 `--animate-girar` (o similar) y `--animate-aparecer` definidos en `@theme` con sus `@keyframes`; usados como `motion-safe:animate-*` — leer `src/style.css` + `check-estatico` — brief §Detalle memorable, P7
- C3.5 Con `prefers-reduced-motion: reduce` emulado, el sello no gira y no queda ninguna animación en marcha (`getAnimations().length === 0`) — `browser_evaluate` — brief §Movimiento, P7
- C3.6 El sello es SVG con `textPath`, su texto es decorativo (`aria-hidden="true"`) y no tapa el contenido del hero a 360 — leer HTML + captura — brief §Detalle memorable, O8
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: Rendimiento 95 (LCP 2,9 s por la foto: falta `fetchpriority="high"`, JPG de 232 KB para un hueco de 520 px), CLS 0,031, sello a 8 px del borde a 360, autor de la foto sin identificar · Lighthouse a11y 100 / BP 100 / SEO 100 · T1, T2, T2b sin regresiones
  · 2026-10-11 · ✅ PASA tras cambiar la foto (WebP 800×600, 155 KB, Łukasz Rawa / Unsplash, `fetchpriority="high"`) · Rendimiento 95 → 97, LCP 2,9 → 2,6 s, CLS 0,023 · queda: ~102 KiB de ahorro posible en la imagen (`srcset`/más compresión)

## T4 · Cómo funciona (3 pasos) — ✅
**Qué:** sección `#como-funciona` con 3 pasos (eliges → tostamos → te llega),
icono Lucide, número y texto corto.
**Depende de:** T1
**Criterios:**
- C4.1 `section` con `h2` y 3 bloques de paso con `h3` — `sonda-pagina` (encabezados) — O1, O6
- C4.2 Contenedor flex con `flexDirection` `column` a 360 y `row` a 1440 — `browser_evaluate` — O6
- C4.3 Contenido de cada paso centrado (`alignItems`/`textAlign: center`) — `browser_evaluate` + captura — O6
- C4.4 Iconos SVG inline decorativos con `aria-hidden="true"` (sin texto dentro de imágenes) — leer HTML — O8, brief §Contenido
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: pegatinas de número a < 3 px del borde a 360 (no se cortan, pero justas), `role="list"` sin probar en Safari/VoiceOver, 2 «>12 utilidades» heredados de T2 · `flexDirection` column/row/row a 360/768/1440, 3 pasos del mismo ancho y centrados · contraste mín. 5,37:1 (tomate) · Lighthouse a11y/BP/SEO 100, Rendimiento 97 · T1–T3 sin regresiones

## T5 · Cafés del mes (rejilla de tarjetas) — ✅
**Qué:** sección `#cafes` con ≥ 6 tarjetas `article`: foto, origen, proceso,
notas de cata, precio y botón; hover «se aplasta».
**Depende de:** T1, T2 (estilo de botón y sombra)
**Criterios:**
- C5.1 `#cafes article` ≥ 6, cada una con `img`, `h3`, texto, precio y botón/enlace — `browser_evaluate` — O5, O1
- C5.2 `gridTemplateColumns` da 1 / 2 / 3 columnas a 360 / 768 / 1440 — `browser_evaluate` — O5
- C5.3 Todas las imágenes de tarjeta con el mismo alto, `aspect-*` + `object-cover` y `deformadas` vacío — `sonda-pagina` + `browser_evaluate` — O5
- C5.4 Tarjetas con `p-*`, `rounded-*`, `border-2` y la sombra dura; hover con `transition` y `motion-safe:hover:-translate-*` — `check-estatico` — O5, brief §Detalle memorable
- C5.5 Datos realistas: 6 orígenes distintos, precios entre 12 y 17 € con formato `14,90 €` — leer HTML — brief §Contenido
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: Rendimiento 97 → 93–94 (LCP 3,1 s; fotos lazy de tarjetas compiten con la del hero en móvil), nombre accesible «Lo quiero : Etiopía…» con espacio antes de «:» · grid 1/2/3 columnas a 360/768/1440, 6 fotos del mismo alto sin deformar, 13/13 interactivos con foco visible · créditos de las 6 fotos comprobados abriendo sus páginas · Lighthouse a11y/BP/SEO 100 · T1–T4 sin regresiones

## T6 · Glosario de cata en bento — ✅
**Qué:** sección «¿Qué es eso de las notas de cata?» con 5–6 cajas (acidez,
cuerpo, dulzor, tueste, proceso…) de distintos tamaños, explicadas sin
tecnicismos.
**Depende de:** T1
**Criterios:**
- C6.1 `section` con `h2` y cada caja con `h3` — `sonda-pagina` (encabezados) — O1
- C6.2 Rejilla `grid` con al menos una celda `span 2` en columna y otra en fila a 1440 — `browser_evaluate` (`gridColumn`, `gridRow`) — P6
- C6.3 A 360 todas las cajas en una columna (`gridTemplateColumns` con 1 valor) — `browser_evaluate` — brief §Móvil
- C6.4 Colores de las cajas solo con tokens (crema, menta, tomate, espresso), sin `[#…]` — `check-estatico` — O2
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: cajas con espacio vacío a 1440 (Notas, Cuerpo) y a 768 (Acidez), la sombra de la caja oscura apenas se ve (para design-reviewer) · mosaico 4×3 sin huecos a 1440 (Notas 2×2, Cuerpo 1×2, Tueste y Proceso 2×1), 2 columnas a 768, 1 a 360 · contraste mín. 5,37:1 en 29 textos · iconos Lucide comparados con los originales: coinciden · Lighthouse a11y/BP/SEO 100, Rendimiento 93–94 · T1–T5 sin regresiones

## T7 · Planes de suscripción — ✅
**Qué:** sección `#planes` con 3 planes (`article`), el del centro destacado
con etiqueta «⭐ Más popular» colocada con `absolute`.
**Depende de:** T1, T2 (botón)
**Criterios:**
- C7.1 `#planes article` = 3, cada uno con `h3`, precio, lista `ul` de ventajas y botón — `browser_evaluate` — O1
- C7.2 Etiqueta con `position: absolute` dentro de una tarjeta `position: relative` — `browser_evaluate` — P3
- C7.3 El plan destacado se distingue (borde/fondo/escala distintos) y es el 2.º a 360 y el central a 1440 — captura + `browser_evaluate` — P3, brief §Decidido
- C7.4 A 360 los planes apilados; a 1440 en fila — `browser_evaluate` — brief §Móvil
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: botones a `#newsletter` aún sin destino (T9), «Más popular» va antes del `h3` (no se oye al saltar por encabezados), a 768 h2 a la izquierda y tarjetas centradas · etiqueta `absolute` en article `relative` sin tapar el h3 en 3 anchos, destacado central a 1440 y 2.º a 360 · «12,90 € al mes» en el árbol de accesibilidad · 16/16 interactivos con foco visible · Lighthouse a11y/BP/SEO 100, Rendimiento 93 · T1–T6 sin regresiones

## T8 · Preguntas frecuentes — ✅
**Qué:** sección `#faq` con ≥ 5 preguntas de principiante en
`<details>`/`<summary>`, con flecha que gira al abrir.
**Depende de:** T1
**Criterios:**
- C8.1 `#faq details` ≥ 5, cada uno con `summary` — `browser_evaluate` — P4
- C8.2 Se abre con Tab + Enter desde el teclado (`open === true`) — `browser_press_key` + `browser_evaluate` — P4, O8
- C8.3 La flecha usa `group` + `group-open:rotate-180` — `check-estatico` — P4
- C8.4 Preguntas de principiante («¿Molido o en grano?», «¿Puedo cancelar?», «¿Qué cafetera necesito?») — leer HTML — brief §Contenido
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: foco de los summary fino y del mismo color que el borde («doble borde»), «eliges otro de los seis» puede sugerir que los orígenes no rotan · 6 details abren/cierran con Enter y Espacio a 360 y 1440, 22/22 interactivos con foco visible · giro con `transition-transform` + `motion-reduce:transition-none` (con reduce: 180deg sin animación), aceptado por el verificador como cumplimiento de la intención de P7 · chevron-down comparado con Lucide: coincide · Lighthouse a11y/BP/SEO 100, Rendimiento 94 · T1–T7 sin regresiones

## T9 · Pie con newsletter — ✅
**Qué:** `footer` con ≥ 3 columnas (enlaces, contacto, redes) + newsletter y
línea de copyright con borde superior.
**Depende de:** T1
**Criterios:**
- C9.1 ≥ 3 columnas a 1440 (`left` distinto, `top` parecido) y apiladas a 360 — `browser_evaluate` + capturas — O7
- C9.2 Enlaces en `ul > li > a`; iconos de redes con nombre accesible (`aria-label` o texto oculto) — `browser_evaluate` + `sonda-pagina` (interactivos) — O1, O8
- C9.3 Línea de copyright con `borderTopWidth` > 0 — `browser_evaluate` — O7
- C9.4 Campo de email con `<label>` asociado (`labels.length ≥ 1`) y botón alcanzable con Tab — `browser_evaluate` + `sonda-foco` — O8
- C9.5 Existe un elemento con `id="newsletter"` (destino de los 3 botones de planes de T7) — `browser_evaluate` — T7
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: redes ficticias con `href="#"`, newsletter sin backend (el correo queda en la URL, sin confirmación) · pie 4 / 2×2 / 1 columnas a 1440 / 768 / 360, `#newsletter` es el destino de los planes, 33/33 interactivos con foco visible · franja crema de la captura a 360 descartada (artefacto de la captura: el pie llega al final) · iconos Lucide comparados: coinciden · unidades: px solo en bordes, sombras y outline · Lighthouse a11y/BP/SEO 100, Rendimiento 92 · T1–T8 sin regresiones

## Arreglos de la revisión de diseño (2026-10-11)

> Salen del informe del agente `design-reviewer` (sin críticos; 6 importantes
> y 4 sugerencias). El alumno eligió hacer los 10. Mismas reglas: tokens,
> `rem` en letra y espaciados, `px` solo en bordes/sombras/outline.

## R1 · Utilidades comunes y nombres (revisión 7) — ✅
**Qué:** refactor sin cambio visual. `paso-tarjeta` → `tarjeta-pegatina`;
`@utility pegatina` con lo común de `cafe-proceso` y `plan-etiqueta` (la
posición y el giro, en el HTML); `@utility menu-enlace` para los 3 enlaces
del menú.
**Depende de:** T2, T4–T9
**Criterios:**
- CR1.1 No queda ningún `paso-tarjeta`, `cafe-proceso` ni `plan-etiqueta` en `index.html` ni `src/style.css`; existen `tarjeta-pegatina`, `pegatina` y `menu-enlace` — leer ficheros — revisión 7
- CR1.2 Las pegatinas de proceso (T5), «Más popular» (T7) y «1 correo al mes» (T9) siguen en la misma posición, giro y estilo (estilos calculados iguales que antes: `position`, `top/right/left`, `rotate`, `box-shadow`, `border`) — `browser_evaluate` + capturas — sin regresión
- CR1.3 Los 3 enlaces del menú usan `menu-enlace` (cada uno con ≤ 12 utilidades); hover, active y foco iguales que en T2 — `check-estatico` + `sonda-foco` — tailwind-clases-limpias. *(Corregido el 2026-10-11: decía que desaparecería el aviso del `<ul>`, pero ese aviso es de las 20 clases del propio `<ul>`, casi todas variantes `md:` del panel móvil/fila de escritorio; extraerlas a `@utility` escondería los breakpoints. Aviso aceptado por el alumno.)*
- CR1.4 Sin cambio visual: capturas a 360 y 1440 equivalentes a las de antes del refactor — capturas — sin regresión
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: los 2 «>12 utilidades» (summary, y ul del menú aceptado en CR1.3) · comparado con HEAD: 21 420 estilos calculados (reposo, hover, active, foco; 360/768/1440) con 0 diferencias y capturas de página completa con 0 píxeles distintos · 33/33 interactivos con foco visible · Lighthouse a11y/BP/SEO 100, Rendimiento 92 · T1–T9 sin regresiones

## R2 · Hero y marca (revisión 4, 6 y 3b) — ✅
**Qué:** el ☕ del logo (cabecera y pie) y del sello pasa a ser el icono
Lucide `coffee` con `currentColor`; pegatina «Tostado el lunes» en la
esquina contraria al sello; línea «Desde 12,90 € al mes, sin permanencia»
bajo los botones del hero.
**Depende de:** R1 (`pegatina`), T2, T3, T9
**Criterios:**
- CR2.1 No queda el emoji ☕ en `index.html`; logo de cabecera, logo del pie y centro del sello usan el SVG `coffee` de Lucide (comparado con el original), `aria-hidden`, color de tokens — leer HTML + `browser_evaluate` — revisión 4, O2
- CR2.2 Pegatina «Tostado el lunes» con `pegatina` en el bloque de la foto del hero, en la esquina contraria al sello; no tapa texto ni botones y no desborda a 360 / 768 / 1440 — `browser_evaluate` + capturas — brief §Dirección visual
- CR2.3 Bajo los botones del hero, texto «Desde 12,90 € al mes, sin permanencia» (coherente con el plan Curioso) con contraste ≥ 4,5:1 — leer HTML + `sonda-pagina` — revisión 3, brief §Para quién
- CR2.4 El nombre accesible del logo sigue siendo «tueste» (el icono no se lee) — árbol de accesibilidad — O8
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: los 2 «>12 utilidades» de siempre · 3 SVG con los 4 `d` idénticos al original de Lucide; ningún ☕ · pegatina sin solapes con sello ni texto a 360/768/1440 (10,46:1) · precio 9,96:1 · logo = `link "tueste"` · 0 contraste fallido, sin scroll horizontal, foco 12/12 + 11/11, reduced motion sin animaciones · Lighthouse a11y/BP/SEO 100, Rendimiento 92 (LCP 3,3 s) · T2, T3, T9 y R1 sin regresiones

## R3 · Bento sin huecos (revisión 1) — ✅
**Qué:** quitar `lg:auto-rows-fr`; Cuerpo con barras de intensidad por café;
notas más grandes en Notas; sombra tomate en la caja oscura; mosaico a 768
sin cajas medio vacías.
**Depende de:** R1, T6
**Criterios:**
- CR3.1 Sin `auto-rows-fr` en `#glosario`; ninguna caja con más de ~4rem de espacio vacío bajo su contenido a 768 y 1440 (medir alto del contenido frente al de la caja) — `browser_evaluate` + capturas — revisión 1
- CR3.2 Sigue cumpliendo P6 a 1440: al menos un `span 2` en columna y otro en fila, sin huecos en la rejilla; a 768, sin huecos; a 360, 1 columna — `browser_evaluate` — P6, C6.2, C6.3
- CR3.3 Cuerpo muestra una barra de intensidad por café (`ul` con nombre + barra decorativa) con un texto que la explique para el lector de pantalla — leer HTML + árbol de accesibilidad — brief §Detalle memorable (C)
- CR3.4 La caja oscura tiene sombra visible: token `--shadow-pegatina-tomate` (o similar) en `@theme` y aplicado — leer `src/style.css` + captura — brief §Dirección visual
- CR3.5 *(Añadido el 2026-10-11, tras el aviso de la primera verificación.)* También a 1024 y 1280 ninguna caja tiene más de ~4rem libres: el mosaico de 4 columnas pasa de `lg:` a `xl:` y entre 1024 y 1279 se usa el de 2 — `browser_evaluate` + capturas — revisión 1
**Verificación:** 2026-10-11 · ✅ PASA · 0 fallos, avisos: los 2 «>12 utilidades» de siempre · espacio libre máx. 3,69rem a 1440 (Cuerpo) y 0 a 768 · rejilla 4×3 / 2×5 / 1 col sin huecos · 6 barras con «cuerpo N de 5» en el árbol de accesibilidad · sombra tomate calculada · contraste mín. 5,37:1, sin scroll horizontal, foco 13/13, reduced motion sin animaciones · Lighthouse a11y/BP/SEO 100, Rendimiento 91 (LCP 3,3 s, igual) · T5, T6 y R1 sin regresiones; resto de la página igual que HEAD · aviso: a 1024 Notas y Cuerpo quedaban con ~6,5rem libres → arreglado (CR3.5)
**Verificación (arreglo, CR3.5):** 2026-10-11 · ✅ PASA · mosaico de 4 columnas de `lg:` a `xl:` · espacio libre máx.: 0 a 768 y 1024 (2×5), 1,5rem a 1279, 3,69rem a 1280 y 1440 (4×3), 0 huecos en todos · resto de la página idéntico a HEAD en 6 anchos · Lighthouse a11y/BP/SEO 100, Rendimiento 91 (LCP 3,3 s)

## R4 · Planes: elegir plan y empujar el de entrada (revisión 2, 3a, 8 y 9) — ✅
**Qué:** aviso sin JS en `#newsletter` cuando se llega desde un plan; pegatina
«Para empezar» en Curioso; «Más popular» después del `h3`; tarjetas
alineadas con el título a 768; precios alineados a 1440; retocar «Sea el café
que sea».
**Depende de:** R1, T7, T9
**Criterios:**
- CR4.1 Al pulsar un botón de plan, `#newsletter` es `:target` y se ve un aviso tipo «¡Buena elección! Déjanos tu correo…» que no aparece si se llega de otro modo (`group-target:` o equivalente, sin JS) — `browser_click` + `browser_evaluate` — revisión 2, brief §Para quién
- CR4.2 Curioso lleva una pegatina «Para empezar» con `pegatina`, sin tapar su `h3` en 360 / 768 / 1440; Mañanero sigue siendo el destacado central (P3) — `browser_evaluate` + capturas — revisión 3, P3
- CR4.3 En el DOM, «Más popular» va después del `h3` de Mañanero (sin cambio visual) — leer HTML + captura — revisión 8, O8
- CR4.4 A 768 el borde izquierdo de las tarjetas coincide con el del `h2`; a 1440 los 3 precios están a la misma altura (±2px) — `browser_evaluate` — revisión 8
- CR4.5 «Sea el café que sea» sustituido por un texto más claro — leer HTML — revisión 9
**Verificación:** 2026-10-12 · ✅ PASA · 0 fallos, avisos: los 2 «>12 utilidades» de siempre · aviso oculto al cargar, visible tras cada «Elegir…» y oculto tras enviar el form (ahora a `#newsletter-form`) · «Para empezar» sin tapar el h3 en 5 anchos · «Más popular» tras el h3 sin cambio visual (misma posición que HEAD) · tarjetas en x=32 como el h2 a 768 · precios a 0 px de diferencia a 1024/1280/1440 · resto de la página igual que HEAD · foco visible (incluido pie tras llegar desde un plan), reduced motion sin animaciones · Lighthouse a11y/BP/SEO 100, Rendimiento 91 (LCP 3,3 s) · T7, T9, R1, R2 sin regresiones · aviso: al llegar a #newsletter el foco no se mueve, el lector no oye el aviso salvo que lea el bloque

## R5 · FAQ: foco y texto (revisión 5 y 9) — ⬜
**Qué:** el foco de las preguntas, por fuera de la pegatina como en los
botones; «eliges otro de los seis» → texto que no sugiera que los orígenes
no cambian.
**Depende de:** R1, T8
**Criterios:**
- CR5.1 Con Tab en un `summary`, el outline queda fuera del borde de la pegatina (offset positivo, en el `details` vía `:has(> summary:focus-visible)` o similar) y es visible a 360 y 1440 — `sonda-foco` + capturas — revisión 5, O8
- CR5.2 Las 6 preguntas siguen abriéndose y cerrándose con Enter y Espacio; la flecha gira — `browser_press_key` + `browser_evaluate` — P4, T8
- CR5.3 El texto de «¿Y si no me gusta…?» ya no dice «otro de los seis» — leer HTML — revisión 9
**Verificación:** —

## R6 · Rendimiento: srcset de las fotos (revisión 10) — ⬜
**Qué:** versiones más pequeñas de la foto del hero y de las 6 de tarjetas, con
`srcset` + `sizes`, para que el móvil descargue menos.
**Depende de:** T3, T5
**Criterios:**
- CR6.1 La foto del hero y las 6 de tarjetas tienen `srcset` con al menos 2 anchos y `sizes` coherente con su hueco; los ficheros existen y responden 200 — leer HTML + `browser_network_requests` — revisión 10, D7
- CR6.2 A 360 el navegador descarga la versión pequeña (`currentSrc`) y a 1440 la grande; ninguna imagen se deforma — `browser_evaluate` + `sonda-pagina` — O5
- CR6.3 Lighthouse móvil: Rendimiento ≥ 92 (antes 92) y LCP no peor que 3,3 s; a11y/BP/SEO siguen a 100 — puerta 4 — D7
**Verificación:** —

## T10 · README, capturas y verificación del conjunto — ⬜
**Qué:** `README.md` con tema, cómo arrancar, opción de Tailwind (Vite) y por
qué, capturas móvil/escritorio, checklist O/P/D y bitácora de IA; Lighthouse
de accesibilidad y repaso final de todos los criterios del lab.
**Depende de:** T1–T9, R1–R6
**Criterios:**
- C10.1 README con los apartados: tema, `npm install` + `npm run dev`, opción de Tailwind y por qué, checklist — leer `README.md` — O9
- C10.2 Capturas a 360 y 1440 enlazadas en el README y los ficheros existen — leer README + listar ficheros — O9
- C10.3 Bitácora con ≥ 3 `### Prompt` / `### Qué me dio` / `### Qué corregí y por qué` — contar en `README.md` — O9
- C10.4 Lighthouse Accesibilidad ≥ 90 y nota apuntada en el README — puerta 4 de `verify-task` + leer README — O8
- C10.5 Si el README trae clases de ejemplo, `@source not "../README.md";` en el CSS — leer ficheros — CLAUDE.md
- C10.7 README explica el D4: cuándo crear una utilidad en CSS y cuándo repetir clases, y por qué abusar de `@apply` es mala práctica; y las pegas de accesibilidad de la hamburguesa (D1) — leer `README.md` — D4, D1
- C10.8 Créditos de imágenes en el README: foto del hero de Łukasz Rawa (https://unsplash.com/@lukasz_rawa · https://unsplash.com/photos/brown-coffee-beans-on-black-surface-fmc-tFMMiBs, Unsplash License) y las que se añadan en T5 — leer `README.md` — brief §Contenido
- C10.6 Repaso de `criterios-lab.md`: O1–O9 PASA en la página completa — `verify-task` sobre el conjunto — O1–O9
**Verificación:** —

## Cobertura
| Requisito | Tareas |
|---|---|
| O1 Estructura semántica | T1, T2, T3, T4, T5, T6, T7, T9 |
| O2 Tema con `@theme` | T1, T6 |
| O3 Cabecera flex | T2 |
| O4 Hero | T3 |
| O5 Rejilla de tarjetas | T5 |
| O6 Pasos con flexbox | T4 |
| O7 Pie | T9 |
| O8 Accesibilidad mínima | T2, T3, T4, T8, T9, T10 (+ comunes de `verify-task` en todas) |
| O9 README + bitácora | T10 |
| P3 Plan destacado | T7 |
| P4 FAQ sin JS | T8 |
| P6 Bento | T6 |
| P7 Animaciones con `motion-safe:` | T2, T3, T5 |
| D1 Hamburguesa sin JS | T2 |
| D4 Utilidades propias (`@utility`) | T2b, R1 (+ explicación en el README: T10) |
| Revisión de diseño (10 puntos) | R1 (7) · R2 (4, 6, 3b) · R3 (1) · R4 (2, 3a, 8, 9) · R5 (5, 9) · R6 (10) |
| brief §Secciones (8) | T2 cabecera · T3 hero · T4 pasos · T5 cafés · T6 glosario · T7 planes · T8 FAQ · T9 pie |
| brief §Móvil | T2, T6, T7 |
| brief §Contenido | T4, T5, T8 |
| brief §Dirección visual | T1, T2 |
| brief §Detalle memorable (aplastar + sello) | T2, T3, T5 |
| brief §Movimiento | T3 |
