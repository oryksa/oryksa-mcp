---
name: oryksa-support-chat
description: Add an AI customer support chat (AI employee) to the website being built, using the ORYKSA AI Employees MCP server. Use when the user asks for a support chat, chatbot, AI assistant, AI agent, live chat, customer service, FAQ bot or WhatsApp assistant on their site, or asks to connect ORYKSA.
---

# Add an ORYKSA AI support chat to the site

ORYKSA gives the site an AI employee that answers customers 24/7 in the website chat (and later on WhatsApp, Telegram and the phone). The agent must KNOW the business, so the most important step is teaching it the site content.

## Steps

1. **Status.** Call `oryksa_status`. If the tools are missing or unauthorized, tell the user to connect the MCP server (`claude mcp add --transport http oryksa-ai-employees https://mcp.oryksa.com`, then `/mcp` to sign in).
2. **Business profile.** Call `oryksa_setup_business` with facts you find in the project: business name, what it does, services, prices, opening hours, location, contacts, languages of the site, tone. Only send what the project or the user states. Never invent prices or policies.
3. **Learn the site (the key step).** Scan the repository for customer-facing content:
   - pages and routes (`app/`, `pages/`, `src/pages`, `src/routes`, `*.html`, `*.astro`, `*.vue`, `*.svelte`)
   - components with copy (hero, pricing, services, FAQ, footer with address and hours, about, contact)
   - content and data files (`content/`, `*.md`, `*.mdx`, `*.json`, `*.yaml`, CMS seeds, product/menu/price lists)
   - i18n dictionaries when the site is multilingual (send each language)

   For each page, extract the VISIBLE text a customer would read (no code, no class names, no imports). Then write a dense `business_summary` fact sheet: what is sold, every price, hours, location and areas served, booking/ordering, delivery, payment methods, policies (returns, cancellation), contacts. Call `oryksa_learn_site` with `pages` and `business_summary`.
   If the site is already live and public, `oryksa_learn_from_url` can crawl it instead (it may miss content rendered only in the browser).
4. **FAQ.** Call `oryksa_add_faq` for important questions that have clear answers in the project.
5. **Widget.** Call `oryksa_get_widget_snippet` with the detected framework and insert the code ONCE in the root layout so it appears on every page (it has the ORYKSA look, do not restyle it). Use `oryksa_customize_chat` for the agent's photo if the user has one, and `oryksa_set_allowed_domains` with the production domain.
6. **Test.** Call `oryksa_test_agent` with 2-3 real customer questions (a price, the opening hours, how to book). If an answer is wrong or missing, fix the content and call `oryksa_learn_site` again.
7. **Tell the user** what was set up, and give the links from `oryksa_links` (dashboard, connect WhatsApp by QR, plans).

## Keep it in sync

When the site's content changes (new prices, services, pages), call `oryksa_learn_site` again with the full current content. It replaces the previous version.

## Rules

- Never put secrets in the site: the widget token is public by design, nothing else is needed.
- Never invent business facts. Ask the user when something important is missing (prices, hours).
- WhatsApp is connected by the owner in the dashboard (QR code); do not try to do it.
