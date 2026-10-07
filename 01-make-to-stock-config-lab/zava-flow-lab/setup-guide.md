# Setup guide — Zava DIY make-to-stock configuration lab

Allow 20 minutes on your own machine before the session. If all checks
pass, you walk in and start working.

## What you need

- **VS Code** with the GitHub Copilot Chat extension, **agent mode**
  available (the model picker dropdown in the Copilot Chat sidebar).
- **A GitHub Copilot seat with agent mode** — Pro, Business, or Enterprise.
- **The D365 ERP MCP server** configured in VS Code — this is what lets
  Copilot read and write configuration in your D365 environment. Setup:
  [Connect to the Dynamics 365 ERP MCP server by using Visual Studio
  Code](https://learn.microsoft.com/en-us/dynamics365/fin-ops-core/dev-itpro/copilot/mcp-server-vscode).
- **The MS Learn MCP server** configured in VS Code — this is what lets
  Copilot look up the authoritative D365 configuration steps instead of
  relying on possibly-stale training data. Setup: [Microsoft Learn MCP
  Server overview](https://learn.microsoft.com/en-us/microsoftlearn-mcp/overview).
- **A Dynamics 365 Tier 2 (Standard Acceptance Testing) or UDE (Unified
  Development Environment)** connected to Dataverse, with a legal entity
  you can configure freely. A Tier 1 (cloud-hosted or developer VM) or CHE
  environment is **not sufficient** — the ERP MCP needs the Dataverse
  connection. If you don't have one, ask your facilitator.
- **Python 3**, for reading the two reference Excel workbooks
  (`reference/*.xlsx`) — Copilot will use it to pull exact figures out of
  them rather than approximating from the decision log's prose. Verify
  with `python --version`. If it's missing, download it from
  [python.org](https://www.python.org/downloads/) or ask Copilot to set it
  up for you.
- This lab folder, unzipped, opened as your VS Code workspace root (**File
  → Open Folder…**, select the folder itself — not a parent or child of it).

That's it — everything else the lab needs (the decision log, the workshop
transcripts, the reference workbooks, the skill file) ships inside the
folder.

> **Model selection:** if you have a choice in the model picker, use an
> Anthropic model (Sonnet or Opus) — these currently have the strongest
> multi-tool MCP calling for this kind of task.

## Opening the workspace

1. Unzip the lab folder.
2. In VS Code: **File → Open Folder…** → select the unzipped folder.
3. Open the Copilot Chat sidebar and switch it to **agent mode**.
4. In the tool picker, confirm both **D365 ERP MCP** and **MS Learn MCP**
   are listed and enabled.

## Verification (single step)

In Copilot Chat, agent mode, paste:

> "Read .github/skills/make-to-stock-configuration/skill.md and summarize
> what it's for in one sentence. Then open reference/zava_item_bom_master.xlsx
> and tell me the finished good's item ID and its StandardCost. Then confirm
> you can see tools from both the D365 ERP MCP server and the MS Learn MCP
> server."

**You're ready if** Copilot correctly describes the skill (configuring a
make-to-stock item — released products, BOM, route, production order — in
D365, via both MCP servers), **reports `FG-CABINET-100` at a standard cost
of $83.90** (proving it can actually read the workbook, not just see the
filename), **and confirms it can see both servers' tools.**

If the skill can't be found, re-check that you opened the lab folder itself
as the workspace root, not a folder above or below it. If either MCP server
isn't visible, it isn't configured correctly yet — follow that server's
setup guide above or ask your facilitator; nothing else in the lab will
work without both.

If the response works, you've also just completed Phase 0 of the exercise
— keep it, it's your starting point.

## If something goes wrong

1. **Ask Copilot itself.** Paste the error and ask "what does this mean,
   and how do I fix it?" — that's the core skill this lab is building.
2. **Apply your own D365 knowledge.** If a configuration step fails, read
   what the ERP MCP actually returned, identify what's wrong functionally
   (wrong sequence, missing prerequisite, wrong entity), and instruct
   Copilot explicitly on how to work around it.
3. **Ask a neighbour or the facilitator.**

See you in the room.
