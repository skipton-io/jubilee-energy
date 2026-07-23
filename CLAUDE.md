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

## What the Webflow Data API can and cannot reach

Established by working the audit backlog. Check here before promising a fix.

**Reachable:**

- Page `<title>` and meta description — `data_pages_tool > bulk_update_pages` (up to 100 at a time).
  Pass `seo.title` *and* `seo.description` together; the object is replaced, not merged.
- Per-page JSON-LD — `bulk_update_pages_schema_markup` (up to 25 at a time).
- CMS item fields, including the `title-tag` and `meta-description` SEO fields that the Blog Post
  and Product Category templates bind to — `data_cms_tool > update_collection_items`.
  Partial `fieldData` is safe; omitted fields keep their values. Updates stage as drafts.
- Site-wide head/footer custom code — `data_scripts_tool > set_site_freeform_code`. This is the
  route for site-level CSS and JS fixes.

**Not reachable — needs a human in the Designer or another system:**

- **Canonical URL.** No canonical field exists on the page object at all. Page Settings → SEO only.
- **Element markup and styles** — attributes such as `aria-hidden`, heading levels, alt text on
  non-CMS images, class definitions, component structure.
- **Response headers** (CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy) and
  `/.well-known/*` paths. These sit with Cloudflare, which fronts the site.
- **DNS and TLS**, including the apex domain. Cloudflare.

## Gotchas

- `set_site_freeform_code` **replaces the entire block**. Always `get_site_freeform_code` first
  and re-send the existing content with your addition appended, or you will silently delete the
  google-site-verification meta, the GA4 tag and the GTM container.
- CMS collections carry long rich-text bodies, so a full `list_collection_items` easily exceeds
  the tool output limit. The result is written to a file; parse it with python rather than
  refetching in smaller batches.
- The `Product Categories` collection has slug `products`, so `/products/<x>` pages are Product
  *Category* items — not items of the separate `Products` collection.
- Several CMS items are `isDraft: true` while still having a `lastPublished` date. Check the flag
  before assuming an item is live.
