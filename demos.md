# Demos

[← Back to course home](README.md)

The lecture follows one case, **StudyMatch**: a simple app where students find, create and join study sessions. The data in the demos is fictional. The app is not the point; the method is.

Videos for each demo are listed in [videos.md](videos.md).

| Demo | Loop | Agent | You act as | Starts from | Ends with |
| --- | --- | --- | --- | --- | --- |
| [D1](#demo-1--from-discovery-to-product-framing) | Product | Product Assistant | Product Owner | `docs/design-thinking/` | `product.md` + backlog of `spec.md` |
| [D2](#demo-2--a-backlog-item-becomes-ready) | Product | Product Assistant | Product Owner | one `spec.md`, status backlog | the same `spec.md`, status ready |
| [D3](#demo-3--plan-build-and-verify-us-004) | Execution | Planner, Coder | System Engineer | a Ready `spec.md` | `plan.md`, branch, tests, gate result |
| [D4](#demo-4--from-feedback-to-a-merged-change) | Both | All three | PO and System Engineer | user feedback | merged PR, story done |

---

## Clip – Vibe coding StudyMatch

StudyMatch is built one prompt at a time, with no spec: sign-in, then creating sessions, then an overview of my sessions.

**Watch for:** how fast it goes, and what is never written down or tested. Every decision ends up only in the code.

---

## Demo 1 – From discovery to product framing

**Playbook steps 1–3 · Product loop · Diamond 1 (Discover, Define) into Develop**

1. **Copy** – the PO copies the design-thinking material into `docs/design-thinking/`. *Watch: the agent reads files, not chat.*
2. **Frame** – the Product Assistant reads the evidence and drafts problem, users and scope. *Watch: every line traces to a source.*
3. **Ask** – the Product Assistant asks the PO along the way, one question at a time. *Watch: answers become decisions.*
4. **product.md** – the Product Assistant writes `product/product.md`; the PO approves the framing. *Watch: evidence, assumption and decision are labelled.*
5. **Backlog** – the Product Assistant creates one folder with a `spec.md` per derived story. *Watch: `status: backlog`, traced to the framing.*

**Rule:** no source, no story.

---

## Demo 2 – A backlog item becomes Ready

**Playbook step 4 · Product loop · Diamond 2 (Develop)**

1. **Pick** – the PO picks one `spec.md` with `status: backlog` (US-004 *Edit or delete my session*).
2. **Draft** – the Product Assistant drafts observable acceptance criteria. *Watch: behaviour, not implementation.*
3. **Ask** – one open question at a time; each answer goes into `spec.md`.
4. **Check** – the five Ready checks, plus an explicit out-of-scope list.
5. **Handover** – `status: ready`. The spec goes to the Planner.

**The five Ready checks**

- [ ] `spec.md` exists, with a story ID
- [ ] Traces to evidence (design-thinking sources and the framing)
- [ ] Product intent is clear (user need and value)
- [ ] Testable acceptance criteria (observable behaviour, one per line)
- [ ] No blocking questions left

No GitHub yet: the story lives in `product/stories/` until the Planner and Coder take it.

---

## Demo 3 – Plan, build and verify US-004

**Playbook steps 5–6 · Execution loop · the Sprint**

1. **Plan** – the Planner writes `plan.md` for US-004. *Watch: the engineer checks the facts and that every acceptance criterion is mapped to a test.*
2. **Branch** – the Coder opens a feature branch. *Watch: the branch name carries the story ID.*
3. **Build** – the Coder implements the plan and writes the tests. *Watch: human prerequisites (such as a Supabase setting) stay with the engineer.*
4. **Gate** – lint, types, tests, build and Playwright run locally with `npm run verify`. *Watch: PASS or FAIL per acceptance criterion.*

**A good plan** checks facts instead of guessing, shows rejected options, decides on migrations, maps every acceptance criterion to a test, and names human checkpoints. It never changes the *what*: a gap in the spec goes back to the PO.

---

## Demo 4 – From feedback to a merged change

**Playbook step 7 → back to step 3 · both loops**

Feedback from a user test: *"Hide Sign In when I'm signed in!"*

1. **Story** – the Product Assistant classifies the feedback (usability issue, inside MVP scope) and writes US-008. The PO approves the story and its acceptance criteria.
2. **Plan** – the Planner writes the plan; the engineer approves it.
3. **Build** – the Coder branches, builds, tests and opens the Pull Request.
4. **Close** – the PO tests the Preview and accepts; the engineer merges. *Watch: `spec.md` moves to `status: done`.*

**Feedback categories:** defect · usability issue · changed requirement · new user need · feature idea · invalidated assumption · no action.
**Possible outcomes:** change an existing story · create a new story · update `product.md` · take no action.

Feedback never becomes code directly. It goes through the Product Assistant and a PO decision first.
