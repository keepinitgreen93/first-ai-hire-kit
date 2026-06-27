# Connect Your AI Employee's Tools (Composio)

Your AI employee gets a lot smarter when it can reach out to the web ... read your competitors, pull real customer language from forums and reviews, and research your market. The way it gets those abilities is **Composio**.

> **This is mostly a Day 2 step.** Day 1 is just hiring and training your AI employee. You connect tools on Day 2, right before the **deep customer research** (the Dream Customer Avatar). The Day 2 video walks you through every click.

---

## What Composio is (and why it matters)

**Composio** is the hub that plugs tools into your AI employee. Instead of setting up each tool separately, you connect them once in Composio, then connect Composio to your AI employee. One hub, many tools.

- **Get Composio 👉 https://composio.dev**

*(Your **Product Champ** connection is separate ... Composio can't host custom connectors, so that one plugs straight into Hermes. See "Connect Product Champ" below.)*

---

## The two research tools for your Dream Customer Avatar

These two give your AI employee real eyes on the web so it can build your customer avatar from your customers' *actual words*, not generic guesses:

| Tool | What it does | Get it |
|---|---|---|
| **Perplexity** | Deep web research ... pulls and synthesizes insights across many sources at once. | https://www.perplexity.ai |
| **Firecrawl** | Reads specific pages ... your site, Reddit threads, reviews, forums ... and pulls the exact quotes. | https://www.firecrawl.dev |

Your AI employee uses **Perplexity first, then Firecrawl** (and falls back to whatever else it has). Connect at least one.

---

## How to connect them (high level)

1. Sign up for **Composio** (link above).
2. In Composio, **connect Perplexity and/or Firecrawl** (you'll create a free/cheap account on each and paste the key into Composio).
3. **Connect Composio to your AI employee** (in Hermes) so it can use those tools.
4. That's it ... your AI employee can now research the web.

*The Day 2 video shows the exact screens. Don't worry about getting it perfect alone.*

---

## Connect Product Champ (your CRM)

Your **Product Champ** connection works a little differently. Composio can't host custom connectors, so this one plugs **straight into Hermes** as an MCP server:

1. Get your own copy of the **GHL connector** 👉 https://github.com/keepinitgreen93/Go-High-Level-MCP-2026-Complete (click **Fork**, or **Download ZIP**).
2. In Product Champ, create a **Private Integration Token** and copy your **Location ID**.
3. Add the connector to **Hermes as an MCP server**, and put your **token + Location ID** in its `.env`. That points it at *your* account only.

Now your AI employee can post, tag, message, and manage leads in your Product Champ. *(The Day 2 video walks every click.)*

---

## What you'll use them for

- **Day 2 ... Dream Customer Avatar (deep research).** Your AI employee runs the `dream-customer-avatar` skill: it scans where your customers actually talk and builds a data-driven avatar in their own words. This is what makes every piece of content land. *(See [`.claude/skills/dream-customer-avatar/`](./.claude/skills/dream-customer-avatar/).)*
- **Day 4 ... Content research.** The same web access helps your AI employee see what's working in your space and feed your content engine.

---

*Powered by TAMI ... Truly Authentic Marketing Intelligence.*
