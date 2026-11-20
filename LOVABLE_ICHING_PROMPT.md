Add one new tool page to the Frequency Tool Suite: **I Ching**. Do not change any existing tool, product, credits logic, Stripe setup, authentication or membership permissions.

**Links**
- I Ching API (on the same Vercel service as Creative Homeopathy): `https://creative-homeopathy-decoder.vercel.app/api/decode-iching`

**Credits check first**
The API reserves credits with the existing Supabase function `reserve_tool_credit(p_user_id, p_tool_name)` using the tool name `i-ching`, and refunds with `refund_tool_credit` if a reading fails. Check whether `reserve_tool_credit` only accepts known tool names (for example from a tools table or a check constraint). If it does, add `i-ching` the same way `creative-homeopathy` was added. Change nothing else about credits.

# The I Ching tool page

Use the same URL pattern as the other tool pages (for example `/i-ching`). Signed-in only, like Creative Homeopathy.

Build it exactly like the Creative Homeopathy page: same page layout, the same rounded gradient header card (`ToolHero`), the same "Your Frequency Tool Suite credits" row with "1 credit per reading · signed in as {email}", the same login handling, the same results components and Copy button behaviour. Add it to `src/lib/tools.ts` (category "Scans & Focus", status available), the Tools dropdown menu, the Tool Suite page and the signed-in dashboard, next to Creative Homeopathy.

**Branding for this tool only**
- Header gradient: `linear-gradient(115deg, #8FC7BB 0%, #B9D9C4 55%, #E9D19A 100%)` (Soft Jade → Sage Mist → Warm Gold)
- Header text in deep jade `#15332F`
- Page background `#F7FBF9`; body text `#1F2B2A`; secondary text `#4C5A57`
- Accent for small text and links `#2A6B5E`; soft tints `#E4F0EC` (jade) and `#FBF1DC` (gold)
- Primary buttons, selected states, numbered circles and loading bars use the gradient with deep jade text
- Fonts: Fraunces for headings, Inter for body (same as the other tools)
- Tool Suite card artwork: same style as the Creative Homeopathy card, in the jade → gold gradient

**Header**
- Kicker: HIGH VIBRANCY LIVING
- Title: I Ching
- Description: Explore the I Ching database in your Healy Resonance app with simple, personalised guidance. Pop in your hexagram numbers and the focus you held to see the theme that connects them, what each one is asking of you and a few small steps to take.
- Credibility line (italic): Built by Abbey Smith — Experienced Frequency Practitioner & Trainer

**Tool Suite card description:** Pop in your Healy I Ching results and get a plain-English reading: the theme, what each hexagram is asking of you, and small steps to take.

**Form card: "Your scan results"**
- Textarea "Your focus or question", placeholder "e.g. What do I need to focus on as I launch my new holistic business?"
- Helper text above the rows: Add the hexagram number for each result. The Healy name is optional: names vary between sources, but the number never does.
- 5 numbered rows, label "Your results, strongest first". Each row has a small number input "No." (1 to 64) and a text input "Name on your Healy (optional)". As soon as a valid number is typed, show "Usually called: {name}" under the row in the accent colour, using the list below. Outside 1 to 64: "Use a number from 1 to 64" in red.
- Textarea: "What's going on right now? (optional)", placeholder "e.g. trained in aromatherapy and sound therapy, feeling nervous about putting myself out there"
- Toggle "Who is this reading for?": Me / A client (default Me)
- Buttons: "Decode my results" (primary; says "Decode these results" when A client is selected) and "Clear"
- Small note: Please don't include names or anything that could identify you or a client.
- Validation: at least one number, and every row that's used needs a number from 1 to 64. Otherwise show "Add at least one hexagram number." or "Each result needs a hexagram number from 1 to 64."

**Hexagram names (number: name)**
1 The Creative, 2 The Receptive, 3 Difficulty at the Beginning, 4 Youthful Folly, 5 Waiting, 6 Conflict, 7 The Army, 8 Holding Together, 9 Small Taming, 10 Treading, 11 Peace, 12 Standstill, 13 Fellowship, 14 Great Possession, 15 Modesty, 16 Enthusiasm, 17 Following, 18 Work on What Has Spoiled, 19 Approach, 20 Contemplation, 21 Biting Through, 22 Grace, 23 Splitting Apart, 24 Return, 25 Innocence, 26 Great Taming, 27 Nourishment, 28 Great Exceeding, 29 The Abyss (Water), 30 Clarity (Fire), 31 Influence, 32 Duration, 33 Retreat, 34 Great Power, 35 Progress, 36 Darkening of the Light, 37 The Family, 38 Opposition, 39 Obstruction, 40 Deliverance, 41 Decrease, 42 Increase, 43 Breakthrough, 44 Coming to Meet, 45 Gathering Together, 46 Pushing Upward, 47 Oppression, 48 The Well, 49 Revolution, 50 The Cauldron, 51 The Arousing (Thunder), 52 Keeping Still, 53 Development, 54 The Marrying Maiden, 55 Abundance, 56 The Wanderer, 57 The Gentle (Wind), 58 The Joyous (Lake), 59 Dispersion, 60 Limitation, 61 Inner Truth, 62 Small Exceeding, 63 After Completion, 64 Before Completion

**Calling the tool**
POST to `https://creative-homeopathy-decoder.vercel.app/api/decode-iching` with headers `Content-Type: application/json` and `Authorization: Bearer {the member's Supabase access token}` (exactly as the Creative Homeopathy page sends it). Body:
```json
{ "focus": "optional text", "items": [{ "number": 46, "healy_name": "The Rise" }], "context": "optional text", "audience": "me" }
```
`audience` is "me" or "client". Only send rows that have a number. `number` must be a whole number from 1 to 64.

While waiting, show a loading card ("Reading your hexagrams together…") with soft gradient shimmer bars, and disable the button.

**Response (200)**
```json
{
  "theme_title": "…", "theme_summary": "…",
  "hexagrams": [{ "number": 46, "name": "Pushing Upward", "healy_name": "The Rise", "action": "Rise steadily", "plain": "…", "for_you": "…", "reflection": "…" }],
  "questions": ["…", "…", "…"], "steps": ["…", "…", "…"], "routine": "…", "next_step": "…",
  "creditsRemaining": 4
}
```
Update the credits number on the page from `creditsRemaining`.

**Errors** (the response has `error` and `code`): same handling as Creative Homeopathy. Show `error` in a small message under the button. 401 `login_required` → prompt to log in. 402 `no_credits` → show the message with a link to upgrade the membership. 429 → "Too many readings in a short time. Wait a minute, then try again." Anything else → "Something went wrong getting your reading. Try again in a moment." The API already refunds the credit when a reading fails, so the page doesn't need to.

**Results layout** (beside the form on desktop, below it on mobile)
1. Small status row: chip "Your reading" + "Generated just now", and a "Copy reading" button that copies the whole reading as plain text, ending with: "For education and reflection only. Not a prediction or professional advice. frequencytoolkit.com"
2. Theme card: deep jade `#1D4A43` background, a thin gradient strip along the top, kicker "THE THEME THAT CONNECTS THEM" in gold `#E6C98A`, `theme_title` as a large Fraunces heading in white, `theme_summary` below, then a small line "Focus: {focus}" (only if a focus was given).
3. One card per hexagram: gradient number circle showing the number, heading "{number} · {name}", "On your Healy: {healy_name}" in small text (only if given and different from `name`), `plain` text, "For you now:" in bold + `for_you`, a small jade-tint pill "Key: {action}", then a dashed divider and "Reflect:" + `reflection`.
4. Two cards side by side: "Questions to sit with" (numbered list) and "Small steps this week" (bullets).
5. "Suggested routine" card.
6. "A sensible next step" card on a soft gradient tint with `next_step` and two buttons: "See the Power Hour" (/healy-power-hour) and "Join the membership" (https://high-vibrancy-living.mn.co/).

Before the first reading, show this example in the results area with a gold chip "Example reading" and the note "From a real class demo scan. Add your own results to decode them.", and pre-fill the form with the same focus and results. Showing the example never uses a credit.
- Focus: "I've trained in aromatherapy massage and sound therapy and I'm ready to start my business. What do I need to focus on at this stage?"
- Context: "Just about to start reflexology training too. Lots of anxiety and nerves about putting myself out there."
- Theme: "Grow steadily, without needing to prove it". Summary: "Together these results describe someone on the threshold of something new. Growth is coming, but it's the slow, steady kind, like a young tree. There may be a few bumps along the way, and that's normal. Bringing your skills together and taking one calm step at a time matters more than a big launch."
- 46 Pushing Upward (Healy: The Rise). Key: Rise steadily. Plain: "A time of steady, patient growth. Results may not show straight away, but sustained effort is building something real." For you now: "It's better to begin and learn as you go than to wait until everything feels perfect. Your training is the root system; now it's time to grow." Reflect: "What's one small step you could take this week, even if it feels imperfect?"
- 8 Holding Together (Healy: The Agreement). Key: Unite. Plain: "Bringing things together around a shared centre. It's about unity, inside and out." For you now: "Your aromatherapy, sound and reflexology work may want to come together as one offer, rather than three separate things. It's also a call to gather supportive people around you." Reflect: "How could your different skills work together as one experience?"
- 21 Biting Through (Healy: The Criminal Trial). Key: Cut through. Plain: "An obstacle is in the way, and it needs to be worked through clearly and honestly, not avoided." For you now: "A few challenges will come up as you move from training into real clients. That's a balancing act, not a sign to stop. Speak up about what you need." Reflect: "What's one blockage you could gently chew over and move through?"
- 64 Before Completion (Healy: Before Completion). Key: Finish with care. Plain: "You're on the threshold. The transition is nearly made, and the last steps need care and attention." For you now: "You're between training and practice. Trust that it's unfolding as it should, and give the final details the care they deserve." Reflect: "What needs to be in place before you feel ready to open your doors?"
- 26 Great Taming (Healy: The Taming). Key: Hold steady. Plain: "Strength held in stillness. Real power doesn't need to prove itself." For you now: "You don't have to show everyone you can do this all at once. Try your sound baths with friends first and let your confidence build." Reflect: "Where could you let go of the pressure to prove yourself?"
- Questions: "What would starting small and imperfect look like for you?" / "Who are the people who already believe in you, and how could you involve them?" / "What does success feel like in your body, before it shows up in your bookings?"
- Small steps: "Invite two or three friends to a practice sound bath this month." / "Write one sentence that describes how your skills come together." / "Note any obstacles in your journal as they come up, and one way through each."
- Routine: "Vibrate these results for about 6 minutes a day, or overnight, for the next week. Breathe slowly while they run, then jot one line in your Reading Journal. Re-scan on the same focus in one to two weeks to see what's shifted."
- Next step: "Starting something new brings up a lot. A Healy Power Hour is a good place to scan together and make a simple, steady launch plan."

**Footer disclaimer** (same style as the other tools): This tool is for education and personal reflection within the Healy framework. It doesn't predict the future, diagnose, treat, cure or prevent any condition, and isn't a substitute for professional medical, legal or financial advice. Readings are generated by AI and can be wrong. Healy and Healy World are trademarks of their respective owner; this tool is independent and not affiliated with or endorsed by Healy World.

Style: rounded cards (18–22px radius), soft subtle shadows, generous white space, smooth gentle hover effects, mobile-friendly. Calm, grounded and harmonious, never neon or overly saturated.
