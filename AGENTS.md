# Reglas de trabajo — My Orca Orden

> Reconstruido tras sobrescribir el original por no verificar antes de escribir.
> **Por favor edita y agrega lo que falte** — esto es lo que recuperé del contexto.

## ⚠️ VERACIDAD ANTE TODO (regla #1, no negociable)

La veracidad de los pasos y ajustes es **imprescindible**. El objetivo principal es ser **certero**, no sonar productivo.

1. **Nunca afirmar "está hecho" sin verificación real.** Verificar ≠ leer el código. Verificar = ejecutarlo y confirmar el resultado observable.
2. **Si la verificación fue parcial, decirlo con precisión.** Ejemplo honesto: "verifiqué el DOM y la lógica, pero NO puedo ver la animación visualmente — necesito que tú la revises". No convertir verificación parcial en conclusión total.
3. **Reportar el nivel de confianza por separado del resultado.** "El test pasó" no es "se ve bien". Un test de DOM no valida una animación visual.
4. **No decorar el reporte.** Nada de "transcripción 1:1" / "idéntico al demo" si en realidad es una adaptación con diferencias reales. Decir qué es igual y qué es distinto.
5. **Si algo falló o no se pudo verificar, decirlo primero**, antes de lo que sí funcionó.

## ⏱️ Eficiencia (aprendido a las malas — 8h en una animación simple)

- Una animación "simple" no merece más de 1-2 iteraciones de prueba. Si falla dos veces, **parar y repensar el enfoque entero** (p. ej. ¿modal vs in-place?) en lugar de retocar parámetros.
- No iterar a ciegas: si no se puede observar el resultado visual, **preguntar al usuario o pedir screenshot** en vez de seguir ajustando números.
- Errores de sintaxis de un solo carácter no son aceptables a estas alturas: revisar el diff antes de correr.
- **Antes de escribir/sobrescribir un archivo, verificar que no existe ya** (un `ls` con espacios mal escapados me hizo creer que este archivo no existía y lo perdí).

## 📁 Estructura del proyecto

- `biblioteca-gsap/index.html` — biblioteca de animaciones GSAP (53 demos, grid de cards, popup modal estilo "Orchestrated easeReverse").
- `demo-gsap.html` — demo de referencia (raíz).
- `backup/` — backups con fecha, ej. `demo-gsap-2026-08-24-flip-flujo.html`.

## 🛠️ Cómo correr

- Todo es **HTML local con CDN GSAP 3.15.0** — abrir con doble click, no hay build ni servidor.
- Verificación headless: Chrome de Windows desde WSL:
  `"/mnt/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --enable-logging=stderr --virtual-time-budget=16000 --dump-dom "file:///C:/..."`

## 🎬 GSAP (notas técnicas verificadas)

- `easeReverse` existe desde GSAP 3.13+; en 3.15.0 funciona.
- `.kill()` **no** revierte props inline — hay que `clearProps` manualmente.
- En headless Chrome con `--virtual-time-budget`, el ticker de GSAP se dilata (~1s de tween ≈ 2-3s virtuales): medir estados finales con waits largos (9000ms+), no con waits reales.
- CSS `transition` en la misma propiedad que GSAP anima = conflicto garantizado (nunca ambos).
- `inner.querySelectorAll(":scope > *")` no funciona como se espera dentro de elementos anidados — usar `gsap.utils.toArray(inner.children)` o `[...inner.children]`.

## 📋 Convenciones

- Español en UI y comentarios de código.
- Archivos nuevos de la biblioteca: nombrar con fecha en `backup/`.
- Commits en español, formato `feat: celdas #N-M — descripción`.
