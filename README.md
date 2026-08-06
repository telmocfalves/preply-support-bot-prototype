# Preply support bot — prototype

An interactive prototype of a customer-facing support bot covering two intents: **refund requests**
and **tutor no-shows**.

The screen is split deliberately. On the left is what the customer sees. On the right is the rule
that produced it. The point of the prototype is that the bot never decides anything — a rules engine
returns an outcome, and the bot explains it.

## Run it

Open `index.html` in a browser. There is no build step and no dependencies beyond the Figtree
webfont.

## Publish it

1. Create a public repository and add `index.html` at the root.
2. Go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**, then `main` / `/ (root)`.
4. Save. The URL appears at the top of that page within a minute or two.

The repository needs to be public for the link to open without a GitHub login.

## What to try

Ten scenarios, five per intent. Three worth seeing in order:

1. **Refund → "Charged a while ago"** — the bot declines warmly and correctly, using the 28-day
   window. Most of the volume in this intent is declines.
2. **Refund → "I need another refund"** — the repeat-request check fires *before* eligibility is
   evaluated. An otherwise valid request still stops here. This is the fraud control.
3. **Refund → "Cancelled, no lessons used"** — the only flow that pauses. Nothing moves until the
   identity check is passed, and the console shows the payment write blocked while it waits.

Also worth a look: **Tutor didn't join → "The lesson didn't really happen"**, where the attendance
record contradicts the customer. The bot states what the record shows and stops rather than arguing.

## Reading the rules engine

Each check shows the underlying call beneath it. Reads are dim, writes are pink. The colour
difference makes it obvious how rarely the bot writes anything.

| Tag | Meaning |
| --- | --- |
| `pass` | Rule satisfied |
| `fail` | Rule not satisfied — no automated resolution |
| `flag` | Risk pattern — routed to a person regardless of eligibility |
| `hold` | Blocked pending something else, usually the identity check |

## A note on the data

Policy rules, time windows and outcomes follow Preply's published help centre — the 28-day refund
window, the 30-day review period, refunds to the original payment method only, the 15–20 minute wait
before reporting an absent tutor, and auto-confirmation at 15 minutes in the Classroom or 72 hours
outside it.

Names, amounts, dates and record IDs are illustrative. The API calls are a plausible sketch, not real
endpoints. Intent is pre-set per scenario, so this demonstrates the decision architecture rather than
language understanding.

---

Prepared by Telmo · interview case study
