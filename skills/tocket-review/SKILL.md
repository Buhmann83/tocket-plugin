---
name: tocket-review
description: Review what is coming up in Tocket and tidy it. Use when the user asks what is due today, this week or next week, wants a daily or weekly review, asks about open payments, deadlines or reminders, or wants to tick off, reschedule or complete entries.
---

# Review and tidy Tocket

Use the tools of the `tocket` MCP server. Answer the user's actual question first; do not recite everything.

## Pick the right read

| The user wants | Call |
| - | - |
| "What's coming up?", planning, a first look | `tocket_overview`: open to-dos and payments, events of the next 14 days, reminders, topic list, plus `user.now`, language and time zone. One call answers most questions. |
| A visual day ("show me my day", "what does tomorrow look like?") | `tocket_show_day` with `day` as `YYYY-MM-DD`, or without it for today. The view shows the data itself, so add only a short remark afterwards. |
| A time range ("next week", "in October") | `tocket_search` with `from_date` and `to_date` and no query. |
| A specific thing (name, place, invoice number, IBAN, amount) | `tocket_search` with `query`; narrow with `types` and `status`. |
| Everything about one topic, or before changing it | `tocket_get_topics` with the `topic_id` from a search hit. |
| A saved screenshot | `tocket_get_screenshot` with its `id`. If it is not found, the id may be stale: search for the topic, load it and use the current id. |

## Shape of a good review

1. Overdue and due today first, then the rest of the period in date order.
2. Open payments with amount, recipient and due date.
3. Things the user is waiting for (`type: waiting`) with what they are waiting on.
4. Hints and reminders that need preparation (bring along, check-in windows).
5. Low-certainty entries the user should confirm.

Keep it short. Group by day or topic, and leave out empty sections.

## Making changes

All writes go through `tocket_apply`, as many changes as needed in one atomic call.

- Tick off: `todos: [{ "id": "...", "status": "done" }]`. Mark a payment `paid` the same way under `payments`.
- Reschedule: send only the changed field (`due`, `start`, `end`). Sending `null` clears a field.
- Add to an existing topic: `topics: [{ "id": "...", "todos": [ ... ] }]`.
- Set a reminder: add a hint with `reminder.before` (`"30m"`, `"2h"`, `"1d"`, `"1w"`) or `reminder.at`.

Carry out small, unambiguous changes directly and confirm briefly. Ask only when it is unclear which entry is meant.
Delete (`delete: true`) only duplicates and mistakes, and say that you did. Completed items are marked done or paid.

Times are in the user's local time, `YYYY-MM-DD` or `YYYY-MM-DD HH:MM`, relative to `user.now`. Reply in
`user.language`.
