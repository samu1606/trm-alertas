# 💵 TRM Alertas — Setup Guide

> TRM Colombia diaria + alertas de tipo de cambio por WhatsApp

---

## Arquitectura

```
open.er-api.com (gratis) ──cada 6h──▶ n8n ──Evolution API──▶ WhatsApp
```

**Sin API key. Sin registro. Sin límites.** La API es 100% gratuita.

---

## API

- **Endpoint:** `https://open.er-api.com/v6/latest/USD`
- **Response:** JSON con 160+ monedas, incluyendo COP
- **Rate limit:** ~1500 requests/mes (plan gratuito de ExchangeRate-API)
- **Frecuencia de actualización:** Cada 24h (datos diarios oficiales)

---

## Paso 1: Importar workflows en n8n

1. Entrar a http://148.230.90.171:5678
2. Importar `001-trm-diario.json` — envío diario 8am
3. Importar `002-trm-umbral.json` — alertas por umbral
4. Reemplazar `{{EVO_API_KEY}}` y `{{PHONE_NUMBER}}`

## Paso 2: Configurar umbrales (002-trm-umbral)

Editar nodo "Verificar Umbral":
```javascript
const UMBRAL_ALTO = 3300;  // Alertar si TRM > $3,300
const UMBRAL_BAJO = 3100;  // Alertar si TRM < $3,100
```

## Paso 3: Horario (001-trm-diario)

- Cambiar schedule a "Cron": `0 13 * * *` (8am Colombia = 13:00 UTC)
- O usar "Every 24 hours"

## Paso 4: Activar

Click ⚡ Activate en ambos workflows.

---

## Workflows incluidos

| # | Archivo | Función |
|---|---------|---------|
| 1 | `001-trm-diario.json` | Envío diario con TRM + tasas LATAM |
| 2 | `002-trm-umbral.json` | Alerta cuando la TRM cruza umbrales |

---

## Rates incluidos

- COP (Colombia)
- MXN (México)
- BRL (Brasil)
- PEN (Perú)
- CLP (Chile)
- EUR (Euro)
