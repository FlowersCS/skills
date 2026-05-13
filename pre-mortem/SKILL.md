---
name: premortem
description: "Assume plan failed 6 months later, work backward to find causes. Breaks AI's agreeable pattern."
triggers:
  - "premortem this"
  - "what could kill this"
  - "stress test this plan"
  - "find blind spots"
ignore:
  - simple feedback
  - factual questions
  - requests for multiple perspectives
metadata:
  author: flowerscs
  version: "1.1"
---

## Purpose

Identify failure modes before execution by assuming plan already failed. Breaks AI's agreeable pattern.

## When to Execute

- Product/feature about to be built
- Launch with money/reputation at stake
- Hire, pivot, partnership decisions
- Any high-stakes commitment

## When NOT to Execute

- Vague ideas without plan (help plan first)
- Single correct answer questions (just answer)
- Decisions already made and irreversible

## Constraints

- DO NOT modify files under `/config` without approval
- DO NOT invent context
- DO NOT soften failures — be direct
- DO NOT skip context threshold
- DO NOT run agents sequentially — always parallel
- DO NOT trigger on requests for multiple perspectives
- MAX 300 words per failure analysis agent, 3 sentences chat summary

## Execution Flow

### Step 1: Gather Context

Context flows from brainstorming → sdd-explore → premortem. Leverage it.

**A. Current conversation** — Includes brainstorming discussion + sdd-explore findings already in context. Extract the plan/decision and key findings.

**B. Workspace scan** — Check for:
- `AGENTS.md` or `CLAUDE.md`
- Project files, briefs, plans

**C. Engram (Use MCP)** — Query for:
- `sdd/{change}/explore` artifacts from this session's sdd-explore phase
- Brainstorming decisions and user preferences from this project
- Previous decisions about this project/area

Max 5 reads, 3 globs. Identify anchor files.

### Step 2: Evaluate Context Sufficiency

PROCEED if you have ALL THREE:

1. **WHAT** — (one sentence description)
2. **WHO** — (audience, stakeholders)
3. **SUCCESS** — Expected outcome (failure = inverse of success)

If MISSING: Ask ONE question at a time. Conversational, not interrogatory. Infer when possible.

### Step 3: Set Frame

After gathering sufficient context, state explicitly:

> "I have enough context. Running premortem. Premise: 6 months have passed. [Plan/launch/decision] has failed. It is done. We look back to understand what went wrong."

### Step 4: Generate Raw Failure Reasons

Prompt to self: "Plan failed 6 months later. Generate every genuine reason why. Exhaustive. Specific. Grounded in details. No padding."

Each reason MUST be: specific to plan, grounded in details, genuine threat.

### Step 5: Deep Analysis Agents (ALL IN PARALLEL)

Launch ONE sub-agent per failure reason (MAX 6). Group similar failure reasons if more than 6.

**Sub-agent prompt template:**

You are a researcher in a premortem analysis. You have been assigned one specific failure reason to analyze in depth.

THE PLAN:
[full context: what, who, success criteria, relevant workspace context]

PREMORTEM FRAME: 6 months have passed. This plan has failed.

YOUR ASSIGNED FAILURE REASON: [specific failure reason from step 4]

Your job is to go deep on this failure. Write the story of how it actually unfolded. Be specific. Use details from the plan. Make it feel real, like a case study of something that actually happened.

Your output must include:

1. FAILURE STORY: 2-3 paragraphs narrative of how this specific failure unfolded. Use plan details. Name specific moments where things went wrong and why.
2. UNDERLYING ASSUMPTION: The single thing the user took for granted that made this failure possible. One sentence.
3. EARLY WARNING SIGNS: 1-2 concrete, observable signals the user could watch for that would indicate this failure mode is starting to develop. Must be things that can actually be seen or measured, not vague feelings.

Keep total response under 300 words. Be direct. Do not soften.

ORCHESTRATOR: Inject ALL gathered context into THE PLAN field above. Include:
- Conversation summary from brainstorming
- Key findings from sdd-explore (read from Engram if needed)
- User preferences and constraints from Engram
Sub-agents have NO memory — they only see what you pass here.

### Step 6: Synthesis

1. Most Likely Failure
2. Most Dangerous Failure
3. Hidden Assumption
4. Revised Plan (specific changes)
5. Pre-Launch Checklist (3-5 items)

### Step 7: Save Outputs

- `docs/premortem/premortem-transcript-[timestamp].md` — full analysis (synthesis + all agent outputs)
- Engram — key findings via mem_save with project context

Report saved file path in one line. Concise.

## Example

**Input:** "premortem this: launching $297 workshop for marketing directors at 10-50 person companies, 50 seats"

**Chat Output:** "Most likely: audience mismatch — targeting people needing approval for $297. Hidden assumption: directors self-identify as reachable audience. Key revision: run $47 pilot first."

## Definition of Done

- [ ] Context threshold met
- [ ] Failure reasons exhaustive
- [ ] Max 6 agents, grouped if needed
- [ ] Agents launched parallel
- [ ] Outputs under word limits
- [ ] Transcript saved, Engram updated

