# AI Guidelines for n8n Workflows

This file defines how AI assistants should generate or modify n8n workflows in this repository.

---

## Environment

- n8n server is **self-hosted and already running**. Never generate Docker Compose, installation scripts, or infrastructure setup.
- Target version: **n8n 2.x** (2.7.3+). The 2.x branch has significant differences from 1.x — always verify node compatibility.
- Always check [https://docs.n8n.io/](https://docs.n8n.io/) for current node documentation before proposing or generating workflow changes.

---

## Project Structure

- All n8n workflow files live in the `n8n/` folder.
- Workflow JSON files must be valid importable n8n exports.
- Each workflow gets its own `.json` file with a descriptive name.
- Keep a `README.md` in `n8n/` with setup instructions and credential requirements.

---

## Node Preferences

- **LLM integration**: Use **AI Agent node** + **OpenRouter Chat Model** sub-node. Do not use raw HTTP Request nodes for LLM calls.
- **AI Agent type**: In n8n 2.x, the AI Agent node is always a Tools Agent (the agent type selector was removed in 1.82+). Do not reference or set deprecated agent type parameters.
- **Notifications**: Use the built-in **Telegram node** (Send Message operation, HTML parse mode).
- **Email**: Use **Email Trigger (IMAP)** for monitoring mailboxes.
- Prefer built-in n8n nodes over custom HTTP requests whenever a native node exists.

---

## Workflow Design Rules

- Use **Sticky Notes** inside workflows as inline documentation. Include:
  - Overview of what the workflow does
  - Setup instructions (which credentials to configure)
  - Customization guide (where to edit rules/config)
- Use **Code nodes** (JavaScript, typeVersion 2) for data transformation and rule logic.
- Keep classification/filtering config as a clearly labeled object at the top of Code nodes so it's easy to find and edit.
- Never hard-code credentials or secrets in workflow JSON. Use n8n credential references.
- Set `appendAttribution: false` on Telegram nodes — no "sent by n8n" footer.

---

## n8n 2.x Compatibility

Key differences from 1.x to be aware of:

- AI Agent node has no agent type selector (always Tools Agent).
- OpenRouter Chat Model node is a **sub-node** that connects via `ai_languageModel` connection type, not `main`.
- IF node uses `typeVersion: 2.2` with structured conditions format.
- Code node uses `typeVersion: 2`.
- Execution order setting should be `"v1"`.
- When in doubt about a node's typeVersion or parameters, check the n8n docs — do not guess.

---

## LLM Usage

- Use **rules-first** approach: deterministic classification before LLM to minimize API costs.
- **Sanitize** email content before sending to LLM (redact PII, strip quotes, truncate).
- Set LLM temperature to **0** for classification tasks.
- Enforce **JSON-only output** from LLM via system prompt.
- Fail-safe: if LLM response cannot be parsed, default to **P1** (important), never silently drop.

---

## Error Handling

- LLM parse failures must fail-safe to P1 (never miss important mail).
- Use n8n's built-in error handling and retry where available.
- Execution history in n8n serves as the log — no external logging needed for MVP.
