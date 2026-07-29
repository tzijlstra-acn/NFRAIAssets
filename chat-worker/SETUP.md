# NFR Asset Chat — Cloudflare Worker Setup

This Worker acts as a secure proxy between the NFRAIAssets page and the Anthropic API.
Your API key is stored as a Cloudflare secret — it never appears in the client-side code.

## Prerequisites

- Node.js 18+ installed
- A Cloudflare account (free tier is enough — 100,000 requests/day)
- An Anthropic API key from https://console.anthropic.com

## Deploy in 5 minutes

### 1. Install Wrangler CLI

```
npm install -g wrangler
```

### 2. Login to Cloudflare

```
wrangler login
```

A browser window will open. Log in and authorise Wrangler.

### 3. Deploy the Worker

From this `chat-worker/` folder:

```
wrangler deploy
```

Wrangler will print your Worker URL, e.g.:
`https://nfr-asset-chat.YOUR-SUBDOMAIN.workers.dev`

Copy this URL — you will need it in step 5.

### 4. Add your Anthropic API key as a secret

```
wrangler secret put ANTHROPIC_API_KEY
```

When prompted, paste your key (it starts with `sk-ant-...`). Press Enter.
The key is encrypted at rest — never visible after this step.

### 5. Update the chat widget URL in index.html

Open `NFRAIAssets/index.html` and find this line near the bottom:

```javascript
var WORKER_URL = 'YOUR_CLOUDFLARE_WORKER_URL';
```

Replace the placeholder with your Worker URL from step 3:

```javascript
var WORKER_URL = 'https://nfr-asset-chat.YOUR-SUBDOMAIN.workers.dev';
```

### 6. Commit and push

```
git add index.html
git commit -m "feat: connect chatbot to live Cloudflare Worker"
git push
```

The chatbot will be live on GitHub Pages within ~60 seconds.

## Updating the system prompt

Edit the `SYSTEM_PROMPT` constant in `worker.js`, then re-run `wrangler deploy`.

## Costs

- Cloudflare Workers free tier: 100,000 requests/day, no credit card required.
- Anthropic API (claude-haiku-4-5-20251001): approximately $0.001 per conversation turn.
  A typical session of 5 exchanges costs less than $0.01.
