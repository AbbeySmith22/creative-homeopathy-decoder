Add a new digital product: **Creative Homeopathy Made Simple** (a 17-page PDF guide).

Set it up exactly the same way as the existing **Vibe Guide**: a sales page, a Stripe checkout, an orders table and automatic delivery after payment. Copy the Vibe Guide's flow, page layout, design system and fulfilment logic. Do not change the Vibe Guide or any other existing product, tool, credits, Stripe setup, authentication or membership permissions.

## 1. Product, checkout and delivery (mirror the Vibe Guide)
- Stripe product: "Creative Homeopathy Made Simple", one-off payment of **7 USD**.
- Sales page route: `/creative-homeopathy-guide`. Checkout route: `/creative-homeopathy-checkout` (same pattern as `/vibe-checkout`).
- Orders table: `creative_homeopathy_orders`, with the same columns, policies and fulfilment logic as `vibe_guide_orders` (Stripe session id, payment intent id, email, environment, status, fulfilment attempts, lease, last error, fulfilled at, timestamps).
- Delivery: after a successful payment, send the buyer their download the same way the Vibe Guide is delivered. The file is the uploaded PDF **Creative-Homeopathy-Made-Simple.pdf**. Show a thank-you page with a download button as well.
- Track the purchase in `marketing_events` the same way as the Vibe Guide (product: "creative-homeopathy-guide").
- Add the guide to the "Go deeper" section on the homepage and to the Guides menu, with the card text: "$7 · PDF guide + printables", title "Creative Homeopathy Made Simple", description "Finally understand your Creative Homeopathy scan results: diagrams, step-by-step scanning, AI decoding and two printables."

## 2. Sales page `/creative-homeopathy-guide`
Use the same layout, fonts, colours, buttons and spacing as the Vibe Guide page. Every "buy" button goes to `/creative-homeopathy-checkout`. Use the uploaded images where noted.

**Hero** (image: `ch-hero.png`)
- Kicker: CREATIVE HOMEOPATHY MADE SIMPLE
- H1: Creative Homeopathy, *made simple.* (italic part in the brand violet)
- H2: Ran a Creative Homeopathy scan… and have no idea what your results actually mean?
- Paragraphs: "You're not the only one. 348 remedies, five names you've never heard of, and a stress pattern that sounds like a riddle." / "This guide explains the database in plain English, with clear diagrams, so you can read your results, link them to what's going on in your body and your life, and know what to do next."
- Button: "Yes, I want the guide". Small text beside it: "17-page digital guide + printables · Today: $7 USD"

**Section: why I made it** (image: `ch-page-stress.png`)
- H2: Honestly? It's one of the most powerful databases on the Healy… and the most *overlooked.*
- "Creative Homeopathy is brilliant for the things that feel stuck: patterns that keep repeating, beliefs you can't quite shake, and physical issues that seem to have a deeper story behind them."
- "But the results are short. A remedy name and a one-line stress pattern don't tell you much on their own, so most people scan once, feel confused and go back to the same few programs."
- "So I put everything I use into one simple guide: what the database is, why it works the way it does, how to read your results, and how to decode any remedy in seconds."
- Secondary button: "Give me the simple version"

**Section: What exactly is it?** (centred card)
"A 17-page digital guide to the Creative Homeopathy database, with diagrams, a step-by-step scan walkthrough, the Creative Homeopathy tool, ready-made AI prompts and two printables. It's the guide I wish I'd had the first time I ran this scan."

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
- Button "Try the tool for free" → the Creative Homeopathy tool page. Small text: "Create a free account at frequencytoolkit.com"
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
- Kicker: YOUR SPECIAL OFFER
- H2: Get Creative Homeopathy *Made Simple* today
- Ticks: The full 17-page digital guide / Clear diagrams of the energy layers, stress and resonance / Works with the Creative Homeopathy tool, plus copy-and-paste AI prompts / Printable Quick Reference Card and Scan Journal
- Price card: "17-page guide + printables", "$7 USD", button "I want the guide for $7", and "Questions first? Email Info@highvibrancyliving.com"

**FAQ**
- Do I need to know homeopathy? Not at all. The guide starts from zero and keeps everything in plain English.
- Is this an official Healy guide? No. It's my own independent guide, not official Healy World training or material.
- How does the Creative Homeopathy tool work? It's part of the Frequency Tool Suite on frequencytoolkit.com. Create a free account, enter your top 5 results and get a full reading. Each reading uses one Tool Suite credit. You don't need the tool to use the guide: page 12 also gives you prompts for your own AI.
- Which Healy app do I need? The Creative Homeopathy database in the Healy resonance app.
- How do I get it? Straight away. It's a PDF you can read on your phone, tablet or computer, or print at home.

**Final call to action**
- H2: Stop staring at remedy names and *hoping for the best.*
- "You already have the database. Get the 17-page guide and printables for $7 today."
- Button: "Yes, Abbey, send me the guide"

Footer and legal notes: same as the Vibe Guide page.
