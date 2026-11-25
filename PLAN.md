# Amigo invisible familiar — plan de trabajo

## Objetivo

Una página web estática (un único `index.html`, sin servidor ni dependencias) para hacer el sorteo del amigo invisible de la familia (más de 15 personas), con exclusiones, en la que cada participante recibe por WhatsApp un enlace personal que solo le muestra a quién le regala.

## Cómo funcionará

1. **El organizador** abre la página, escribe la lista de participantes, define las exclusiones y pulsa "Sortear".
2. La página genera **un enlace por persona**. El organizador ve solo nombres y botones ("Enviar por WhatsApp" / "Copiar enlace"), **nunca el resultado**.
3. **Cada participante** abre su enlace en el móvil, ve "Hola, Ana" y un botón "Descubrir" que revela a quién le toca (así nadie lo ve de reojo).

## Requisitos

- **Varios sorteos a la vez**: habrá dos (primos y adultos). Cada sorteo tiene su nombre, su lista, sus grupos, sus exclusiones y sus enlaces, y se cambia entre ellos con pestañas. El nombre del sorteo aparece en el mensaje y en la página del participante, para que quien esté en los dos (o los padres que reenvían el enlace de un niño) sepa de cuál es cada enlace.
- **Participantes**: pegar la lista de nombres (uno por línea) o añadirlos uno a uno. Se avisa si hay nombres repetidos.
- **Exclusiones**:
  - **Grupos / hogares**: las personas del mismo grupo no se regalan entre sí (parejas, gente que vive junta).
  - **Exclusiones sueltas**: "A no le regala a B" para casos concretos.
- **Sorteo**:
  - Nadie se regala a sí mismo y se respetan todas las exclusiones.
  - Aleatoriedad con `crypto.getRandomValues`.
  - Si las reglas hacen el sorteo imposible (p. ej. un grupo con más de la mitad de la gente), se dice claramente en vez de quedarse colgado.
- **Enlaces**:
  - El resultado va en el fragmento de la URL (`#...`), codificado para que no se lea a simple vista. El fragmento no llega a ningún servidor.
  - Botón de WhatsApp con `https://wa.me/?text=...`, que abre WhatsApp para elegir el contacto, con un mensaje ya redactado.
- **Guardado**: la lista y el último sorteo se guardan en el navegador del organizador (`localStorage`) para poder reenviar un enlace perdido sin repetir el sorteo. Repetir el sorteo pide confirmación porque invalida los enlaces ya enviados.
- **Diseño**: en español, pensado primero para móvil y con un tono festivo pero sencillo.

## Límite de privacidad (a tener en cuenta)

Sin servidor, quien tenga un enlace puede abrirlo. El organizador no ve los resultados en pantalla, pero técnicamente podría abrir los enlaces de otros antes de enviarlos. Para una familia es suficiente; evitarlo del todo exigiría un servidor con cuentas.

## Fases

1. **Esqueleto**: `index.html` con las dos vistas (organizador / participante) según haya o no fragmento en la URL.
2. **Vista organizador**: pestañas de sorteos, participantes, grupos y exclusiones sueltas, guardado en `localStorage`.
3. **Algoritmo de sorteo**: búsqueda con vuelta atrás en orden aleatorio, con detección de casos imposibles.
4. **Enlaces y WhatsApp**: codificar cada asignación, botones de copiar y WhatsApp, y la confirmación al repetir el sorteo.
5. **Vista participante**: saludo, botón de revelar, mensaje si el enlace está dañado.
6. **Pruebas**:
   - 1.000 sorteos automáticos con una familia de ejemplo de más de 15 personas y grupos, comprobando que nunca se rompe ninguna regla.
   - Casos imposibles (grupo demasiado grande, 2 personas en el mismo grupo).
   - Abrir los enlaces en vista móvil.
7. **Publicación**: subirla a un alojamiento gratuito para que la familia pueda abrir los enlaces (por decidir: GitHub Pages, Netlify o similar).

## Estado (25 de noviembre de 2026)

- Fases 1 a 6 hechas: la página está en `index.html` y probada (1.000 sorteos sin fallos, enlaces en vista móvil, enlaces cortados, dos sorteos a la vez).
- Fase 7 hecha: publicada en GitHub Pages en https://pasango.github.io/amigo-invisible/ (repositorio `pasango/amigo-invisible`, rama `main`). Cada `git push` a `main` la vuelve a publicar.
- Por decidir: si se usa un dominio propio. Hay que decidirlo antes de hacer los sorteos de verdad, porque las listas se guardan por dirección web.

## Fuera de alcance por ahora (se puede añadir después)

- Lista de deseos, presupuesto y fecha en el mensaje, no repetir el emparejamiento del año pasado.
- Envío automático por email.
