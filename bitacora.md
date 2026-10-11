# Bitácora de IA — Tueste

Se va rellenando sobre la marcha, tarea a tarea. En T10 pasa al `README.md`
(apartado «Bitácora de IA», O9).

**Herramienta:** Claude Code (Claude Opus) con las skills y agentes del repo:
`brief-grill` (entrevista), `plan-tasks` (plan), agente `tailwind-html`
(maqueta), `verify-task` (verificación con build, comprobación estática,
navegador real y Lighthouse, hecha por un subagente que no escribió el código).

---

## 1 · Brief del proyecto (2026-10-11)

### Prompt

"Quiero empezar el proyecto tueste" → entrevista de 8 preguntas, una cada vez,
con opciones y una recomendación (público, secciones, móvil, contenido,
personalidad, dirección visual, detalle memorable, movimiento).

### Qué me dio

Un `brief.md`: landing para principiantes del café de especialidad, «primero
explicar y luego vender» (con un glosario de cata en bento), menú hamburguesa
con `<details>`, dirección visual «Pegatina pop» (crema, espresso, tomate,
menta; Bricolage Grotesque + DM Sans; bordes gruesos y sombras duras) y un
sello giratorio en el hero.

### Qué corregí y por qué

- Al fijar la paleta se midió el contraste del acento: texto **blanco sobre
  tomate `#FF5A36` da ≈ 3:1** y no pasa AA (4,5:1). Quedó como regla en el
  brief: sobre tomate, texto espresso (≈ 5,4:1). El botón naranja con texto
  blanco es justo lo que una IA pondría por defecto.
- Descarté la dirección «artesano cálido con marrones» porque es la web de
  café genérica que sale por defecto; elegí «fresco, curioso, desenfadado».

## 2 · T1 · Base y tokens (2026-10-11)

### Prompt

"Construye T1: copia la plantilla, instala Bricolage Grotesque y DM Sans con
`@fontsource-variable`, pon la paleta y la sombra dura en `@theme` y deja el
esqueleto `header` / `main` / `footer`."

### Qué me dio

Proyecto que compila, con `@theme` en `src/style.css` (escala `espresso`
50–950, `tomate-500/600`, `crema`, `menta`, `--shadow-pegatina`), fuentes
importadas en `main.js` y sin `tailwind.config.js` (bien: v4). Pero:

- Los tonos intermedios de `espresso` los puso **a ojo, sin medir contraste**.
- No añadió `<meta name="description">` ni favicon.

### Qué corregí y por qué

- La verificación (`verify-task`) dio PASA con avisos: Lighthouse SEO 90 por
  falta de `meta description` y un 404 de `/favicon.ico` en consola. Añadí la
  `meta description` y un favicon SVG (☕): son parte de una página bien
  hecha y suben el SEO de cara al D7. Reverificado: Lighthouse SEO 90 → 100
  y sin 404 en consola.
- Tailwind v4 escanea también los `.md`: como `brief.md`, `tareas.md` y esta
  bitácora citan clases, se habrían colado en el CSS. Se excluyen con
  `@source not "../tareas.md";` (etc.) en `src/style.css`.
- Pendiente de vigilar: medir el contraste de los tonos intermedios de
  `espresso` cuando se usen como texto (T2 en adelante).

## 3 · T2 · Cabecera con menú hamburguesa sin JS (2026-10-11)

### Prompt

"Construye T2: logo, menú Cafés / Planes / FAQ y botón «Suscríbete» que se
aplasta (texto espresso sobre tomate, sombra dura, `motion-safe:` en los
`translate`). En móvil, hamburguesa sin JS con `<details>`/`<summary>`."

### Qué me dio

Una cabecera flex con un truco limpio: el `<details class="peer">` solo
lleva el `<summary>` (☰/✕) y hace de interruptor; la lista `<ul>` va al lado,
como hermana, y se enseña con `peer-open:flex`. Desde `md` el details
desaparece y la misma lista se pone en fila: una sola lista, sin enlaces
duplicados para el lector de pantalla. Añadió el token
`--shadow-pegatina-lg` en vez de una sombra arbitraria.

### Qué corregí y por qué

- La primera versión no cabía a 360 px: con la palabra «Menú» visible en el
  botón, el logo se partía en dos líneas. Se cambió a un botón ☰ de 44×44 px
  con «Menú» en `sr-only` (lo sigue leyendo el lector de pantalla) y
  `whitespace-nowrap` en el logo.
- La verificación avisó de un coste de accesibilidad de la técnica: como la
  lista está fuera del `<details>`, el lector de pantalla anuncia el botón
  como «expandido» aunque dentro no hay nada. Se usa bien con teclado (abrir y
  Tab lleva a «Cafés») y Lighthouse da 100, pero es la pega que hay que contar
  en el README para el D1.
- El botón quedó con 23 clases, y se va a repetir en hero, tarjetas y planes:
  candidato a `@utility` (D4) antes de copiarlo.
- La IA añadió `ontouchstart=""` en el `<body>` (truco para que iOS Safari
  active `:active` al tocar). Lo quité: no lo sabría explicar en la revisión,
  y solo se pierde el «aplastado» al tocar en iPhone.

## 4 · T2b · Utilidad `btn-pegatina` (D4) (2026-10-11)

### Prompt

"Extrae la forma y el efecto «se aplasta» del botón a `@utility
btn-pegatina`; el color se queda en el HTML. Que se vea y se comporte igual
que en T2."

### Qué me dio

Una utilidad en CSS plano anidado, sin `@apply`: borde, sombra dura,
transición, `:hover` dentro de `@media (hover: hover)`, `:active` y
`:focus-visible`, con los `translate` dentro de
`@media (prefers-reduced-motion: no-preference)` (lo mismo que hace
`motion-safe:`). Todo con tokens (`var(--color-espresso-900)`,
`var(--shadow-pegatina)`…).

### Qué corregí y por qué

- Decidí qué va dentro y qué fuera: dentro, la forma y el efecto, que se
  repiten igual en todas partes; fuera, el color (el secundario «Ver cafés»
  tendrá otro fondo) y el padding (cada sitio tiene su tamaño). El botón
  pasó de 19 clases a 5: `btn-pegatina px-3 bg-tomate-500 text-espresso-900
  md:px-5`.
- Por qué `@utility` y no `@layer components`: vive en la capa de utilidades,
  así que un `bg-*` o `px-*` del HTML la puede cambiar sin pelear por
  especificidad, y admite variantes (`md:btn-pegatina`).
- La verificación comprobó que no cambiaba nada visible (mismos colores,
  sombra y desplazamientos que en T2, también con reduced-motion). Solo
  cambia que ahora la transición anima `box-shadow` y `translate`, no los
  colores, porque el botón no cambia de color.

## 5 · T3 · Hero con sello giratorio (2026-10-11)

### Prompt

"Construye el hero: `h1`, subtítulo con `max-w-xl`, «Suscríbete» y «Ver
cafés» con `btn-pegatina`, una foto de Unsplash y un sello SVG «Recién
tostado ·» que gire despacio en la esquina de la foto, con `motion-safe:`.
Entrada en cascada con `animate-aparecer`."

### Qué me dio

Hero en **grid** (`md:grid-cols-2`): en móvil una columna sin clases extra,
y desde `md` dos columnas iguales; con flex habría que cambiar la dirección y
dar `basis-1/2` a cada hijo. Animaciones propias en `@theme`
(`--animate-girar` 16 s, `--animate-aparecer` 0,6 s) con sus `@keyframes`.
Titular «Café del bueno, sin postureo.»

### Qué corregí y por qué

- La primera versión del sello creaba **12 px de scroll horizontal a 360 px**:
  al girar, las esquinas del cuadrado del SVG se salían del círculo. Se
  arregló con `overflow-hidden` en el círculo (recorte local; la verificación
  comprobó que sin él el scroll vuelve y que no esconde texto).
- Si giraba el sello entero, la sombra dura daba vueltas con él. Ahora el
  círculo y su sombra se quedan quietos y solo gira el SVG del texto.
- El titular de ejemplo («Café recién tostado…») repetía las palabras del
  sello que tiene al lado; se cambió.
- La IA descartó una foto de un paquete porque se veía una marca real.
- **La IA dio un enlace erróneo al autor de la foto**: usó el ID del fichero
  de `images.unsplash.com` como si fuera el de la página
  (`unsplash.com/photos/photo-…` → «Page not found»). El autor queda
  pendiente de identificar. Ni buscando se encontró la página original, así
  que **cambié la foto** por una parecida elegida por mí, con autor y licencia
  a la vista: Łukasz Rawa, Unsplash License.
- Lighthouse avisa de Rendimiento 95: la foto es un JPG de 232 KB para un
  hueco de 520 px y le falta `fetchpriority="high"` (es la imagen más grande
  de la primera pantalla, la que marca el LCP). Con la foto nueva en **WebP
  de 800×600 y 155 KB** (comparé 4 combinaciones de tamaño y calidad) y
  `fetchpriority="high"`: Rendimiento 95 → 97 y LCP 2,9 s → 2,6 s. También
  cambié el `alt`: el anterior decía «brillantes» y estos granos no lo son.

## 6 · T4 · Cómo funciona (2026-10-11)

### Prompt

"Construye «Cómo funciona» después del hero: 3 pasos (eliges → tostamos →
te llega) en `flex flex-col md:flex-row`, con el contenido centrado, icono
de Lucide, número y texto corto. Las tarjetas no se pulsan: sin efecto de
aplastar."

### Qué me dio

Una lista ordenada `<ol>` (son pasos en orden) con tres tarjetas de fondo
menta, tomate y `espresso-100`, iconos de Lucide (`coffee`, `flame`,
`package`) y el número como pegatina redonda girada en la esquina. Extrajo
dos utilidades nuevas, `paso-tarjeta` y `paso-numero`, para no repetir más de
12 clases en cada paso (cada `li` queda en 7 clases). Centrado con
`flex flex-col items-center` (bloques) más `text-center` (líneas del texto).

### Qué corregí y por qué

- Al quitar los números automáticos con `list-none`, Safari deja de
  anunciar el `<ol>` como lista. Se añadió `role="list"` para recuperarlo, y
  el número se dejó como texto dentro del `h3` («1 Eliges tu café») para que
  el orden se oiga también al navegar por encabezados.
- En el texto de la tarjeta tomate se usó `espresso-900` y no
  `espresso-700`, que sobre tomate no llegaría a 4,5:1 (medido: 5,37:1).
- La verificación avisa de que las pegatinas de número quedan a menos de
  3 px del borde de la pantalla a 360 px: no se cortan, pero si crece el
  giro o el desplazamiento, la n.º 1 se saldría.
- Primera tarea después de registrar el **hook `Stop`** del harness (build +
  `check-estatico` al parar si hay cambios en `maquetacion_tueste/`). No está
  comprobado que se ejecutara en esta sesión: puede activarse en la siguiente.

## 7 · T5 · Cafés del mes (2026-10-11)

### Prompt

"Construye la sección `#cafes` después de «Cómo funciona»: 6 tarjetas
`article` en `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3`, cada una con
foto, origen, proceso, notas de cata, precio («14,90 €», bolsa de 250 g) y
botón «Lo quiero» con `btn-pegatina`. Hover que «se aplasta» con
`motion-safe:`. Fotos de Unsplash en WebP, y créditos solo de páginas
abiertas de verdad."

### Qué me dio

Seis tarjetas tipo «etiqueta de paquete» (Etiopía, Colombia, Kenia,
Guatemala, Brasil, Ruanda): foto `aspect-4/3 object-cover` con una pegatina
girada del proceso (menta = lavado, tomate = natural, crema = honey), una
frase con gracia, notas de cata como chips, precio grande y botón. Tres
utilidades nuevas: `cafe-tarjeta` (forma + subir 2 px y sombra de 6 px al
pasar el ratón o al llegar con el teclado al botón), `cafe-proceso` y
`cata-nota`. Fotos de 600×450 en WebP, de 26 a 71 KB.

### Qué corregí y por qué

- El botón dice «Lo quiero» seis veces: se añadió el café en `sr-only`
  («Lo quiero: Kenia Nyeri») para que un lector de pantalla los distinga.
- El hover de la tarjeta va en la `@utility` (con `@media (hover: hover)` y
  `prefers-reduced-motion: no-preference`), como `btn-pegatina`, en vez de
  repetir 4 clases de estado en las 6 tarjetas. Con `:focus-within` también
  sube al llegar con Tab, para que el teclado vea lo mismo que el ratón.
- Se descartó el zoom de la foto al pasar el ratón (opcional del enunciado):
  con la tarjeta subiendo ya hay bastante movimiento.
- Créditos: cada autor salió de abrir la página de la foto en Unsplash, no
  del nombre del fichero. Todas con la Unsplash License (para el README, T10):
  - `cafe-etiopia.webp` · pariwat pannium · https://unsplash.com/photos/coffee-beans-with-white-cup-S8daAB_nJSg
  - `cafe-colombia.webp` · Carlos Felipe Ramírez Mesa · https://unsplash.com/photos/a-person-holding-a-bunch-of-berries-Ni7Cfu8AIMI
  - `cafe-kenia.webp` · Alin Luna · https://unsplash.com/photos/brown-coffee-beans-on-white-ceramic-bowl-lGl3spVIU0g
  - `cafe-guatemala.webp` · Gerson Cifuentes · https://unsplash.com/photos/shallow-focus-photography-of-coffee-bean-W1VqcpcnSHk
  - `cafe-brasil.webp` · Tina Guina · https://unsplash.com/photos/shallow-focus-photography-of-coffee-beans-in-sack-obV_LM0KjxY
  - `cafe-ruanda.webp` · Eiliv Aceron · https://unsplash.com/photos/person-pouring-water-in-clear-glass-jar-XP5zW2ngk9w
- Por el fallo del enlace inventado en T3, la verificación **abrió por su
  cuenta las 6 páginas**: existen, y autor y licencia coinciden con lo
  apuntado. También miró que las fotos no llevaran texto ni marcas (la báscula
  de la de Ruanda, ampliada, tiene etiquetas ilegibles).
- Lighthouse avisa de que el Rendimiento baja de 97 a 93–94 (la foto del
  hero tarda 3,1 s en vez de 2,6 s): en móvil, Chrome descarga ya 3 fotos de
  tarjetas aunque llevan `loading="lazy"`, y compiten con la del hero.
  Pendiente si se va a por el D7 (más compresión o `srcset`).
## 8 · T6 · Glosario de cata en bento (2026-10-11)

### Prompt

"Construye «¿Qué es eso de las notas de cata?» después de los cafés: 6 cajas
(acidez, cuerpo, dulzor, notas de cata, tueste, proceso) con textos sin
tecnicismos, en un bento: 1 columna en móvil, 2 en tablet y un mosaico en
escritorio con al menos un `col-span-2` y un `row-span-2`, sin huecos. Las
cajas no se pulsan."

### Qué me dio

Una lista `<ul role="list">` con 6 cajas (cada una con su `h3`; descartó
`<dl>` porque dentro de un `<dt>` no cabe un encabezado). Mosaico de
4 columnas × 3 filas en escritorio: Notas 2×2 (`sm:col-span-2 lg:row-span-2`),
Cuerpo 1×2 (`lg:row-span-2`), Tueste y Proceso 2×1, con `lg:auto-rows-fr`
para que todas las filas midan lo mismo. Sin `grid-flow-dense`: el orden del
HTML ya encaja el mosaico. Reutilizó `paso-tarjeta` (forma de pegatina sin
efecto de aplastar) y `cata-nota`, sin tocar `style.css`. Iconos de Lucide.

### Qué corregí y por qué

- Al copiar el icono `droplets` de Lucide se colaron unos números mal pegados
  en el SVG; el agente los corrigió. La verificación descargó los 6 iconos
  originales y comparó los trazados uno a uno: coinciden.
- Pedí que la verificación revisara los textos por si decían algo falso del
  café: ninguno lo es. Solo simplifican (el tueste no es solo tiempo, también
  temperatura), algo aceptable para el público del brief.
- Pendiente para la revisión de diseño: a 1440 la caja grande de Notas y la
  de Cuerpo tienen mucho espacio vacío, y la sombra de la caja oscura
  (espresso sobre espresso) apenas se ve, así que pierde el efecto pegatina.
  Y el nombre `paso-tarjeta` ya no describe todos sus usos.
## 9 · T7 · Planes de suscripción (2026-10-11)

### Prompt

"Construye los planes después del glosario: 3 `article` con nombre propio,
precio al mes, ventajas y botón; el del centro destacado con una etiqueta
«⭐ Más popular» colocada con `absolute`. Precios coherentes con las bolsas
sueltas de los cafés (12,90–16,90 €)."

### Qué me dio

Curioso (1 bolsa, 12,90 €/mes), Mañanero ⭐ (2 bolsas, 24,90 €, 12,45 € la
bolsa) e Insomne (4 bolsas, 45,90 €, 11,48 € la bolsa). Rejilla de 1 columna
estrecha y centrada (`max-w-md`) hasta `lg`, y 3 columnas desde `lg`. El
destacado: fondo tomate, sombra larga y `lg:-my-6` en su celda para que
sobresalga 24 px por arriba y por abajo sin usar `scale`. Dos utilidades
nuevas: `plan-etiqueta` (la pegatina, `absolute` sobre el article, que es
`relative` por `paso-tarjeta`) y `plan-ventaja` (fila con el check de
Lucide).

### Qué corregí y por qué

- A 768 px las tres columnas dejaban unos 165 px de texto por tarjeta y el
  precio grande no cabía: se apilan hasta `lg`.
- El precio se ve como «12,90 € /mes», pero «/mes» lleva `aria-hidden` y un
  «al mes» en `sr-only`: la verificación comprobó en el árbol de
  accesibilidad que se lee «12,90 € al mes». Igual con la ⭐ decorativa.
- El botón del destacado no podía ser tomate sobre tarjeta tomate: va en
  espresso con texto crema (15,3:1).
- La verificación comprobó la aritmética de «te sale a X € la bolsa»
  (24,90 / 2 = 12,45; 45,90 / 4 = 11,475 ≈ 11,48).
- Pendiente:
  - Los botones apuntan a `#newsletter`, que no existe hasta T9.
  - «Más popular» va antes del `h3`, así que quien salta de encabezado en
    encabezado no lo oye.
  - El brief quería empujar el plan de entrada, pero P3 destaca el del
    centro. Lo decido en la revisión de diseño.
## 10 · T8 · Preguntas frecuentes (2026-10-11)

### Prompt

"Construye la FAQ después de los planes: 6 preguntas de principiante en
`<details class="group">` sin JavaScript, con una flecha que gire al abrir
(`group-open:rotate-180`), y la transición con `motion-safe:`. Respuestas que
no contradigan lo que ya dice la página."

### Qué me dio

«Pregunta sin miedo»: a 1440, título en la columna izquierda (1/3) y las
6 preguntas en los 2/3 de la derecha (`lg:grid-cols-3` + `lg:col-span-2`);
apilado por debajo de `lg`. Cada pregunta es una pegatina que pasa a menta al
abrirse (`open:bg-menta`), con la flecha `chevron-down` de Lucide. Tres
utilidades nuevas: `faq-item`, `faq-pregunta` y `faq-flecha`. No metió `h3`
dentro del `summary`, porque el `summary` funciona como un botón y algunos
lectores pierden el encabezado o el botón.

### Qué corregí y por qué

- **La IA no hizo lo que le pedí, y tenía razón.** Le pedí la transición
  con `motion-safe:`, pero `check-estatico` marca FALLA el
  `group-open:rotate-180` si no lleva `motion-safe:` o `motion-reduce:`, y si
  ponía `motion-safe:` en el giro, con movimiento reducido la flecha no
  giraría y se perdería la señal de abierto/cerrado. Dejó
  `transition-transform group-open:rotate-180 motion-reduce:transition-none`:
  con reduce la flecha queda girada al instante, sin animación. La
  verificación lo dio por bueno: lo que se mueve es la transición, y con
  reduce desaparece.
- La flecha la copió de memoria; la verificación la comparó con el fichero
  original de Lucide y coincide.
- El comentario del CSS decía que el foco iba «4 px hacia dentro» y el
  código ponía `-6px`; corregí el comentario.
- Las respuestas no contradicen la página (sin permanencia, envío de nuestra
  cuenta, 250 g ≈ 30 tazas…). Añaden dos datos nuevos compatibles: «2 o 3
  días laborables» y que las cafeteras de cápsulas no sirven.
- Pendiente para diseño: el foco de las preguntas es fino y del mismo color
  que el borde (parece un doble borde), y «eliges otro de los seis» puede
  hacer pensar que los orígenes no cambian cada mes.
## 11 · T9 · Pie con newsletter (2026-10-11)

### Prompt

"Construye el pie: al menos 3 columnas que se apilen en móvil (marca,
enlaces, contacto y redes, newsletter con `id="newsletter"`), copyright con
borde superior. Unidades según el tema 05: letra y espaciados en `rem`, `px`
solo en bordes, sombras y outline. Iconos de redes solo si existen de verdad
en Lucide."

### Qué me dio

Pie oscuro (`espresso-900` con texto crema) de 4 columnas en escritorio,
2×2 en tablet y apilado en móvil. La newsletter es una pegatina menta con
`<label>` visible, `type="email" required autocomplete="email"` y el botón
`btn-pegatina`; es el destino de los botones de los planes. Token nuevo
`--shadow-pegatina-crema` (la sombra espresso no se ve sobre fondo oscuro) y
tres utilidades: `pie-titulo`, `pie-enlace` (44 px de alto táctil) y
`pie-campo`.

### Qué corregí y por qué

- **Unidades**: me di cuenta de que había `px` en el CSS y lo revisé con el
  tema 05 de Lemoncode. Todos estaban en bordes, sombras, outline, el radio
  de píldora y el desplazamiento del botón (que tiene que medir lo mismo que
  su sombra). Letra y espaciados ya iban en `rem` (`--spacing` = 0,25rem).
  El único caso dudoso, el `text-[11px]` del sello, son unidades del
  `viewBox` del SVG y escalan con él: lo dejé. Para el pie se le pasó la
  regla de forma explícita y la verificación lo comprobó.
- Lucide no tiene iconos de marcas: el agente comprobó que `instagram` y
  `youtube` dan 404 y usó iconos genéricos que sí existen (cámara, música,
  vídeo) con el nombre de la red visible. La verificación comparó los 5
  iconos con los originales: coinciden.
- El agente vio una franja crema bajo el pie en una captura a 360 px. La
  verificación demostró que era cosa de la captura: el pie llega justo al
  final de la página.
- Pendiente, honesto para una maqueta: las redes son ficticias
  (`href="#"`) y la newsletter no tiene backend (al enviar, el correo queda
  en la URL y no hay mensaje de confirmación).
## 12 · Revisión de diseño (2026-10-11)

### Prompt

"Revisa el diseño contra el brief (agente `design-reviewer`, que no edita):
como mucho 10 puntos en Crítico / Importante / Sugerencia, con arreglo
concreto. Pregunta clave: ¿podría ser la web de cualquier otro sitio?"

### Qué me dio

Veredicto: se siente «Pegatina pop» de verdad (bordes, sombras duras,
pegatinas giradas, sello, aplastado en todo lo clicable). Sin críticos. Lo
que la acercaba a «web de cualquiera»: un hero de foto de catálogo sin la
pegatina «Tostado el lunes» del brief, y el ☕ del logo, que es el emoji del
sistema y sale lila (el único color fuera de la paleta, en lo primero que se
ve).

### Qué corregí y por qué

- Encontró un fallo de UX que ninguna verificación había visto: «Elegir
  Curioso» llevaba a un formulario que promete un boletín, no la
  suscripción. Las verificaciones solo comprobaban que el destino existiera.
- Resolvió una contradicción del brief (empujar el plan de entrada frente a
  destacar el central) sin romper P3: pegatina «Para empezar» en Curioso y
  «Desde 12,90 € al mes» en el hero.
- Descartó dos avisos de verificaciones anteriores (el espacio en «Lo quiero :
  Etiopía» y las pegatinas de número pegadas al borde).
- Decidí hacer los 10 puntos. Entran en el plan como tareas R1–R6, cada una
  con sus criterios y su `verify-task`.
## 13 · R1 · Utilidades comunes y nombres (2026-10-11)

### Prompt

"Refactor sin cambio visual: `paso-tarjeta` → `tarjeta-pegatina`; una
`@utility pegatina` con lo común de `cafe-proceso` y `plan-etiqueta` (la
posición, el giro y el color, en el HTML); `@utility menu-enlace` para los
enlaces del menú. Mide los estilos antes y después."

### Qué me dio

Las tres utilidades y el HTML limpio: cada pegatina es ahora
`pegatina absolute top-3 left-3 -rotate-3 bg-menta` (se lee dónde va y cómo
gira) y cada enlace del menú tiene una sola clase. Midió 686 valores antes y
después: 0 diferencias.

### Qué corregí y por qué

- **El fallo fue mío, en el plan**: el criterio CR1.3 prometía que
  desaparecería el aviso de «más de 12 clases» del `<ul>` del menú, pero
  `menu-enlace` limpia los enlaces, no el `<ul>`. Sus 20 clases son casi todas
  variantes `md:` (panel en móvil, fila en escritorio): meterlas en una
  `@utility` escondería los breakpoints. Corregí el criterio con una nota que
  lo explica, en vez de forzar un refactor peor.
- Para un refactor, «se ve igual» no vale a ojo: la verificación construyó la
  versión del último commit y la nueva, y comparó 21 420 estilos calculados
  (en reposo, hover, active y foco, a 360/768/1440) y capturas píxel a píxel:
  0 diferencias.

## 14 · R2 · Hero y marca (2026-10-11)

### Prompt

"Cambia el ☕ del logo (cabecera y pie) y del centro del sello por el icono
`coffee` de Lucide, con `currentColor`. Añade una pegatina «Tostado el lunes»
en la esquina de la foto contraria al sello y la línea «Desde 12,90 € al mes,
sin permanencia» bajo los botones del hero. El logo tiene que seguir
llamándose «tueste» para el lector de pantalla."

### Qué me dio

Los tres SVG, la pegatina (`pegatina absolute -bottom-4 left-4 -rotate-3
bg-menta`, reutilizando la utilidad de R1) y la línea del precio, con el
12,90 € en negrita y la misma entrada animada que los botones. Esta tarea
la hizo el agente principal directamente, sin `tailwind-html`, porque eran
cuatro cambios pequeños en `index.html`.

### Qué corregí y por qué

- **El icono, copiado del original y no de memoria**: tras lo del enlace
  inventado de Unsplash en T3, el SVG se descargó del repositorio de Lucide
  y la verificación comparó los 4 trazos uno a uno. Solo cambia el tamaño
  (`size-7/8/9/11` en vez de `width/height="24"`).
- El emoji tenía su tamaño por `text-3xl md:text-4xl` en el círculo del
  sello; al quitarlo, esas clases ya no hacían nada y se quitaron también
  (está dicho en la verificación, para que no parezca que se borró algo
  para que pase).
- El SVG del sello no lleva `aria-hidden` propio porque todo el sello ya lo
  lleva. La verificación lo confirmó en el árbol de accesibilidad real de
  Chrome. El snapshot del MCP de Playwright sí lo enseñaba, pero es una
  rareza de esa herramienta.