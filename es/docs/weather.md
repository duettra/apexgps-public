# Tiempo

ApexGPS puede mostrar las condiciones actuales, una previsión por horas y un avance de 7 días para cualquier punto que te interese.

## Lo que verás

El tiempo está **activado por defecto** desde 1.32.2. En cuanto tengas un punto GPS:

- Aparece un pequeño chip sobre la barra de stats con las condiciones de tu posición actual.
- Tocar un waypoint muestra una línea «Tiempo aquí» en el panel del waypoint.
- Tocar cualquiera de las dos abre una hoja con el desglose completo.

Cuando el tiempo está activado, tu latitud/longitud se envía a Open-Meteo (una API pública gratuita) en cada consulta de previsión. Si prefieres que no se hagan llamadas de red, abre **Ajustes → Tiempo** y desactiva **Mostrar tiempo** — el chip y la línea «Tiempo aquí» desaparecen, y ApexGPS deja de contactar con Open-Meteo.

## Lo que muestra el chip

El chip rota entre varios estados:

| Estado | Aspecto | Significado |
|---|---|---|
| Reciente | un icono del tiempo y luego `24° · 12 km/h NE` | Actualizado en los últimos 15 minutos. |
| Envejeciendo | `… · hace 32 min` | Más de 15 minutos pero probablemente exacto. |
| Caducado / sin red | `⚠ … · hace 2 h` (gris) | Más de una hora o sin conexión. Toca el chip para refrescar manualmente. |

El chip no desaparece una vez activado el tiempo — solo cambia su aspecto para indicar la fiabilidad de los datos.

## La hoja de previsión

Toca el chip (o la línea «Tiempo aquí» de un waypoint) para abrir la hoja completa. De arriba a abajo:

- **Ahora** — icono grande del tiempo + temperatura, «sensación», y una fila para viento / humedad + precip máx / punto de rocío + UV / presión + ocaso.
- **Franja de previsión con selector 24h / 8h / 2h** — ocho iconos (versión de día o de noche según el orto / ocaso local de ese paso) con la temperatura y, debajo, la **probabilidad de precipitación**. El porcentaje solo aparece a partir del 20 %, para que una previsión seca se mantenga despejada. El pequeño selector sobre la franja cambia la ventana:
  - **24h** — todo el día de un vistazo, una celda cada 3 horas.
  - **8h** — las próximas ocho horas, una celda por hora.
  - **2h** — las próximas dos horas en pasos de 15 minutos, para detectar un chubasco o una tormenta a punto de llegar (hasta ~45 minutos antes que la vista horaria). La opción **2h** aparece donde hay datos de 15 minutos (gran parte de Europa y Norteamérica); en otros lugares verás solo **24h** y **8h**.
- **Tendencia de presión** (verde) — gráfico de 24 horas, útil para detectar un frente que se acerca.
- **Tendencia de humedad** (azul claro) — gráfico de 24 horas.
- **Próximos 7 días** — una fila de iconos por día con máxima / mínima y la probabilidad de precipitación cuando alcanza el 20 % o más.

Hay un botón refrescar (↻) en la cabecera de la hoja que ignora la caché y obtiene datos frescos. Sin conexión, los datos previos permanecen y el indicador de caducidad del chip sigue visible.

## Previsiones conscientes de la altitud en cumbres

Si un waypoint tiene altitud guardada (manual, fijada por GPS o importada de un GPX con `<ele>`), esa altitud se envía a Open-Meteo para que el modelo sepa que es una cumbre y no un punto de valle a la misma lat / lon. Lo mismo para el chip: usa tu altitud GPS. En una cumbre de 1500 m esto suele corregir la temperatura prevista en ~9 °C respecto a una consulta solo por coordenadas.

## Auto-refresco

Con el tiempo activado, el chip se actualiza cada 15 minutos mientras la app está abierta y en línea. En modo avión, el chip mantiene los últimos datos conocidos con un indicador «caducado» hasta reconectar.

## Cómo leer los iconos

Los símbolos del tiempo los dibuja ApexGPS, así que se ven **exactamente igual en cualquier teléfono**. El color
transmite el significado:

| Color | Significado |
|---|---|
| Azul | Precipitación — llovizna, lluvia, aguanieve o nieve |
| Ámbar | Cielo despejado (el sol; de noche, el cielo despejado muestra una luna gris sencilla) |
| Gris | Nubes o niebla |
| Rojo | Tormenta |

El detalle del icono sigue a la previsión: lluvia débil, lluvia y lluvia fuerte son tres símbolos distintos, igual que
nieve, nieve fuerte y aguanieve. Como la probabilidad de precipitación también aparece como número, nunca dependes de
interpretar un símbolo pequeño.

Hasta la versión 1.47.1 estos símbolos eran emojis que aportaba el propio teléfono, de modo que dos teléfonos podían
mostrar dibujos distintos — o ninguno — para la misma previsión. Resuelto desde 1.48.0.

## Fuentes de datos

Las previsiones vienen de **[Open-Meteo](https://open-meteo.com)**, una API pública gratuita que combina ECMWF, GFS, ICON y otros modelos globales de primer nivel. Gratis para uso personal, sin cuenta ni clave de API.

## Limitaciones

- **Lluvia convectiva en regiones áridas** es difícil de predecir para cualquier modelo. Espera fallos ocasionales en tormentas relámpago en zonas como las montañas Hajar de los EAU. La probabilidad es honesta al respecto, pero los eventos localizados pueden caer fuera de la rejilla.
- **Avisos de fenómenos severos** no forman parte de esta función. ApexGPS no envía notificaciones push cuando se acercan tormentas.
- **Tiempo a lo largo de una ruta** no está soportado — las previsiones son consultas por punto, no «cómo estará el tiempo en este sendero dentro de 2 horas». Puedes tocar waypoints individuales a lo largo de una ruta para comprobar.
