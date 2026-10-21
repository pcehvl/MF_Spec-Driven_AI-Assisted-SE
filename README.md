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
| Slides (PDF) | [lectures/Beyond_VibeCoding_Spec-Driven_AI-Assisted_Development.pdf](lectures/Beyond_VibeCoding_Spec-Driven_AI-Assisted_Development.pdf) | The full slide deck |
| Lecture notes | [lectures/notes.md](lectures/notes.md) | The key ideas of each part, in text |
| Videos | [videos.md](videos.md) | Lecture recording and the four demo clips |
| Demos | [demos.md](demos.md) | What each demo shows, what to watch for, and the files involved |
| Template | [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template) | The harness you build your own MVP with |

---

## The lecture in six parts

| Part | Topic | Demo |
| --- | --- | --- |
| 01 | From vibe coding to agentic development: why a plausible app is not a finished app | Clip: vibe-coded StudyMatch |
| 02 | Harness and engineering control: rules, permissions and checks in the repo | – |
| 03 | Two loops: the product loop and the execution loop | – |
| 04 | The product loop: from evidence to Ready specs and user stories | Demo 1 · Demo 2 |
| 05 | The execution loop: from a Ready story to an accepted increment | Demo 3 · Demo 4 |
| 06 | The MVP playbook: seven steps and the harness you take home | – |

---

## The MVP playbook in seven steps

One story at a time, through both loops.

Run every step from **Build**, OpenCode's main agent. It hands each step to the agent that owns it (see the [Setup Guide](setup/SETUP.md#7-opencode-meet-the-agents), §7).

**Product loop – Product Assistant drafts, you decide as Product Owner**

1. **Collect** – set up, then add your design-thinking material to `docs/design-thinking/`.
2. **Frame** – the Product Assistant drafts `product/product.md`; you decide as PO.
3. **Story** – the Product Assistant writes one `spec.md` with testable acceptance criteria.
4. **Ready** – refine one question at a time until the spec is Ready.

**Execution loop – Planner and Coder build, you control as System Engineer**

5. **Plan** – the Planner writes `plan.md`; you approve it as engineer.
6. **Build** – the Coder builds on a feature branch, runs `npm run verify` and opens the Pull Request.
7. **Accept, learn** – accept as PO, merge and deploy as engineer, test with users. Feedback becomes new or revised stories.

> Generated code alone is never Done. The PO accepts, the engineer merges.

---

## Getting help

- Check **Troubleshooting** at the end of the [Setup Guide](setup/SETUP.md#12-troubleshooting) first.
- Still stuck? Post the exact command, the full error text and your operating system in the course channel. *[Lecturer: add the link to the course channel / LMS page.]*
