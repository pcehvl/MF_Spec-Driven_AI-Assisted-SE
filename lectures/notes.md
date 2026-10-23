# Lecture Notes – Beyond Vibe Coding

[← Back to course home](../README.md) · [Slides (PDF)](Beyond_VibeCoding_Spec-Driven_AI-Assisted_Development.pdf) · [Lecture video](https://youtu.be/dxGCkEh3MBU)

**Main message:** with an agent you get a new coworker that knows more, has seen more and has the tools to design, build, test and deliver faster than ever. Make the most of it. But you are still in charge, and you are responsible for what gets shipped. AI makes understanding software engineering more important, not less.

**Case:** StudyMatch, a simple app where students find, create and join study sessions. The app is not the point. The method is.

---

**About the video:** the recording follows the current version of the lecture, in the same order as these notes. The *Video* link under each part jumps to where that part starts in the recording.

---

## Part 01 – From vibe coding to spec-driven development

*Video: [02:49](https://youtu.be/dxGCkEh3MBU?t=169)*

**Vibe coding** (Andrej Karpathy, February 2025): describe what you want, run it, ask for changes until it looks right. Nobody reads the code; the only test is what you see on screen.

- Every requirement and decision lives in one place: the code. No spec, no record of why.
- Each session starts from scratch, and the result is often weak on maintainability, correctness and security.
- **When is vibe coding fine?** When all three hold: the code is *throwaway*, the stakes are *low*, and errors *show on use*.

**The agent as a new coworker.** It knows more, has seen more code, has the tools, and gets better with every model. The question is no longer *what the agent can do*, but *what you ask it to do*.

**The software engineering workflow stays the same.** Requirements, design, implementation, testing, review, integration – each with its own artefact. The agent changes the code and runs the checks; the engineer owns the other phases, with the agent assisting. Scrum still delivers one increment per Sprint, and feedback becomes new requirements.

| Phase | Who | Artefact |
| --- | --- | --- |
| Requirements | Engineer specifies the outcome and criteria (agent assists) | User story / use case |
| Architecture / design | Engineer decides the design (agent assists) | Design model |
| Implementation | Agent changes the code on a bounded task | Code on a branch |
| Testing | Agent writes and runs the checks | CI results |
| Review | Engineer inspects the diff and the evidence (agent assists) | Pull request |
| Integration | Engineer accepts and merges (agent assists) | Git history |

**Agentic programming** adds a spec and a review to the loop: you specify, the agent builds, you verify against the spec, and new learning goes into the spec. Unlike vibe coding, it needs engineering knowledge: the spec carries intent across sessions and agents, and the engineer verifies structure, correctness and security.

**Specs give us control over the agent** (cf. Brooks, *No Silver Bullet*, 1987: the hardest part is deciding precisely what to build):

- *Determinism* – same prompt, different code: the code varies, the criteria do not.
- *Control* – the agent acts on its own: scope and constraints set the limits.
- *Verifiable* – no criteria, no done: the acceptance criteria give PASS or FAIL.
- *Review* – the agent writes faster than you can read: check against the spec, not every line.

> The agent varies. The spec and its checks do not. That is where the control is.

**What we mean by a spec.** A spec lives in the repo and outlives the chat. It has four parts:

| Part | Contains | Example (`login-lockout.md`) |
| --- | --- | --- |
| User story | Who needs what, and why | As a user, I want my account locked after five failed logins, so attackers cannot guess my password. |
| Acceptance criteria | Testable conditions: PASS or FAIL | Five failed attempts lock the account for 15 minutes · a successful login resets the counter |
| Constraints | Security, privacy and compliance rules | Every lockout is logged for audit |
| Out of scope | What the agent must not change | Password reset flow |

**Two files: what to build, then how.**

| | `spec.md` – what and why | `plan.md` – how |
| --- | --- | --- |
| Contains | Story, acceptance criteria, constraints, scope | Architecture, tech stack, data model, interfaces |
| Reviewed by | Product Owner and engineer | Engineer and tech lead |
| Changes when | The need changes | The design or technology changes |
| The agent uses it to | Know when the work is done | Know how to build it |

> One reason to change per file. Swap the tech without rewriting the need.

---

## Part 02 – Two loops

*Video: [21:09](https://youtu.be/dxGCkEh3MBU?t=1269)*

- **Product loop** – *are we building the right thing?* Discover, define story and spec, learn from use. Led by people: users and the Product Owner. They decide what to build and accept the value.
- **Execution loop** – *are we building it right?* Plan, build and verify, review. Led by the System Engineer; the Planner and Coder do the work, and the agentic loop runs here.
- The handover: a **chosen story + spec** goes in (Sprint Planning), an **accepted increment** comes back (Sprint Review).

**Two roles, as in Scrum.** In this course you play both: decide as PO, build and merge as System Engineer.

| Product Owner · product loop | System Engineer · execution loop |
| --- | --- |
| Owns the *what* and the *why* | Owns the *how* and the quality |
| Vision, scope and priority | Approves the Planner's plan |
| Stories and acceptance criteria | Steers the Coder, stops drift |
| Answers refinement questions | Owns the gate and the Definition of Done |
| Accepts the increment in the Preview | Reviews the code in the pull request |
| Decides on feedback | Merges and deploys after acceptance |

**Methods you know:** Design Thinking (Double Diamond) decides *what* to build. Scrum builds it in increments, and the Definition of Done decides when an increment is ready. Agents work inside each Sprint.

**Weekly sprints can become daily.** When agents code and test, that work takes hours, not days. In the same week we can run five sprints instead of one, and ship more.

**We can reach the MVP sooner.** The MVP – the first increment users can try – closes the first execution loop. Agents give us the capacity to get there faster, which leaves time for user tests and the next stories.

> Agents speed up the execution loop. Only people using the product can close the product loop.

---

## Part 03 – The harness and engineering control

*Video: [27:22](https://youtu.be/dxGCkEh3MBU?t=1642)*

**How an agent works:** prompt and context go in; the agent loops *plan → act → observe → refine* using tools (tests, files, APIs via MCP) and memory. The engineer stays outside the loop: sets the goal, grants the tools, approves the result. Human-in-the-loop: the agent stops for review before a change is accepted.

**Agent = LLM + harness.** You define the work (intent, user and technical requirements, technical plan), approve it (review, Preview) and own the result. The harness sits between you and your systems (GitHub, Supabase, Vercel), and it lives in the repo:

| Part | In the repo | Purpose |
| --- | --- | --- |
| Context | `AGENTS.md`, `product/`, `docs/` | What the agent knows before it starts |
| Roles | `.opencode/agents/` | Build (OpenCode's main agent) hands work to Product Assistant (what), Planner (how), Coder (build) |
| Skills | `.opencode/skills/` | Six reusable methods, e.g. product-management |
| Permissions | front-matter in `.opencode/agents/*.md` | What each agent may edit and run: allow, ask, deny |
| Checks | `e2e/`, npm scripts | Lint, types, tests, build, Playwright before every PR |

> Same model, better harness, better result. If it is not in the repo, the agent does not know it.

**Three agents in the template harness – separate roles, skills and files**

| | Product Assistant | Planner | Coder |
| --- | --- | --- | --- |
| Owns | Why and what | How | Build and verify |
| Does | Turns discovery into framing, writes and refines stories, sets story status, classifies feedback | Plans exactly one Ready story, checks the code first, maps every AC to a test, names risks and human checkpoints | Opens the Issue and a feature branch, implements the plan with tests, runs the full gate, opens the PR, deploys on acceptance |
| Skills | `product-management`, `feedback-management` | `plan-story` | `implement-story`, `ui-design`, `deploy` |
| Writes | `product.md`, `spec.md` | `plan.md` | code, tests, `verification.md`, PR |
| Never | code or technical plans | implements or changes acceptance criteria | edits requirements or merges alone |

> You always work in Build. It delegates each step to the role agent that owns it; the role agents run as subagents with their own, narrower permissions.

**An agent is a role, a skill is a method.**

| | Agent file – who and what | Skill – how |
| --- | --- | --- |
| Where | `.opencode/agents/product-assistant.md` | `.opencode/skills/product-management/SKILL.md` |
| Contains | Role, limits, permissions, hand-off | Description, steps, template |
| Loaded | Always, while the agent runs | Only when the task needs it |
| Answers | Who acts, and what it may touch | How the task is done, every time |

- **Anatomy of an agent file:** the *front-matter* sets permissions and is enforced by the harness (e.g. the Product Assistant may edit `product/` only, and has no shell). The *prompt* sets the role and limits, and the hand-off (the Product Assistant delivers `spec.md` for the Planner).
- **Anatomy of a skill:** a *description* that decides when the agent loads it, the *steps* – the same method every time – and a *template* (e.g. the four parts of `spec.md`).

> Keep roles small and methods reusable. Six skills, three agents: fix a skill once, and every agent gains.

**Guardrails: what the agent cannot do.** Three levels of control:

| Enforced – the harness blocks it | Checked – the gate catches it | Approved – a human decides it |
| --- | --- | --- |
| Deterministic: the agent cannot do it, whatever the prompt says | Automatic: the change fails before it reaches a human | The agent stops and waits |
| Permissions per agent: allow, ask, deny | Lint, types, tests, build, Playwright | Engineer approves `plan.md` |
| Branch protection: no push to main | Row Level Security tested per table | Engineer reviews the pull request |
| Secrets only in `.env`, never in code or prompts | Secret scanning and dependency audit | PO accepts in the Preview |
| No migrations or deletes against production | Out-of-scope files flagged in the diff | Engineer merges and deploys |

> A rule in a prompt is a wish. A rule in the harness is a guardrail.

---

## Part 04 – The product loop

*Video: [38:21](https://youtu.be/dxGCkEh3MBU?t=2301)*

**Product framing** turns evidence into `product/product.md`. The Product Assistant drafts, the PO decides:

| Step | Product Assistant | Product Owner |
| --- | --- | --- |
| Problem | Clusters the evidence, drafts problem statements | Picks the problem worth solving |
| Target users | Proposes personas from interview notes | Confirms who the MVP is for |
| Key question | Turns assumptions into testable hypotheses | Chooses the one question to answer |
| MVP scope | Lists what is in and out, flags risks | Draws the MVP line |
| Success metric | Suggests measurable criteria | Sets the target, e.g. 30 % join a session in week 1 |

Every statement in `product.md` is labelled as evidence, assumption, hypothesis or decision. No story is written until the PO approves the framing.

**Traceability:** evidence (E1–E6 in `docs/design-thinking/`) → framing (F1–F4 in `product.md`) → backlog (one `spec.md` per story). Rule: *no source, no story.* One story, one `spec.md`: it holds the status and traces back to the evidence.

**Product framing gave us the Product Backlog** – Scrum terms, and where they live in the repo:

| In Scrum | In our repo | Where |
| --- | --- | --- |
| Product Backlog | `product/stories/`, one folder per user story | Framing |
| Product Backlog Item | One `spec.md` per user story, `status: backlog` | Framing |
| Backlog refinement | The Product Assistant asks, the PO answers | Refinement |
| Definition of Ready | Five Ready checks pass, then `status: ready` | Refinement |

**Definition of Ready – five checks:** `spec.md` exists with an ID · traces to evidence · intent is clear · testable acceptance criteria · no blocking questions. The Product Assistant asks the PO one question at a time and writes each answer into the spec. No GitHub yet: the story lives in `product/stories/` until the Planner and Coder take it.

---

## Part 05 – The execution loop

*Video: [1:04:45](https://youtu.be/dxGCkEh3MBU?t=3885)*

Agents build, the System Engineer controls, the PO accepts:

| Step | Agent | Human | Output |
| --- | --- | --- | --- |
| 1 Plan | Planner reads `spec.md` and the code, writes the plan | Engineer checks the plan fits the story | `plan.md` |
| 2 Implement | Coder opens a GitHub Issue and a branch, then builds | Engineer watches, stops it if it drifts | Issue + branch |
| 3 Test | Coder runs the full gate, checks every AC | Engineer reads the AC verdicts | AC: PASS / FAIL |
| 4 Pull request | Coder self-reviews, opens the PR and stops | Engineer reviews the code, PO tries the Preview | Pull request |
| 5 Accept | Coder deploys on the engineer's go | PO accepts, engineer merges | Deployed MVP |

**The Planner** writes only `plan.md`: facts about the existing system (not guesses), the approach and rejected options, a migration (or the reason for none), every acceptance criterion mapped to a test, and human checkpoints it cannot fake. It ends with an implementation sequence – for US-004: migration and RLS policy, edit form and save, delete, hide actions for others, tests and a Playwright journey. The plan holds the *how*, never the *what*: it may not weaken, redefine or expand an acceptance criterion, and a gap goes back to the PO.

**Green is not done: reading the evidence.** The agent writes both the code and the tests, so a green gate only proves the code agrees with itself.

| The question | How you check it | Who |
| --- | --- | --- |
| Does it test the right thing? | ACs written by the PO, each mapped to a test | PO |
| Are the tests honest? | Read the test diff: nothing skipped, weakened or mocked away | Engineer |
| Did it stay in scope? | The diff matches `plan.md`, no files outside it | Engineer |
| Does it work for the user? | Walk through every AC in the Preview | PO |

> The gate checks the code. You check the gate.

**Feedback goes back through the product loop.** The Product Assistant classifies it (defect, usability issue, changed requirement, new user need, feature idea, invalidated assumption, no action), and the PO decides the outcome: change an existing story, create a new story, update `product.md`, or take no action. Feedback never becomes code directly.

---

## Part 06 – The MVP playbook

*Video: [1:32:55](https://youtu.be/dxGCkEh3MBU?t=5575)*

**Seven steps, two loops.** Same seven steps in both tracks; only the tools in the execution loop differ.

*Product loop – the agent drafts, you decide as Product Owner*

1. **Collect** – gather design-thinking evidence about users and the problem.
2. **Frame** – who it is for, which problem, which outcome; you decide as PO.
3. **Story** – one spec per story, with testable acceptance criteria (`ACn`).
4. **Ready** – refine one question at a time until the story is Ready.

*Execution loop – the agent plans and builds, you control as System Engineer*

5. **Plan** – the agent proposes a technical plan; you approve it as engineer.
6. **Build and verify** – the agent builds the smallest slice and checks every `ACn`.
7. **Accept and learn** – accept PASS or FAIL per `ACn` as PO, release, test with users. Feedback → new or revised stories.

**Your take-home harness – OpenCode track** ([MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template)). One repo, both loops in OpenCode; the app code and its checks live next to the specs.

```text
your-mvp/
├─ AGENTS.md
├─ product/
│  ├─ product.md
│  └─ stories/US-XXX-<slug>/   spec.md · plan.md · verification.md
├─ docs/
│  ├─ design-thinking/
│  └─ architecture.md · ui-guidelines.md
├─ .opencode/                  agents/ · skills/
├─ src/ · e2e/
├─ supabase/migrations/
└─ .github/                    PR template
```

- `product/` – `product.md` holds the framing. One folder per story *is* the backlog: no `backlog.md`, no Issues.
- `.opencode/` – Product Assistant, Planner and Coder, with six skills.
- `plan.md` · `verification.md` – the Planner's plan you approve as engineer, and the Coder's evidence you read before you accept as PO.
- `npm run verify` – lint, types, tests, build and E2E, before the PR and again before merge.

**Your take-home harness – Lovable track** ([MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template), see the [Lovable track](https://pcehvl.github.io/MF_Spec-Driven_AI-Assisted-SE-Lovable/)). A product repo in OpenCode, the app in Lovable: `/handover` sends `spec.md` to Lovable, `/return` brings `acceptance.md` back, and the execution loop runs in Lovable with `/harness-init`, `/build-story`, `/accept-story` and `/publish`.

**Getting started:** run `npm install` and `npm run verify`, add your design-thinking results to `docs/design-thinking/`, and start in the product loop, one story at a time.

> One story at a time, through both loops. Generated code alone is never Done.

---

## References

- Andrej Karpathy, post on X, 2 February 2025 – [x.com/karpathy/status/1886192184808149383](https://x.com/karpathy/status/1886192184808149383)
- Frederick P. Brooks, "No Silver Bullet: Essence and Accidents of Software Engineering", 1987
- Anthropic, [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents), 2024
- Martin Fowler, [Agentic Programming](https://martinfowler.com/bliki/AgenticProgramming.html), 2026
- Schwaber & Sutherland, [The Scrum Guide](https://scrumguides.org/scrum-guide.html), 2020
