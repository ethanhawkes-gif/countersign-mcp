# Countersign by Signalry: verify AI agent claims (remote MCP server)

**Server URL: `https://countersign.signalry.ai/mcp`** (Signalry, countersign.signalry.ai; not affiliated with other services named Countersign).

![Countersign](https://countersign.signalry.ai/icon-400.png)

Verify an AI agent's "done" against the real outcome. Countersign runs deterministic public checks (HTTP status and text, DNS records, TLS certificates, GitHub PR state, public git refs) and records the result as a hash-chained receipt anyone can open. No LLM judges anything.

- Remote MCP endpoint (Streamable HTTP): `https://countersign.signalry.ai/mcp`
- No install, no account, no API key to connect
- First claim in one call, MCP or HTTP: `start {"subject": "<a hostname you operate>"}`, or `curl -sS -X POST https://countersign.signalry.ai/v1/start -H "content-type: application/json" -d '{"subject":"<a hostname you operate>","public":true}'`
- Server card: https://countersign.signalry.ai/.well-known/mcp/server-card.json · OpenAPI: https://countersign.signalry.ai/openapi.json · Status: https://countersign.signalry.ai/v1/health

Why: coding agents' own work reports are often wrong. In 435 real work reports that Yang et al. sampled and annotated, 162 (37.24%) contained at least one unfaithful claim ([arXiv 2611.25760](https://arxiv.org/abs/2611.25760)). How Countersign handles each of the paper's four failure types: https://countersign.signalry.ai/why#how-often-agent-reports-are-wrong-uf-1

## Install

| Client | Config |
|---|---|
| Cline | [llms-install.md](llms-install.md) |
| Kilo Code | [llms-install.kilocode.md](llms-install.kilocode.md) · marketplace entry [kilocode/MCP.yaml](kilocode/MCP.yaml) |
| opencode | [llms-install.opencode.md](llms-install.opencode.md) |
| OpenHands | [llms-install.openhands.md](llms-install.openhands.md) · catalog entry [openhands/countersign.json](openhands/countersign.json) |
| Anything else | point a Streamable HTTP MCP client at `https://countersign.signalry.ai/mcp` |

## Tools

| Tool | What it does | Writes? |
|---|---|---|
| start | key plus a first claim checked live on a hostname you operate, one call | yes (one unlisted claim) |
| try_check | live https and TLS check of a hostname | no |
| request_key | a csk_ key now (`{"public": true}`) | key only |
| submit_claim | record a claim and verify it now | yes |
| verify_claim | re-run your own claim's checks | yes (receipt) |
| check_reply_gate | before saying "done": cited claims must be verified and fresh | no |
| get_track_record | an agent's public track record | no |

A claim counts as verified (`counts_as_verified: true`) only when the service itself observed the checks passing. Anonymous claims are labelled self_declared and never count.

## Privacy and data

Opt-in by design: nothing leaves the agent unless it calls a tool, and a call carries only the subject, a one-line statement and the checks. Countersign reads the public target itself and never receives prompts, transcripts, code or workspace files. No IP addresses are stored. https://countersign.signalry.ai/privacy

## About this repository

This repository (https://github.com/ethanhawkes-gif/countersign-mcp) holds only the listing files: this README, the install docs, the logo, the Kilo marketplace entry, the OpenHands catalog entry and the MCP Registry server.json. The service itself is hosted at https://countersign.signalry.ai; there is no code here to build or run. The same files are served live at https://countersign.signalry.ai/start/listings.

## Operator

Countersign by Signalry (https://signalry.ai). Security contact: https://countersign.signalry.ai/.well-known/security.txt
