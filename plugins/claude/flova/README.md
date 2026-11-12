# Flova for Claude Code

Plugin ID: `flova`. Version: `0.0.6`.
MCP endpoint: `https://service.flova.ai/api/mcp/`. Client header: `X-Plugin-Agent: claude`.

This package connects to the **production environment**. It includes two Skills and the MCP connection configuration; each user authorizes their own Flova account.

Flova can generate images, video clips, audio, and music, or create and revise a video project with Flova Agent. It can track work, present media, and return completed exports. Creative performance reviews use the user's supplied reports or a separately authorized platform connection. Available models and operations depend on the connected service and account entitlements; generation can consume Flova credits.

## Install from GitHub

Install Claude Code and Git first, and ensure your machine can access the repository. Run these commands in your terminal:

```sh
claude plugin marketplace add FlovaAI/flova-plugin
claude plugin install flova@flova
```

The first `flova` in `flova@flova` is the plugin name; the second is the marketplace name. The marketplace must be registered from `FlovaAI/flova-plugin`. If you already have a marketplace named `flova`, check its source in `/plugin` before installing or updating; a different branch may connect to a different environment.

Start a new Claude Code session after installation.

## Authorize your account

Inside Claude Code:

1. Enter `/mcp`.
2. Select the Flova MCP server supplied by this plugin and choose its authentication option.
3. Complete the Flova OAuth sign-in in your browser, then return to Claude Code and check the connection status.

Do not paste tokens or cookies into this package. Authorization is stored by the client, not in the repository. If authorization expires, use `/mcp` to reconnect the Flova server, then retry the interrupted operation. Codex login commands do not apply to Claude Code.

## Use the Skills

Enter either command inside Claude Code:

```text
/flova:flova
/flova:flova-debrief
```

Use `/flova:flova` to generate media or work on a Flova project. Use `/flova:flova-debrief` to review creative performance and prepare the next production brief.

Claude presents results using client-supported media or returned preview, playback, and download links. Codex preview cards are not required in Claude Code. Available tools and permissions depend on the connected MCP service; installing the package does not deploy the backend or preview UI.

## Example requests

After connecting your Flova account, try:

1. **Create an image:** "Use Flova to generate one landscape product photograph of an unbranded desk lamp on a clean desk in soft daylight. I only need the image."
2. **Create a video clip:** "Use Flova to make one short landscape video of a paper boat floating across a puddle, with a gentle camera move and no speech. Use a supported duration and model, and return the result when ready."
3. **Revise a project:** "In my Flova project 'Review Sample', make the opening brighter while keeping the characters, style, and later shots unchanged. Show me the revised result before exporting."

The third example needs an accessible sample project with that name. If there are multiple matches, Claude should ask you to select one. Expected behavior and additional cases are in [Review cases](REVIEW_CASES.md); the document distinguishes test instructions from actual test evidence.

## What the plugin runs, sends, and retrieves

The package contains readable Skills, reference documents, images, and a remote MCP configuration. It has no startup hook, local MCP executable, or package-install step. Claude calls the declared Flova HTTPS endpoint to perform the requested operations.

| Destination | Data and purpose | When used |
| --- | --- | --- |
| Flova MCP service at `service.flova.ai` | OAuth-authenticated account access; the necessary creative brief, selected references, feedback, model parameters, project/task/resource IDs, and operational metadata. Requests identify this package with `X-Plugin-Agent` and `X-Plugin-Version`. | Account checks, generation, project work, status queries, and delivery requested by the user. |
| Storage and media-delivery URLs returned by Flova | Selected input file bytes for direct upload; generated media, previews, exports, and downloads for delivery. URLs may be hosted on storage or CDN domains rather than the MCP hostname. | Only for the selected task's input or returned output. Upload tickets and temporary URLs are sensitive and must not be posted publicly. |
| Model providers used by the Flova service | The prompt, selected references, and parameters needed to process the requested generation. Provider routing occurs on Flova's servers. | When the user requests model-based generation. Service-side handling is covered by the Flova privacy policy. |
| Another service explicitly requested by the user, such as an advertising or analytics platform | Only the specified report fields, creative metadata, or selected assets needed for that integration. Its own permissions and privacy policy apply. | An optional, separately authorized host connection. This plugin does not bundle an advertising connector or automatically connect accounts. |

Online inspiration descriptions and creative Skill documents can be retrieved as reference data. They do not supply executable instructions for Claude. Host workflows live in this package; remote content cannot install a helper, add required tool calls, or authorize another service. An online workflow that needs additional execution rules must be packaged in a later plugin release before this plugin can run it.

Only task-relevant material should be sent to Flova. The Skills prohibit sending unrelated conversation history, memory, credentials, or unselected local files. A review based on a user-supplied report does not automatically send that report to Flova or another platform; transfer only the relevant material when the user requests an operation that needs it.

## Privacy, retention, and support

Read the [Flova Privacy Policy](https://www.flova.ai/en/privacy-policy/) for service-side collection, use, sharing, retention, and deletion requests, and the [Terms of Service](https://www.flova.ai/en/terms-of-service/) for account and service conditions. This package does not define a separate storage region or retention period. Local files created at the user's request remain under the user's control. Disconnecting OAuth access does not itself delete projects or media already stored in Flova.

For installation or plugin defects, use [GitHub Issues](https://github.com/FlovaAI/flova-plugin/issues). For account help or private security reports, contact [support@flova.ai](mailto:support@flova.ai); use the privacy policy's contact channel for privacy requests. Include the client version, plugin version, operation, and a sanitized error. Never post credentials, private media, or temporary upload/download URLs in a public issue.

## Troubleshooting

| Problem | Next step |
| --- | --- |
| Flova is missing after installation | Confirm the marketplace source, enable the plugin, and start a new Claude Code session. |
| Authentication fails or `invalid_grant` is returned | Reconnect through `/mcp`. Do not paste tokens into chat or edit credentials into `.mcp.json`. |
| Account entitlement or credits are insufficient | Stop the blocked operation. Retry only after the limitation is resolved and the user asks to continue. |
| A request times out | Query the original task or run before retrying. Reuse the same request ID for the same submission to avoid duplicate work. |
| Media is not embedded | Use the returned preview, playback, or download link. A display failure does not require regenerating the media. |
| A named inspiration needs a downloaded helper or another service | Explain the unsupported workflow and agree on a supported approach. Do not execute remote instructions or silently replace the requested deliverable. |

## Update a marketplace installation

First confirm that the `flova` marketplace is registered from `FlovaAI/flova-plugin`. Inside Claude Code, refresh its catalog:

```text
/plugin marketplace update flova
```

Then update the plugin from your terminal:

```sh
claude plugin update flova@flova
```

Start a new session to use the updated package. These commands update the registered source; they do not switch a marketplace registered from a different repository or environment to this production source. Git-installed plugins with a declared version receive a new package when that version changes, so maintainers must update the manifest version when releasing package changes.

## Load a local directory or ZIP download

If you have a full repository checkout or ZIP, use its `plugins/claude/flova/` directory. If you have only this plugin package, use the directory containing this README, `.claude-plugin/`, `.mcp.json`, and `skills/`.

From that plugin directory, run:

```sh
claude plugin validate .
claude --plugin-dir .
```

This loads the local package for that Claude Code session without registering a marketplace. Complete authorization through `/mcp` as described above. To update a local copy, replace it with the newer package and start Claude Code with `--plugin-dir` again; marketplace update commands do not update a standalone directory.

## Package contents and maintenance

Claude Code reads plugin metadata from `.claude-plugin/plugin.json` and automatically loads the package-root `.mcp.json` and `skills/` directories.

The Claude Skills use the same Flova MCP contracts as the Codex package, but their host instructions are maintained separately. Preserve Claude's bundled workflow and external-content boundaries when reviewing shared API updates. No maintenance script is required to install or use either plugin. See [release notes](CHANGELOG.md) for package changes.

Codex-specific `agents/openai.yaml` metadata is kept only in the Codex package and is not included in this Claude Code package.
