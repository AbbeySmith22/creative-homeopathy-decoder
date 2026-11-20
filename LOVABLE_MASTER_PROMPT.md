This prompt has two parts. Build both. Do not change any existing tool, product, credits logic, Stripe setup, authentication or membership permissions.

**Files uploaded with this prompt**
- `Creative-Homeopathy-Made-Simple.pdf` — the guide buyers receive
- `ch-hero.png` — sales page hero image
- `ch-page-stress.png`, `ch-page-resonance.png`, `ch-page-results.png`, `ch-page-quickref.png`, `ch-page-scan.png` — page previews for the sales page

**Links**
- Creative Homeopathy API (already live): `https://creative-homeopathy-decoder.vercel.app/api/decode`

# PART 1 — The Creative Homeopathy tool page

Add a new tool page to the Frequency Tool Suite: **Creative Homeopathy**. Use the same URL pattern as the other tool pages.

Build it exactly like the existing tool pages (Potency Checker, Focus Generator): same page layout, the same rounded gradient header card, the same "Your Frequency Tool Suite credits" row with "1 credit per reading · signed in as {email}", the same login handling, and add it to the Tools menu and Tool Suite list. Do not change any existing tool, credits logic, Stripe, authentication or membership permissions.

**Branding for this tool only**
- Header gradient: `linear-gradient(115deg, #B78AE5 0%, #E9A0BA 55%, #F4C78B 100%)` (Radiant Orchid → Soft Coral → Golden Apricot)
- Header text in deep plum `#3E2B47` (not white, so it stays readable on the apricot end)
- Page background warm white `#FFF9F7`; body text deep plum `#493453`; secondary text `#7A6680`
- Accent for small text and links `#7B4FB3`; soft tints `#F7EEFB` (orchid) and `#FDF0E6` (apricot)
- Primary buttons, selected states, numbered circles and loading bars use the gradient with plum text
- Fonts: Fraunces for headings, Inter for body (same as the other tools)

**Header**
- Kicker: HIGH VIBRANCY LIVING
- Title: Creative Homeopathy
- Description: Explore the Creative Homeopathy database in your Healy Resonance app with simple, personalised guidance. Pop in your top 5 results to see the theme that connects them, what each one means and one sensible next step.
- Credibility line (italic): Built by Abbey Smith — Experienced Frequency Practitioner & Trainer

**Form card: "Your scan results"**
- Helper text: Add the remedy name, the stress pattern, or both. The stress pattern is the line under each remedy in the Healy app.
- 5 numbered rows. Each row has two inputs: "Remedy name, e.g. Bromium" and "Stress pattern, e.g. Withdrawal out of insecurity".
- Textarea: "What's going on for you right now? (optional)", placeholder "e.g. hives that flare around my cycle, feeling stretched thin looking after everyone"
- Toggle "Who is this reading for?": Me / A client (default Me)
- Buttons: "Decode my results" (primary; says "Decode these results" when A client is selected) and "Clear"
- Small note: Please don't include names or anything that could identify you or a client.
- At least one row needs a remedy name or a stress pattern; otherwise show "Add at least one result: a remedy name, a stress pattern, or both."

**Calling the tool**
POST to `https://creative-homeopathy-decoder.vercel.app/api/decode` with headers `Content-Type: application/json` and `Authorization: Bearer {the member's Supabase access token}` (the same way the Potency Checker sends it). Body:
```json
{ "items": [{ "remedy": "Bromium", "pattern": "The urge to help, distracting from one's own suffering" }], "context": "optional text", "audience": "me" }
```
`audience` is "me" or "client". Only send rows that have something in them.

While waiting, show a loading card ("Reading your five results together…") with soft gradient shimmer bars, and disable the button.

**Response (200)**
```json
{
  "theme_title": "…", "theme_summary": "…",
  "remedies": [{ "name": "…", "pattern": "…", "plain": "…", "body_areas": ["…"], "reflection": "…", "confidence": "high|medium|low" }],
  "questions": ["…", "…", "…"], "routine": "…", "next_step": "…",
  "creditsRemaining": 4
}
```
Update the credits number on the page from `creditsRemaining`.

**Errors** (the response has `error` and `code`): show `error` in a small message under the button. 401 `login_required` → prompt to log in. 402 `no_credits` → show the message with a link to upgrade the membership. 429 → "Too many readings in a short time. Wait a minute, then try again." Anything else → "Something went wrong getting your reading. Try again in a moment."

**Results layout** (beside the form on desktop, below it on mobile)
1. Small status row: chip "Your reading" + "Generated just now", and a "Copy reading" button that copies the whole reading as plain text with the disclaimer at the end.
2. Theme card: deep plum `#493453` background, a thin gradient strip along the top, kicker "THE THEME THAT CONNECTS THEM" in apricot, `theme_title` as a large Fraunces heading in white, `theme_summary` below.
3. One card per remedy: gradient number circle, the remedy name as the heading (or the pattern if there's no name), the original pattern in small italics under the name, `plain` text, `body_areas` as small orchid-tint pill tags (if empty: "Add the remedy name to see body links" when there's no name, otherwise "No clear body links"), and a dashed divider then "Reflect:" + `reflection`. If `confidence` is "low", add a small apricot note: "The AI isn't confident about this remedy. Check it against a trusted homeopathy reference."
4. Two cards side by side: "Questions to sit with" (numbered list) and "Suggested routine".
5. "A sensible next step" card on a soft gradient tint with `next_step` text and two buttons: "See the Power Hour" (/healy-power-hour) and "Join the membership" (https://high-vibrancy-living.mn.co/).

Before the first reading, show this example in the results area with an apricot chip "Example reading" and the note "From a real class demo scan. Add your own results to decode them.", and pre-fill the form with the same five results:
- Bromium: "The urge to help, distracting from one's own suffering". Plain: "Keeping busy helping others so you don't have to look at your own pain." Body: Respiratory system, Glands and thyroid, Skin. Reflect: "Where are you so busy caring for others that your own needs go unmet?"
- "Disconnected from wholeness, pain of separation". Plain: "Feeling cut off from yourself or from others, and the quiet ache that comes with it." Reflect: "When did you first start to feel on the outside?"
- "Showing no feelings". Plain: "Keeping emotions hidden to stay safe, strong or in control." Reflect: "Which feeling have you been holding back lately?"
- "Used to being the fifth wheel". Plain: "Expecting to be the extra one, the person who doesn't quite fit in." Reflect: "Where in your life do you assume there isn't room for you?"
- "Withdrawal out of insecurity". Plain: "Pulling back when you feel unsure, rather than risking being seen." Reflect: "What would you do this week if you weren't afraid of being seen?"
- Theme: "Giving outward, holding back inward". Summary: "Together these results point to a pattern of focusing on everyone else while your own feelings stay tucked away. There's a thread of not quite belonging, and of pulling back when you feel unsure, which can leave you feeling separate even when you're surrounded by people."
- Questions: "If you gave yourself the care you give others, what would change first?" / "Where do you feel most like you belong, and how can you spend more time there?" / "Your skin may be asking to be noticed. What might it be trying to tell you?"
- Routine: "Vibrate these results for about 6 minutes a day, or overnight while you sleep, for the next 7 days. Breathe slowly while it runs, then jot one line in your Scan Journal about anything that comes up. Re-scan next week to see what has shifted."
- Next step: "The belonging theme here runs deep. A Healy Power Hour is a good place to look at it together and build a simple plan."
- Context box pre-filled: "Hives that keep coming up around my cycle since having my baby. Busy looking after everyone."

**Footer disclaimer** (same style as the other tools): This tool is for education and personal reflection within the Healy framework. It doesn't diagnose, treat, cure or prevent any medical condition, and isn't a substitute for professional care. Readings are generated by AI and can be wrong. Healy and Healy World are trademarks of their respective owner; this tool is independent and not affiliated with or endorsed by Healy World.

Style: rounded cards (18–22px radius), soft subtle shadows, generous white space, smooth gentle hover effects, mobile-friendly. Make it feel colourful, harmonious and high vibe, but never neon or overly saturated.

---

# PART 2 — The guide: sales page, checkout and delivery

Add a new digital product: **Creative Homeopathy Made Simple** (a 17-page PDF guide + a bonus video masterclass).

Set it up exactly the same way as the existing **Vibe Guide**: a sales page, a Stripe checkout, an orders table and automatic delivery after payment. Copy the Vibe Guide's flow, page layout, design system and fulfilment logic. Do not change the Vibe Guide or any other existing product, tool, credits, Stripe setup, authentication or membership permissions.

## 2a. Product, checkout and delivery (mirror the Vibe Guide)
- Stripe product: "Creative Homeopathy Made Simple", one-off payment of **7 USD**. On the page, always show the usual price crossed out next to it: ~~$14~~ **$7** ("Usually $14", "50% off · Limited time only"). Stripe only charges $7.
- Sales page route: `/creative-homeopathy-guide`. Checkout route: `/creative-homeopathy-checkout` (same pattern as `/vibe-checkout`).
- Orders table: `creative_homeopathy_orders`, with the same columns, policies and fulfilment logic as `vibe_guide_orders` (Stripe session id, payment intent id, email, environment, status, fulfilment attempts, lease, last error, fulfilled at, timestamps).
- Delivery: after a successful payment, send the buyer their download the same way the Vibe Guide is delivered. The file is the uploaded PDF **Creative-Homeopathy-Made-Simple.pdf**. Put a "Download your guide" button in the delivery email and on the thank-you page. The bonus video masterclass is NOT part of this delivery yet: Abbey will send it to buyers by email later. Instead, add one line to the delivery email and thank-you page: "Your bonus video masterclass will arrive in a separate email."
- Track the purchase in `marketing_events` the same way as the Vibe Guide (product: "creative-homeopathy-guide").
- Add the guide to the "Go deeper" section on the homepage and to the Guides menu, with the card text: "~~$14~~ $7 · PDF guide + bonus video masterclass", title "Creative Homeopathy Made Simple", description "Finally understand your Creative Homeopathy scan results: diagrams, step-by-step scanning, AI decoding and two printables."

## 2b. Sales page `/creative-homeopathy-guide`
Use the same layout, fonts, colours, buttons and spacing as the Vibe Guide page. Every "buy" button goes to `/creative-homeopathy-checkout`. Use the uploaded images where noted.

**Hero** (image: `ch-hero.png`)
- Kicker: CREATIVE HOMEOPATHY MADE SIMPLE
- H1: Creative Homeopathy, *made simple.* (italic part in the brand violet)
- H2: Ran a Creative Homeopathy scan… and have no idea what your results actually mean?
- Paragraphs: "You're not the only one. 348 remedies, five names you've never heard of, and a stress pattern that sounds like a riddle." / "This guide explains the database in plain English, with clear diagrams, so you can read your results, link them to what's going on in your body and your life, and know what to do next."
- Button: "Yes, I want the guide". Small text beside it: "17-page guide + bonus video masterclass · ~~$14~~ $7 USD · 50% off for a limited time" (the $14 crossed out)

**Section: why I made it** (image: `ch-page-stress.png`)
- H2: Honestly? It's one of the most powerful databases on the Healy… and the most *overlooked.*
- "Creative Homeopathy is brilliant for the things that feel stuck: patterns that keep repeating, beliefs you can't quite shake, and physical issues that seem to have a deeper story behind them."
- "But the results are short. A remedy name and a one-line stress pattern don't tell you much on their own, so most people scan once, feel confused and go back to the same few programs."
- "So I put everything I use into one simple guide: what the database is, why it works the way it does, how to read your results, and how to decode any remedy in seconds."
- Secondary button: "Give me the simple version"

**Section: What exactly is it?** (centred card)
"A 17-page digital guide to the Creative Homeopathy database, with diagrams, a step-by-step scan walkthrough, the Creative Homeopathy tool, ready-made AI prompts and two printables. Plus a bonus video masterclass where I walk you through a real scan and show you how to use the tools. It's the guide I wish I'd had the first time I ran this scan."

**Section: Inside the guide** (6 cards, then 3 page images in a row: `ch-page-resonance.png`, `ch-page-results.png`, `ch-page-quickref.png`)
1. The plain-English overview: What Creative Homeopathy is, where it comes from, and why your symptoms can be read as messages.
2. Diagrams that make it click: Your energy layers, how stress reaches the body, and how resonance narrows 348 remedies to your top 5.
3. Step-by-step scanning: Exactly where to find it in the app, how to run it, and how long to vibrate your results.
4. How to read your results: Three simple questions, plus a real example linking a physical issue to the stress pattern behind it.
5. Decode results in seconds: How to use the Creative Homeopathy tool, plus copy-and-paste prompts if you prefer your own AI.
6. Two printables: A Quick Reference Card for beside your Healy and a Scan Journal to spot your patterns over time.

**Section: the tool** (warm white background; use the Creative Homeopathy tool's orchid → coral → apricot gradient `linear-gradient(115deg, #B78AE5 0%, #E9A0BA 55%, #F4C78B 100%)` for the button and preview)
- Kicker: NEW IN THE FREQUENCY TOOL SUITE
- H2: Decode your results in seconds with the *Creative Homeopathy* tool
- "The guide teaches you how to read your results. The tool does the decoding for you. Pop in your top 5 and get one clear reading, written in plain English."
- Ticks: The theme that connects all five results / What each remedy means, and the body areas it's traditionally linked with / Reflection questions, a vibration routine and a sensible next step / A "for a client" mode for practitioners
- Button "Try the tool for free" → the Creative Homeopathy tool page from Part 1 (logged-out visitors go to sign-up first, then land on the tool). Small text: "Create a free account at frequencytoolkit.com"
- On the right, a small preview card of a reading: gradient header "Creative Homeopathy"; a deep plum theme card with the kicker "THE THEME THAT CONNECTS THEM", the title "Carrying everything while feeling unseen" and the small text "Taken together, these results may point to a season where you feel cut off from the closeness you want, while pushing on and putting your own needs last."; a remedy card "1 · Staphisagria – Feeling isolated, inner ties to others cut off" with "You may feel alone even when people are nearby, as if the warm connection you want has been cut." and pills Urinary system / Skin / Genital organs; then "+ 4 more results, reflection questions and your routine".

**Section: This guide is for you if…** (image: `ch-page-scan.png`)
- You've run Creative Homeopathy but don't know what to do with the results
- Something keeps repeating, and you want to understand the pattern behind it
- You'd love to link a physical issue to what's going on emotionally
- You scan clients and want to explain results in a way that clicks

No testimonials section for now (it will be added once there are reviews of this guide).

**Section: What this is, and what it is not**
"A simple, educational way to understand the Creative Homeopathy database and reflect on your results."
- It is not a diagnostic tool.
- It does not prescribe treatment or promise a particular physical, emotional or financial outcome.
- It does not replace the official instructions, professional healthcare or your own common sense.

**Offer section** (same style as the Vibe Guide's "Your special offer")
- Kicker: LIMITED-TIME OFFER · 50% OFF
- H2: Get Creative Homeopathy *Made Simple* today
- Ticks: The full 17-page digital guide / Clear diagrams of the energy layers, stress and resonance / Works with the Creative Homeopathy tool, plus copy-and-paste AI prompts / Printable Quick Reference Card and Scan Journal / **Bonus:** a video masterclass with me, with a live demo of scanning and using the tools (use a small play icon for this one)
- Price card: logo, "17-page guide + printables + bonus video masterclass", then "Usually ~~$14~~" in muted grey beside a large "$7", a small apricot pill "50% off · Limited time only", button "I want the guide for $7", and "Questions first? Email Info@highvibrancyliving.com"

**FAQ**
- Do I need to know homeopathy? Not at all. The guide starts from zero and keeps everything in plain English.
- Is this an official Healy guide? No. It's my own independent guide, not official Healy World training or material.
- How does the Creative Homeopathy tool work? It's part of the Frequency Tool Suite on frequencytoolkit.com. Create a free account, enter your top 5 results and get a full reading. Each reading uses one Tool Suite credit. You don't need the tool to use the guide: page 12 also gives you prompts for your own AI.
- How do I get it? Straight away. You'll get the PDF to read on your phone, tablet or computer (or print at home). Your bonus video masterclass follows by email.
- What's in the bonus masterclass? A video with me explaining the database and showing a live demo: running a Creative Homeopathy scan, reading the results, and using the tools to decode them. Watch it once and the guide will make even more sense.

**Final call to action**
- H2: Stop staring at remedy names and *hoping for the best.*
- "You already have the database. Get the 17-page guide, printables and bonus video masterclass for $7 today (usually $14)."
- Button: "Yes, Abbey, send me the guide"

Footer and legal notes: same as the Vibe Guide page.

# PART 3 — Connect the two
- On the tool page (Part 1), add a small line under the form card: "New to Creative Homeopathy? Get the guide: Creative Homeopathy Made Simple" linking to `/creative-homeopathy-guide`.
- On the thank-you page after buying the guide (Part 2), add a third button: "Try the Creative Homeopathy tool" linking to the tool page.
- Add both to the site navigation: the tool under Tools, the guide under Guides.
