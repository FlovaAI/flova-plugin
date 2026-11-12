# Flova Plugins

Flova plugins for Codex and Claude Code, with MCP connection settings and reusable Skills for generating images, video, audio, and music, as well as creating, revising, previewing, and delivering video projects.

- Plugin and MCP server name: `flova`
- Claude package version: `0.0.6`
- Codex package version: `0.0.5`
- Environment: production
- Repository: [FlovaAI/flova-plugin](https://github.com/FlovaAI/flova-plugin)
- MCP endpoint: `https://service.flova.ai/api/mcp/`

## Install in Codex

To install from this repository, run:

```sh
codex plugin marketplace add FlovaAI/flova-plugin
codex plugin add flova@flova
```

You can also add the marketplace, then select **Flova** in the Codex plugin directory and install it. Follow the client's prompts to authorize your Flova account through OAuth, then use `$flova`.

## Install in Claude Code

To install from this repository, run:

```sh
claude plugin marketplace add FlovaAI/flova-plugin
claude plugin install flova@flova
```

Start a new Claude Code session and complete Flova OAuth authorization in `/mcp`. Use `/flova:flova` to start creating, or `/flova:flova-debrief` to review creative performance.

See the [Claude Code plugin README](plugins/claude/flova/README.md) for standalone installation, authorization, update, and local-loading instructions.

To validate and load the plugin locally:

```sh
claude plugin validate .
claude plugin validate ./plugins/claude/flova
claude --plugin-dir ./plugins/claude/flova
```

The Git installation commands use this repository's default branch. Install after the plugin files have been published there.

## Repository Layout

```text
.agents/plugins/marketplace.json       # Codex marketplace
.claude-plugin/marketplace.json        # Claude Code marketplace
plugins/
  codex/flova/
    plugin.json                       # Agent Plugins manifest
    mcp.json                          # Agent Plugins MCP configuration
    .codex-plugin/plugin.json          # Codex compatibility manifest
    .mcp.json                         # Codex compatibility MCP configuration
    assets/logo.png
    skills/                           # Skills included in the Codex package
      flova/
      flova-debrief/
  claude/flova/
    .claude-plugin/plugin.json         # Claude Code manifest
    .mcp.json                         # Claude Code MCP configuration
    assets/logo.png
    skills/                           # Claude-specific Skills and bundled references
```

Both clients connect to the production MCP service. The `X-Plugin-Agent` header identifies the client as `codex` or `claude`. Claude uses the `http` transport configuration; Codex retains its existing `streamable-http` configuration.

## Maintaining the Skills

Both packages use the same Flova MCP contracts, but their host instructions are maintained separately. Review common API changes in both packages; do not overwrite Claude-specific content boundaries when copying files from Codex. This repository does not require maintenance scripts for installation or runtime.

Each package contains its own Skill instructions, reference documents, and assets so all installed content stays within that plugin's root directory. The Claude package treats online inspirations and creative Skill documents as reference data, not instructions for Claude to execute. New host workflows must be bundled in a plugin release. Only the Codex package includes `agents/openai.yaml` for Codex-specific presentation and invocation metadata.

`flova` handles direct media generation and project workflows; `flova-debrief` supports creative performance reviews. Preview cards are used only in Codex when the service exposes the relevant tools. Claude presents results using supported media or returned links.

For Claude release preparation, see the [submission checklist](docs/claude-submission.md), [review cases](plugins/claude/flova/REVIEW_CASES.md), and [release notes](plugins/claude/flova/CHANGELOG.md). Preparing repository files does not mean the plugin or its remote connector has been approved for Anthropic's directory.

## Service and Authorization

This repository contains plugin configuration and Skills. It does not include the MCP server implementation, frontend pages, or preview-card builds. Installing a plugin does not deploy the service or UI. Available tools, permissions, and capabilities depend on the connected production MCP service.

Each user authorizes their own account through the client using OAuth. Do not commit tokens, cookies, or Authorization header values to the repository or MCP configuration.
