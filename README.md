# Pi Security for Kiro

Bring Pi Security into Kiro to understand security findings, review your security posture, and get practical guidance. This Power packages six workflows and one connection to Pi's hosted MCP service. There is no local server, build step, API key, or additional agent to configure.

**Compatibility:** this package targets Kiro IDE and optional Kiro CLI v3 using the modern [Agent Plugins format](https://kiro.dev/docs/powers/create/).

## What you can do

| Workflow | Try asking | Boundary |
| --- | --- | --- |
| [Investigate a finding](skills/investigate-pi-finding/SKILL.md) | “Help me understand Pi finding FND-123.” | Read-only evidence and interpretation |
| [Review security posture](skills/review-pi-security-posture/SKILL.md) | “Review my Pi threat-model and Code Gatekeeper posture.” | Read-only, scoped to the applications and results you select |
| [Get remediation guidance](skills/get-pi-remediation-guidance/SKILL.md) | “Show me Pi's remediation guidance without changing code.” | Fetches existing guidance; preparing a package plan requires confirmation |
| [Start a design review](skills/start-pi-security-design-review/SKILL.md) | “Queue a Pi security review of this pasted Markdown.” | Queues one review only after confirmation |
| [Import a Markdown report](skills/ingest-pi-markdown-report/SKILL.md) | “Import example-report.md into Pi; here is its Markdown.” | Starts processing only after confirmation |
| [Use secure-development guidance](skills/use-pi-secure-development-playbook/SKILL.md) | “Ask Pi for secure-development guidance for this task.” | Guidance, not automatic code changes; feedback requires confirmation |

`FND-123` and `example-report.md` are synthetic examples, not real records. Use references returned by Pi or supplied by an authorized user; never guess workspace names, repository slugs, or record IDs. Design reviews accept supported Confluence/Notion links or pasted Markdown. Report import requires both a `.md`/`.markdown` filename and pasted Markdown; these workflows do not upload local files or attachments.

## Get started on macOS

The following steps are for you to perform when you are ready to connect. Use a customer-free project and test workspace first.

### 1. Install Kiro IDE

Follow [Kiro's official installation guide](https://kiro.dev/docs/getting-started/installation/):

1. Download the macOS installer from [Kiro](https://kiro.dev/downloads/). Intel and Apple Silicon Macs are supported.
2. Run the installer and open Kiro.
3. Sign in with a supported provider: Google, GitHub, AWS Builder ID, or your organization's identity provider. Kiro sign-in is separate from Pi sign-in.
4. Import editor settings only if you want to. Review the privacy controls below before opening a project, then open a customer-free folder you are comfortable sharing with Kiro.

You also need a Pi Security account with access to the intended Pi workspace. No Pi CLI or local runtime is needed for this Power.

### 2. Import this folder as a Power

Following [Kiro's local-folder installation instructions](https://kiro.dev/docs/powers/installation/):

1. Open the **Powers** panel (the ghost icon with a lightning bolt).
2. Choose **Add Custom Power** → **Import power from a folder**.
3. Select this package's root folder—the one containing `plugin.json`, `mcp.json`, and `skills/`—and click **Install**.
4. Ask a Pi Security question to activate the Power, or use **Try the power**.

Kiro manages modern Power MCP connections internally and may prefix the server's displayed name. Do not copy `mcp.json` into your user or project settings, add a second Pi server, or look for a legacy `POWER.md`. The bundled connection may not appear in `~/.kiro/settings/mcp.json`; that is expected.

### 3. Sign in to Pi and check the workspace

The Power declares only `https://mcp.pi.security/mcp` using `streamable-http`. [Agent Plugins leaves authentication to the client](https://agent-plugins.org/plugin-authors/mcp-servers); there are no portable OAuth credential fields to fill in. [Kiro documents browser-based OAuth for remote MCP servers](https://kiro.dev/docs/mcp/configuration/).

1. When Kiro connects and offers OAuth sign-in, complete Pi's sign-in in your browser. Choose the intended Pi workspace if prompted. Review the requested access before approving it.
2. Ask: **“Use the Pi Security MCP `whoami` tool to show my authenticated Pi workspace and granted scopes. Do not run any other Pi tools yet.”** This means Pi's tool, not the terminal's `whoami` command.
3. Check that the returned workspace (tenant) is the one you intend to use. Do not proceed if it is missing, unclear, or wrong.

Your project folder, repository, and Kiro account do not select or switch the Pi workspace; the Pi OAuth session does. A successful Kiro login alone does not verify Pi access. Repeat the Pi identity check after reconnecting or changing accounts and before each write.

If the Power does not expose the expected tools or OAuth cannot finish, stop and report a sanitized error. Do not substitute tokens, custom client credentials, headers, a proxy, or a different endpoint. This package does not promise that every Kiro version can complete Pi's OAuth flow.

### 4. Start read-only; confirm each change

Begin with a scoped finding or posture question. Check the evidence and coverage shown in the response. Before a write, the skill must show the Pi workspace, action, and inputs and wait for your fresh confirmation. For reports and designs it previews a non-sensitive summary and content size rather than repeating the full document. Earlier permission to “help” or “review” is not permission to submit data.

Keep Kiro's tool approval prompts enabled. A queued review, started import, or generating plan is still in progress—not a completed result. If the response to a write is unclear, do not retry automatically; it may already have been accepted.

## Optional Kiro CLI v3

The IDE is sufficient. If you also want terminal access, [Kiro's installation guide](https://kiro.dev/docs/getting-started/installation/) documents this macOS/Linux installer command. Review the official installer before running it; it downloads and executes a script on your machine:

```sh
curl -fsSL https://cli.kiro.dev/install | bash
```

Kiro's guide says the official CLI is not distributed through Homebrew. Run `kiro-cli` and follow its browser sign-in prompt, then review the CLI privacy controls below. In a customer-free project folder, start the [v3 terminal UI](https://kiro.dev/docs/cli/v3/) with:

```sh
kiro-cli --v3
```

[Kiro CLI v3 documents automatic pickup of IDE-installed Powers](https://kiro.dev/docs/cli/v3/new-features/). Install this Power through the IDE first; do not create a separate CLI MCP configuration or custom agent. Ask a Pi Security question to activate it and use `/mcp` to inspect the connection. Complete Pi OAuth if requested and repeat the Pi `whoami` check—do not assume IDE credentials or workspace selection carry over.

CLI discovery, authentication, and tool behavior for this package remain manual compatibility checks. If v3 does not pick up the Power, stop rather than adding a workaround. See [Kiro's Power installation guidance](https://kiro.dev/docs/powers/installation/) for current supported behavior.

## Privacy controls

Review [Kiro's data-protection guide](https://kiro.dev/docs/privacy-and-security/data-protection/) before opening sensitive projects or connecting Pi. Its current instructions are:

- **IDE:** **Settings → User → Application → Telemetry and Content**. Uncheck **Data Sharing and Prompt Logging: Usage Analytics And Performance Metrics** and **Data Sharing and Prompt Logging: Content Collection for Service Improvement** to opt out of those collections.
- **CLI:** open **Preferences** in the Kiro CLI application. Turn off **Telemetry** and **Share Kiro content with AWS**. Check these separately rather than assuming the IDE settings apply. If your version exposes different controls, consult the current official guide before sharing data.

Free-tier and individual subscriptions have telemetry and service-improvement content collection enabled by default. A paid individual subscription is not the same as an enterprise account. Kiro says enterprise users are automatically opted out of those collections, while administrators control user-activity reporting.

Opting out does **not** make Kiro local-only or remove all storage. Kiro processes prompts, code context, responses, and request metadata to provide its service. Its policy describes abuse-detection exceptions, including up to 60 days of input retention for free-tier users and up to 30 days for classifier-flagged traffic on OpenAI GPT models. Review the current policy for your account and model.

Pi tool arguments and results also cross the Pi service boundary. Only send content you are authorized to share; sanitize designs, reports, and feedback before submitting them. Kiro's controls do not change Pi's data handling. See [Pi's privacy policy](https://www.pi.security/privacy-policy), [terms](https://www.pi.security/terms-and-conditions), and this package's [security guidance](SECURITY.md).

## Reconnect or remove

Use the Power's connection as displayed by Kiro; its server name may be prefixed.

- **IDE:** in the MCP servers panel, use **Reconnect** for connection trouble or **Re-authenticate** when offered for an expired token. Manage removal from the installed Power's controls. See [Kiro MCP tools](https://kiro.dev/docs/mcp/usage/) and [OAuth configuration](https://kiro.dev/docs/mcp/configuration/).
- **CLI:** `/mcp` shows the connection. Kiro documents `/mcp auth` for re-authentication, `/mcp cancel-auth` for a stuck browser flow, and `/mcp logout` for clearing a server's stored credentials. Select the Pi Power's server, not an unrelated connection.
- **Wrong Pi workspace:** stop, clear the Pi connection's authentication using Kiro's available controls, and sign in again. If your client has no suitable control, ask support rather than editing credential files. Run Pi `whoami` again before other requests.
- **Removal:** remove the installed Power and check that its skills and tools are no longer available in a fresh IDE/CLI session. Do not assume removal revokes OAuth access or deletes stored conversations or submissions; manage those separately with Kiro and Pi as needed.

Never replay an uncertain write while troubleshooting a connection.

See [SECURITY.md](SECURITY.md) for private reporting and support.

## License and attribution

Licensed under [MIT](LICENSE).

Kiro and Agent Plugins documentation linked above describe the client and package contracts; their examples are not bundled here. This package does not imply endorsement by Kiro or AWS.
