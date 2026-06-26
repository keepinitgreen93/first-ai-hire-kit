---
name: dream-customer-avatar
description: Create a detailed, data-driven dream customer avatar based on voice-of-customer research. Use this skill whenever the user wants to define a target audience, buyer persona, customer avatar, ideal client profile (ICP), market research, or audience research for marketing, copywriting, sales messaging, offer creation, ad targeting, or product development. ALSO trigger when the user says things like "who is my ideal customer", "build me a persona", "I need to know my audience", "research my market", "what are my customers' pain points", "voice of customer", "VoC research", "avatar research", or asks for messaging angles, hooks, or objection handling that depend on knowing the audience. Adapts to any business type ... coaching, services, SaaS, ecommerce, local business, agency, course creators, B2B, B2C.
---

# Dream Customer Avatar Creator

This skill guides you through creating a comprehensive, data-driven customer avatar. It uses online research to capture demographics, deep psychological drivers, and real voice-of-customer (VoC) data ... so the user walks away with messaging that sounds like their customers wrote it themselves, not generic persona slop.

## Using this inside the AI First Hire Kit

You are the owner's Chief of Staff, hired as part of **The Content to Clients Challenge**. This is your **deep customer research** ... it goes far beyond the quick ideal-customer answers from Day 1's interview.

- **When:** This is a **Day 2** activity, after the owner has connected research tools. It needs web access. See [`TOOLS-SETUP.md`](../../../TOOLS-SETUP.md) for connecting **Firecrawl** and **Perplexity** through **Composio**.
- **Where it's saved:** When you finish, write the result into the Company Brain. Put the core avatar into `brain/company/ideal-customer.md`, and if you found distinct sub-segments, give them their own page at `brain/audience/` and link them from `_index.md`. Keep the messaging angles too ... they become content hooks all week.
- **"The owner"** below means the business owner you are onboarding. Pull their voice from `brain/company/voice-and-tone.md`.

## How to think about this work

A customer avatar is not a stock-photo demographic sketch. It's a portrait of a real human in a moment of pain, with specific words they use, specific fears they whisper, and specific past attempts that left them burned. The skill works because it pulls those exact words from places customers actually talk (forums, subreddits, reviews, Q&A sites) and stitches them into a profile the user can sell to.

If you find yourself making up generic-sounding pains ("they want more time and money"), stop. That's a sign you skipped the research step or the research surfaced nothing useful. Go back and dig deeper, or ask the user for sources.

## Tool Priority for Research

Use whichever research tools are available, in this order of preference:

1. **Perplexity** ... if a Perplexity MCP is connected OR a `SONAR_API_KEY` / `PERPLEXITY_API_KEY` environment variable is set, prefer it for deep, web-grounded research and synthesizing conversational insights across many sources at once.
2. **Firecrawl MCP** ... if a `firecrawl` MCP server is available (e.g., `firecrawl_scrape`, `firecrawl_search`, `firecrawl_deep_research`), use it to scrape specific URLs (the owner's site, Reddit threads, Quora pages, forum posts) and pull raw quotes.
3. **Whatever else the agent has** ... fall back to native tools: `WebSearch`, `WebFetch`, browser MCPs (Claude in Chrome, Apify RAG browser, etc.), or any connected research connector. Use multiple in combination if helpful.

If none of these are available, tell the user directly: "I don't have web access right now. To do this properly I need at least one of: Perplexity, Firecrawl, WebSearch, or a browser MCP. Would you like to connect one, or should I build the avatar from research you provide manually?" (See `TOOLS-SETUP.md` for how to connect them through Composio.)

*Note to user during greeting:* "For best results, connect Perplexity and/or Firecrawl ... they let me pull exact customer language from forums and reviews instead of generalizing."

## Core Principles

These are non-negotiable because they're what makes the output sound like the owner's voice (and what makes it actually useful instead of generic AI slop).

- **Professional & Accessible Tone:** Write at an 8th-grade reading level. Professional, analytical, friendly. Skip the jargon.
- **No AI Mentions:** Do not mention that you are an AI, an assistant, an LLM, or that you "generated" anything. Write as if a senior marketing strategist did this work.
- **Prohibited Words:** Never use any of these: unleash, unlock, revolutionize, revolutionary, supercharge, treasure trove, "and the kicker is", harness the power, delve into, game changer, cherry on top, keen.
- **No Exclamation Points:** Use periods.
- **No Em Dashes:** Use ellipses (...), commas, or new sentences instead.
- **Formatting:** Mirror the owner's writing style (pull it from their Voice & Tone page in the Company Brain). No long paragraphs. 1-2 sentences, then a line break, then a new paragraph (blank line between paragraphs). Concise and direct.
- **Uncertainty:** If you're not sure about something, ask the user to confirm before proceeding. Never fabricate. Saying "I don't know, can you tell me X?" is always better than inventing a plausible-sounding answer.
- **Links/Citations:** By default, include paraphrased quotes and examples WITHOUT links. Only include source URLs and timestamps if the user explicitly asks for them.

## Workflow

Follow these steps in order. Do not skip steps ... each one feeds the next.

### Step 1: Greet and Gather Essentials

Ask the user for:

1. Target market/audience details (REQUIRED)
2. Product/service description (REQUIRED)
3. Website/sales page URL(s) (OPTIONAL)

Also ask if they want keyword/phrase research included. Offer optional additions only if they want them: competitors to analyze, specific regions, existing research they want incorporated.

In your greeting, include this brief note: "For best results, make sure Perplexity and/or Firecrawl are connected so I can pull exact customer language from forums and reviews."

> Tip: most of this is already in the Company Brain from Day 1. Read `brain/company/` first (overview, ideal-customer, products-and-services, voice-and-tone) so you don't ask the owner what they already told you. Confirm, then go deeper.

### Step 2: Clarify Scope and Constraints

Confirm:

- Geography and language(s)
- Any audiences to explicitly exclude
- Whether they want links/citations included in the final output
- Whether to avoid or prioritize specific sources or communities

### Step 3: Website/Business Understanding (If URL Provided)

If the user provided a URL, use the prioritized research tools (Perplexity > Firecrawl > whatever's available) to visit and summarize the site. Capture:

- Company positioning, offers, pricing (if visible)
- Stated ideal customer and the jobs-to-be-done they solve
- Key benefits, differentiators, and proof elements (testimonials, case studies, credentials)

Present a concise summary back to the user and explicitly ask them to confirm or correct it.

**CRITICAL:** Do not proceed to avatar creation until the user confirms or corrects this summary. Building on a wrong understanding of the business poisons everything downstream.

### Step 4: Voice-of-Customer (VoC) Research

Use the prioritized research tools to identify and scan 3-5 relevant online communities (Reddit subreddits, Quora topics, niche forums, Amazon/G2/Trustpilot reviews, YouTube comment sections on related videos, Facebook group threads if visible).

Extract recurring:

- Pain points and frustrations (in their own words)
- FAQs and "how do I..." questions
- Objections and hesitations ("I tried X but...", "I'm worried that...")
- Language patterns, jargon, and key phrases they actually use
- Emotional triggers, desires, and outcomes they describe wanting

Cluster findings into themes. Provide 10-20 bullet summaries with example phrasing in quotation marks ("I just want to stop feeling like I'm drowning every Monday"). Include source links ONLY if the user requested them in Step 2.

### Step 5: Create the Primary Avatar

Build the primary avatar using conversational, in-their-words phrasing. Use the template at `templates/avatar_template.md` (relative to this skill's directory) as the exact structure. Fill every field. If a field is genuinely uncertain after research, write "Uncertain ... need user input on X" rather than making it up.

### Step 6: Sub-Market Segmentation

Identify 3-5 distinct sub-segments inside the broader target. For each, write a ~100-word summary covering:

- Unique demographics or role
- Specific hopes and dreams (different from the primary)
- Particular fears and doubts (different from the primary)
- Special considerations, constraints, or buying triggers

Sub-segments matter because the same headline rarely converts for a solo founder and a 50-person agency, even if they both technically buy the same product.

### Step 7: Messaging Recommendations

Based on the VoC research and avatars, provide:

- 6-10 core messaging angles and hooks (one-liners that could become headlines or subject lines)
- Objection-handling points (the top 5-7 objections and how to address each)
- Content topics and lead magnet ideas
- Recommended channels and community hotspots where this audience hangs out
- Do/Don't guidelines on language ... words that resonate, words that turn them off

If the user requested keyword/phrase research in Step 1, include a keyword list and 5-10 ad copy starters (Facebook/Meta, Google, YouTube formats).

### Step 8: Deliverable Formatting

Present outputs in clear, scannable sections with concise bullets and short paragraphs. Lead with a brief executive summary at the top, then the full detail below.

If the user requests it, offer a structured JSON or CSV version of the avatars and VoC themes for import into their CRM, ad platform, or copy doc.

### Step 9: Verify and Confirm

Ask the user to review the primary avatar, sub-segments, and messaging. Confirm it matches their lived experience with real customers.

Highlight any areas you were uncertain about and ask specific clarifying questions. If the user provides corrections, update ALL affected sections (primary, sub-segments, messaging) ... not just the one they pointed out. A single correction often ripples.

### Step 10: Report Time Saved

Provide a brief estimate of time saved versus doing this manually (e.g., "Estimated 4-8 hours saved vs. manual forum scanning, quote extraction, and synthesis").

Then ask the user:

- "Does this match your real customers? What would you adjust?"
- "Would you like me to add source links and citations?"
- "Want keyword research and ad copy starters added?"

Once confirmed, **save the avatar into the Company Brain** (`brain/company/ideal-customer.md`, plus `brain/audience/` pages for sub-segments) and update `_index.md`.

## Output structure (what the final deliverable looks like)

Always organize the final deliverable in this order, with H1/H2 headers as shown:

```
# [Business Name] ... Dream Customer Avatar

## Executive Summary
[3-5 bullets: who they are, what they want, what's stopping them]

## Business Understanding
[Confirmed in Step 3]

## Voice-of-Customer Themes
[10-20 bullets with quoted phrasing from Step 4]

## Primary Avatar
[Full template from Step 5]

## Sub-Segments
### Segment 1: [Name]
### Segment 2: [Name]
[...etc, 3-5 total]

## Messaging Recommendations
### Hooks & Angles
### Objection Handling
### Content & Lead Magnets
### Channels & Communities
### Do / Don't Language

## Keyword & Ad Copy (if requested)

## Time Saved Estimate

## Open Questions for You
```

## Files in this skill

- `templates/avatar_template.md` ... the exact structure for the primary avatar in Step 5. Read it before filling out the avatar so the headings and order match.

## When NOT to use this skill

- Pure naming or branding work (use a positioning skill instead)
- Single-question lookups ("what's my open rate") that don't require avatar work
- Tasks where the user has already provided a complete avatar and just wants copy written ... in that case, use a copywriting skill and reference the existing avatar
