# Whatsable WhatsApp MCP server

Whatsable is a hosted WhatsApp MCP server on the official WhatsApp Business API. It connects Claude, ChatGPT, Cursor, Claude Code, Codex, VS Code and other MCP clients to your own WhatsApp Business numbers. It is free to start.

- **Server URL:** `https://api.whatsable.app/mcp`
- **Transport:** streamable HTTP
- **Auth:** OAuth (the AI app registers itself, then you sign in and approve), or a Whatsable API key as a Bearer header
- **Website:** [whatsable.app/whatsapp-mcp](https://whatsable.app/whatsapp-mcp?utm_source=github&utm_medium=readme&utm_campaign=mcp_listing)
- **Docs:** [docs.whatsable.app/mcp](https://docs.whatsable.app/mcp)

There is no code to install in this repository. The server is hosted. This repository holds the setup steps, the tool list and the [`server.json`](server.json) for the official MCP Registry.

## What it does

Ask your AI app things like:

- "Who wrote to me today and is still waiting for an answer? Draft a reply to each."
- "Follow up with Maria tomorrow at 10:00, unless she replies before then."
- "Send the order_shipped template to these 20 customers."
- "Give me my daily WhatsApp report: chats, failures, handoffs, and what is scheduled."

It reads conversations, sends messages and Meta-approved templates, schedules follow-ups, and manages contacts and labels. On paid plans it also runs broadcasts, manages AI agents and reports on team response times. One connection can use one, several or all of your WhatsApp numbers.

When you connect, you choose the access: **Full access**, **Send and read**, or **Read only**. The AI app sees only the tools that access allows. Read only cannot send or change anything.

## Plans

| Plan | Price | MCP server | Limits |
|---|---|---|---|
| Free | $0 | Yes | 1 WhatsApp number, unlimited messages with up to 100 people a month, templates, scheduled messages, last 30 days of history |
| Work | $19/month | Yes | 500 people a month, team inbox, broadcasts |
| Growth | $39/month | Yes | 1,500 people a month, AI agent |

Every new account starts with 14 days of Pro, no card needed, counted from the first connected number. Pro and Agency also include the MCP server and add API keys. Bulk Message does not include the MCP server. Meta bills its template fees separately, with no Whatsable markup. Full limits: [docs.whatsable.app/mcp/plans](https://docs.whatsable.app/mcp/plans).

[Sign up for free](https://console.whatsable.app/sign-up?product=connector&utm_source=github&utm_medium=readme&utm_campaign=mcp_listing). An owner or admin of the Whatsable organization connects the AI app.

## Connect

### Claude (claude.ai, desktop, mobile)

1. In Claude, open Plugins, then the Connectors filter, and choose **Add custom connector**.
2. Name it `Whatsable`. As the remote MCP server URL, enter `https://api.whatsable.app/mcp`.
3. Under OAuth client, choose **Register automatically**, then **Add**. (Do not choose "Use Claude's published identity".)
4. Sign in to Whatsable, choose the organization, the access and the numbers, then **Approve**.

On Team and Enterprise, an owner adds it once under Organization settings → Connectors.

### ChatGPT (web)

1. Open Plugins. Choose **+**, then **Add custom MCP server**.
2. Name it `Whatsable` and enter `https://api.whatsable.app/mcp`.
3. Under authentication, choose **OAuth**. Leave static client credentials empty.
4. Sign in to Whatsable, choose the organization, access and numbers, then **Approve**.
5. Confirm, choose **Create as a plugin**, check the tools, then **Install**.

Start a new chat, type `@` and select Whatsable. Sending needs a Business, Enterprise or Edu workspace. On Pro, ChatGPT can only read.

### Cursor

Open Customize → MCPs and add a new MCP server. In `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "whatsable": {
      "url": "https://api.whatsable.app/mcp"
    }
  }
}
```

Save, then choose **Connect** on the Whatsable entry and approve in the browser.

### Claude Code

```bash
claude mcp add --transport http whatsable https://api.whatsable.app/mcp
```

```bash
claude mcp login whatsable
```

Add `--scope user` to use it in every project. Check it with `claude mcp get whatsable`.

### OpenAI Codex

```bash
codex mcp add whatsable --url https://api.whatsable.app/mcp
```

```bash
codex mcp login whatsable
```

In the IDE extension, open the gear menu, select MCP servers, then Add server. Choose Streamable HTTP and enter the URL.

### VS Code (GitHub Copilot agent mode)

Run **MCP: Add Server**, choose **HTTP**, enter `https://api.whatsable.app/mcp` and name it `whatsable`. Or add it by hand to `~/.copilot/mcp-config.json`:

```json
{
  "mcpServers": {
    "whatsable": {
      "type": "http",
      "url": "https://api.whatsable.app/mcp",
      "tools": ["*"]
    }
  }
}
```

Sign in when VS Code asks, then use Copilot Chat in Agent mode.

### Any other MCP client

Add `https://api.whatsable.app/mcp` as a remote streamable HTTP server. If the client cannot run OAuth, send an API key (Pro and Agency) as `Authorization: Bearer <your Whatsable API key>`. Keep the key in an environment variable, never in a committed file.

Setup guides for Gemini, Grok, Perplexity, Copilot Studio, Windsurf, Zed, Cline, Continue, LibreChat and Cherry Studio: [whatsable.app/whatsapp-mcp](https://whatsable.app/whatsapp-mcp?utm_source=github&utm_medium=readme&utm_campaign=mcp_listing).

## Tools

The server lists its own tools, and the list depends on the access you approve and your plan.

| Group | Tools |
|---|---|
| Account and numbers | `get_account`, `get_setup_status`, `list_numbers`, `list_members`, `create_number_connect_link` |
| Messages | `list_messages`, `get_message`, `send_text_message`, `send_template_message`, `send_media_message`, `list_saved_replies`, `upload_media`, `react_to_message` |
| Conversations and AI | `list_conversations`, `get_conversation`, `read_conversation`, `set_conversation_unread`, `add_conversation_note`, `delete_conversation_note`, `set_conversation_ai`, `set_contact_follow_ups`, `cancel_ai_retry`, `mark_figures_checked` |
| Templates | `list_templates`, `get_template`, `create_template`, `delete_template` |
| Contacts and labels | `find_contacts`, `get_contact`, `create_contact`, `update_contact`, `list_labels`, `add_contact_label`, `remove_contact_label`, `create_label`, `update_label`, `delete_label` |
| Scheduled messages | `schedule_message`, `list_scheduled_messages`, `get_scheduled_message`, `cancel_scheduled_message` |
| Broadcasts | `list_broadcasts`, `get_broadcast`, `list_broadcast_recipients`, `create_broadcast`, `cancel_broadcast`, `set_broadcast_status` |
| AI agents | `list_ai_bots`, `get_ai_bot`, `read_ai_bot_text`, `list_knowledge_files`, `list_ai_bot_files`, `create_ai_bot`, `update_ai_bot`, `duplicate_ai_bot`, `delete_ai_bot`, `assign_ai_bot_to_number` |
| Analytics | `get_message_analytics`, `get_template_analytics`, `get_conversation_analytics`, `get_ai_analytics`, `get_broadcast_analytics` |
| Help | `get_guide` |

Ask your assistant "What can you do with Whatsable?" and it explains its own tools.

## How it differs from other WhatsApp MCP servers

There are three kinds of WhatsApp MCP server:

| Kind | What it is | When it fits |
|---|---|---|
| Personal WhatsApp Web bridges | Open-source servers that log in to a personal WhatsApp account through WhatsApp Web (QR code) and run on your machine | Personal experiments. They use an unofficial connection, so the number can be banned, and they stop when your machine is off |
| Meta's WhatsApp Business Tools MCP | Meta's own MCP for setting up and debugging WhatsApp Business accounts | Setup and troubleshooting. It does not send customer conversations |
| Hosted messaging MCP on the official API (Whatsable) | A hosted server on Meta's official WhatsApp Business Platform | Real customer conversations: replies, templates, follow-ups, broadcasts and several numbers, with no ban risk from an unofficial login |

Whatsable and Meta's Business Tools MCP work well together: Meta's for account setup, Whatsable for the conversations. With Meta Coexistence you can keep using the WhatsApp Business app on the same number.

## Links

- [WhatsApp MCP overview](https://whatsable.app/whatsapp-mcp?utm_source=github&utm_medium=readme&utm_campaign=mcp_listing)
- [MCP docs](https://docs.whatsable.app/mcp)
- [Plans and limits](https://docs.whatsable.app/mcp/plans)
- [Server card](https://whatsable.app/.well-known/mcp/server-card.json)

## License

The text in this repository is under the [MIT License](LICENSE). The hosted Whatsable service has its own terms at [whatsable.app](https://whatsable.app/?utm_source=github&utm_medium=readme&utm_campaign=mcp_listing).
