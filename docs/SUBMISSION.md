# Claude directory submission

This repository is one plugin at the repository root. It packages the existing Paperpal skills and references the hosted Paperpal MCP server. It does not contain that server's implementation or OAuth credentials.

## Source fields

Open [Claude's developer portal](https://claude.ai/directory/manage), select **Submit new**, then **Plugin bundle**. Use:

| Portal field | Value |
| --- | --- |
| Repository | `https://github.com/cactuscommunications/paperpal-mcp` |
| Plugin path | Leave blank; the plugin is at the repository root. |
| Branch | `main` |
| Plugin name | `paperpal` |
| Display name | Paperpal |
| Initial version | `1.0.0` |

Connect the GitHub account that has push access to the repository in the Claude organization that will own the listing. The repository is public. The portal reads listing fields from `.claude-plugin/plugin.json` and the description from `README.md`.

Run **Validate** on the submitted commit and resolve any blocking findings. A local `claude plugin validate` pass does not establish directory name availability or a passing directory security scan.

## Pair the remote connector

Anthropic asks authors who also operate the remote MCP server to submit it as an **MCP connector** separately. If Paperpal already has a connector submission or listing in the intended organization, use that existing entry. Use the exact same URL in the connector entry and this plugin:

```text
https://edit-connector.paperpal.com/mcp
```

The base URL `https://edit-connector.paperpal.com/` returns service information; the MCP protocol is mounted at `/mcp`. The protected-resource metadata also identifies the `/mcp` URL. Submit both entries from the same organization to pair the listings and avoid duplicate tools.

The connector entry owns OAuth configuration. For the static client ID and secret route, select **Anthropic-held client credentials** and coordinate with `mcp-review@anthropic.com`. Register `https://claude.ai/api/mcp/auth_callback` in the SSO client for hosted Claude apps. Send only the client ID in the initial email; Anthropic supplies a secure method for transferring the secret. Keep both client credentials and reviewer account credentials out of this repository. Claude Code uses a separate OAuth flow; test it independently if it is part of the intended distribution.

Four tools are exposed to the model: `show_document_upload`, `check_attached_document`, `get_check_run_status`, and `get_check_run_findings`. The widget calls four app-only tools: `upload_document`, `start_document_checks`, `record_product_event`, and `record_check_feedback`. A portal display of four model tools is consistent with that visibility contract.

## Package verification

With a current Claude Code installation, run from this repository's root:

```sh
claude plugin validate . --strict
```

Check that the package contains:

```text
.claude-plugin/plugin.json
.mcp.json
README.md
LICENSE
assets/paperpal.png
skills/check-manuscript/SKILL.md
skills/review-results/SKILL.md
skills/manuscript-readiness/SKILL.md
skills/paperpal-product-knowledge/SKILL.md
```

All paths must stay inside the plugin. Commit regular files, not symlinks, submodules, or Git LFS pointer files. Publish readable source with no credentials, personal documents, or environment files.

### Preparation verification — 2026-10-13

| Check | Observed result |
| --- | --- |
| Claude Code `2.1.290`, `plugin validate --strict` | Passed. |
| Copied skills compared with the existing Paperpal plugin | All four files identical. |
| Package credentials and stale development endpoint scan | No matches. |
| Production `/health` | Returned `ok: true`. |
| Production `/ready` | Returned `ok: true`, `database: ready`. |
| Unauthenticated production `/mcp` | Returned `401` with the protected-resource metadata pointer. |
| Protected-resource metadata | Identified `https://edit-connector.paperpal.com/mcp` and issuer `https://sso.editage.com`. |
| Portal Validate, authenticated Claude tool/UI scenarios, and directory review | Not run as part of repository preparation; complete in the intended organization. |

## Host verification before submitting

Use a dedicated reviewer/test account and controlled sample manuscripts. Run these scenarios in Claude with the plugin enabled, recording the actual result. They are verification scenarios, not claims that execution has already passed.

| Scenario | Prompt or action | Expected behavior |
| --- | --- | --- |
| Upload and checks | “Check my manuscript for plagiarism and reference issues.” | Show the upload card, let the user select the file/checks and confirm, then show the returned run. |
| Chat attachment | Attach a controlled DOCX and ask “Check this manuscript.” | Use a real host-provided downloadable attachment when available; otherwise show the upload card. Do not fabricate a URL or use a host-private path as a remote URL. |
| Refresh | After starting a real run, ask “Refresh the Paperpal check status.” | Call `get_check_run_status` with the established run ID; do not start a second check. |
| Findings | After completed results, ask “What should I fix first?” | Retrieve findings for the existing run, distinguish incomplete checks, and summarize only returned evidence. |
| Grammar | Upload a controlled DOCX and select Grammar. | Return the Paperpal editor hand-off. Do not describe Grammar as a health-check report. |
| Unsupported format/check combination | Upload a PDF and select Grammar if the host allows selection. | Explain that Grammar is unavailable for PDFs; do not invent Grammar results. |
| Unsupported MCP capability | “Find recent papers about this topic with Paperpal.” | Explain the connector's scope and direct the user to the relevant Paperpal web feature. |
| Results without a run | In a fresh conversation, ask “Show my manuscript's previous results.” | Request an upload/attachment workflow; do not invent a run ID or claim to search a document library. |

Confirm the connector server advertises accurate titles and read/write/destructive annotations. Fix a missing tool annotation on the server rather than in this plugin's skill files.

For the separate MCP App connector listing, prepare the carousel screenshots requested by Anthropic: 3–5 PNGs, at least 1000 pixels wide, cropped to the app response, with each prompt supplied separately. Reviewer sign-in/access instructions belong in the portal's secure **Test & launch** fields. The login URL is `https://edit.paperpal.com`; use the designated reviewer account and enter its credentials only in those secure fields.

## Submission and updates

The authorized publisher must answer the portal's data-handling questions from the service's actual practices and complete its legal/policy acknowledgments. This repository links the existing Paperpal service policies; it does not create new retention or deletion promises.

Submit for review only after the portal validation and host checks are complete. A passing package is separate from a passing security scan, review approval, and publication. For later releases, update `version` and `CHANGELOG.md`, commit to the tracked branch, and follow the portal's version and publishing controls.

## Official references

- [Plugin structure and testing](https://claude.com/docs/plugins/build)
- [Plugin manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference)
- [Plugin pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist)
- [Submit a plugin](https://claude.com/docs/plugins/submit)
- [Submit a connector](https://claude.com/docs/connectors/building/submission)
- [Connector authentication](https://claude.com/docs/connectors/building/authentication)
