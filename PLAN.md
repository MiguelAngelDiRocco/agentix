# Agentix — Plan de lanzamiento

> Tu hoja de ruta concreta desde "tengo el MVP" hasta "tengo ventas".

---

## ✅ Lo que ya tenés (en este paquete)

1. **`builder.html`** — El producto. Builder visual funcional, runs en cualquier navegador, sin backend, usa la API key del usuario. Es el MVP real, no un mockup.
2. **`index.html`** — Landing page completa con copy, pricing y CTA al builder.
3. **`README.md`** — README profesional para el repo de GitHub.
4. **`agents/customer-support-classifier.agentix.json`** — Primer agente de ejemplo, listo para importar y correr.
5. **Este archivo** — El plan.

---

## 🚀 Esta semana (5 días para estar live)

### Día 1 (Lunes) — Probar y arreglar
- [ ] Abrí `builder.html` en tu navegador
- [ ] Configurá tu API key (Settings → API Key)
- [ ] Importá `customer-support-classifier.agentix.json` y corré el agente
- [ ] Probá crear uno desde cero
- [ ] Anotá cada bug o fricción que encuentres
- [ ] Pasale la lista a Claude Code para que arregle todo de una

### Día 2 (Martes) — GitHub
- [ ] Creá el repo: `github.com/tuusuario/agentix`
- [ ] Subí los archivos
- [ ] Editá el README: cambiá los placeholders (`YOUR_USERNAME`, links de GitHub)
- [ ] Activá GitHub Pages → settings → Pages → main branch → root
- [ ] Tu landing va a quedar en `https://tuusuario.github.io/agentix/`
- [ ] El builder en `https://tuusuario.github.io/agentix/builder.html`

### Día 3 (Miércoles) — Crear 5 agentes más
Para tener marketplace seed cuando lances. Cada uno toma ~20 min:

1. **Email summarizer** → Trigger (email) → Claude (resumir en 3 bullets) → Output
2. **Tweet generator** → Trigger (artículo) → Claude (generar 5 tweets) → Output
3. **Lead qualifier** → Trigger (info de lead) → Claude (puntuar 1-10 con razón) → Condition (>7) → Output
4. **Code reviewer** → Trigger (snippet) → Claude (review) → Output
5. **Translator ES↔EN** → Trigger → Claude (traducir) → Output

Guardá cada uno como `.agentix.json` en `agents/`.

### Día 4 (Jueves) — Video demo
- [ ] Grabá un screencast de 60 segundos: abrir el builder, dragear 3 nodos, conectarlos, correr
- [ ] Usá Loom o OBS (gratis)
- [ ] Subilo a YouTube como unlisted
- [ ] Pegá el embed en el README y en la landing

### Día 5 (Viernes) — Lanzar
- [ ] **Show HN** en Hacker News: `Show HN: Agentix – Open source visual builder for AI agents (your API key, your tokens)`
- [ ] **Reddit**: r/LocalLLaMA, r/SideProject, r/SaaS, r/OpenSource
- [ ] **Twitter/X**: thread con el video demo
- [ ] **LinkedIn**: post en español apuntando al mercado LATAM
- [ ] **Indie Hackers**: post de lanzamiento
- [ ] **Product Hunt**: schedule para el siguiente martes (mejor día PH)

---

## 📊 Métricas a trackear (semana 1-4)

| Métrica | Cómo medirla | Objetivo mes 1 |
|---------|--------------|----------------|
| GitHub stars | obvio | 100 |
| Visitas a la landing | Plausible (gratis) o Cloudflare Analytics | 2,000 |
| Builder opens | event en JS → log a Plausible | 500 |
| Agentes creados | mismo, contar exports | 100 |

> No agregues Google Analytics. Mata la credibilidad open-source. Plausible o Umami self-hosted son perfectos.

---

## 💰 Marketplace (mes 2-3)

Esto es lo que genera el dinero. El builder es el imán, el marketplace es la caja.

### Stack mínimo para el marketplace
- **Frontend**: misma estética, otra página HTML (`marketplace.html`)
- **Backend**: Supabase (free tier) — auth + tabla `agents` + tabla `purchases`
- **Pagos**: Stripe Checkout (no necesitás backend custom, link directo)
- **Storage**: el JSON del agente lo guardás como columna text en Supabase
- **Entrega**: después del pago, Stripe webhook → genera URL firmada → email al comprador

### Primeros agentes a vender (los hacés vos)
Mismo enfoque que el ejemplo, pero más completos. Precios sugeridos:

| Agente | Precio | Nicho |
|--------|--------|-------|
| Customer Support Classifier Pro | $29 | E-commerce |
| Lead Qualifier (B2B) | $49 | Sales / SDR |
| Invoice Data Extractor | $39 | Contadores |
| WhatsApp Auto-Responder | $49 | LATAM PyMEs ← tu sweet spot |
| Content Idea Generator | $19 | Creators |
| Meeting Notes → Action Items | $29 | Productividad |
| RAG Q&A Bot (sobre PDFs) | $79 | Empresas |
| Cold Email Personalizer | $49 | Sales |
| Reddit Trend Tracker | $29 | Marketing |
| SEO Keyword Researcher | $39 | Marketing |

**Math**: si vendés 10 unidades de cada uno en mes 2 = 100 ventas × $40 promedio = $4,000. 70% para vos = **$2,800/mes**. Sin pagar un solo token.

---

## 🎯 Las 3 reglas

1. **El builder es y siempre será gratis.** No le pongas paywall a nada. Esa es tu ventaja moral y de marketing.
2. **Vos no pagás tokens. Nunca.** Cada vez que pienses en agregar un feature, preguntate: "¿esto me hace gastar tokens?". Si la respuesta es sí, no lo hagas.
3. **Open source de verdad.** MIT license, código limpio, PRs bienvenidos. Esto te da credibilidad y comunidad gratis.

---

## ⚠️ Lo que NO hagas

- ❌ No agregues "AI Agentix Cloud" en mes 1. Distrae del builder. Cloud va en mes 6 si las métricas justifican.
- ❌ No vendas suscripciones al builder. Rompé el modelo.
- ❌ No metas ads en ningún lado. Mata el OSS.
- ❌ No persigas enterprise hasta tener 1000+ users en GitHub. No te van a dar bola.
- ❌ No traduzcas al español todavía. El mercado de developers OSS es 95% inglés. Hacé la versión ES después del primer viral.

---

## 🔧 Mejoras técnicas para iterar con Claude Code

Cuando estés listo para la v0.2, esto es lo primero que pediría:

1. **Pan & zoom del canvas** (usá `transform: translate + scale`)
2. **Múltiples conexiones por nodo** (in/out para condition que tenga then-branch / else-branch reales)
3. **Variables globales** del agente (`{{vars.email}}`, `{{vars.api_key}}`)
4. **Loop node** para iterar sobre arrays
5. **Parser node** (JSON path, regex) para extraer campos del output
6. **Webhook trigger** (necesita backend mínimo — probá Cloudflare Workers free tier)
7. **Templates en el sidebar** — galería de agentes que se importan con un click

Con Claude Code podés tener todo eso en 2 fines de semana de trabajo.

---

## 📞 Si te trabás

Volvé a este chat. Ya tenés el contexto del proyecto entero. Pedime:

- "Agregale [feature X] al builder"
- "Generame el agente [Y] para el marketplace"
- "Escribime el copy del Show HN"
- "Diseñame la landing del marketplace"
- "Armame el código de Stripe checkout"

Todo eso lo puedo hacer en una sola pasada.

---

**Ahora abrí `builder.html` y empezá. El producto está vivo.**
