# Jubilee Energy

Webflow site maintained through the Webflow MCP, tracked in Linear.

## Key identifiers

| Thing | Value |
| --- | --- |
| Webflow site | `Jubilee Energy Website` — site ID `62f43b2326c698a49188bfb5` |
| Production domains | `www.jubileeenergy.com`, `jubileeenergy.com` |
| Webflow staging | `jubilee-energy-website.webflow.io` |
| Linear project | [Jubilee Energy](https://linear.app/codo-digital/project/jubilee-energy-6646d7e899ef) |
| Linear team | Codo Digital (`COD`) |

There is a `[Backup] JE` site (`67a950db7bcf9729613a4e03`) in the same workspace — never edit it.

## Ticket workflow

All work in this project is tracked by a Linear ticket in the **Jubilee Energy** project. Move
the ticket through these states as the work progresses:

1. **In Progress** — set this *before* touching the Webflow site.
2. **In Review** — set this as soon as the coding/design changes are complete in Webflow.
   The site is *not* published at this point; changes sit unpublished in the Designer.
3. **Done** — only after the site has been **manually published** by a human *and* QA has
   passed against the live site. Never move a ticket to Done off the back of an unpublished
   change, and never publish the site to move a ticket along.

Notes:

- Publishing is a manual, human step. Do not call `publish_site` unless explicitly asked.
- Leave a comment on the ticket describing what changed and where, so QA has something to
  test against.
- If a ticket turns out not to be actionable in Webflow (DNS, Cloudflare, hosting headers),
  say so on the ticket and leave it out of In Review rather than closing it.
