# Nyay AI — Legal Rights Advisor (India)

A simple, agentic AI that helps ordinary people understand their legal rights under Indian law. Built with plain HTML/CSS/JS + Anthropic Claude API.

---

## Project Structure

```
legal-advisor/
├── index.html    ← Main UI
├── style.css     ← Styling
├── app.js        ← Logic + API calls
└── README.md     ← This file
```

---

## Setup (Local — 5 minutes)

### 1. Get an Anthropic API Key
- Go to https://console.anthropic.com
- Create an account → Go to "API Keys" → Create a new key
- Copy the key (starts with `sk-ant-...`)

### 2. Add your API key
Open `app.js` and replace line 3:
```js
const API_KEY = "YOUR_ANTHROPIC_API_KEY";
```
with your actual key:
```js
const API_KEY = "sk-ant-api03-xxxx...";
```

### 3. Run it
Just open `index.html` in your browser — no server needed for local use.

> ⚠️ For production/public deployment, move the API key to a backend (see below).

---

## Deploy Online (Free Options)

### Option A: GitHub Pages (Easiest — Static)
> ⚠️ Only do this if you add a backend proxy (don't expose API key publicly)

1. Create a GitHub account at https://github.com
2. Create a new repository called `nyay-ai`
3. Upload all 3 files (index.html, style.css, app.js)
4. Go to Settings → Pages → Source: main branch
5. Your site will be live at `https://yourusername.github.io/nyay-ai`

### Option B: Netlify (Recommended — Free + Easy)
1. Go to https://netlify.com and sign up
2. Drag and drop your project folder onto the Netlify dashboard
3. It deploys instantly with a public URL

### Option C: Vercel
1. Go to https://vercel.com
2. Connect your GitHub repo or drag-drop the folder
3. Auto-deploys on every change

---

## ⚠️ IMPORTANT: Securing Your API Key for Production

**Never expose your API key in public frontend code.**

For a public site, add a simple backend proxy:

### Quick Backend Proxy (Node.js / Express)

Create `server.js`:
```js
const express = require('express');
const fetch = require('node-fetch');
const app = express();
app.use(express.json());
app.use(express.static('.'));  // serves your HTML/CSS/JS

app.post('/api/chat', async (req, res) => {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,  // set in environment
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify(req.body)
  });
  const data = await response.json();
  res.json(data);
});

app.listen(3000, () => console.log('Running on http://localhost:3000'));
```

Then in `app.js`, change the API_URL:
```js
const API_URL = "/api/chat";  // hits your proxy instead
```

Deploy this backend on **Railway** (https://railway.app) or **Render** (https://render.com) — both are free.

---

## Customization

### Add more quick topics
In `index.html`, copy any `.topic-btn` block and change the `onclick` text.

### Change the AI's behavior
In `app.js`, edit the `SYSTEM_PROMPT` string. You can:
- Focus it on a specific state's laws
- Add Hindi language support
- Restrict it to one domain (e.g., only tenant rights)

### Add Hindi support
Add this to the system prompt:
```
If the user writes in Hindi, respond in Hindi using simple language.
```

---

## Roadmap Ideas
- [ ] Hindi / regional language support
- [ ] "Draft a legal notice" feature
- [ ] State-specific law modules
- [ ] Voice input (Web Speech API)
- [ ] PWA for offline use
- [ ] WhatsApp bot integration

---

## Tech Stack
- Frontend: HTML, CSS, Vanilla JS
- AI: Anthropic Claude (claude-sonnet-4)
- No framework, no build step — pure and simple

---

## Legal Disclaimer
This tool provides general legal information, not legal advice. Users should consult a qualified lawyer for their specific situation.
