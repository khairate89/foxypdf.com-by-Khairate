# foxypdf.com-by-Khairate
FoxyPDF.com by Abdessamad KHAIRATE
# FoxyPDF.com - Project Handoff

Last updated: 2026-03-03

## 1. Project Summary

This repository is a full-stack PDF tools platform (Node.js + SSR HTML/CSS/JS) with:

- Multi-tool PDF processing workflows
- Async job system + status tracking
- Locale-aware routing + SEO pages
- Authentication + plan scaffolding
- Privacy UX (auto-delete + manual delete)

The product brand is now **FoxyPDF.com**.

## 2. Current Product Direction

Primary UX direction:

- iLovePDF-inspired layout patterns
- Wide workspace with clear left (document/work area) and right (settings/actions) separation
- Fast, simple, low-friction interaction model
- Reference-driven visual parity: homepage and generic tool pages now follow a compress-tool style composition (hero card, split workbench cards, trust cards, soft cloud background).

Brand direction:

- Name: **FoxyPDF.com**
- Header/footer branding with inline custom **fox-head SVG** + wordmark
- Primary action color: red
- Clean light UI for tool workspaces

## 3. What Is Already Achieved

### Core platform

- Tool routing + page rendering from shared templates
- Async jobs and job status page with progress stages
- Queue + workers for implemented tools
- Locale/subdomain routing and SEO/canonical/hreflang support
- Global Google Analytics instrumentation (`gtag.js`) injected from shared layout so all locales/routes report page views consistently.
- Auth routes and session controls
- Auth system upgrade (account security + recovery):
  - Email verification required before email/password login.
  - Forgot password flow with one-time reset link tokens.
  - Forgot username flow (account email reminder).
  - Resend verification email flow.
  - Basic anti-bruteforce protections (temporary lockout after repeated failed logins + per-IP auth rate limiting).
  - Optional social OAuth sign-in routes for Google, Facebook, and Microsoft (env-config driven with graceful fallback when not configured).
- Automatic environment variable bootstrap from root `.env` / `.env.local` for runtime and AI tool configuration.
- Advanced SEO architecture pass:
  - New `/use-cases` hub + detail pages for long-tail problem intent
  - Tool-page depth sections (best practices, common problems, related searches)
  - Cross-linking between tools, how-to guides, and use-case pages
  - Breadcrumb JSON-LD for tool, tools hub, how-to, and use-case pages
  - Sitemap expansion to include use-case URLs for every locale subdomain
  - Added production-ready legal/support pages with indexable SEO routes: `/contact`, `/about`, `/privacy`, `/terms` (plus aliases `/privacy-policy`, `/terms-of-service`), including localized metadata and breadcrumb schema.
  - Added `Article` JSON-LD on use-case detail pages (with existing `HowTo`, `FAQPage`, and `BreadcrumbList`) for stronger rich-result semantics.
  - Global tool-page FAQ normalization upgraded to long-tail phrasing across the full tool catalog (no generic "How do I use X?" template blocks).
  - Tool meta generation hardened with intent-oriented title/description defaults and trust-signals appending only when missing.
  - Production caching policy now differentiates high-traffic HTML, secondary HTML, SEO infrastructure files, and static assets with explicit CDN/browser TTLs plus stale-while-revalidate/stale-if-error for faster repeat loads.

### New non-AI tools (implemented)

- **Compress For Email** (`/compress-for-email`)
  - Target-size compression flow for email attachment constraints.
- **Extract Images** (`/extract-images`)
  - Extract embedded PDF images per selected pages and export ZIP/single image.
- **PDF Metadata** (`/pdf-metadata`)
  - View metadata as JSON/TXT, edit metadata fields, remove metadata fields, optional report archive.
- **Remove Blank Pages** (`/remove-blank-pages`)
  - Blank-page detection with sensitivity controls, optional duplicate-page removal, optional report archive.
- **Split By Bookmark** (`/split-by-bookmark`)
  - Split by top-level bookmark destinations with ZIP output and optional split report.
- **Batch PDF Processor** (`/batch-pdf-processor`)
  - Upload multiple PDFs and apply one operation to all files in one run (`compress`, `pdf_to_text`, `pdf_to_word`).
- **Workflow Builder** (`/workflow-pdf`)
  - Build and run chained workflows (`compress`, `ocr`, `split`) against uploaded files with reusable local workflow profiles.
  - Editor now supports dynamic step management (add, remove, reorder, and per-step settings).
  - Workflow deletion now removes rows cleanly (no forced auto-recreate placeholder workflow).
  - Step picker now loads from the full tools menu (registry-driven), and workflow payloads use `tool_key` + `options` per step.
  - "Create new workflow" is hardened to always insert a new row immediately, open it in the editor, and work across legacy DOM id variants.
- **Developer tools (implemented)**
  - `public_api_access` (`/public-api-access`): generates API docs in `md/txt/json` formats.
  - `webhook_pdf_automation` (`/webhook-pdf-automation`): generates webhook payload templates and signature examples.
- **New conversion/tools wave (implemented)**
  - `pdf_to_powerpoint` (`/pdf-to-powerpoint`)
  - `powerpoint_to_pdf` (`/powerpoint-to-pdf`)
  - `text_to_pdf` (`/text-to-pdf`)
  - `repair_pdf` (`/repair-pdf`)
  - `compare_pdf` (`/compare-pdf`)
  - `remove_duplicate_pages` (`/remove-duplicate-pages`)
  - `summarize_pdf` (`/summarize-pdf`)
  - `translate_pdf` (`/translate-pdf`)
  - `ai_redaction` (`/ai-redaction`)
  - `pdf_read_aloud` (`/pdf-read-aloud`)
  - All above are wired end-to-end: route + SEO content + worker execution + menu/hub visibility.
  - AI-labeled tools now use real provider-backed processing (OpenAI):
    - `summarize_pdf`: LLM summary + key points
    - `translate_pdf`: LLM translation (chunked for long text)
    - `ai_redaction`: LLM sensitive-data detection + redaction report/output
    - `pdf_read_aloud`: TTS MP3 generation (single MP3 or ZIP of tracks for long docs)
- **Catalog expansion from PDF24 gap analysis (implemented, non-AI)**
  - Added registry/routes + worker execution for:
    - `pdf_converter`, `convert_to_pdf`, `convert_from_pdf`
    - `create_pdf`, `webpage_to_pdf`, `view_pdf`
    - `excel_to_pdf`, `docx_to_pdf`, `doc_to_pdf`, `xlsx_to_pdf`, `xls_to_pdf`, `rtf_to_pdf`, `epub_to_pdf`
    - `pptx_to_pdf`, `ppt_to_pdf`, `jpg_to_pdf`, `png_to_pdf`
    - `pdf_to_jpg`, `pdf_to_png`, `pdf_to_docx`, `pdf_to_pptx`, `pdf_to_xlsx`, `pdf_to_rtf`, `pdf_to_epub`, `pdf_to_html`
    - `remove_pdf_metadata`, `change_pdf_metadata`, `web_optimize_pdf`, `pdf_to_secure_pdf`, `pdf_to_pdfa`, `generate_password`
  - Added UI accept filters and no-upload allowances for new no-file tools.
  - Worker coverage is preserved: all registry keys are implemented and test suite passes.

## 4. Run/Test Commands

```bash
npm install
npm run dev
npm test
```

Notes:

- There is currently no `npm run build` script.
- Dev server is configured with file watching in `src` + `public`.


## 5. Open Work / Next Priorities

1. Validate UX and production behavior for automation tools (`batch_pdf_processor`, `workflow_pdf`) across locales/routes.
2. Expand Workflow Builder step catalog beyond `compress/ocr/split` while preserving async pipeline guarantees.
3. Extend bookmark splitting parser coverage (named destinations and deeper outline structures).
4. Add tool-specific tests for `extract_images` and `split_by_bookmark` with fixture PDFs containing images/bookmarks.
5. Keep global FoxyPDF visual consistency and continue UX polish across all tools/homepage.
6. Improve rich in-browser preview depth for Office formats (currently metadata-first, browser-limited preview messaging).
7. After non-AI stabilization, begin AI roadmap tools:
   - `/summarize-pdf`
   - `/extract-tables`
   - `/smart-redact`
6. Continue expanding advanced PDF-editing tools still missing from competitor parity (`bookmark_pdf`, `flatten_pdf`, `crop_pdf`, `fill_out_pdf`, `create_fillable_pdf_form`, `annotate_pdf`).
7. Add infrastructure-as-code or scripted deployment (currently deployed manually to one DigitalOcean droplet).

## 8. Product Roadmap Source

Primary roadmap document:

- `NEXT_GENERATION_PDF_TOOLS_ROADMAP.md`

When implementing roadmap items:

- Add the completed route/tool to section 3 (achieved work)
- Update section 7 priorities
- Append changelog entry with date and delivered items

