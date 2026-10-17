# Kling 2.6 Motion Control API (`kling-v2-6-motion-control`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0571 per second** on the APIMart route — un único endpoint compatible con OpenAI en `https://api.apimart.ai/v1`, sin plan mensual. *(observado el 2026-10-12)*

**[Obtener una clave de API](https://go.apimart.ai/k-cfbc0c)** · **[Precios en vivo](https://go.apimart.ai/k-516ba3)** · **[Página del modelo](https://go.apimart.ai/k-0b09eb)** · [⚡ Inicio rápido en 60 segundos](#quickstart)

**Por qué los equipos usan Kling 2.6 Motion Control (`kling-v2-6-motion-control`) a través de APIMart**

- **Una clave, todo el catálogo.** La misma base URL `https://api.apimart.ai/v1` y la misma cabecera `Authorization` dan acceso a Kling 2.6 Motion Control (`kling-v2-6-motion-control`) y a más de 300 modelos de imagen, vídeo y lenguaje: cambia el campo `model`, no tu cliente.
- **Mínimo de $1, pago por uso.** Sin suscripción ni plan prepago que dimensionar por adelantado: recarga desde $1 y úsalo en llamadas.
- **El cargo vuelve en la respuesta.** Cada tarea completada informa del importe facturado (`cost` / `credits_cost`); una tarea marcada como `failed` se reembolsa.
- **Asíncrono por diseño.** Envía, toma el `task_id`, consulta `GET /v1/tasks/{id}`: el batching y los reintentos son trabajo de cola normal, no una integración a medida.

<!-- /conv-kit:v1 -->



Kling V2.6 Motion Control on APIMart uses a reference image plus a reference video to transfer subject motion into generated clips. Use kling-v2-6-motion-control with std (720p) and pro (1080p) modes; output length follows the reference video and billing is per second. Add an optional prompt, set character orientation, and control sound or watermark settings. It brings Kling AI Motion Control to your own app through one API: use kling-v2-6-motion-control as the model.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `kling-v2-6-motion-control` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-0b09eb) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| default | 0.0714 | 0.05712 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-516ba3) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-0b09eb) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"kling-v2-6-motion-control","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "kling-v2-6-motion-control", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**¿Cuál es el id del modelo Kling 2.6 Motion Control?**

`kling-v2-6-motion-control`. Consulta la tabla de rutas de arriba.

**¿Cómo se factura?**

**from $0.0571 per second** on the APIMart route. La tarea completada informa del importe exacto en su campo `cost`; una tarea fallida se reembolsa.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-cfbc0c) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [kling-v2-6-motion-control-prompts-es](https://github.com/{{OWNER}}/kling-v2-6-motion-control-prompts-es)
- **Same model, other languages** — 10 more language builds: English `kling-v2-6-motion-control-api` · 中文 [kling-v2-6-motion-control-api-cn](https://github.com/v0k1hdrf-4jv/kling-v2-6-motion-control-api-cn) · Deutsch `kling-v2-6-motion-control-api-de` · Français [kling-v2-6-motion-control-api-fr](https://github.com/il0xj8m/kling-v2-6-motion-control-api-fr) · Relay/gateway `kling-v2-6-motion-control-api-gateway` · Bahasa `kling-v2-6-motion-control-api-id` · 日本語 [kling-v2-6-motion-control-api-ja](https://github.com/swttr92mb2hr6k/kling-v2-6-motion-control-api-ja) · 한국어 [kling-v2-6-motion-control-api-ko](https://github.com/hnkcrt6/kling-v2-6-motion-control-api-ko) · Português [kling-v2-6-motion-control-api-pt](https://github.com/x-xvyf2/kling-v2-6-motion-control-api-pt) · Русский `kling-v2-6-motion-control-api-ru`
- **More models in this language** — [midjourney-api-es](https://github.com/xbuu0/midjourney-api-es) · [gemini-2-5-flash-image-preview-api-es](https://github.com/ekw3w/gemini-2-5-flash-image-preview-api-es) · [nano-banana-api-es](https://github.com/gbm5mfl1/nano-banana-api-es) · [gemini-3-1-flash-image-preview-api-es](https://github.com/x3kcczm8l29xn/gemini-3-1-flash-image-preview-api-es) · [nano-banana-2-api-es](https://github.com/d63hy9t-4pp/nano-banana-2-api-es) · [gemini-3-pro-image-preview-api-es](https://github.com/uv9d8/gemini-3-pro-image-preview-api-es)
- **Topic hubs** — every build of this model, all languages: [kling-v2-6-motion-control](https://github.com/topics/kling-v2-6-motion-control) · [kling-v2-6-motion-control-api](https://github.com/topics/kling-v2-6-motion-control-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-0b09eb> | `apimart.ai/es/model/kling-v2-6-motion-control` |
| Live pricing | <https://go.apimart.ai/k-516ba3> | `apimart.ai/es/pricing` |
| Get an API key | <https://go.apimart.ai/k-cfbc0c> | `apimart.ai/es/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Kling 2.6 Motion Control is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
