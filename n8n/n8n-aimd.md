# n8n AI.md

<span style="color: rgb(34, 34, 34); font-family: -apple-system, 'system-ui', 'Segoe UI', Oxygen, Ubuntu, Roboto, Cantarell, 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif; font-size: 3.425em; font-weight: 400;">🧠 AI.md — Guidelines for AI Assistance in n8n Project</span>

## ⚙️ Project Context

This project is part of the **n8n automation ecosystem** — a workflow automation platform based on **Node.js, TypeScript, and Docker**, using **PostgreSQL or MongoDB** as backend databases.  
AI contributions should strictly follow **n8n’s open-source architecture, coding standards, and documentation style**.

---

## 📘 Primary References

When in doubt, **always rely on official documentation**:

<table id="bkmrk-area-reference-n8n-d"><thead><tr><th>Area</th><th>Reference</th></tr></thead><tbody><tr><td>n8n Docs</td><td>🔗 [https://docs.n8n.io](https://docs.n8n.io/)</td></tr><tr><td>n8n GitHub</td><td>🔗 [https://github.com/n8n-io/n8n](https://github.com/n8n-io/n8n)</td></tr><tr><td>n8n Developer Guide</td><td>🔗 [https://docs.n8n.io/integrations/creating-nodes/](https://docs.n8n.io/integrations/creating-nodes/)</td></tr><tr><td>Node &amp; Credential Templates</td><td>🔗 [https://github.com/n8n-io/n8n/tree/master/packages/nodes-base](https://github.com/n8n-io/n8n/tree/master/packages/nodes-base)</td></tr><tr><td>Contribution Guide</td><td>🔗 [https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md](https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md)</td></tr><tr><td>Coding Conventions</td><td>🔗 [https://docs.n8n.io/development/code-conventions/](https://docs.n8n.io/development/code-conventions/)</td></tr><tr><td>n8n Docker Setup</td><td>🔗 [https://docs.n8n.io/hosting/installation/docker/](https://docs.n8n.io/hosting/installation/docker/)</td></tr></tbody></table>

---

## 💡 AI Usage Principles

### ✅ Do

- Use **TypeScript** for all source contributions.
- Follow **official code style** (Prettier, ESLint, n8n conventions).
- Place **tests only in `/tests/` directory**.
- Store **documentation files in `/docs/` directory**, except `README.md`.
- Keep **JavaScript/TypeScript files under 800 lines** — split logic into modules if needed.
- Prefer **async/await** over callbacks.
- Use **`n8n-workflow` and `n8n-core`** utilities instead of re-implementing.
- Always include **input/output schemas** for new nodes and credentials.
- Add **typed interfaces** (`INodeExecutionData`, `INodeProperties`, etc.).
- Write **unit tests** with `jest` following n8n’s example test suite.

### 🚫 Don’t

- Don’t generate inline CSS/JS inside HTML files — keep them modular.
- Don’t use deprecated Node APIs or libraries not listed in `package.json`.
- Don’t push example or temporary test files outside `/tests/`.
- Don’t hardcode secrets, tokens, or URLs — use **environment variables** or **credential nodes**.
- Don’t modify Docker entrypoints unless necessary for compatibility.
- Don’t assume root-level privileges inside Docker containers.

---

## 📦 File &amp; Directory Rules

<table id="bkmrk-path-description-%2Fpa"><thead><tr><th>Path</th><th>Description</th></tr></thead><tbody><tr><td>`/packages/nodes-base/nodes/`</td><td>Built-in n8n nodes. Follow official node templates.</td></tr><tr><td>`/packages/nodes-base/credentials/`</td><td>Custom credentials. Must include `displayName`, `name`, `properties`, `authenticate()`.</td></tr><tr><td>`/packages/core/`</td><td>Core workflow engine logic — **do not modify** unless extending officially.</td></tr><tr><td>`/packages/workflow/`</td><td>Type definitions and helpers — extend here for new data types.</td></tr><tr><td>`/docs/`</td><td>All architecture, AI, and feature documentation.</td></tr><tr><td>`/tests/`</td><td>All test files. Use Jest.</td></tr><tr><td>`/docker/`</td><td>Container definitions. Keep `.env` and config templates updated.</td></tr></tbody></table>

---

## 🧩 Templates

### 🧠 Node Template

```ts
import { INodeType, INodeTypeDescription } from 'n8n-workflow';

export class ExampleNode implements INodeType {
  description: INodeTypeDescription = {
    displayName: 'Example Node',
    name: 'exampleNode',
    group: ['transform'],
    version: 1,
    description: 'Performs an example transformation',
    defaults: { name: 'Example Node' },
    inputs: ['main'],
    outputs: ['main'],
    properties: [
      {
        displayName: 'Input',
        name: 'input',
        type: 'string',
        default: '',
        required: true,
      },
    ],
  };

  async execute() {
    const items = this.getInputData();
    const returnData = items.map((item) => ({ json: { value: item.json.input } }));
    return this.prepareOutputData(returnData);
  }
}

```

### 🔐 Credential Template

```ts
import { ICredentialType, INodeProperties } from 'n8n-workflow';

export class ExampleApi implements ICredentialType {
  name = 'exampleApi';
  displayName = 'Example API';
  properties: INodeProperties[] = [
    {
      displayName: 'API Key',
      name: 'apiKey',
      type: 'string',
      default: '',
    },
  ];
}

```

---

## 🧱 Best Practices

<table id="bkmrk-area-recommendation-"><thead><tr><th>Area</th><th>Recommendation</th></tr></thead><tbody><tr><td>**Security**</td><td>Use `.env` or credential nodes. Never expose sensitive values.</td></tr><tr><td>**Logging**</td><td>Use `this.logger` in nodes; avoid `console.log()`.</td></tr><tr><td>**Performance**</td><td>Reuse connections; avoid excessive API calls in loops.</td></tr><tr><td>**Testing**</td><td>Include sample input/output in `/tests/nodes/`.</td></tr><tr><td>**Schema Validation**</td><td>Always validate inputs and outputs; return consistent structures.</td></tr><tr><td>**Versioning**</td><td>Increment node version when breaking changes occur.</td></tr><tr><td>**Docs**</td><td>For new nodes, create a short Markdown doc in `/docs/nodes/`.</td></tr></tbody></table>

---

## 🧰 Local Development Commands

```bash
# install dependencies
pnpm install

# build packages
pnpm build

# start n8n locally
pnpm start

# run tests
pnpm test

# run eslint
pnpm lint

```

---

## 🧑‍💻 Contributing with AI

When AI contributes or refactors code:

- Always include a **short commit message** describing *intent, not just action*.
- Use consistent formatting (Prettier + ESLint).
- When generating code or docs, **link to relevant n8n documentation** for maintainers.
- For unclear architecture decisions, prefer following **existing node examples** rather than inventing new patterns.

---

## 🧩 Recommended Examples for AI Reference

- Example Node: [HTTP Request Node](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/nodes/HttpRequest/HttpRequest.node.ts)
- Example Credential: [Slack API Credentials](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/credentials/SlackApi.credentials.ts)
- Example Test: [HTTP Request Node Test](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/nodes/HttpRequest/test/HttpRequest.node.test.ts)

---

## 🧾 Notes for Documentation AI

When generating documentation or README updates:

- Use [n8n Markdown style](https://docs.n8n.io/reference/markdown-guide/).
- Include code snippets, version compatibility, and usage examples.
- Keep documentation under `/docs/`, not in root.

---

## 🧩 Suggested AI.md Add-ons

Optionally include:

- `AI_MODEL.md` — to specify which LLMs or prompts to use.
- `AI_EXAMPLES.md` — with example prompts for node creation.
- `AI_REVIEW_GUIDE.md` — explaining review checklist for AI-generated PRs.