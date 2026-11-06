# Flova for Claude Code

Plugin ID: `flova`. Version: `0.0.5`.
MCP endpoint: `https://service.flova.ai/api/mcp/`. Client header: `X-Plugin-Agent: claude`.

This package connects to the **production environment**. It includes two Skills and the MCP connection configuration; each user authorizes their own Flova account.

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

Claude follows the service's presentation instructions using client-supported media or returned preview, playback, and download links. Codex preview cards are not required in Claude Code. Available tools and permissions depend on the connected MCP service; installing the package does not deploy the backend or preview UI.

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

The Skills are shared with the Codex package, including reference documents and assets. When updating them, keep the Skill instructions, reference documents, and assets identical across both packages. No maintenance script is required to install or use either plugin.

Codex-specific `agents/openai.yaml` metadata is kept only in the Codex package and is not included in this Claude Code package.
