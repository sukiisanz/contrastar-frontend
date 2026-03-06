# Contrastar.cl — Frontend

Agregador de medios chilenos que agrupa noticias por evento real y muestra cómo lo cubre cada medio según su posición editorial. Inspirado en Ground News, gratuito y sin publicidad.

🌐 **[contrastar.cl](https://contrastar.cl)**

---

## ¿Qué hace?

Toma noticias de 70 medios chilenos con espectro editorial de −2 (izquierda) a +2 (derecha), las agrupa por evento concreto usando IA, y muestra el mismo hecho desde distintas perspectivas editoriales — sin que el usuario tenga que abrir cinco diarios.

---

## Páginas

- **Feed principal** — clusters de eventos con barra de espectro editorial por cobertura
- **Vista de evento** — todas las noticias de un evento agrupadas por tendencia editorial
- **Catálogo de medios** — los 70 medios con su score, propietario y región. Incluye sistema de votación ciudadana para señalar medios que podrían estar mal calibrados

---

## Stack

| Capa | Tecnología |
|---|---|
| Hosting | GitHub Pages |
| Backend / procesamiento | Google Apps Script |
| Base de datos | Google Sheets |
| IA agrupadora | Claude Haiku vía API Anthropic |
| Fuentes | RSS de 70 medios chilenos |

Frontend estático puro — sin frameworks ni dependencias de build.

---

## Espectro editorial

Cada medio tiene un score de −2.0 a +2.0 asignado por el equipo basado en línea editorial, propiedad y cobertura histórica. La comunidad puede votar si cree que un medio está mal ubicado — los votos son señal de revisión, no decisión automática.

| Rango | Zona |
|---|---|
| −2.0 a −0.5 | Izquierda y centro-izquierda |
| −0.4 a +0.4 | Centro |
| +0.5 a +2.0 | Centro-derecha y derecha |

---

## Estado

MVP en producción · Recibiendo feedback · Marzo 2026
