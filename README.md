# algotrace

A Claude skill that teaches data structures and algorithms by drawing every step.

I built this because every AI assistant I tried during placement prep did the same thing: I'd paste a solution with one wrong loop bound, and instead of telling me where I went wrong, it would hand me a rewritten, fully correct solution. I'd read it, nod, and learn nothing. algotrace is the opposite. It coaches by default, it draws by default, and it only gives you complete code when you say, in plain words, that you want it.

## How it works

Drop the folder into your Claude skills directory. From then on, any message about LeetCode, DSA, debugging, or interview prep routes to one of six modes automatically. There are no commands to remember.

| You say | Mode | What you get |
|---|---|---|
| "visualize binary search on [2,5,8,...]" | visualize | A frame-by-frame trace, either inline or as an interactive HTML page with prev/next/play controls |
| "explain monotonic stack, I don't get it" | tutor | Definition, intuition, a mandatory small trace, pitfalls, and one check question |
| "I'm stuck on LC 3, don't spoil it" | hint | A five-level ladder: observation, pattern, invariant, technique, skeleton. One level per message |
| paste code + "why does this fail?" | debug | The specific bug, proven with a trace to the exact step where your code goes wrong |
| "just give me the full code in Java" | solution | Clean commented code with complexity analysis. Only fires on an explicit ask |
| "mock interview me, medium, 45 min" | interview | Timed phases, hints that cost points, a scored rubric at the end |

Debug mode is the reason this exists. Paste a broken sliding-window attempt and algotrace will not rewrite it. It traces your code on the failing input, shows you the window that never gets formed, and asks one question that leads you to the bug. Ask again and you get a one-line diff. Your code stays yours.

## The visual language

Everything renders in four colors with fixed meanings, on plain ink and paper. No gradients, no glow, no dashboard chrome.

| Color | Meaning |
|---|---|
| blue | current pointer, active element |
| green | correct, found, in-window |
| red | wrong, eliminated, out of bounds |
| gray | processed, inert |

Short traces render inline in chat as SVG or tables. Full walkthroughs become a single self-contained HTML file with step controls and keyboard navigation. Open `assets/visualizer-template.html` in a browser to see the baked-in binary search demo, or `demos/sliding-window.html` for a fixed-size window example. Both work offline with no dependencies.

## An example of what debug mode does

Given this attempt at max-sum-of-size-k:

```python
for i in range(k, len(nums) - 1):
    window += nums[i] - nums[i - k]
```

algotrace traces the failing input, shows that the last window is never visited, and after you ask for the fix:

```diff
-    for i in range(k, len(nums) - 1):
+    for i in range(k, len(nums)):
```

One line. Not a rewrite.

## Install

Claude Code:

```bash
git clone https://github.com/YOUR_USERNAME/algotrace ~/.claude/skills/algotrace
```

Start a new session and talk about any DSA problem. The skill triggers on its own.

Claude.ai or Claude desktop: create a project, upload `SKILL.md`, the `modes/` files, `assets/style-contract.md`, `assets/visualizer-template.html` and `docs/patterns-cheatsheet.md` to project knowledge, and add one line to the project instructions: "Follow SKILL.md: route messages to the matching mode file and obey assets/style-contract.md."

## What else is in here

- `docs/patterns-cheatsheet.md` — the ten patterns that cover most OA problems: signal words, when to use, template shape, complexity.
- `docs/study-plan.md` — an eight-week placement prep plan that pairs each week with specific skill modes.
- `scripts/testgen.py` — a small stdlib-only generator for edge-case test inputs (arrays, strings, trees, graphs). Useful when your solution passes the samples but fails hidden tests.
- `examples/` — two abridged transcripts showing what a debug session and a hint ladder actually look like.

## Repo layout

```text
algotrace/
├── SKILL.md                       entry point: triggers and intent router
├── modes/                         six mode files, one per behavior
├── assets/
│   ├── style-contract.md          the formatting and color rules every mode follows
│   └── visualizer-template.html   interactive step player, binary search demo baked in
├── demos/
│   └── sliding-window.html        second demo: fixed-size window, max sum
├── docs/
│   ├── patterns-cheatsheet.md     ten patterns with templates
│   └── study-plan.md              eight-week prep plan
├── examples/                      sample session transcripts
├── scripts/
│   └── testgen.py                 edge-case input generator
├── CONTRIBUTING.md
├── README.md
└── LICENSE
```

## What it does not do

It does not track your progress across sessions, it does not maintain a problem list, and it will not stop you from asking for the answer. The full solution is always one honest sentence away. The point is that you have to say it out loud.

Python, Java, C++ and JavaScript are first-class throughout. Other languages work; those four get idiom checks and language-specific bug tables.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). The most useful contributions are new worked examples in the mode files and new baked demos.

## License

MIT. See [LICENSE](LICENSE).
