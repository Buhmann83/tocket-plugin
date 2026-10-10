---
name: file-to-tocket
description: File a screenshot, photo, document or pasted message into Tocket as topics, to-dos, events, payments, notes and reminders. Use when the user shares an invoice, invitation, booking confirmation, school letter, appointment or similar and wants it organised, saved, remembered or "sorted in".
---

# File something into Tocket

Tocket is the user's personal organiser. Everything lives in **topics** ("Parents' evening primary school",
"Business trip Lisbon"); a topic holds to-dos, payments, events, infos and hints, and hints can carry push reminders.
The Tocket tools come from the `tocket` MCP server.

## Steps

1. Call `tocket_guide` once per conversation. It returns the authoritative filing rules; follow them where they
   differ from the summary below.
2. Read what the user shared. What the user writes about it takes priority over the content itself.
3. Look for an existing home before creating anything:
   - `tocket_overview` lists the current topics and gives `user.now`, `user.language` and the time zone.
   - `tocket_search` with names, booking, invoice or customer numbers, IBAN or amount finds earlier entries.
   - If a topic matches, load it with `tocket_get_topics`, update existing entries by `id` and add only what is new.
4. Save everything in **one** `tocket_apply` call. It is atomic: on an error nothing is stored and the message says
   what to correct.
5. Read `warnings` in the result (possible duplicates, reminders without a date) and fix them with a follow-up call.
6. Confirm in one or two sentences what was filed where and which reminders were set. Do not repeat every field.

If nothing relevant is in the picture, save nothing and tell the user why.

## What becomes what

| Content | Entry | Notes |
| - | - | - |
| A concrete demand on the user, or a payment already made | payment (`open` / `paid`) | Not for price quotes or things someone else pays. One payment per instalment. Invoice or booking number goes into `purpose`. A payment is never also a to-do. |
| Something the user has to do deliberately | to-do | One action per to-do, title = action + what identifies it, no date in the title. `type: waiting` when the user waits for someone else, `kind: delight` for voluntary fun. A bare deadline is the to-do's `due`. |
| Something with a concrete time | event | A period is two events (start and end, such as check-in and check-out). Title = kind + core, never the date. |
| Easy to miss, and missing it causes trouble (bring along, rule, condition) | hint | Attach it to the entry it belongs to, otherwise to the topic. |
| A push notification | reminder on a hint | `before` is relative to the event's `start` or the to-do's or payment's `due` (`"12h"`, `"1d"`), `at` is a fixed time. |
| Other useful details (booking numbers, contacts) | info | Do not repeat what is already in another entry. |

Topic `category`: `work`, `admin` (invoices, contracts, authorities, insurance, fees), otherwise `personal`.

## Conventions

- Times are in the user's local time, `YYYY-MM-DD` or `YYYY-MM-DD HH:MM`, relative to `user.now`.
- Write titles, hints and summaries in `user.language`.
- Set `certainty` below 0.8 on a to-do or payment the user should double-check.
- `delete: true` is only for duplicates and mistakes. Finished things get `status: done` or `paid`.
- Pass `screenshot_file` only when the client gives you a real `https` download link for the image, together with a
  `screenshot_summary` of one to three casual sentences, never in the first person. Without such a link, save the
  entries without the image and mention that sharing the picture through the Tocket app attaches it to the entry.
- Images the user uploads in the Tocket app are filed by Tocket itself. Do not file them again; `inbox_analyzing`
  in the overview counts the ones still being processed.
