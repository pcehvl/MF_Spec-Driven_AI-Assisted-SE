[← MakersForge 2026 – all tracks](https://pcehvl.github.io/MakersForge-2026/)

# Beyond Vibe Coding – From Concept to MVP

**Spec-driven, AI-assisted software engineering with agents.**
Course material for the lecture by Asst. Prof. Per Christian Engdal, Western Norway University of Applied Sciences (HVL), 2026.

Everything you need after the lecture is linked from this page: the setup guide, the slides and notes, the videos and demos, and the template you build your own MVP from.

---

## Start here

1. **Set up your machine** → follow the [Setup Guide](setup/SETUP.md) (60–90 minutes).
2. **Create your own repo** → open [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template) and click **Use this template**.
3. **Watch the lecture** → see [Videos](videos.md), then follow the [MVP Playbook](#the-mvp-playbook-in-seven-steps) below.

---

## Course material

| What | Link | Use it for |
| --- | --- | --- |
| Setup Guide | [setup/SETUP.md](setup/SETUP.md) | Installing Git, Node.js, OpenCode, Supabase and Vercel, and getting `npm run verify` green |
| Slides (PDF) | [lectures/Beyond_VibeCoding_Spec-Driven_AI-Assisted_Development.pdf](lectures/Beyond_VibeCoding_Spec-Driven_AI-Assisted_Development.pdf) | The full slide deck (current version) |
| Lecture notes | [lectures/notes.md](lectures/notes.md) | The key ideas of each part, in text |
| Videos | [videos.md](videos.md) | Lecture recording and the four demo clips |
| Demos | [demos.md](demos.md) | What each demo shows, what to watch for, and the files involved |
| Template | [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template) | The harness you build your own MVP with |

---

## The lecture in six parts

| Part | Topic | Demo |
| --- | --- | --- |
| 01 | From vibe coding to spec-driven development: why a plausible app is not a finished app, and why specs address it | Clip: vibe coding StudyMatch |
| 02 | Two loops: the product loop and the execution loop | – |
| 03 | Harness and engineering control: rules, permissions and checks in the repo | – |
| 04 | The product loop: from requirements and evidence to Ready specs and user stories | Demo 1 · Demo 2 |
| 05 | The execution loop: building the MVP, from a Ready story to an accepted increment | Demo 3 · Demo 4 |
| 06 | The MVP playbook: seven steps and the harness you take home, with Lovable as an option | – |

The [lecture recording](videos.md), the [slides](lectures/Beyond_VibeCoding_Spec-Driven_AI-Assisted_Development.pdf) and the [lecture notes](lectures/notes.md) all follow the current version of the lecture.

---

## The MVP playbook in seven steps

One story at a time, through both loops. The same seven steps apply in both tracks; only the tools in the execution loop differ (see the [Lovable track](https://pcehvl.github.io/MF_Spec-Driven_AI-Assisted-SE-Lovable/)).

Run every step from **Build**, OpenCode's main agent. It hands each step to the agent that owns it (see the [Setup Guide](setup/SETUP.md#7-opencode-meet-the-agents), §7).

**Product loop – the Product Assistant drafts, you decide as Product Owner**

1. **Collect** – set up, then add your design-thinking evidence about users and the problem to `docs/design-thinking/`.
2. **Frame** – the Product Assistant drafts `product/product.md`: who it is for, which problem, which outcome. You decide as PO.
3. **Story** – the Product Assistant writes one `spec.md` per story, with testable acceptance criteria (`ACn`).
4. **Ready** – refine one question at a time until the story is Ready.

**Execution loop – the Planner and Coder plan and build, you control as System Engineer**

5. **Plan** – the Planner writes `plan.md`; you approve it as engineer.
6. **Build and verify** – the Coder builds the smallest slice on a feature branch, checks every `ACn` with `npm run verify` and opens the Pull Request.
7. **Accept and learn** – accept PASS or FAIL per `ACn` as PO, merge and deploy as engineer, test with users. Feedback becomes new or revised stories.

> Generated code alone is never Done. The PO accepts, the System Engineer merges.

---

## Your take-home harness

One repo, both loops in OpenCode. The app code and its checks live next to the specs. Start from [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template).

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

| Where | Loop | What it holds |
| --- | --- | --- |
| `product/` | Product | `product.md` holds the framing. One folder per story is the backlog: no `backlog.md`, no Issues |
| `.opencode/` | Both | Product Assistant, Planner and Coder, with six skills |
| `stories/<id>/plan.md` · `verification.md` | Execution | The Planner's plan you approve as engineer, and the Coder's evidence you read before you accept as PO |
| `src/` · `e2e/` · `supabase/` · `.github/` | Build and checks | `npm run verify`: lint, types, tests, build and E2E, before the PR and again before merge |

---

## Getting help

- Check **Troubleshooting** at the end of the [Setup Guide](setup/SETUP.md#12-troubleshooting) first.
- Still stuck? Post the exact command, the full error text and your operating system in the course channel. *[Lecturer: add the link to the course channel / LMS page.]*
