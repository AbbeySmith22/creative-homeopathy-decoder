Add a new tool page to the Frequency Tool Suite: **Creative Homeopathy**.

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
