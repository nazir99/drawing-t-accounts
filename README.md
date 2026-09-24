# drawing-t-accounts

Instructions that teach an AI model to draw general ledger activity as textbook T accounts, the way an accountant or auditor expects to read them. It works in any medium: an HTML page, a NetSuite Suitelet, Word, Excel, PDF or markdown.

`SKILL.md` is plain markdown with no code and no dependencies, so it works with any LLM. It uses the Agent Skills format (a short name and description header, then the instructions), which some tools load automatically, but any model can follow it if you give it the file.

It was built for NetSuite cost accounting (work orders, WIP, inventory, absorption), but the rules apply to any ledger.

## Why

Asked to "show this as T accounts", an AI model usually gets the accounting right and the picture wrong. The usual problems:

- Debits and credits are stacked as two separate lists, so a 5 Sep credit sits next to a 3 Sep debit and the timeline is lost.
- Balances show up as signed numbers or as "Ending balance (Cr)" instead of sitting on the side they fall.
- Descriptions are copied from system memos ("WO500 asm build qty 100") instead of plain words.
- A filtered T (one work order's slice of Raw Materials) gets called "abnormal" because it shows a credit.

This skill fixes those problems and gives the model a normal-balance reference for every NetSuite account type.

## What the skill covers

- **Normal balances by NetSuite account type**, with SuiteQL `accttype` codes: whether a debit or a credit increases each type (assets, liabilities, equity, revenue, COGS, expense). It also lists the contra accounts that run opposite to their type, like accumulated depreciation, owner draws, sales returns and labor or overhead absorption.
- **Abnormal balances**: how to spot a balance on the wrong side and say so in words.
- **Layout rules**:
  - A title above each T.
  - Shaded Debit and Credit headers.
  - Only three rules: under the headers, down the middle, above the foot.
  - One entry per row, sorted by date across both sides.
  - The foot row on the side the balance falls.
  - `Balance, nothing` at zero.
- **Filtered Ts**: a T limited to one order or document shows movement, not the account balance, and is labeled `Net for <scope>`.
- **Telling a cost-flow story**:
  1. The answer first.
  2. The account in question.
  3. The key event as one journal entry.
  4. Where the other side went.
- **A copy-ready HTML and CSS example**, plus how to draw the same thing in Word or Excel.
- **Common mistakes** and their fixes.

## Install

**Any LLM (ChatGPT, Gemini, Claude.ai, a local model, your own app):** paste the contents of `SKILL.md` into the system prompt, custom instructions or project instructions, or attach it as a file and say "follow this when drawing T accounts".

**Tools that load skills from a folder:** clone the repo into the tool's skills directory. The folder name must match the skill name.

| Tool | Command |
|---|---|
| Claude Code | `git clone https://github.com/nazir99/drawing-t-accounts.git ~/.claude/skills/drawing-t-accounts` |
| OpenAI Codex CLI | `git clone https://github.com/nazir99/drawing-t-accounts.git ~/.codex/skills/drawing-t-accounts` |

To update a clone later, run `git pull` inside the folder.

**Coding assistants with rules files (Cursor, Copilot, Windsurf and similar):** copy `SKILL.md` into that tool's project rules or instructions file.

## Use

In tools that load skills, you don't need to name it: the model picks it up when a request matches. Otherwise, mention it once. Example requests:

- "Show me how cost flowed through the GL for WO500 as T accounts."
- "Explain why this WIP balance isn't zero."
- "Add a GL impact tab to this Suitelet, drawn as T accounts."
- "Build a Word exhibit for the auditors showing the close entry."



## Example

`examples/` holds a small test case: eight GL lines for a work order, `wo500-gl.csv`. The rows are out of date order, the memos are in system shorthand, the WIP ends 50.00 below zero, and a labor absorption account (COGS type) carries a credit balance.

`examples/wo500-t-accounts.html` is what a model produced from that file with the skill loaded. Open it in a browser. It shows:

- The finding first: WIP is 50.00 below zero because the completion took out more than went in.
- The WIP T, with "Left in WIP 50.00" on the Credit side.
- The completion as one journal entry.
- The other-side Ts, each labeled "Net for WO500", with none of them falsely flagged.

## How it was tested

The same request and data were given to a fresh model twice: once without the skill (baseline) and once with it. Without the skill, the model drew debits and credits as separate stacked columns and showed a signed ending balance. With the skill, it followed the layout. The gaps that run surfaced were fixed, such as filtered Ts and absorption accounts, and a second run confirmed the fixes.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The skill. This is the only file a model needs. |
| `examples/wo500-gl.csv` | Sample GL lines used for testing |
| `examples/wo500-t-accounts.html` | Output produced with the skill |
| `LICENSE` | MIT |

## Contributing

Issues and pull requests are welcome, especially for other ERPs' account types and other output media. If you change a rule, rerun the example with and without the skill and describe the difference in the pull request.

## License

MIT. See `LICENSE`.
