# Day 1 Onboarding — the flow you run

You are the owner's Chief of Staff (see [`chief-of-staff.md`](./chief-of-staff.md)). Run these steps **in order**, with the owner, in one sitting (~20–30 min of their time). Talk to them like a person. Ask one thing at a time.

---

## Step 0 — Set up the workspace

You need a home on the owner's computer to work from.

1. Create a working folder in the owner's user profile, named for their business. For example:
   - Windows: `C:\Users\<them>\Documents\<Business> AI\`
   - Mac: `~/Documents/<Business> AI/`
2. Copy the **`company-brain-template/`** from this kit into that folder and rename it to `Company Brain`. This is where everything you learn will live.
3. Confirm with the owner: *"I've set up your workspace at [path]. This is where your business's brain will live. Ready to start?"*

> The brain is plain markdown, so it works with any agent and opens cleanly in **Obsidian** for a visual view.

---

## Step 1 — Interview the owner

Ask **one question at a time.** Listen, then ask a natural follow-up if the answer is thin. Don't move on until you genuinely understand each area. Cover:

**Their story**
- How did you get into this? What's the short version of your story?

**The business**
- What does your business do, in one plain sentence?
- Where are you based / where do you serve?

**Who they serve**
- Who *exactly* is your best customer? Describe one real person.
- What problem are they dealing with right before they come to you?
- What words do *they* use to describe that problem? What keeps them up at night?
- What do they secretly worry about when choosing someone like you?

**The offers**
- What do you actually sell? Walk me through each offer.
- (If they'll share) What does it cost?
- What makes you different from the other options they're weighing?

**The voice**
- How do you want to come across? (e.g., warm, no-nonsense, funny, expert.)
- Words or phrases you love and use a lot?
- Words you'd *never* be caught saying?

**The proof**
- Any results, wins, or happy-customer stories you can share? (Real ones only — get specifics: who, what changed, numbers if they have them.)

---

## Step 2 — Research them

If they have a website, go read it (use whatever web/scrape tool you have available — e.g. Firecrawl). Pull their services, their existing voice, their proof, their About story. Use it to **fill gaps and confirm** what they told you. Note anything that contradicts the interview and ask them about it.

If they don't have a website, skip this — the interview is enough to start.

---

## Step 3 — Build the Company Brain

Now write everything into the brain. Use the **`/brain`** skill (`.claude/skills/brain/SKILL.md`) as your system. If your environment supports it, run `/brain init` to bootstrap; otherwise just create the files directly using the template.

Fill in these pages in `Company Brain/brain/company/` (templates are already there):

- **`overview.md`** — who they are, their story, what the business does, where it serves.
- **`ideal-customer.md`** — the exact buyer, their problem, their words, their fears.
- **`products-and-services.md`** — every offer, what it includes, pricing if shared, and what makes them different.
- **`voice-and-tone.md`** — how they sound: tone, words they love, words they ban. **This is the anti-slop file — get it right.**
- **`proof.md`** — real wins and customer stories, with specifics.

Add a `people/` page for the owner, and `offers/` pages if an offer needs its own page. Follow the writing standards in the `/brain` skill (clear, factual, their voice in the examples — never invent).

Then update `brain/_index.md` so everything is catalogued.

---

## Step 4 — Prove it

Write the owner **one short social post** in *their* voice, about a real problem their customers have (pull it straight from `ideal-customer.md` and `voice-and-tone.md`). Keep it tight and human.

Show it to them and say: *"Here's a first post in your voice. Tell me what's off — too formal, wrong word, not how you'd say it — and I'll fix it."*

---

## Step 5 — Lock in the voice (the supervision loop)

- Take their feedback. Fix the post.
- **Write what you learned back into `voice-and-tone.md`** (e.g., "owner says 'clients' not 'customers'", "never use the word 'utilize'"). This is how you stop sounding like generic AI — permanently.
- Repeat until they say *"yep, that sounds like me."*

---

## Done — what the owner should have

- ✅ You — their Chief of Staff — set up and trained.
- ✅ A **Company Brain** built from their own answers, viewable in Obsidian.
- ✅ One piece of content in their real voice.

Tell them: *"That's Day 1. Tomorrow we connect me to the system that publishes your content and captures your leads."*

Then **stop.** Don't run ahead into Day 2 work.
