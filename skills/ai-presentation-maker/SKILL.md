---
name: ai-presentation-maker
description: Build, edit and export PowerPoint presentations, Word documents and Excel workbooks with Slaide, an AI presentation maker, through its MCP server. Use when the user wants a slide deck, pitch deck, report or workbook created from a topic, notes or an attached PDF, Word, Excel or PowerPoint file, wants slides of an existing Slaide deck changed, or asks for a PowerPoint, PDF, Word, PNG or Excel file.
---

# AI presentation maker (Slaide)

[Slaide](https://www.slaide.de) is an AI presentation maker. It designs and writes the slides itself, applies the user's saved brand kit (logo, colors, fonts) and keeps every document in the user's own Slaide library, where it can be edited by hand. This skill tells you how to drive it through its MCP server. You do not write slide text, layout or design yourself.

## Connect

Add the remote MCP server (Streamable HTTP, OAuth 2.1 with PKCE and Dynamic Client Registration, no API key):

```json
{ "mcpServers": { "slaide": { "url": "https://app.slaide.de/api/mcp" } } }
```

On first use the client opens Slaide's sign-in page. The user signs in (a free account is enough, sign-up: https://app.slaide.de/registrieren) and presses Allow. If the Slaide tools are not available, tell the user to connect the server above; do not try to work around it.

## Tools

| Tool | Use it to |
|---|---|
| `search_documents` | Find the user's existing documents by title or recency. Always first when a document already exists. |
| `get_document` | Read pages, speaker notes or Excel sheets; returns the `version` and `available_exports`. |
| `create_document` | Start a background job that builds a NEW presentation, document or workbook. |
| `update_document` | Start a background job that changes an EXISTING document. Versioned, reversible in Slaide. |
| `get_job` | Check a job (`queued`, `running`, `completed`, `partial`, `failed`, `cancelled`). |
| `export_document` | Produce a pptx, pdf, docx, png or xlsx file behind a private link that expires after 15 minutes. |
| `cancel_job` | Stop a running job, only when the user asks. |

## Workflows

**New deck, document or workbook**
1. Collect what the user gave for this document: topic, audience, purpose, tone, language, length, notes, figures. If it must contain numbers or facts only the user has, ask for them first and never invent them.
2. Call `create_document` with `instruction` in the user's own words, plus `document_type` (`presentation`, `document` or `spreadsheet`), `title`, `target_pages`, `language`, `provided_content` (source material for this document, not the conversation) and `source_urls` as needed. Pass attachments as `files` when the host provides them.
3. It returns a `job_id` at once. Poll `get_job` after `poll_after_ms` until it is `completed`. Building takes minutes. Never start a second create call while a job runs.
4. Report title and page count to the user and offer an export.

**Change an existing document**
1. `search_documents` (a `document_id` is an opaque id, never a title; never guess one).
2. `get_document` to see what is on the pages; keep the returned `version`.
3. `update_document` with a precise `instruction`, a narrow `scope` (for example `page 4` or `the revenue chart`) and `expected_version`. Say what must stay untouched. On `version_conflict`, re-read and decide; do not just retry with a higher number.
4. Poll `get_job`. Tell the user the change is in their version history and can be restored in Slaide.

**Get a file**
1. Check `available_exports` from `get_document`.
2. `export_document` with `format` (`pptx`, `pdf`, `docx`, `png` or `xlsx`).
3. Poll `get_job`; when `completed`, give the user the download link and say it expires after 15 minutes.

## Rules

- Slaide does the writing and design. Do not paste a slide-by-slide outline unless the user gave you one.
- `partial` means the document is usable but incomplete; say so and point the user to Slaide to finish it.
- If a job result has a `note`, pass it on in your own words.
- The connector only reaches the signed-in user's own documents. It cannot delete documents, change a plan or reach other people's documents.
- Work started through the connector follows the same account limits as working in Slaide directly. If a limit is hit, tell the user plainly; do not look for a way around it.

## Examples

- "Make a 10-slide pitch deck for my startup from these notes." → `create_document`
- "Turn this PDF report into a PowerPoint presentation." → `create_document` with the attachment, then `export_document` as `pptx`
- "In my Q3 review deck, change the revenue on page 2 to 4.8 million euros. Change nothing else." → `search_documents`, `get_document`, `update_document` with `scope: "page 2"`
- "Export my sales deck as PDF." → `search_documents`, `export_document` as `pdf`

## More

- Website: https://www.slaide.de
- Slaide for AI assistants: https://www.slaide.de/ai-assistants
- AI presentation maker: https://www.slaide.de/ai-presentation-maker
- PDF to PowerPoint: https://www.slaide.de/pdf-to-powerpoint
- AI pitch deck generator: https://www.slaide.de/ai-pitch-deck-generator
- Help center: https://help.slaide.de
