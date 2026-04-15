# 🎭 Teatro Player

Reproductor de videos para teatro en vivo. Alterna loops de escena con transiciones, controlado por un operador.

## Cómo funciona

- Cargás MP4s en orden: **loop escena 1 → transición 1→2 → loop escena 2 → transición 2→3 → …**
- Cada loop se repite sin corte mientras la escena transcurre.
- El operador aprieta **ESPACIO** (o el botón rojo en pantalla) para cambiar de escena.
- El loop actual termina su iteración, la transición se reproduce entera, y el próximo loop arranca en bucle hasta el siguiente cambio.

## Uso

Abrí `teatro.html` en tu navegador (Chrome/Safari/Firefox). O serví la carpeta con un server local:

```bash
python3 -m http.server 8765
# luego abrir http://localhost:8765/teatro.html
```

### Controles durante la función

- **ESPACIO** / click en botón rojo → cambiar escena
- **ESC** → salir
- **H** → ocultar/mostrar HUD

## Features

- **Auto-detección de orden** para nombres tipo `1_...`, `t_1_2_1`, `2_...`, etc.
- Dos elementos `<video>` alternados con pre-carga del siguiente → cambios sin flash negro.
- Loop manual con doble detección (`timeupdate` + `ended`) para handoff robusto.
- Pantalla completa automática.
