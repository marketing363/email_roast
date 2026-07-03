# Cold Email Roaster — Deploy to Vercel (free, ~5 minutes)

This is a working React app with a small backend that keeps your Anthropic API
key safe. The browser never sees the key — it calls `/api/roast`, and the server
calls Anthropic. That's the whole reason this works on your own domain.

---

## What you need first (2 free accounts + 1 paid key)

1. A **GitHub** account — https://github.com (free)
2. A **Vercel** account — https://vercel.com (free; sign in with GitHub)
3. An **Anthropic API key** — https://console.anthropic.com → API Keys → Create Key
   - This is the only thing that costs money. Each roast is a fraction of a cent,
     but set a spending limit in the console so a viral spike can't surprise you.
     (Console → Settings → Limits.)

---

## Step 1 — Put the code on GitHub

Easiest way, no command line:
1. Go to https://github.com/new and create an empty repo called `cold-email-roaster`.
2. On the repo page click **"uploading an existing file"**.
3. Unzip the folder I gave you and drag ALL its contents in (the `api` folder,
   `src` folder, `index.html`, `package.json`, etc). Do NOT drag `node_modules`.
4. Click **Commit changes**.

---

## Step 2 — Deploy on Vercel

1. Go to https://vercel.com/new
2. Click **Import** next to your `cold-email-roaster` repo.
3. Vercel auto-detects Vite. Leave every setting as-is.
4. Before clicking Deploy, open **Environment Variables** and add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** paste your key from the Anthropic console
5. Click **Deploy**. Wait ~1 minute.

You'll get a live URL like `cold-email-roaster.vercel.app`. That's it — it works.

---

## Step 3 — (optional) Your own domain

In the Vercel project: **Settings → Domains → Add**. Point `roast.bitscale.ai`
(or whatever) at it. Free.

---

## Testing it

Open your URL, paste the sample bad email, hit roast. If you see a roast, you're done.

If you see an error:
- **"Server missing ANTHROPIC_API_KEY"** → you skipped Step 2.4, or typed the name
  wrong. It must be exactly `ANTHROPIC_API_KEY`. Re-add it, then redeploy
  (Deployments → ⋯ → Redeploy).
- **A 404 model error** → the model name may have changed. Open `api/roast.js`,
  find the line `model: "claude-sonnet-4-6"`, and check the current model string
  at https://docs.claude.com/en/docs/about-claude/models — swap in the current one.
- **429 / rate limit** → you're on the free Anthropic tier hitting limits, or a
  spike. Add credit or raise limits in the Anthropic console.

---

## Tuning the comedy (the important part)

All the jokes live in ONE place: `api/roast.js`, the `buildPrompt` function.
Edit the wording there, commit to GitHub, and Vercel redeploys automatically in
about a minute. You do NOT need to touch the frontend to change the humor.

When a roast falls flat, note the exact line, then tweak the examples in the
`=== HOW A JOKE ACTUALLY WORKS ===` section to steer it. Real output beats
guessing — now that it's live, you can finally see what actually lands.

---

## Cost & safety notes

- Set a hard spend limit in the Anthropic console. This is a public tool; anyone
  can use it, so cap it before you post the link anywhere.
- Consider adding basic rate limiting later (e.g. limit per IP) so one person
  can't run up your bill. Ask me and I'll add it.
- The voice playback ("hear the roast") uses the browser's built-in voice. It is
  NOT a celebrity voice. For a real character voice you'd add a paid TTS service
  (like ElevenLabs) to `api/` — ask me when you're ready.
