# Claude Submission Preparation

This checklist separates repository preparation from the service, account, and policy work required for Anthropic's directory. Local validation is not directory approval.

## Candidate source

| Field | Value |
| --- | --- |
| Repository | `FlovaAI/flova-plugin` |
| Plugin path | `plugins/claude/flova` |
| Preparation branch | `feature/claude` |
| Candidate package version | `0.0.6` |
| Remote MCP endpoint | `https://service.flova.ai/api/mcp/` |

Enter `feature/claude` in the portal's branch field explicitly; it contains a slash. A local branch is not available to the portal until it is committed and pushed. For a production listing, deliberately choose the branch or tag to track. The README's normal installation commands and documentation URL use `main`; candidate documentation remains on the preparation branch until merged. Revalidate after changing the tracked commit.

## Repository preparation

- [x] Use an HTTPS remote MCP configuration and OAuth through the host.
- [x] Match the manifest version and `X-Plugin-Version` header.
- [x] Document setup, examples, data destinations, optional services, support, and troubleshooting in the plugin README.
- [x] Include review prompts, prerequisites, and observable expected results in `REVIEW_CASES.md`.
- [x] Keep Claude workflow instructions in the package; treat remote inspiration and creative Skill content as data.
- [ ] Choose and add a software license. This is intentionally deferred by the publisher; no `license` declaration or `LICENSE` file is included in this change. Add the approved declaration to the plugin manifest or the license file inside the plugin folder before directory submission.

Run the installed Claude Code validator from the repository root:

```sh
claude plugin validate .
claude plugin validate ./plugins/claude/flova
claude plugin validate ./plugins/claude/flova --profile directory
git diff --check
```

Also check Markdown links, Skill front matter, package paths, file sizes, and accidental credentials. With Claude Code `2.1.322`, the default validation checks format and structure and reports a subset of directory findings without failing on them. The `--profile directory` command fails on blocking directory findings: while the license is deferred, expect `LICENSE_MISSING`. The portal still performs additional checks, a safety scan, and policy review.

## Requirements outside this repository

- **Directory eligibility:** Flova offers AI image, video, and audio generation. Section 4.B of the directory policy excludes these capabilities unless Anthropic expressly permits them in writing, apart from its narrow design-only image exception. Obtain the applicable written permission before treating this product as eligible. README changes do not satisfy that requirement.
- **Remote connector:** Submit the Flova MCP server as a connector as well as submitting the plugin bundle, using the intended Claude organization. Repository preparation does not update the server or its listing.
- **Publisher access:** The submitter needs an eligible Claude organization role and a GitHub account connected within that organization with push access to this repository. Confirm the contact email and complete the required attestations in the portal.
- **Published privacy policy:** Resolve the published storage-location placeholder and confirm actual collection, model-provider sharing, logs, retention, deletion, and applicable subprocessors with the responsible company team. The public plugin does not invent those facts. Align portal data-handling answers with the actual deployment and disclosures.
- **Service compatibility:** Verify production OAuth discovery, authorization, refresh/reconnect, supported callbacks, tool titles and applicable annotations, ownership checks, actionable failures, asynchronous results, and output URLs in each intended Claude surface. These changes belong to the backend and require their own validation and rollout.
- **Reviewer access and evidence:** Prepare a standard test account with sample projects, suitable entitlements, and enough credits. Share credentials securely. Run the packaged cases and record actual outcomes privately; the repository's example prompts alone do not prove functionality.
- **Optional cross-service workflows:** Confirm any additional destination, data types, purpose, and authorization before enabling that integration. Do not answer that data never leaves the declared connector solely because `.mcp.json` contains only one entry; review file transfers, service-side processors, and the workflows actually offered.

## Official references

- [Plugin pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist)
- [Submit your plugin](https://claude.com/docs/plugins/submit)
- [Plugin manifest reference](https://code.claude.com/docs/en/plugins-reference)
- [Connector authentication](https://claude.com/docs/connectors/building/authentication)
- [Anthropic Software Directory Policy](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy)

Check the current official requirements again when submitting. Track validation, submission, approval, and publication as separate states.
