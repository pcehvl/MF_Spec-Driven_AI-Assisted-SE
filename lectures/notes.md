# Lecture Notes – Beyond Vibe Coding

[← Back to course home](../README.md) · [Slides (PDF)](Beyond_VibeCoding.pdf) · [Lecture video](https://youtu.be/9rTKgsyKrA0)

**Main message:** with an agent you get a new coworker that knows more, has seen more and has the tools to design, build, test and deliver faster than ever. Make the most of it. But you are still in charge, and you are responsible for what gets shipped. AI makes understanding software engineering more important, not less.

**Case:** StudyMatch, a simple app where students find, create and join study sessions. The app is not the point. The method is.

---

## Part 01 – From vibe coding to agentic development

*Video: [02:33](https://youtu.be/9rTKgsyKrA0?t=153)*

**Vibe coding** (Andrej Karpathy, February 2025): describe what you want, run it, ask for changes until it looks right. Nobody reads the code; the only test is what you see on screen.

- Every requirement and decision lives in one place: the code. No spec, no record of why.
- Fine for throwaway prototypes. Looking right says nothing about edge cases, user data or real user needs.

**The agent as a new coworker.** It knows more, has seen more code, has the tools, and gets better with every model. The question is no longer *what the agent can do*, but *what you ask it to do*. You would not tell a new colleague "build something, I'll look at it on screen".

**The workflow stays the same.** Requirements, design, implementation, testing, review, integration – each with its own artefact. What changes is who does implementation and testing:

| Phase | Who | Artefact |
| --- | --- | --- |
| Requirements | Engineer | User story / use case |
| Architecture / design | Engineer | Design model |
| Implementation | Agent | Code on a branch |
| Testing | Agent | Check results |
| Review | Engineer | Pull request |
| Integration | Engineer | Git history |

**Without specs, the agent guesses** (cf. Brooks, *No Silver Bullet*, 1987):

- *Vague in, vague out* – the agent fills every gap with a guess.
- *The chat forgets* – each session starts from zero; the spec stays in the repo.
- *Same prompt, different code* – the model varies; the spec and the checks do not.
- *No criteria, no done* – without acceptance criteria nobody can say PASS or FAIL.

> The spec is the contract between you and the agent. Write it before the code.

**Agentic programming** adds a spec and a review to the loop: you specify, the agent builds, you verify against the spec, and new learning goes into the spec.

---

## Part 02 – The harness and engineering control

*Video: [16:18](https://youtu.be/9rTKgsyKrA0?t=978)*

**How an agent works:** prompt and context go in; the agent loops *plan → act → observe → refine* using tools (tests, files, APIs via MCP) and memory, until it thinks it is done. The engineer stays outside the loop: sets the goal, grants the tools, approves the result. Human-in-the-loop: the agent stops for review before a change is accepted.

**Agent = LLM + harness.** The harness is everything around the model, kept in the repo:

| Part | In the repo | Purpose |
| --- | --- | --- |
| Context | `AGENTS.md`, `product/`, `docs/` | What the agent knows before it starts |
| Roles | `.opencode/agents/` | Build (OpenCode's main agent) hands work to Product Assistant (what), Planner (how), Coder (build) |
| Skills | `.opencode/skills/` | Six reusable methods, e.g. product-management |
| Permissions | front-matter in `.opencode/agents/*.md` | What each agent may edit and run: allow, ask, deny |
| Checks | `e2e/`, npm scripts | Lint, types, tests, build, Playwright before every PR |

> Same model, better harness, better result. If it is not in the repo, the agent does not know it.

**One main agent, three role agents, two human roles**

| | Product Assistant | Planner | Coder |
| --- | --- | --- | --- |
| Owns | Why and what | How | Build and verify |
| Writes | `product.md`, `spec.md` | `plan.md` | code, tests, `verification.md`, PR |
| Never | code or technical plans | implements or changes acceptance criteria | edits requirements or merges alone |

> You always work in Build. It delegates each step to the role agent that owns it; the role agents run as subagents with their own, narrower permissions.

- **Product Owner** – owns the *what* and accepts the value.
- **System Engineer** – owns the *how*, the quality gate and the merge.
- In this course you play both roles. Notice which hat you wear.

---

## Part 03 – Two loops

*Video: [21:28](https://youtu.be/9rTKgsyKrA0?t=1288)*

- **Product loop** – *are we building the right thing?* Discover, define story and spec, learn from use. Led by users and the Product Owner.
- **Execution loop** – *are we building it right?* Plan, build and verify, review. Led by the System Engineer; the agents do much of the work.
- The handover: a **Ready story** goes in, an **accepted increment** comes back.

**Methods you know:** Design Thinking (Double Diamond) decides *what* to build. Scrum builds it in increments. Agents work inside each Sprint. The agents do not change the methods, only the speed.

**Weekly sprints can become daily.** When agents code and test, that work shrinks from days to hours, while planning and review still take human time. More sprints means more feedback and an earlier MVP. Caution: this only works when stories are small and Ready, and someone has time to review every day.

> Agents speed up the execution loop. Only people using the product can close the product loop.

---

## Part 04 – The product loop

*Video: [26:46](https://youtu.be/9rTKgsyKrA0?t=1606)*

**Product framing** turns evidence into `product/product.md`. The Product Assistant drafts, the PO decides:

| Step | Product Assistant | Product Owner |
| --- | --- | --- |
| Problem | Clusters the evidence, drafts problem statements | Picks the problem worth solving |
| Target users | Proposes personas from interview notes | Confirms who the MVP is for |
| Key question | Turns assumptions into testable hypotheses | Chooses the one question to answer |
| MVP scope | Lists what is in and out, flags risks | Draws the MVP line |
| Success metric | Suggests measurable criteria | Sets the target, e.g. 30 % join a session in week 1 |

Every statement in `product.md` is labelled as evidence, assumption, hypothesis or decision. No story is written until the PO approves the framing.

**Traceability:** evidence (E1–E6 in `docs/design-thinking/`) → framing (F1–F4 in `product.md`) → backlog (one `spec.md` per story). Rule: *no source, no story.*

**A backlog item is a spec that says what to build:**

- **User story** – As a *role*, I want *goal*, so that *value*. One user, one need, one value; small enough for one vertical slice.
- **Acceptance criteria** – observable behaviour, each one PASS or FAIL, covering the edge cases that matter.
- **Context and limits** – constraints, explicit out-of-scope, status and trace to evidence.

**Definition of Ready – five checks:** `spec.md` exists with an ID · traces to evidence · intent is clear · testable acceptance criteria · no blocking questions. The Product Assistant asks the PO one question at a time and writes each answer into the spec.

---

## Part 05 – The execution loop

*Video: [53:46](https://youtu.be/9rTKgsyKrA0?t=3226)*

Agents build, the engineer controls, the PO accepts:

| Step | Agent | Human | Output |
| --- | --- | --- | --- |
| 1 Plan | Planner reads `spec.md` and the code | Engineer checks the plan fits the story | `plan.md` |
| 2 Implement | Coder opens a GitHub Issue and a feature branch, then builds | Engineer watches, stops drift | Issue + branch |
| 3 Test | Coder runs the full gate, checks every AC | Engineer reads the AC verdicts | PASS / FAIL per AC |
| 4 Pull request | Coder self-reviews, opens the PR and stops | Engineer reviews code, PO tries the Preview | Pull request |
| 5 Accept | Coder deploys on the engineer's go | PO accepts, engineer merges | Deployed increment |

**The Planner** writes only `plan.md`: facts about the existing system (not guesses), the approach and rejected options, migrations (or why none), every acceptance criterion mapped to a test, and human checkpoints. The plan holds the *how*, never the *what*: it may not weaken, redefine or expand an acceptance criterion, and a gap goes back to the PO.

**Feedback goes back through the product loop.** The Product Assistant classifies it (defect, usability issue, changed requirement, new user need, feature idea, invalidated assumption, no action), and the PO decides the outcome: change a story, create a story, update `product.md`, or no action. Feedback never becomes code directly.

---

## Part 06 – The MVP playbook

*Video: [1:19:24](https://youtu.be/9rTKgsyKrA0?t=4764)*

1. **Collect** – set up, add design-thinking material to `docs/design-thinking/`.
2. **Frame** – Product Assistant drafts `product.md`; you decide as PO.
3. **Story** – one `spec.md` with testable acceptance criteria.
4. **Ready** – refine one question at a time.
5. **Plan** – Planner writes `plan.md`; you approve as engineer.
6. **Build** – Coder builds on a feature branch, runs `npm run verify`, opens the PR.
7. **Accept, learn** – accept as PO, merge and deploy as engineer, test with users. Feedback → new or revised stories.

**Getting started:** run `npm install` and `npm run verify` (lint, types, tests, build and E2E – before the PR and again before merge), add your design-thinking results to `docs/design-thinking/`, and start in the product loop, one story at a time.

> One story at a time, through both loops. Generated code alone is never Done.

---

## References

- Andrej Karpathy, post on X, 2 February 2025 – [x.com/karpathy/status/1886192184808149383](https://x.com/karpathy/status/1886192184808149383)
- Frederick P. Brooks, "No Silver Bullet: Essence and Accidents of Software Engineering", 1987
- Anthropic, [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents), 2024
- Martin Fowler, [Agentic Programming](https://martinfowler.com/bliki/AgenticProgramming.html), 2026
- Schwaber & Sutherland, [The Scrum Guide](https://scrumguides.org/scrum-guide.html), 2020
