# Contrastar.cl — Frontend

Agregador de medios chilenos que agrupa noticias por evento real y muestra cómo lo cubre cada medio según su posición editorial. Inspirado en Ground News, gratuito y sin publicidad.

🌐 **[contrastar.cl](https://contrastar.cl)**

---

## ¿Qué hace?

Toma noticias de 70 medios chilenos con espectro editorial de −2 (izquierda) a +2 (derecha), las agrupa por evento concreto usando IA, y muestra el mismo hecho desde distintas perspectivas editoriales — sin que el usuario tenga que abrir cinco diarios.

---

## Páginas

### `index.html` — Feed principal
- Carga clusters de eventos desde Google Sheets publicado como CSV
- Filtra por tendencia editorial (izquierda / centro / derecha / plural) y categoría
- Muestra barra de espectro por cluster con los medios que lo cubrieron
- El timestamp de última actualización se deriva del cluster más reciente (sin hoja CONFIG separada)

### `evento.html` — Vista de evento individual
- Muestra todas las noticias de un cluster agrupadas por columna: izquierda / centro / derecha
- Recibe el `id_cluster` por parámetro en la URL

### `medios.html` — Catálogo de medios
- Lista los 70 medios con score editorial, propietario y región
- Filtros por tendencia y alcance (Nacional / Regional)
- Buscador por nombre, propietario o región
- **Sistema de votación ciudadana**: los usuarios pueden señalar si creen que un medio está mal calibrado. Los votos no cambian el score directamente — son señal de revisión para el equipo
- Anti-spam vía FingerprintJS (open source, sin API key)
- Panel de auditoría oculto: `Ctrl+Shift+A`

---

## Stack

| Capa | Tecnología |
|---|---|
| Hosting | GitHub Pages |
| Backend / procesamiento | Google Apps Script |
| Base de datos | Google Sheets (NOTICIAS_ACTIVAS, CLUSTERS_ACTIVOS, MEDIOS, + archivos) |
| IA agrupadora | Claude Haiku (`claude-haiku-4-5`) vía API Anthropic |
| Fuentes | RSS de 70 medios chilenos |

El frontend es HTML/CSS/JS estático puro, sin frameworks ni dependencias de build. Lee datos desde Google Sheets publicado como CSV en cada carga.

---

## Espectro editorial

Cada medio tiene un `tendencia_score` de −2.0 a +2.0 asignado manualmente por el equipo:

| Rango | Descripción |
|---|---|
| −2.0 a −1.5 | Izquierda (PC Chile, PTS, colectivos activistas) |
| −1.4 a −0.5 | Centro-izquierda (CIPER, The Clinic, Cooperativa, CNN Chile) |
| −0.4 a +0.4 | Centro (TVN, Fast Check, Bío-Bío, El Dínamo) |
| +0.5 a +1.0 | Centro-derecha (Canal 13, La Tercera, Mega) |
| +1.1 a +2.0 | Derecha (El Mercurio/Emol, El Líbero, Radio Agricultura) |

Los scores se actualizan en la hoja MEDIOS del Google Sheet. Un script de Apps Script (`actualizarScore.gs`) propaga automáticamente los cambios a todas las hojas de noticias y clusters vía trigger `onEdit`.

---

## Estructura de datos relevante

**`distTendencia`** en clusters: `{ idMedio: score }` — cada medio aparece una sola vez, sin importar cuántas noticias tenga en el cluster. Esto garantiza representación justa en el espectro.

**`totalNoticias`** = `Object.keys(distTendencia).length` — número de medios únicos, no de noticias.

---

## Archivos relacionados (no en este repo)

| Archivo | Descripción |
|---|---|
| `agrupador.gs` | Script principal de Apps Script: ingesta RSS, agrupación IA, guardado en Sheets |
| `actualizarScore.gs` | Trigger `onEdit` que propaga cambios de score desde MEDIOS a todas las hojas |

---

## Costos operativos

| Concepto | Estimado |
|---|---|
| Claude Haiku (agrupador, ~cada 4h) | ~$20/mes |
| Hosting (GitHub Pages) | $0 |
| Google Sheets | $0 |
| **Total** | **~$20/mes** |

---

## Estado

MVP en producción · Recibiendo feedback · Marzo 2026
