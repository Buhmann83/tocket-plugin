# Tocket for Claude

Tocket is a personal organiser: you send it a screenshot or photo of an invoice, invitation, booking or school letter,
and it becomes topics with to-dos, events, payments, notes and reminders. This plugin connects Claude to your Tocket
account, so you can file things from a conversation and ask what is coming up.

## What you can ask

- "What's coming up this week?"
- "Show me my day."
- "File this" with a photo of an invoice or an invitation
- "Which payments are still open?"
- "When is the parents' evening and what do I have to bring?"
- "Remind me one day before the dentist appointment to bring my insurance card."
- "Tick off 'bake a cake'."

## What the plugin contains

| Component | Purpose |
| - | - |
| MCP server `tocket` | A remote server at `https://letkimdoit.com/mcp` (Streamable HTTP, OAuth 2.1). It provides the tools below. |
| Skill `file-to-tocket` | How Claude turns a shared picture or message into the right entries without creating duplicates. |
| Skill `tocket-review` | How Claude reviews what is due and ticks off, reschedules or completes entries. |

The plugin has no hooks, runs no local programs and installs no packages.

### Tools

| Tool | Access | What it does |
| - | - | - |
| `tocket_overview` | read | Open to-dos and payments, upcoming events, reminders and the topic list |
| `tocket_search` | read | Search all entries by keyword and date range |
| `tocket_get_topics` | read | Load complete topics with all entries, hints and reminders |
| `tocket_show_day` | read | Interactive day view in the conversation |
| `tocket_get_screenshot` | read | Show one of your saved screenshots |
| `tocket_guide` | read | The filing rules Claude follows |
| `tocket_intro` | read | A short introduction to Tocket |
| `tocket_apply` | write | Create, change, complete, move or delete entries in one atomic call |

`tocket_apply` is the only tool that changes data. It can delete entries, so Claude asks for your permission before
it runs.

## Requirements

- A Tocket account. You can create one while connecting, with Google or with email and password.
- Optional: the [Tocket app for Android](https://play.google.com/store/apps/details?id=chrisdub.kim.app). Push
  reminders are delivered through the app, and it can pick up new screenshots in the background.

## Setup

1. Install the plugin.
2. The first time Claude uses a Tocket tool, a browser window opens. Sign in to Tocket and allow the connection.
3. Ask "What can Tocket do for me here?" to get started.

To disconnect, remove the Tocket connection in Claude. To have your account and data deleted, email the address
under Support.

## Data and privacy

- Claude sends to Tocket only what a tool call contains: your search terms, and the entries you ask it to save or
  change. Tocket returns the entries of the signed-in account and nothing else.
- Your Tocket data is stored on Google Cloud. Tocket does not read your conversation, Claude's memory or your files.
- Tocket records payments as entries. It never transfers money.
- Privacy policy: <https://letkimdoit.com/privacy/en>
- Terms of service: <https://letkimdoit.com/landing/terms.html>

## Support

Email <hello@letkimdoit.com>.

## License

MIT, see [LICENSE](LICENSE). The license covers the files in this repository. The Tocket service is subject to its
own terms of service.
