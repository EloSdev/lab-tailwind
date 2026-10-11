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

## T2 · Cabecera con menú hamburguesa sin JS — ⬜
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
**Verificación:** —

## T3 · Hero con sello giratorio — ⬜
**Qué:** `h1`, subtítulo, botón principal «Suscríbete» y secundario «Ver
cafés», foto, sello SVG «Recién tostado ·» girando sobre la esquina de la foto
y entrada en cascada.
**Depende de:** T1, T2 (reutiliza el estilo de botón)
**Criterios:**
- C3.1 Un solo `h1` en la página, dentro del hero; 2 enlaces/botones y 1 `img` con `alt` no vacío — `sonda-pagina` (encabezados, imágenes) — O1, O4
- C3.2 A 360 texto e imagen apilados (`top` distinto); a 1440 lado a lado (`left` distinto, `top` parecido) — `browser_evaluate` + capturas — O4
- C3.3 Párrafo con `max-w-prose` o `max-w-xl` (ancho ≤ ~75ch a 1440) — `check-estatico` + `browser_evaluate` — O4
- C3.4 `--animate-girar` (o similar) y `--animate-aparecer` definidos en `@theme` con sus `@keyframes`; usados como `motion-safe:animate-*` — leer `src/style.css` + `check-estatico` — brief §Detalle memorable, P7
- C3.5 Con `prefers-reduced-motion: reduce` emulado, el sello no gira y no queda ninguna animación en marcha (`getAnimations().length === 0`) — `browser_evaluate` — brief §Movimiento, P7
- C3.6 El sello es SVG con `textPath`, su texto es decorativo (`aria-hidden="true"`) y no tapa el contenido del hero a 360 — leer HTML + captura — brief §Detalle memorable, O8
**Verificación:** —

## T4 · Cómo funciona (3 pasos) — ⬜
**Qué:** sección `#como-funciona` con 3 pasos (eliges → tostamos → te llega),
icono Lucide, número y texto corto.
**Depende de:** T1
**Criterios:**
- C4.1 `section` con `h2` y 3 bloques de paso con `h3` — `sonda-pagina` (encabezados) — O1, O6
- C4.2 Contenedor flex con `flexDirection` `column` a 360 y `row` a 1440 — `browser_evaluate` — O6
- C4.3 Contenido de cada paso centrado (`alignItems`/`textAlign: center`) — `browser_evaluate` + captura — O6
- C4.4 Iconos SVG inline decorativos con `aria-hidden="true"` (sin texto dentro de imágenes) — leer HTML — O8, brief §Contenido
**Verificación:** —

## T5 · Cafés del mes (rejilla de tarjetas) — ⬜
**Qué:** sección `#cafes` con ≥ 6 tarjetas `article`: foto, origen, proceso,
notas de cata, precio y botón; hover «se aplasta».
**Depende de:** T1, T2 (estilo de botón y sombra)
**Criterios:**
- C5.1 `#cafes article` ≥ 6, cada una con `img`, `h3`, texto, precio y botón/enlace — `browser_evaluate` — O5, O1
- C5.2 `gridTemplateColumns` da 1 / 2 / 3 columnas a 360 / 768 / 1440 — `browser_evaluate` — O5
- C5.3 Todas las imágenes de tarjeta con el mismo alto, `aspect-*` + `object-cover` y `deformadas` vacío — `sonda-pagina` + `browser_evaluate` — O5
- C5.4 Tarjetas con `p-*`, `rounded-*`, `border-2` y la sombra dura; hover con `transition` y `motion-safe:hover:-translate-*` — `check-estatico` — O5, brief §Detalle memorable
- C5.5 Datos realistas: 6 orígenes distintos, precios entre 12 y 17 € con formato `14,90 €` — leer HTML — brief §Contenido
**Verificación:** —

## T6 · Glosario de cata en bento — ⬜
**Qué:** sección «¿Qué es eso de las notas de cata?» con 5–6 cajas (acidez,
cuerpo, dulzor, tueste, proceso…) de distintos tamaños, explicadas sin
tecnicismos.
**Depende de:** T1
**Criterios:**
- C6.1 `section` con `h2` y cada caja con `h3` — `sonda-pagina` (encabezados) — O1
- C6.2 Rejilla `grid` con al menos una celda `span 2` en columna y otra en fila a 1440 — `browser_evaluate` (`gridColumn`, `gridRow`) — P6
- C6.3 A 360 todas las cajas en una columna (`gridTemplateColumns` con 1 valor) — `browser_evaluate` — brief §Móvil
- C6.4 Colores de las cajas solo con tokens (crema, menta, tomate, espresso), sin `[#…]` — `check-estatico` — O2
**Verificación:** —

## T7 · Planes de suscripción — ⬜
**Qué:** sección `#planes` con 3 planes (`article`), el del centro destacado
con etiqueta «⭐ Más popular» colocada con `absolute`.
**Depende de:** T1, T2 (botón)
**Criterios:**
- C7.1 `#planes article` = 3, cada uno con `h3`, precio, lista `ul` de ventajas y botón — `browser_evaluate` — O1
- C7.2 Etiqueta con `position: absolute` dentro de una tarjeta `position: relative` — `browser_evaluate` — P3
- C7.3 El plan destacado se distingue (borde/fondo/escala distintos) y es el 2.º a 360 y el central a 1440 — captura + `browser_evaluate` — P3, brief §Decidido
- C7.4 A 360 los planes apilados; a 1440 en fila — `browser_evaluate` — brief §Móvil
**Verificación:** —

## T8 · Preguntas frecuentes — ⬜
**Qué:** sección `#faq` con ≥ 5 preguntas de principiante en
`<details>`/`<summary>`, con flecha que gira al abrir.
**Depende de:** T1
**Criterios:**
- C8.1 `#faq details` ≥ 5, cada uno con `summary` — `browser_evaluate` — P4
- C8.2 Se abre con Tab + Enter desde el teclado (`open === true`) — `browser_press_key` + `browser_evaluate` — P4, O8
- C8.3 La flecha usa `group` + `group-open:rotate-180` — `check-estatico` — P4
- C8.4 Preguntas de principiante («¿Molido o en grano?», «¿Puedo cancelar?», «¿Qué cafetera necesito?») — leer HTML — brief §Contenido
**Verificación:** —

## T9 · Pie con newsletter — ⬜
**Qué:** `footer` con ≥ 3 columnas (enlaces, contacto, redes) + newsletter y
línea de copyright con borde superior.
**Depende de:** T1
**Criterios:**
- C9.1 ≥ 3 columnas a 1440 (`left` distinto, `top` parecido) y apiladas a 360 — `browser_evaluate` + capturas — O7
- C9.2 Enlaces en `ul > li > a`; iconos de redes con nombre accesible (`aria-label` o texto oculto) — `browser_evaluate` + `sonda-pagina` (interactivos) — O1, O8
- C9.3 Línea de copyright con `borderTopWidth` > 0 — `browser_evaluate` — O7
- C9.4 Campo de email con `<label>` asociado (`labels.length ≥ 1`) y botón alcanzable con Tab — `browser_evaluate` + `sonda-foco` — O8
**Verificación:** —

## T10 · README, capturas y verificación del conjunto — ⬜
**Qué:** `README.md` con tema, cómo arrancar, opción de Tailwind (Vite) y por
qué, capturas móvil/escritorio, checklist O/P/D y bitácora de IA; Lighthouse
de accesibilidad y repaso final de todos los criterios del lab.
**Depende de:** T1–T9
**Criterios:**
- C10.1 README con los apartados: tema, `npm install` + `npm run dev`, opción de Tailwind y por qué, checklist — leer `README.md` — O9
- C10.2 Capturas a 360 y 1440 enlazadas en el README y los ficheros existen — leer README + listar ficheros — O9
- C10.3 Bitácora con ≥ 3 `### Prompt` / `### Qué me dio` / `### Qué corregí y por qué` — contar en `README.md` — O9
- C10.4 Lighthouse Accesibilidad ≥ 90 y nota apuntada en el README — puerta 4 de `verify-task` + leer README — O8
- C10.5 Si el README trae clases de ejemplo, `@source not "../README.md";` en el CSS — leer ficheros — CLAUDE.md
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
| brief §Secciones (8) | T2 cabecera · T3 hero · T4 pasos · T5 cafés · T6 glosario · T7 planes · T8 FAQ · T9 pie |
| brief §Móvil | T2, T6, T7 |
| brief §Contenido | T4, T5, T8 |
| brief §Dirección visual | T1, T2 |
| brief §Detalle memorable (aplastar + sello) | T2, T3, T5 |
| brief §Movimiento | T3 |
