---
name: product-champ-workflows
description: Generate ready-to-paste prompts for the Product Champ (GoHighLevel) AI Workflow Builder. Use when the owner wants to build or automate a workflow in Product Champ / GHL ... especially the keyword comment-to-DM automation (reply to a comment, open a DM, tag the contact, and follow up). Trigger on "build my comment to DM workflow", "make a Product Champ workflow", "automate my DMs", "set up the comment automation", "workflow builder prompt".
---

# Product Champ Workflow Prompt Builder

Product Champ (and GoHighLevel) has an **AI Workflow Builder**: you describe a workflow in plain language and it builds it for you. This skill turns the owner's goal into a tight, specific prompt they paste into that builder ... so the workflow comes out right the first time.

You are the owner's AI employee. You do not build the workflow inside Product Champ yourself ... you write the prompt, the owner pastes it into the AI Workflow Builder, and Product Champ builds it.

## Using this inside the AI First Hire Kit

This is a **Day 2** skill ... use it after the owner has connected Product Champ (see [`TOOLS-SETUP.md`](../../../TOOLS-SETUP.md)). The flagship recipe is the **keyword comment-to-DM** automation. Pull defaults from the Company Brain (offer name, voice, booking link, tags) so the prompt sounds like them and you do not re-ask what you already know.

## How the AI Workflow Builder likes its prompts

- **Keep it to about 1,200 characters.** One workflow per prompt, described step by step. Specific beats clever.
- Name the workflow, state the trigger, then number the actions in order.
- Put the owner's REAL details in the final prompt (their keyword, message text, tag, call to action) ... not placeholders.
- Plain language. No jargon. The builder reads it like instructions.

## Recipe 1 (flagship): Keyword Comment-to-DM

**What it does:** when someone comments a keyword on a post, the system publicly replies, sends them a DM with the owner's offer / call to action, tags them, and follows up if they go quiet.

**Ask the owner (or pull from the brain) before you write the prompt:**

1. The **keyword** people will comment (e.g. "GUIDE", "INFO", "START").
2. Which **post / offer** this is for, and the **platform** (Instagram, Facebook).
3. The **DM message** (what to say) and the **call to action** (a booking link, a freebie link, or "reply YES").
4. The **tag** to add (e.g. "IG Comment Lead").
5. Whether to **follow up** if they do not reply (default: yes ... 2 messages over 3 days).

**Then generate this prompt** (fill every [bracket] with their real answer, keep it about 1,200 characters):

```
Build a workflow named "[KEYWORD] Comment-to-DM". Trigger: a new comment on my [PLATFORM] posts where the comment text contains the keyword "[KEYWORD]" (not case sensitive). When it triggers, do this in order: 1) Post a short, friendly PUBLIC reply to that comment in my voice telling them I just sent them a DM ... vary the wording each time so it never looks automated. 2) Send the commenter a direct message that says: "[DM MESSAGE]" and include my call to action: [CTA ... e.g. book a call: [LINK] / grab it here: [LINK] / reply YES]. 3) Create or update the contact from that commenter and add the tag "[TAG]". 4) If they do not reply within a day, send 2 friendly follow-up DMs over the next 3 days, each adding a little value and pointing back to [CTA], then stop. Only run this when the comment contains the keyword. Keep every message in my brand voice ... short, human, no spammy or salesy language. The moment they reply, stop the automation and notify me.
```

Show the owner the finished prompt and say: *"Copy this, open Product Champ → Automation → AI Workflow Builder, paste it, and let it build. Then turn the workflow on. Want me to tweak the keyword, the DM, or the follow-up first?"*

## Recipe 2: Review Request (Google reviews)

**What it does:** after a job or appointment is done, it automatically asks the customer for a review, routes happy ones to Google, and quietly catches unhappy ones in private first.

**Ask the owner (or pull from the brain):**

1. What marks a customer as "served" ... an appointment marked complete, a job done, or a tag you add (e.g. "Served").
2. Their **Google review link** (from their Google Business Profile ... the "Get more reviews" / "Ask for reviews" link).
3. How to ask ... text, email, or both.
4. How long to wait after the job (default: a few hours, same day).

**Then generate this prompt** (about 1,200 characters, fill the brackets):

```
Build a workflow named "Review Request". Trigger: when a contact's appointment is marked completed [or: when the tag "[SERVED TAG]" is added]. When it triggers, do this in order: 1) Wait [WAIT ... e.g. 3 hours]. 2) Send the customer a friendly [text and/or email] in my voice, thanking them and asking how it went with a simple 1-to-5 question. 3) If they answer 4 or 5 (happy): send my Google review link [GOOGLE REVIEW LINK] and ask them to share a quick review, mentioning what we did for them. 4) If they answer 1 to 3 (unhappy): do NOT send the public link ... apologize, ask what went wrong, and notify me right away so I can make it right. 5) Tag happy reviewers "Promoter" and unhappy ones "Needs Follow-up". 6) If there is no response, send one gentle reminder a day later, then stop. Keep every message short, warm, and human, in my brand voice. Never beg or bribe for reviews.
```

Tell the owner to paste it into Product Champ → Automation → AI Workflow Builder, build it, then turn it on and test it on themselves first.

## Other workflows (same method)

The owner may want others (new-lead follow-up, appointment reminders, review requests, win-back, etc.). Use the same shape: ask for the trigger and the actions and their details, then write ONE focused ~1,200-character prompt that names the workflow, states the trigger, and numbers the steps, in their voice. One workflow per prompt.

## Guardrails

- Never invent the owner's links, offers, or tags ... ask, or pull from the brain.
- Keep messaging human and compliant ... no spam, no fake urgency, follow the platform's rules.
- Always say "AI employee," never "bot." Use ellipses, not em-dashes.
- After the owner builds it, remind them to **test it once** with a real comment before relying on it.
