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
