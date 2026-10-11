# Brief: Tueste — tostadero de café de especialidad

Landing del laboratorio final del módulo 1 (enunciado en `proyecto/enunciado.md`:
obligatorios O1–O9, opcionales P1–P7, desafíos D1–D7).

## Para quién y para qué
- **Público:** curiosos que se inician en el café de especialidad. Beben café
  de supermercado y quieren probar algo mejor, pero les intimidan los
  tecnicismos (tueste, notas de cata, en grano o molido…).
- **Objetivo:** que pierdan el miedo y se suscriban (sobre todo al plan de
  entrada). La web explica sin tecnicismos y con humor; los datos de cata
  siguen ahí para quien sepa algo más.

## Páginas / secciones
Una sola página. Orden: **primero explicar, luego vender**.
1. **Cabecera:** logo + menú (Cafés, Planes, FAQ) + botón «Suscríbete».
2. **Hero:** titular potente, subtítulo, botón principal «Suscríbete» y
   secundario «Ver cafés», foto + sello giratorio.
3. **Cómo funciona:** 3 pasos (eliges → tostamos → te llega a casa). Sube
   antes de los cafés porque al principiante primero hay que quitarle el miedo.
4. **Cafés del mes:** ≥ 6 tarjetas (origen, proceso, notas de cata, precio,
   botón).
5. **«¿Qué es eso de las notas de cata?»:** mini glosario visual (acidez,
   cuerpo, dulzor, tueste, proceso…) en **bento grid** (P6).
6. **Planes:** 3 planes, el del centro destacado con etiqueta «⭐ Más popular»
   (P3).
7. **FAQ:** plegable con `<details>`/`<summary>` (P4).
8. **Pie:** ≥ 3 columnas (enlaces, contacto, redes) + newsletter + copyright.

## Móvil
- Mobile first. Tarjetas 1 → 2 → 3 columnas (`sm` / `lg`).
- **Menú:** en móvil, hamburguesa **sin JS** con `<details>`/`<summary>` (D1),
  usable con teclado. Desde `md` el menú se ve en línea.
- Hero, pasos, bento, planes y columnas del pie se apilan en una columna en
  móvil.

## Contenido
- Real: nada (Tueste es ficticio).
- Inventado (por la IA, con datos realistas):
  - 6–8 cafés con orígenes reales (Etiopía Yirgacheffe, Colombia Huila, Kenia
    Nyeri, Guatemala, Brasil, Ruanda…), proceso (lavado / natural), notas de
    cata creíbles y precios entre 12 y 17 €.
  - 3 planes con nombres propios y precios coherentes.
  - FAQ con preguntas de principiante («¿Molido o en grano?», «¿Puedo
    cancelar?», «¿Qué cafetera necesito?»…).
  - **Fotos** de Unsplash / Pexels (granos, tueste, taza, paquete) con `alt`
    descriptivo; **iconos Lucide** para los pasos y el glosario.
- El alumno puede retocar textos sobre la marcha; cada corrección a lo que dio
  la IA va a la bitácora (O9).

## Personalidad
**Fresco, curioso, desenfadado.** Café de especialidad sin postureo: tutea,
titulares con gracia, nada de «artesano cálido con marrones» de manual.

## Dirección visual — «Pegatina pop»
El paquete de café como objeto gráfico.
- Paleta (a `@theme` como tokens):
  - Fondo crema `#FFF4E0`.
  - Marca *espresso* `#2B1A12`, con escala de tonos (al menos 50 / 500 / 900).
  - Acento tomate `#FF5A36`.
  - Toque menta `#9ED9C3`.
  - Contraste: sobre tomate, texto *espresso* (≈ 5,4:1), **nunca blanco**
    (≈ 3:1, no pasa). El tomate no sirve como color de texto sobre crema;
    para texto, *espresso*.
- Tipografía: **Bricolage Grotesque** (titulares, gorda, con carácter) +
  **DM Sans** (texto). Se instalan con `@fontsource-variable`, no desde
  Google Fonts.
- Rasgos: bordes gruesos de 2 px en *espresso*, sombras duras desplazadas
  sin difuminar (token de sombra en `@theme`), etiquetas giradas tipo
  pegatina («Tostado el lunes»).
- Referencia de la cabecera: `☕ tueste` en minúscula y gordo a la izquierda ·
  enlaces sencillos · botón **Suscríbete** tomate con borde *espresso* y
  sombra dura que «se hunde» al pulsarlo.

## Detalle memorable
1. **Todo lo clicable «se aplasta»:** botones y tarjetas con sombra dura;
   al pasar el ratón suben un poco y la sombra crece; al pulsar se hunden y la
   sombra desaparece. Movimientos con `motion-safe:`.
2. **Sello giratorio en el hero:** sello circular SVG con «Recién tostado ·
   Recién tostado ·» en `textPath`, girando despacio en la esquina de la foto
   (animación propia en `@theme`, con `motion-safe:`).

## Movimiento
**Sutil.** Solo: hover/pulsación de botones y tarjetas, el sello girando y
la entrada en cascada del hero (`animate-aparecer` con retardos). Nada al hacer
scroll, sin JS de animación. Todo lo que se mueve lleva `motion-safe:` (P7).

## Stack
HTML + Vite 8 + Tailwind v4 con `@tailwindcss/vite` (opción 1 del enunciado),
copiado de `_plantilla-html/`. Sin frameworks JS.

## Decidido por la IA (revísalo)
- Menú de cabecera: Cafés, Planes, FAQ (el glosario y «Cómo funciona» solo
  por scroll).
- 3 planes (no 2) para que tenga sentido destacar el del centro.
- Dentro de la escala *espresso* habrá tonos intermedios para bordes y textos
  secundarios; la menta solo como fondo de pegatinas o bloques, no para texto.
- El plan destacado va en el centro en escritorio; en móvil, en su orden natural
  (2.º).
- Opcionales/desafíos que ya salen del brief: P3, P4, P6, P7, D1. El resto
  (P1 modo oscuro, P2 cabecera cristal, P5 formulario con estados, D4
  `@utility`…) se decide en el plan.
- El detalle C («etiqueta de paquete» en las tarjetas: barra de intensidad,
  notas como pegatinas) queda como idea para el diseño de las tarjetas si
  sobra tiempo.
