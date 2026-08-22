# Contributing

Thanks for helping improve this free coding interview guide. Contributions of all sizes are welcome: fixing a typo, sharpening a recognition cue, correcting a complexity, adding a pattern page, or writing a problem walkthrough.

## Ground rules

- Keep it original. Do not paste content from paid courses (including the DesignGurus course) or other copyrighted sources. Write explanations in your own words.
- Do not copy problem statements from LeetCode or any other site. Describe the problem in your own words, or link out to it.
- Keep it concise and practical. Favor clear summaries, tables, and short code templates over long essays.
- No em dashes. Use commas, colons, parentheses, or periods instead. (House style.)
- Explain the recognition cue, not just the solution. "How do I know this is the right pattern?" is the question this repo exists to answer.

## What belongs here, and what does not

This repo is a free companion to a paid course, and the line between them is deliberate.

| Belongs here | Does not belong here |
|---|---|
| Pattern explanations and recognition cues | Full worked solutions to every problem |
| Short code templates, 15 to 30 lines | Complete, tested, multi-language solution files |
| The approach and the invariant for a problem | Line by line solution walkthroughs |
| Cheat sheets, checklists, roadmaps | Data structure tutorials (see Grokking Data Structures) |
| Comparison pages between two approaches | System design content (see the grokking-system-design repo) |

We also do not accept tooling. No build scripts, no CLIs, no vendored JavaScript. This is a content repository, and it stays readable on GitHub with no install step.

## How to add a pattern

1. Copy `patterns/_template.md` to `patterns/your-pattern.md`.
2. Fill in every section. The "Recognize it when" section is the most important one, so write it first.
3. Add a row to the table in `patterns/README.md` and, if it is a core pattern, to the table in the root `README.md`.

## How to add a problem walkthrough

1. Copy `problems/_template.md` to `problems/your-problem.md`.
2. Keep it at the "approach and invariant" level. State the idea, the complexity, and the edge cases. Do not write the full solution.
3. Add a row to the index table in `problems/README.md`.

## Style

- One concept per file.
- Use relative links between files so they work on GitHub.
- Code templates in Python by default, because it reads closest to pseudocode. Other languages go in `templates/`.
- Prefer simple ASCII or Mermaid diagrams that render on GitHub.
- Write complexity as O(n), O(n log n), O(2^n). Say what n is.

## Pull requests

Open a pull request with a short description of what you changed and why. Small, focused pull requests are easier to review and merge.
