# Instrucciones para Copilot

## Arquitectura

- Es un clon de Asteroids en JavaScript vanilla y Canvas 2D. No usa dependencias, módulos, bundler ni paso de compilación.
- `index.html` define el canvas de 800 × 600 y carga `game.js` mediante una etiqueta `<script>` normal. Toda la lógica vive en `game.js`; sus variables y clases de nivel superior son globales por diseño.
- El arranque ocurre al final de `game.js`: `initGame()` prepara el estado inicial y `requestAnimationFrame(loop)` inicia el ciclo. Cada frame calcula `dt`, limitado a 0,05 segundos, y llama a `update(dt)` y `draw()`.
- `update(dt)` gestiona la entrada, la simulación, las colisiones y las transiciones de estado. Los estados son `'playing'`, `'dead'` y `'gameover'`. `draw()` renderiza las entidades y el HUD.
- El estado de la partida se mantiene en variables globales, incluidas las colecciones `bullets`, `asteroids`, `particles`, `powerups` y `comets`. `initGame()` las inicializa y `nextLevel()` prepara la siguiente ronda.

## Patrones del proyecto

- Las entidades son clases con `update(dt)`, `draw()` y una marca `dead`. Actualiza las entidades en `update()`, dibújalas en `draw()` y elimina las marcadas como muertas filtrando sus colecciones.
- El movimiento usa segundos mediante `dt`. Las entidades que atraviesan el espacio aplican `wrap(valor, dimensión)` en ambos ejes; conserva ese comportamiento toroidal.
- Mantén las constantes de ajuste (velocidad, radio, duración, puntuación) cerca de la clase o función que las usa. Para el tamaño del canvas, actualiza conjuntamente `index.html` y las constantes `W`/`H` de `game.js`.
- La entrada se lee mediante los objetos globales `keys` y `justPressed`; usa `pressed(code)` para acciones de una sola pulsación y evita añadir listeners redundantes.
- Los efectos visuales se dibujan con el `ctx` global del canvas. Sigue el uso existente de `ctx.save()`/`ctx.restore()` al cambiar transformaciones o estilos locales.
- La persistencia existente usa `localStorage` directamente. No añadas un sistema de configuración o persistencia alternativo sin necesidad.

## Guía de cambios y clean code

- Antes de cambiar código, localiza el flujo y los patrones relacionados en `game.js`; implementa el cambio en el punto de responsabilidad existente.
- Prefiere cambios pequeños y directos. Evita duplicar estado, lógica de colisión, listeners o abstracciones que no aporten una necesidad concreta.
- Usa nombres descriptivos y constantes para valores de ajuste; conserva el estilo de JavaScript y la estructura del archivo.
- Los textos visibles, comentarios nuevos y documentación del proyecto deben estar en español.
- No introduzcas dependencias, módulos, herramientas de build o infraestructura de pruebas para cambios ordinarios.
- No hay suite de pruebas, linter ni build configurados. Para validar cambios, comprueba el diff y abre `index.html` en un navegador (o sirve el directorio con `npx serve .`) para probar la ruta modificada.
