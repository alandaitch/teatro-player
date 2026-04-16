# Teatro Player

Reproductor de video para teatro en vivo. Un archivo HTML, abrís en el navegador, arrastrás tus loops y transiciones, y cambiás de escena apretando ESPACIO.

Alternativa gratuita y liviana a software profesional como QLab (US$1400+) para el caso específico de **loops + transiciones controladas por operador**.

---

## Advertencias

- **Proyecto personal, sin mantenimiento garantizado.** Lo armé para una necesidad puntual. Si encontrás un bug, podés abrir un issue o forkear — pero no prometo responder.
- Probado en Chrome y Safari en macOS. En Windows/Linux *debería* andar pero no lo testeé.
- Depende de que tu navegador pueda decodificar los códecs de tus videos. Si un video anda en QuickTime pero no en el player, probá re-encodearlo a H.264/MP4.
- **No es QLab.** No hace audio cues, MIDI, OSC, ni sincronización multi-máquina. Es un reproductor de video, punto.
- Todos los archivos quedan en tu máquina — no se sube nada a ningún servidor.

---

## Cómo funciona

1. Cargás los archivos en orden: `loop escena 1 → transición 1→2 → loop escena 2 → transición 2→3 → …`
2. Cada **loop** se repite sin corte mientras la escena transcurre.
3. El operador aprieta **ESPACIO** (o el botón rojo) para cambiar de escena.
4. El loop actual termina su iteración, la **transición** se reproduce completa, y el próximo loop arranca en bucle hasta el siguiente cambio.

## Setup

Abrí `teatro.html` en el navegador. Eso es todo.

Si preferís servirlo localmente:

```bash
python3 -m http.server 8765 --directory teatro_player
# http://localhost:8765/teatro.html
```

## Uso

### Setup de la función

- Arrastrá videos e imágenes al drop zone (o hacé click). Acepta `.mp4 .mov .webm .mkv .jpg .png .gif .webp`.
- Cada fila muestra si es **Loop** o **Transición**. Click en el pill para cambiar.
- Arrastrá el handle `⋮⋮` a la izquierda para reordenar.
- `📝 Notas` para agregar una nota que el operador va a ver en su panel durante la función (no se proyecta).
- `Cmd+Z` deshace, `Shift+Cmd+Z` rehace. Al eliminar aparece un toast con botón de deshacer.
- `💾 Guardar proyecto` exporta un JSON con el orden, tipos, etiquetas y notas. `📂 Cargar proyecto` lo restaura (tenés que cargar los mismos archivos primero — matchea por nombre).
- `Auto-detectar orden` reordena según el nombre si usás convención `1_loop.mp4`, `t_1_2.mp4`, `2_loop.mp4`, etc.

### Durante la función

El programa abre dos ventanas:

- **Panel de control** (tu monitor): preview actual y siguiente, cola de escenas, botón de cue grande, controles secundarios.
- **Proyección** (pantalla del teatro): solo el video, sin HUD, ponela en fullscreen con `F`.

Controles del operador:

- `ESPACIO` o click en el botón rojo → **cambiar escena**. El loop termina su iteración, dispara la transición, y el próximo loop arranca.
- `←` → escena anterior.
- `B` → blackout (pantalla negra, sin perder la posición).
- `P` → pausar / reanudar.
- `✂ Cortar loop · pasar YA` → corta el loop actual en el instante y salta a la transición sin esperar. Para emergencias o pérdidas de timing.
- Click en el preview "En espera · Siguiente" → reproducción muteada del próximo video en el panel de control, para chequearlo sin mandar audio a la sala.
- Arrastrá el splitter entre el panel central y la cola para ajustar el ancho.

### Si se cierra la proyección por error

Aparece un banner rojo flasheante en el panel de control. Click → reabre la ventana y retoma en la escena donde estabas.

## Imágenes

Además de videos, aceptás imágenes fijas:

- **Imagen como loop**: se queda en pantalla hasta que apretás cue.
- **Imagen como transición**: se muestra 3 segundos y auto-avanza al próximo loop.

## Arquitectura

- Un solo archivo `teatro.html` — sin build, sin dependencias.
- Panel de control y proyección son la misma página abierta dos veces (la segunda con `?mode=output`).
- Se comunican por `BroadcastChannel` (no hay servidor).
- Los archivos nunca salen de tu máquina: se pasan como `File` objects entre ventanas y cada una crea sus propias URLs `blob:` localmente.

---

## Más

Si te interesa lo que hago — vibe coding, agentes de IA, automatizaciones personales — seguime en [**x.com/alandaitch**](https://x.com/alandaitch).
