# Paperpal for Claude

![Paperpal](assets/paperpal.png)

Check an academic manuscript with Paperpal from Claude, review the findings, and decide what needs attention before submission. This plugin combines four Paperpal skills with a connection to Paperpal's hosted MCP service. Each person signs in with their own Paperpal account.

## What you can do

| Check | What the workflow provides |
| --- | --- |
| Grammar | Open a DOCX manuscript in the Paperpal editor for language editing. Grammar is unavailable for PDFs. |
| Plagiarism | Run a similarity check and open the returned report. Similarity is a signal for review, not proof of misconduct. |
| Reference Checks | Review the reference findings returned by Paperpal. |
| Journal Fit | Review Paperpal's submission-readiness findings. These are not an acceptance prediction. |

Upload a DOCX or PDF of up to 25 MB through the Paperpal card, choose the checks, and confirm before they start. Checks can use your Paperpal plan's balance or entitlements. The card shows progress and results; follow-up questions use the same check run.

This connector supports these four checks. AI Detection, AI Review, literature search, rewriting, and other Paperpal web features are outside its current MCP scope.

## Get started

1. Add the Paperpal plugin in Claude and connect its Paperpal connector. For testing before the directory listing is published, download this repository as a ZIP and use **Customize → Plugins → Add → Upload plugin**.
2. Complete the Paperpal sign-in and consent flow when prompted. A Paperpal account is required; available checks depend on its plan and balance.
3. Ask, for example, **"Check my manuscript for plagiarism and reference issues."**
4. Select the manuscript in the Paperpal upload card, choose the checks, and confirm.
5. Once results are available, ask **"What should I fix first?"** or **"Refresh the Paperpal check status."** in the same conversation.

When Claude cannot supply a downloadable chat attachment, Paperpal shows its upload card so you can select the file there. Do not provide a local file path, base64 content, or an invented download URL.

## Included skills

| Skill | Purpose |
| --- | --- |
| `check-manuscript` | Select or upload a manuscript and start the requested checks. |
| `review-results` | Explain findings or refresh a check run already established in the conversation. |
| `manuscript-readiness` | Assess completed findings and prioritize issues without treating incomplete checks as approval. |
| `paperpal-product-knowledge` | Explain Paperpal's MCP scope and route product questions to official sources. |

The skills can be selected by Claude when relevant. In Claude Code, they also have namespaced commands such as `/paperpal:check-manuscript`.

## Connection and data

The plugin uses this remote Streamable HTTP endpoint:

```text
https://edit-connector.paperpal.com/mcp
```

The host discovers Paperpal's OAuth authorization service and handles sign-in. This repository contains no credentials. Do not put an OAuth client secret, access token, password, or manuscript into the public plugin files or a GitHub issue.

Files selected for a check are uploaded to Paperpal. The hosted service processes the requested checks, stores document/check-run records, and returns status and findings. Interactions such as opening reports and submitting feedback can create product-event or feedback records. The plugin itself consists of instructions and connection metadata; it does not run a local server, install dependencies, or execute hooks. See [Paperpal Data Security](https://paperpal.com/data-security) for its data-handling information and linked Privacy Policy, and the [Terms of Use](https://cactusglobal.com/terms-of-use/) for the service's terms.

## Validate or test from source

With a current Claude Code installation:

```sh
git clone https://github.com/cactuscommunications/paperpal-mcp.git
claude plugin validate ./paperpal-mcp --strict
claude --plugin-dir ./paperpal-mcp
```

Local validation checks package structure. Authenticated tool calls and the interactive upload/results experience still need testing in the intended Claude app. Directory submission and release instructions are in [docs/SUBMISSION.md](docs/SUBMISSION.md).

## Help and license

- [Paperpal MCP documentation](https://enormous-feeling-067.notion.site/Paperpal-MCP-3ec00a63965c8048a76be191ac93a7ca)
- [Paperpal Help Center](https://support.paperpal.com/support/solutions)
- [Contact Paperpal](mailto:hello@paperpal.com)
- [Paperpal website](https://paperpal.com)
- [Report a plugin packaging issue](https://github.com/cactuscommunications/paperpal-mcp/issues)

The plugin files are available under the [MIT License](LICENSE). Use of the hosted Paperpal service is governed by its own terms. Paperpal and Cactus Communications names and logos remain their respective trademarks; the software license does not grant trademark rights.
