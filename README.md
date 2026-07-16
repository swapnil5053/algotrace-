# algotrace

A Claude skill that teaches data structures and algorithms by drawing every step.

![MIT license](https://img.shields.io/badge/license-MIT-16a34a)
![Claude Code skill](https://img.shields.io/badge/claude_code-skill-2563eb)
![Dependencies](https://img.shields.io/badge/dependencies-none-94a3b8)

Every AI assistant I tried during placement prep would take my almost-right solution and hand back a rewritten, fully correct one — and I'd learn nothing. algotrace is the opposite: it coaches by default, draws by default, and only gives complete code when you say, in plain words, that you want it.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/demo-dark.gif">
  <img alt="binary search demo, each probe eliminates half" src="assets/demo.gif">
</picture>

Four colors with fixed meanings: blue is the pointer, green is found, red is eliminated, gray is processed. Nothing else, by contract.

## The first five minutes

```bash
git clone https://github.com/swapnil5053/algotrace- ~/.claude/skills/algotrace
```

Open a new Claude Code session and paste any of these:

```text
visualize binary search for 23 in [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
debug this: [your broken code] fails on [1,2,3,4,10], expected 14 got 7
I'm stuck on longest substring without repeating characters. do not spoil it.
```

No commands, no setup. The skill routes on intent.

## The seven modes

| You say | Mode | What you get |
|---|---|---|
| "visualize binary search on [2,5,8,...]" | visualize | A frame-by-frame trace, inline or as an interactive step player |
| "explain monotonic stack" | tutor | Definition, intuition, a small trace, pitfalls, one check question |
| "I'm stuck on LC 3, don't spoil it" | hint | Five levels: observation, pattern, invariant, technique, skeleton. One per message |
| paste code + "why does this fail?" | debug | The specific bug, proven with a trace to the exact step where your code diverges |
| "just give me the full code in Java" | solution | Clean commented code with complexity. Only fires on an explicit ask |
| "mock interview me, medium, 45 min" | interview | Timed phases, hints that cost points, a scored rubric |
| "review day" | review | Spaced-repetition recall drills from your progress log, plus a weakness report |

Debug mode is the reason this exists. It finds the bug, proves it with a trace, and the fix is one line — not a rewrite:

```diff
-    for i in range(k, len(nums) - 1):
+    for i in range(k, len(nums)):
```

## The step players

Full walkthroughs become one self-contained HTML file: prev/next/play, keyboard navigation, playback speed, and a light/dark theme that follows your system. Three demos are baked in — open them straight from the repo:
`assets/visualizer-template.html` (binary search), `demos/sliding-window.html`, `demos/bfs-graph.html` (graph frames).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/screenshots/bfs-graph-dark.png">
  <img alt="bfs step player at the frame where the target enters the queue" src="assets/screenshots/bfs-graph.png">
</picture>

## Also in here

- `docs/patterns-cheatsheet.md` — ten patterns: signal words, when to use, template, complexity.
- `docs/study-plan.md` — eight weeks of placement prep mapped to the modes.
- `scripts/testgen.py` — edge-case input generator for when hidden tests fail you (`python -m unittest discover -s scripts` to test it).
- `examples/` — real transcripts of a debug session and a hint ladder.
- A progress log (`.algotrace/progress.md`, created on first use) that schedules your redos at expanding intervals.

## How it compares

Against the most visible alternatives, July 2026: [algo-sensei](https://github.com/karanb192/algo-sensei), [peppermint leetcode-skill](https://github.com/peppermint-ai-lab/leetcode-skill), [LeetCode Teacher](https://github.com/luqmannurhakimbazman/ashford).

| | algotrace | algo-sensei | peppermint | LeetCode Teacher |
|---|---|---|---|---|
| Visualization | core feature, contractual | roadmap item | ASCII "when helpful" | none |
| Debug your own code | dedicated mode with proof traces | code review | post-submission feedback | none |
| Progress + spaced repetition | log, drills, weakness report | roadmap item | tracking only | profile only |
| Test input generator | yes, unit tested | roadmap item | no | no |
| Enforced visual style | strict contract file | no | no | no |

The one honest trade: peppermint generates original problems and researches company questions; algotrace drills you on problems you bring. If problem supply is your bottleneck, use both.

## Questions people actually ask

**Why not just prompt Claude directly?** You can, and it drifts — one day a wall of text, the next a full solution you didn't want. The skill pins the behavior: one router, one format contract, guardrails that survive long conversations.

**Will it refuse to give me answers?** No. Solution mode is one sentence away, always. It just won't hand you the answer while you're still asking for a hint.

Python, Java, C++ and JavaScript are first-class throughout. Claude.ai users: upload `SKILL.md`, `modes/`, and `assets/style-contract.md` to a project's knowledge and add "Follow SKILL.md" to its instructions.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). A wrong cell in a frame counts as a bug; there is an issue template for exactly that.

MIT licensed.
