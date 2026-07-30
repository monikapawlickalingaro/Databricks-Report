# Report User Trainer — Databricks Edition

A voice AI guide shown alongside a Databricks AI/BI Dashboard **embedded on a web page**. It walks dashboard users through it 24/7 — explaining what each page and visual means and how to navigate — so teams depend less on repeated live training sessions.

## The problem

Every time a client gets a new or updated Databricks dashboard, someone has to run a training session. Those sessions get repeated for new joiners, forgotten details, and follow-up questions. Report User Trainer replaces that recurring effort with an always-available guide that sits right next to the dashboard.

## Approach — dashboard embedded on a web page

- The **Databricks AI/BI Dashboard is embedded on a web page** (an `<iframe>` using the dashboard's published/share embed URL).
- The **ElevenLabs agent is added as a widget** on that same page, floating over the dashboard so users can ask questions while looking at it.
- Dashboard pages are switched by changing the iframe's `src` — the same URL-driven navigation pattern used in the Power BI version of this project, adapted to Databricks' URL structure (see below).

This gives us full control over the page layout, styling, and the placement of the voice widget.

## MVP scope — "structural only"

The MVP guides users through the **structure** of a dashboard, based on a manually curated Q&A knowledge base. It does **not** read the dashboard's live data.

**In scope**
- Web page hosted on GitHub Pages that embeds the Databricks dashboard via an `<iframe>`
- ElevenLabs Conversational AI agent added as a widget on the page
- Voice-driven navigation between dashboard pages
- Manual Q&A knowledge base describing pages, visuals, filters, metrics
- Escalation to a human trainer for questions outside the knowledge base

**Out of scope (deferred to Phase 2)**
- Automated ingestion of dashboard metadata
- Multi-dashboard / multi-tenant management
- Navigation to a specific visual in fullscreen (`fullscreenWidget` parameter) — technically observed as possible, not yet built
- Navigation to a specific filtering - technically observed as possible, not yet built

## A note on Databricks Genie

Databricks dashboards can have a **Genie Agent** attached, which already answers data questions in natural language. This project's voice agent is **not** a replacement for that — it teaches dashboard **structure** (pages, metrics, filters, known pitfalls) and hands off anything data-specific to the dashboard's own Genie Agent or to a human trainer. Keep this boundary in mind when writing the knowledge base and the agent's guardrails.

## Tech stack

| Layer | Tool |
| --- | --- |
| Web page UI | Vanilla HTML / CSS / JS (no framework for MVP) |
| Hosting | GitHub Pages |
| Dashboard embedding | Databricks AI/BI Dashboard embedded on the page via `<iframe>` (requires Databricks login — see below) |
| Voice widget | ElevenLabs — Conversational AI, added as a page widget |
| Knowledge base | Manual Q&A documents, uploaded to the ElevenLabs agent |
| Escalation | Client Tool (opens on-screen form) + Webhook Tool (server-side call to Power Automate / Teams) |

All tools are on Lingaro's approved AI governance stack.

## Project structure

```
databricks-genie-trainer/
├── index.html # Web page that embeds the dashboard + ElevenLabs widget
├── css/style.css # Styling (Databricks-accented, adaptable to client brand)
├── js/main.js # Navigation, escalation, video overlays
├── video/intro-video.mp4 # Auto-generated welcome video (added once dashboard is mapped)
└── docs/ # Dashboard map, Q&A knowledge base, test plan (added once dashboard is chosen)
```

## Embedding the dashboard and the ElevenLabs widget

The page embeds the Databricks dashboard in an `<iframe>` and adds the ElevenLabs agent as a widget:

```html
<!-- Databricks AI/BI Dashboard embedded on the page -->
<iframe
  id="report-frame"
  title="Databricks dashboard"
  src="DATABRICKS_DASHBOARD_EMBED_URL"
  frameborder="0"
  allowFullScreen="true">
</iframe>

<!-- ElevenLabs agent as a widget -->
<elevenlabs-convai agent-id="YOUR_AGENT_ID"></elevenlabs-convai>
<script src="https://unpkg.com/@elevenlabs/convai-widget-embed" async type="text/javascript"></script>
```

Replace `DATABRICKS_DASHBOARD_EMBED_URL` with the dashboard's published/share embed link, and `YOUR_AGENT_ID` with your ElevenLabs agent ID.

### Page navigation — confirmed URL pattern

Unlike Power BI (where the page ID is a query parameter, `&pageName=...`), Databricks appends the page as a **path segment**, and the default/first page has **no page segment at all**:

```
Default page:  DATABRICKS_BASE/published?o=ORG_ID
Other pages:   DATABRICKS_BASE/published/pages/PAGE_ID?o=ORG_ID
```

`main.js` must special-case the default page (no `/pages/...` to append). See `js/main.js` for the working `navigateToDatabricksPage()` implementation.

### Login requirement

Databricks dashboard embedding **requires the viewer to be signed in** to the Databricks workspace (confirmed by testing — unlike Power BI's optional Publish-to-web, there is no anonymous/public embed mode for this dashboard). Workspace admins must also explicitly allow the hosting domain as an approved embed destination before this will work at all (Databricks workspace Settings → embedding).

## Deploy

1. Publish this repo with GitHub Pages (Settings → Pages → deploy from `main`).
2. In the Databricks workspace, add the GitHub Pages domain to the list of allowed embed destinations (required — embedding is blocked by default).
3. Open the GitHub Pages URL — the page shows the embedded dashboard with the ElevenLabs voice widget. Viewers will be prompted to sign in to Databricks if they don't have an active session.

## Roadmap

- **Phase 1 (this MVP):** structural guidance from manual Q&A + voice, on a page that embeds the dashboard. Dashboard to be provided — see `docs/user-stories-looker-databricks.md` for the full breakdown (Epic B).
- **Phase 2:** navigation to specific visuals in fullscreen, filtering through URL, automated metadata ingestion, packaging as a client upsell.
