# Installing Countersign (remote MCP server)

Countersign checks an agent's "done" against the real outcome (a live URL, a PR's state, a DNS record) with deterministic public checks and keeps a hash-chained receipt anyone can open. It is a hosted remote MCP server: there is nothing to clone, build or run, and no API key is needed to connect.

## 1. Add the server

Add this to opencode.json (project root) or ~/.config/opencode/opencode.json:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "countersign": {
      "type": "remote",
      "url": "https://countersign.signalry.ai/mcp?ref=opencode",
      "enabled": true
    }
  }
}
```

## 2. Check the connection

The server lists eight tools: start, try_check, request_key, submit_claim, verify_claim, check_reply_gate, get_track_record and find_tools (which names the rest). If they show up, the install worked. try_check {"subject": "<a hostname>"} is a read-only check that writes nothing.

## 3. First claim (optional, one call): MCP or plain HTTP, same result

Only if the user wants Countersign set up. Over MCP:

    start {"subject": "<a hostname you operate>"}

Or over plain HTTP (no MCP client needed, works mid-session):

    curl -sS -X POST "https://countersign.signalry.ai/v1/start?ref=opencode" -H "content-type: application/json" -d '{"subject":"<a hostname you operate>","public":true}'

Either returns a csk_ API key (shown once; keep it private) and files a first claim that https://<subject>/ answers HTTP 200 with a valid TLS certificate, checked now and unlisted. The response leads with one line: COUNTS or DOESN'T COUNT, the reason, and the fix. first_claim.receipt_url is the receipt.

What makes a claim count: the checks pass AND the statement names what they check (the URL or host). "https://docs.example.org/pricing answers 200 and contains 'Pro plan'" counts; "pricing page is live" does not, even when the check passes.

Later claims: submit_claim {subject, statement, checks} with the key either as an Authorization: Bearer header in the config above (add "headers": {"Authorization": "Bearer csk_..."}) or as "api_key" in the submit_claim arguments. No restart is needed for the api_key route.

## Privacy

Nothing is sent unless the agent calls a tool. A call carries only what the agent puts in it: a subject (hostname, URL or repo and PR number), a one-line statement and the checks to run. Countersign fetches the public target itself; it never receives prompts, code or files. Claims are public by default; pass visibility "unlisted" to keep one out of listings. Details: https://countersign.signalry.ai/privacy

Docs: https://countersign.signalry.ai/start?ref=opencode · Health: https://countersign.signalry.ai/v1/health
