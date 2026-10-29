# Oplane for Claude

AI-powered security analysis for your codebase: threat modeling, implementation assessment, and PR security review, in Claude Code, Cowork and claude.ai.

## What it does

This plugin connects Claude to [Oplane](https://www.oplane.com), giving you:

- **Codebase analysis**: identify security threats across your entire project
- **PR review**: analyze pull requests for security implications
- **Implementation assessment**: check if security requirements are properly implemented
- **Severity management**: adjust requirement severity based on your risk context

Results are saved to Oplane and visible in the [Gravity web interface](https://gravity.oplane.io).

## Prerequisites

- Claude Code, Cowork, or claude.ai
- An [Oplane account](https://gravity.oplane.io)

## Installation

In claude.ai or the Claude desktop app, open **Customize > Plugins**, find **Oplane** under **Discover**, and select **Add**. The plugin is saved to your account, so it is also available in Cowork, and Claude Code downloads it the next time you start a session signed in to the same account (run `/reload-plugins` to load it in a running session).

## Authentication

The Oplane MCP server signs you in with OAuth, so there are no tokens to copy.

- **Claude Code:** run `/mcp`, select the Oplane server, choose **Authenticate**, and log in in your browser.
- **claude.ai and Cowork:** open the Oplane plugin under **Customize > Plugins** and connect it on its **Connectors** tab.

Tokens are issued and refreshed automatically. For CI pipelines and other automation where nobody signs in, use an Oplane application key instead, see [Applications](https://www.oplane.com/docs/administration/applications).

## Usage

### Analyze a codebase

```
/oplane:analyze
```

Performs a full security threat model analysis. Optionally focus on a specific area:

```
/oplane:analyze authentication and session management
```

### Analyze a pull request

```
/oplane:analyze-pr
```

Analyzes the current PR changes for security implications. Provide context:

```
/oplane:analyze-pr PR #123 adds OAuth login flow
```

### Security agent

The plugin also provides a `security-analyzer` subagent that Claude Code and Cowork can invoke automatically when security analysis is needed.

## Recommended setup: CLAUDE.md / AGENTS.md

The single highest-impact step is making threat modeling a standing instruction your agent
reads every session, so security-relevant changes are modeled before each commit or PR,
not just reviewed after a PR opens. Add this to your project's `CLAUDE.md` (and/or
`AGENTS.md`):

```
For changes that could affect security, you MUST threat-model the change using Oplane MCP
before committing. Threat-model the actual diff (e.g. the PR threat model), not a written
summary of it - a model built from your own description only re-tests risks you already
considered. Explicitly consider untrusted-input-inbound (log/audit/template/SQL injection
from external data), not only outward data leakage.
```

## Available tools

The plugin connects your agent to the Oplane MCP server, which provides the tools the skills
use (create a threat model, get implementation advice, record assessments, and more). For the
full list, see the [MCP tool reference](https://www.oplane.com/docs/reference/mcp-tools).

## What the plugin sends

The plugin connects your agent to Oplane's MCP server at `https://gravity.oplane.io/mcp/`.
When you run an analysis, your agent sends Oplane:

- titles and descriptions of the code or change it is threat modeling, which can include
  short code excerpts
- git details such as the repository URL, branch, commit SHA and pull request URL
- the assessment it records for each security requirement, with file and line references

The plugin sends nothing to any other service. It has no hooks or scripts that run on your
machine: the skills and the agent are instructions your agent follows. Oplane stores the
results in your account, as described in the
[privacy policy](https://www.oplane.com/legal/privacy-policy).

## Support

See the [Oplane agent plugins documentation](https://www.oplane.com/docs/reference/agent-plugins) or email [support@oplane.io](mailto:support@oplane.io).

## License

Apache License 2.0. See [LICENSE](LICENSE).
