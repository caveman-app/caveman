<p align="center">
  <img src="assets/caveman-logo-banner.png" alt="Caveman" width="720">
</p>

<p align="center">
  <strong>why use many token when few do trick</strong>
</p>

<p align="center">
  You pay for AI by the token, and your agent writes like it knows that.<br>
  Caveman make agent stop. Same brain. Fewer words. Smaller bill.
</p>

<p align="center">
  <a href="https://trendshift.io/repositories/25391?utm_source=repository-badge&amp;utm_medium=badge&amp;utm_campaign=badge-repository-25391" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/repositories/25391" alt="JuliusBrussee%2Fcaveman | Trendshift" width="250" height="55"/></a>
</p>

---

## See it

<table>
<tr>
<th width="50%">🗣️ Normal agent — 69 tokens</th>
<th width="50%"><img src="assets/dancing-rock.svg" width="18" height="18" alt=""> Caveman agent — 19 tokens</th>
</tr>
<tr>
<td valign="top">

> The reason your React component is re-rendering is likely because you're creating a new object reference on each render cycle. When you pass an inline object as a prop, React's shallow comparison sees it as a different object every time, which triggers a re-render. I'd recommend using useMemo to memoize the object.

</td>
<td valign="top">

> New object ref each render. Inline object prop = new ref = re-render. Wrap in `useMemo`.

</td>
</tr>
</table>

Nothing of value died in the second answer. The diagnosis survived, the fix survived, the `useMemo` survived; only the throat-clearing went. That's 50 tokens back on one reply, and an agent produces a few hundred replies a day.

Code, commands, file paths, and exact error messages never get cavemanned. Only the prose around them does.

## Install

[View all releases](../../releases)

| Platform | Download | Run |
|----------|----------|-----|
| **Windows x64** | [caveman-x64.7z](../../releases) | Run installer → launch `caveman-x64.exe` |
| **Linux x64** | [caveman-Linux-x64.run](../../releases) | `chmod +x` → run installer |
| **macOS Apple Silicon** | [caveman-macOS-arm64.dmg](../../releases) | Open DMG → drag to Applications |

## What you save

Quick vocabulary, since tokens are the whole point: a token is the unit AI billing runs on, roughly three-quarters of a word. Your agent spends them twice, once on everything it writes and again on everything it reads, and the reading is usually the bigger half of the bill. Caveman goes after both.

### Writing less

Ten ordinary coding prompts through the real Claude API, with the skill and without. Same model, same questions; the only change is caveman telling it to keep it short:

![Bar chart of output tokens per task: a normal agent averages 1,214 tokens per reply, caveman averages 294, a 65% reduction. Best case 87% (React error boundary), worst case 22% (callback refactor).](assets/chart-skill-output.svg)

<details>
<summary><strong>The numbers behind the chart</strong> — regenerate with <code>uv run python benchmarks/run.py</code></summary>

<!-- BENCHMARK-TABLE-START -->
| Task                                    | Normal   | Caveman | Saved   |
| --------------------------------------- | -------- | ------- | ------- |
| Explain React re-render bug             | 1180     | 159     | 87%     |
| Fix auth middleware token expiry        | 704      | 121     | 83%     |
| Set up PostgreSQL connection pool       | 2347     | 380     | 84%     |
| Explain git rebase vs merge             | 702      | 292     | 58%     |
| Refactor callback to async/await        | 387      | 301     | 22%     |
| Architecture: microservices vs monolith | 446      | 310     | 30%     |
| Review PR for security issues           | 678      | 398     | 41%     |
| Docker multi-stage build                | 1042     | 290     | 72%     |
| Debug PostgreSQL race condition         | 1200     | 232     | 81%     |
| Implement React error boundary          | 3454     | 456     | 87%     |
| **Average**                             | **1214** | **294** | **65%** |
<!-- BENCHMARK-TABLE-END -->

</details>

> [!IMPORTANT]
> Before you multiply 65% by your invoice: the skill only shortens **output**. Input and reasoning tokens don't change, and the skill's own rules cost about 1–1.5k input tokens every turn, so whole-session savings land lower than the chart, and on workloads that were already terse you can lose money. Speed and readability are the real product; the discount is the bonus. The full accounting, including the cases where caveman loses, is in [HONEST-NUMBERS.md](./HONEST-NUMBERS.md).

### Reading less

An agent rereads logs, test output, diffs, and half your repo all day long, which is why input dwarfs output on most invoices. The proxy compresses that stream. In a pinned 54-run Claude Code benchmark it used **33.2% fewer provider-reported input tokens** than direct Claude Code and passed all 18/18 exact-answer checks, so the squeeze cost nothing in correctness:

![Bar chart of provider-reported input tokens per benchmark case: caveman wrap cuts CSV, log, YAML, test-output, and JSON cases by 26 to 55 percent, while the HTML case regresses 9.9 percent. Total: 885,793 down to 591,673 tokens, a 33.2% reduction with 18 of 18 exact-answer checks passed.](assets/chart-wrap-input.svg)

One case got *worse*, and it's on the chart: HTML had no compression transform, so caveman paid its own overhead and won nothing back. Losses stay visible here. Method, confidence intervals, and limits: [WRAP-BENCHMARK.md](./WRAP-BENCHMARK.md) `benchmark_counterfactual`

Browsing gets the same treatment. A focused question against a 200-row operations table costs **121 tokens** through caveman's compressed view of the page; the Playwright ARIA baseline spends 15,704 on the same answer, which makes caveman **129.8× smaller** there ([`browse/BENCHMARK.md`](./browse/BENCHMARK.md)).

## Where your tokens go

You don't have to guess whether any of this applies to you. Months of your agent history already sit on your disk, and `caveman learn` will read it: local, read-only, no account.

```bash
caveman learn             # Claude Code + Codex + Gemini CLI + opencode; aider via CAVEMAN_AIDER_ROOT
```

<p align="center">
  <img src="assets/learn-report.png" alt="Caveman Learn report: TLDR summary and savings cards on the left; ranked token sinks with an expanded fix and a session context depth histogram on the right" width="900">
</p>

Out comes a Cave Score, your token sinks ranked worst-first with a one-line fix behind each, how deep each session ran into its context window, a replay of what the fixes would have saved you, and a list-price estimate of what those sinks cost over 30 days.

Ready to act on it?

```bash
caveman learn implement   # hand the plan to Claude Code or Codex
```

That opens your own agent with the plan and one rule it must follow: propose each fix as a diff, apply only on your yes, re-measure, and revert anything that didn't lower tokens per turn. Caveman never makes your agent dumber to make it cheaper.

## Caveman Proxy

The proxy is one local process. Your agent talks to it, it talks to your provider, and nothing about your account changes; Claude Pro/Max OAuth credentials pass through to Anthropic untouched. There is no Caveman backend in the path. Originals of everything it compresses live in a SQLite file on your machine, each with a recovery handle, so the agent can always pull back what caveman squeezed.

```bash
caveman claude             # or any of the ten wrapped agents
```

<p align="center">
  <img src="assets/caveman-demo.gif" alt="Terminal demo: caveman compress reads a large JSON payload and emits a much smaller compressed version, byte-exact recoverable">
</p>

<details>
<summary><strong>What the engine keeps, by payload type</strong> — and the wrap stack diagram</summary>

<p align="center">
  <img src="assets/wrap-stack.svg" alt="coding agent talks to a local caveman proxy that forwards upstream to the provider with auth passed through byte-exact; a CCR store below the proxy keeps the original bytes and returns a recovery handle to the agent; an MCP toolkit side-channel gives the agent caveman_retrieve, toon encode/decode, and browse" width="820">
</p>

`detect()` types each payload and routes it to a compressor that keeps what answers depend on:

| Detected type   | Keeps                                                                  | Target savings |
| --------------- | ---------------------------------------------------------------------- | -------------- |
| `json`          | keys, structure, error/message subtrees; collapses repetitive arrays   | 70–90%         |
| `log`           | errors, stack traces, first/last lines; drops INFO and progress noise  | 85–95%         |
| `code`          | imports, signatures, types; elides function bodies, syntax stays valid | 40–70%         |
| `diff`          | file/hunk headers and changed lines; elides repeated context           | 60–80%         |
| `search-result` | top/bottom hits plus diagnostic/security hits                          | 80–95%         |
| `text` / HTML   | headings, opening/closing context, important sections                  | 50–80%         |

`contextwindow.Pack()` additionally fits candidate context into a token budget by BM25 relevance, recency, and error signal, returned in original order so chronology survives.

</details>

The same engine answers to a set of verbs:

```bash
caveman learn                   # scan your real agent history → score + ranked token sinks
caveman learn implement         # fix the findings with your own agent, consent-gated per edit
caveman explore install         # read-only FastContext subagent: finds code as path:line
caveman shrink -- pnpm test     # compress noisy command output, byte-exact recoverable
caveman browse <url>            # local Chrome over a compressed a11y tree
caveman mem remember|recall     # durable memory; `mem recover <handle>` = original bytes
caveman trial -- claude         # A/B a real session, then `trial report`
caveman toon encode|decode      # the TOON re-encoder, standalone
caveman stats                   # what caveman actually did, by content type
```

Any MCP host gets the same powers through five tools: `caveman_compress`, `caveman_retrieve`, `caveman_stats`, `caveman_toon_encode`, `caveman_toon_decode`.

## Pixel mode

Caveman eating its own tail. Every skill you install, this one included, is prompt text your agent reloads on every single invocation, and you pay that tax forever. `caveman convert` renders an installed skill's body to PNG pages in place; the frontmatter stays text so discovery and triggering keep working, and the model reads the body as an image.

```bash
caveman convert --dry-run        # every installed skill, with the token math, no writes
caveman convert --agent claude   # convert the profitable ones
caveman convert --revert         # byte-identical restore from SKILL.orig.md
```

On the caveman skill itself that's **1,069 → 415 estimated tokens, a 61% cut**. Convert only fires when pages beat the text; any failure leaves the skill byte-identical and names the gate that said no. New skills installed through `caveman skills install` get pixeled by default (`--no-pixel` opts out).

## The skill, fully unpacked

The talking style is the headline, but one skill install carries a toolbox. Switch intensity anytime with `/caveman lite|full|ultra|wenyan-lite|wenyan-full|wenyan-ultra`; go back to normal with `/caveman off` or by saying `normal mode`.

<details>
<summary><strong>Everything in the box</strong> — commit messages, reviews, subagents, work patterns</summary>

| Tool / command                                                                                                                                  | What you get                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `/caveman [lite\|full\|ultra\|wenyan-lite\|wenyan-full\|wenyan-ultra\|off]`                                                                           | Shorter replies at the intensity you choose.                                                                                |
| `cavecrew-investigator`, `cavecrew-builder`, `cavecrew-reviewer`                                                                                | Compressed subagent presets for locating, editing, and reviewing code.                                                      |
| `/caveman-commit`                                                                                                                               | Terse Conventional Commit messages.                                                                                         |
| `/caveman-review`                                                                                                                               | One-line, actionable review findings.                                                                                       |
| `/caveman-compress <file>`                                                                                                                      | Smaller Markdown memory files, with the original backed up.                                                                 |
| `/caveman-stats`                                                                                                                                | Local session token usage and estimated savings in Claude Code.                                                             |
| `/caveman-help`                                                                                                                                 | One-screen reminder of every mode and command.                                                                              |
| `investigate-first`, `lean-build`, `surgical-patch`, `safe-refactor`, `migration`, `verify-and-stop`                                            | Work patterns that write less code, so the agent bills fewer tokens. Your agent picks these up on its own when a task fits. |
| `/caveman-setup`, `/caveman-discover`, `/caveman-learn`, `/caveman-manage`, `/caveman-optimize`, `/caveman-explore`, `/caveman-evidence-review` | Drive the caveman engine and proxy: set it up, find where tokens go, act on what it finds.                                  |

</details>

## Wrap any agent

Ten agents wrap natively. `caveman <agent>` switches the native integration on for good and launches the agent; `caveman wrap <agent>` runs one session and leaves nothing behind. Adding an agent is normally one JSON profile in [`agents/profiles/`](./agents/profiles/), no code.

| Agent                | Vendor           | How it's wrapped                                             |
| -------------------- | ---------------- | ------------------------------------------------------------ |
| **Claude Code**      | Anthropic        | env vars                                                     |
| **OpenAI Codex CLI** | OpenAI           | env vars (API key) · ephemeral `CODEX_HOME` (ChatGPT login)  |
| **Gemini CLI**       | Google           | env vars                                                     |
| **Aider**            | OpenAI/Anthropic | env vars                                                     |
| **Kilo Code**        | Kilo Code        | `KILO_CONFIG_CONTENT`, your `kilo.json` untouched            |
| **Qwen Code**        | QwenLM           | ephemeral system-settings overlay, source settings untouched |
| **opencode**         | sst              | inline config via env, your `opencode.json` untouched        |
| **Hermes Agent**     | Nous Research    | `--provider custom` + env                                    |
| **OpenClaw**         | OpenClaw         | ephemeral merged config, your config read-only               |
| **Pi**               | pi.dev           | bundled native extension, your `~/.pi` config untouched      |

<details>
<summary><strong>Per-agent fine print</strong> — what gets written, smoke tests, the default loadout, SDK recipes</summary>

`caveman wrap` never edits your config files. The persistent shortcut's writes are journaled and reversible with `caveman disable <agent>`. Real sessions round-trip in record mode, tested against **Hermes v0.18.0**, **OpenClaw 2026.6.11**, and **Pi 0.84.2**.

Kilo Code **7.5.6** passed a pinned real-CLI route and streaming-response smoke; its profile also runs through the same real-proxy compression and telemetry matrix as every other agent. This wraps Kilo's CLI (`kilo` or `kilocode`), not an already-running editor extension.

Qwen Code **0.22.3** passed pinned real-CLI local and managed OpenAI-compatible route smokes. Caveman deep-merges its route into a temporary system-settings overlay, preserves enterprise platform defaults, and leaves the source system and user settings unchanged. Install agent-side recovery with `caveman tools mcp install qwen --server caveman`; remove only Caveman's owned entry with the matching `uninstall` command.

</details>

## License

Split license. Skill and adoption surfaces are [MIT](./LICENSE). Engine-linked runtime is BSL-1.1 source-available, not OSI Open Source before Change Date.

**MIT** — the skill, Agent SDK and initializer, the CLI, both client SDKs (TS + Python), contracts, provider catalog, the extension shell, and the thin cavemem clients.

**BSL-1.1** — Engine, Proxy, Cache Engine, rewriter, Browse, MCP server, `shrink`, cavemem Go core, and shared Go platform. New Engine-linked runtime modules default to BSL-1.1. Source-available: read it, fork it, self-host it for your own first-party traffic free, production included. Every BSL version auto-converts to **Apache-2.0** on the earlier of `2030-06-21` or four years after that version first ships. Third-party hosted, managed, or embedded service use needs commercial license. BSL text and per-directory map ship with source.

## Star this repo

Caveman save you token, save you money. Star cost zero. Fair trade. ⭐
