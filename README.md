# 🧠 Agent Skills

Custom skills for AI-assisted development, optimized for my personal workflow ecosystem.

## What's this?

A collection of specialized skills designed to work together as a pipeline. Each skill is built for [opencode](https://github.com/opencode-ai/opencode) and follows the Agent Skills spec — a `SKILL.md` file inside a named directory.

These aren't generic templates. They're **workflow-specific tools** tuned for how I actually work: explore → brainstorm → stress-test → execute.

## Skills

| Skill | Purpose | Trigger |
|-------|---------|---------|
| [pre-mortem](./pre-mortem/) | Assume plan failed 6 months later, work backward to find causes | "premortem this", "what could kill this" |

## Workflow Context

These skills assume an ecosystem with:

- **[brainstorming](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md)** (from [superpowers](https://github.com/obra/superpowers)) — collaborative idea refinement before committing
- **[SDD + Engram](https://github.com/Gentleman-Programming/gentle-ai)** (from [gentle-ai](https://github.com/Gentleman-Programming/gentle-ai)) — structured planning phases with persistent memory across sessions

The typical flow:
```
brainstorming → sdd-explore → pre-mortem → sdd-propose → ...
```

Each skill reads context from previous phases via Engram and writes its own findings back. Sub-agents never access Engram directly — the orchestrator handles context injection.

## Structure

```
skills/
├── README.md
└── pre-mortem/
    └── SKILL.md
```

## License

MIT
