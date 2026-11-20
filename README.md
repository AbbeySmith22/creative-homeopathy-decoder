# Creative Homeopathy Decoder

A Frequency Tool Suite tool by **High Vibrancy Living** ([frequencytoolkit.com](https://frequencytoolkit.com)).

Enter the top 5 results from a Healy Creative Homeopathy scan and get a plain-English reading: the theme that connects them, what each result means, the body areas each remedy is traditionally linked with, reflection questions, a vibration routine and a next step.

## How it works

- `index.html` – the page (no build step, no dependencies).
- `api/decode.js` – a Vercel serverless function that sends the results to the Anthropic API and returns the reading as JSON. Your API key stays on the server.
- `api/decode-iching.js` – the I Ching Decoder endpoint (`POST /api/decode-iching`). Same login and shared credits as `/api/decode` (tool name `i-ching`). The Lovable page for it is described in `LOVABLE_ICHING_PROMPT.md`.

## Deploy on Vercel

1. In Vercel, click **Add New → Project** and import this GitHub repository.
2. Framework preset: **Other**. Leave the build and output settings empty.
3. Under **Environment Variables**, add:
   - `ANTHROPIC_API_KEY` – your key from [console.anthropic.com](https://console.anthropic.com)
   - optional: `ANTHROPIC_MODEL` (default `claude-sonnet-5-5`; `claude-haiku-5-5` is cheaper)
   - optional: `RATE_LIMIT_PER_HOUR` (default `20` readings per visitor per hour)
4. Click **Deploy**. Every push to `main` redeploys automatically.

To put it on your own domain (for example `decoder.frequencytoolkit.com`), add it under **Settings → Domains**.

## Notes

- The rate limit is a simple per-visitor safeguard. For a public launch, consider putting the tool behind your site login or membership, and set a monthly spend limit in the Anthropic console.
- The tool is for education and personal reflection only. It does not diagnose, treat, cure or prevent any medical condition.
- Healy is a trademark of its owner. This tool is independent and not affiliated with Healy World.
