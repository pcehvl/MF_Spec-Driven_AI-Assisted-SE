# Setup Guide – MF-MyMVP-Template

[← Back to course home](../README.md)

Everything you need to install and configure before you build your MVP with the MyMVP agentic harness.

**Contents:** [1. Before you start](#1-before-you-start) · [2. Software](#2-the-software-at-a-glance) · [3. Accounts](#3-create-your-accounts) · [4. Install](#4-install-the-software) · [5. Git and GitHub](#5-git-and-github-your-own-copy-of-mymvp) · [6. Project setup](#6-install-the-project-and-run-the-checks) · [7. OpenCode](#7-opencode-meet-the-agents) · [8. Supabase](#8-supabase-database-login-and-migrations) · [9. Vercel](#9-vercel-preview-and-production) · [10. Checklist](#10-ready-checklist) · [11. Security](#11-security-rules) · [12. Troubleshooting](#12-troubleshooting)

## 1. Before you start

Plan about 60–90 minutes to install everything and confirm that `npm run verify` passes on your own copy of MyMVP. Do steps 1–7 now. Do Supabase (step 8) when your first story needs data, and Vercel (step 9) before your first user test.

MyMVP is a **harness**, not a product. It gives you three AI agents (Product Assistant, Planner, Coder), six skills, project rules in `AGENTS.md`, and a ready React + Supabase app shell. You act as Product Owner and engineer: the agents draft, you decide.

**What you need**

- A laptop with macOS 13+, Windows 10/11 or a recent Linux, at least 8 GB RAM and 5 GB free disk.
- Admin rights to install software.
- A terminal you are comfortable in (Terminal on macOS, PowerShell or WSL on Windows).
- You do **not** need Docker. The harness never runs Supabase locally.

**The order of work**

1. Create accounts
2. Install the software
3. Connect OpenCode to a free model, so the agent can help you with the rest
4. Create your own repo from the template and clone it
5. Install project dependencies and run the checks
6. Meet the agents in your repo
7. Connect Supabase (when needed)
8. Connect Vercel (when needed)
9. Tick off the ready checklist

## 2. The software at a glance

Nine tools cover the whole loop; seven are needed on day one.

| Tool | Version | What it is for in MyMVP | When |
| --- | --- | --- | --- |
| Git | 2.40+ | Branches and commits made by the Coder | Day 1 |
| GitHub CLI (`gh`) | 2.x | The Coder opens Pull Requests with `gh pr create` | Day 1 |
| Node.js (includes npm) | 24 LTS | Installs packages and runs `npm run dev` / `npm run verify` | Day 1 |
| OpenCode | 2.x | Runs the Product Assistant, Planner and Coder agents | Day 1 |
| A language model provider | – | The model behind the agents (a free model in OpenCode, or your own provider) | Day 1 |
| VS Code (or another editor) | latest | Reading specs, plans and code; reviewing diffs | Day 1 |
| Playwright Chromium | via npm | Browser for the end-to-end tests in `npm run verify` | Day 1 |
| Supabase account + CLI | latest | Database, Auth, Row Level Security, migrations | First story that stores data |
| Vercel account + CLI | latest | Preview and production hosting of the frontend | First user test |

All other dependencies (React, Vite, Tailwind, shadcn/ui, Vitest, Playwright) are installed by `npm ci` from the repo. Do not install them globally.

## 3. Create your accounts

Create four accounts, all with the same email. Sign up for Supabase and Vercel with **Continue with GitHub** so everything is linked.

| Account | Where | Cost | Notes |
| --- | --- | --- | --- |
| GitHub | [github.com/signup](https://github.com/signup) | Free | Apply for the [GitHub Student Developer Pack](https://education.github.com/pack) with your student email; it includes GitHub Copilot. |
| OpenCode | [opencode.ai](https://opencode.ai) | Free with free models | Sign in when you run `/connect` in OpenCode. Paid models are optional and at your own cost. See below. |
| Supabase | [supabase.com](https://supabase.com) | Free plan | Free projects pause after a week without activity; resume them in the dashboard. |
| Vercel | [vercel.com/signup](https://vercel.com/signup) | Hobby (free) | Choose the Hobby plan for personal, non-commercial projects. |

**Use a free model (recommended).** OpenCode gives you access to free models through OpenCode Zen, with no API key or payment needed. We recommend **MiMo-V2.6-Flash** or **Nemotron 3.5**. Free models may have rate limits, and their availability can change.

**Other providers (optional).** OpenCode works with many providers if you would rather use your own: **GitHub Copilot** (free through the Student Developer Pack), **Anthropic (Claude) or OpenAI** (an existing plan or an API key), or your own **OpenCode Zen / Go** account. These are at your own cost. Set a spending limit on any API key you create.

## 4. Install the software

Follow the block for your operating system, then close and reopen the terminal so new commands are found.

> **MyMVP needs OpenCode 2.** The agent files use the OpenCode 2 permission format. If you have OpenCode 1 from npm, Homebrew or Chocolatey, uninstall it first.

### macOS

```bash
# 1. Apple command line tools (gives you git)
xcode-select --install

# 2. Homebrew (package manager) – see https://brew.sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 3. Tools
brew install git gh
brew install supabase/tap/supabase
brew install --cask visual-studio-code

# 4. Node.js 24 LTS: use the macOS installer from https://nodejs.org (LTS)

# 5. OpenCode 2
curl -fsSL https://opencode.ai/v2/install | bash

# 6. Vercel CLI
npm install -g vercel
```

### Windows (recommended: WSL 2)

OpenCode recommends WSL on Windows, and the harness's shell commands assume a Unix shell. You work inside Ubuntu and edit with VS Code on Windows.

1. Open **PowerShell as Administrator** and run `wsl --install -d Ubuntu`. Restart, open **Ubuntu** from the Start menu, and create a Linux username and password.
2. Install [VS Code for Windows](https://code.visualstudio.com) and the **WSL** extension.
3. Inside the Ubuntu terminal, follow the Linux block below.
4. Keep your code in the Linux file system (for example `~/code`), not under `/mnt/c`. It is much faster.
5. Open a project in VS Code from Ubuntu with `code .`

Without WSL: install Git, GitHub CLI, Node.js LTS and VS Code with `winget install Git.Git GitHub.cli OpenJS.NodeJS.LTS Microsoft.VisualStudioCode`, the OpenCode 2 Windows app from [opencode.ai/download](https://opencode.ai/download), and Supabase CLI with Scoop (`scoop bucket add supabase https://github.com/supabase/scoop-bucket.git` then `scoop install supabase`). Use Git Bash as your terminal.

### Linux and WSL (Ubuntu/Debian)

```bash
# 1. Basics + git + GitHub CLI
sudo apt update && sudo apt install -y git curl unzip gh

# 2. Node.js 24 LTS via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm install 24

# 3. OpenCode 2
curl -fsSL https://opencode.ai/v2/install | bash

# 4. Vercel CLI
npm install -g vercel

# 5. Supabase CLI: download the .deb for your CPU from
#    https://github.com/supabase/cli/releases, then:
sudo apt install ./supabase_*_linux_amd64.deb
```

If `apt install gh` is not found or very old, use the official instructions at [cli.github.com](https://cli.github.com). Supabase CLI cannot be installed with `npm install -g`; if a global install is hard, `npx supabase <command>` works too.

### Check the installation

```bash
git --version        # 2.40 or newer
gh --version         # 2.x
node -v              # v24.x
npm -v
opencode --version   # 2.x
supabase --version
vercel --version
```

### Connect OpenCode now

Connect OpenCode before the next steps, so the agent can help you if something goes wrong.

```bash
mkdir -p ~/code && cd ~/code
opencode
```

1. Type `/connect`, choose **OpenCode**, and sign in in the browser window that opens. (Using your own provider instead? Choose it and log in or paste your API key.)
2. Type `/models` and pick **MiMo-V2.6-Flash** or **Nemotron 3.5** (both are marked Free).
3. Check that **Build** is the active agent (shown at the bottom of the screen). Press **Shift+Tab** if it is not.

**Stuck? Ask Build.** When a command fails in a later step, paste this into OpenCode with your details filled in:

```text
I am setting up the MyMVP course project on <macOS / Windows WSL / Linux>.
I ran: <the exact command>
I got: <the full error text>
Explain the likely cause, then propose a fix. Do not run anything until I approve.
```

Rules while the agent helps you:

- Read every command before you approve it. Ask "why?" if you don't understand it.
- Setup fixes change your machine, never the project. If the agent wants to edit project files or tests to make a check pass, say no and email [pcen@hvl.no](mailto:pcen@hvl.no).
- Never approve `sudo`, deleting files or reading `.env` unless you understand why it is needed.
- If OpenCode itself does not work (install, `/connect`, model limits), use [Troubleshooting](#12-troubleshooting).

## 5. Git and GitHub: your own copy of MyMVP

Each of you gets your own repository made from the MyMVP template. That gives you your own `main` branch, feature branches and Pull Requests, which is what the Coder agent works with.

**Step 1 – Tell Git who you are (once per machine)**

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"   # same email as GitHub
git config --global init.defaultBranch main
```

**Step 2 – Log in with the GitHub CLI**

```bash
gh auth login        # GitHub.com → HTTPS → Login with a web browser
gh auth setup-git    # lets git push use the same login
gh auth status       # should say "Logged in to github.com"
```

**Step 3 – Create your repo from the template**

1. Open [github.com/pcehvl/MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template).
2. Click **Use this template → Create a new repository**. Leave **Include all branches** unticked.
3. Owner: your account. Name: your product, for example `studymatch`. Visibility: Private or Public, your choice.
4. Click **Create repository**.

Or in one command: `gh repo create my-mvp --template pcehvl/MF-MyMVP-Template --private --clone`

**Step 4 – Clone it to your machine**

```bash
mkdir -p ~/code && cd ~/code
gh repo clone <your-username>/<your-repo>
cd <your-repo>
code .               # opens VS Code
```

**How Git is used in the harness**

- `main` is the integration branch. Nobody, human or agent, builds features directly on it.
- The Coder creates one branch per story: `feature/US-001-<slug>` or `fix/US-001-<slug>`.
- The Coder opens one Pull Request per story, titled `US-001: <Title>`, and then stops.
- You review the diff and the evidence in `verification.md`. You merge. The agent asks before `git push` and before `gh pr create`.
- GitHub Issues, Projects and Actions are not used. The backlog lives in `product/stories/`.

Your new repo starts with one commit, an empty backlog in `product/stories/` and a blank `product/product.md` for you to frame with the Product Assistant.

## 6. Install the project and run the checks

The goal of this step is a green `npm run verify` on your own copy, before any agent touches the code. Run all commands from the repo folder.

```bash
# 1. Exact dependencies from package-lock.json
npm ci

# 2. Browser for end-to-end tests
npx playwright install chromium
#    Linux/WSL: npx playwright install --with-deps chromium

# 3. Local environment file (git-ignored)
cp .env.example .env

# 4. Start the app – open the URL it prints (usually http://localhost:5173)
npm run dev          # stop with Ctrl+C

# 5. Run the full quality gate
npm run verify
```

`npm run verify` runs five checks in order and stops at the first failure:

1. `lint` – ESLint code style
2. `typecheck` – TypeScript
3. `test --run` – Vitest unit and component tests
4. `build` – production build with Vite
5. `test:e2e` – Playwright end-to-end tests in a real browser

This is the same gate the Coder must pass before it opens a Pull Request. If a check fails on a fresh clone, fix the setup first: ask Build with the prompt in [§4](#connect-opencode-now), or see [Troubleshooting](#12-troubleshooting). Setup fixes change your machine, never the project: never let the agent weaken a test or edit project files to get a green result.

Leave the two values in `.env` empty for now. You fill them in step 8.

## 7. OpenCode: meet the agents

Now that `npm run verify` is green, quit OpenCode and start it again from the repo folder. Inside the repo it also loads the harness: `AGENTS.md` and the three role agents in `.opencode/`.

```bash
cd ~/code/<your-repo>
opencode
```

1. Make sure **Build** is the active agent (shown at the bottom of the screen).

> **Always work in Build.** Product Assistant, Planner and Coder are subagents with narrow permissions. If you select one of them directly, it cannot do everything a step needs. Ask Build, and it hands the work to the right agent, for example: *"Use the Product Assistant to draft `product/product.md`."*

**Switching agents (OpenCode 2 defaults; see [Keybinds](https://opencode.ai/v2/docs/cli/keybinds) if your version differs)**

| Action | Keys |
| --- | --- |
| Next agent | **Shift+Tab** |
| Agent picker | **Ctrl+X**, then **A** |
| Open a subagent's session | **↓** |
| Previous / next subagent session | **←** / **→** |
| Back to the main (Build) session | **↑** |

| Agent | Role | Writes | Build uses it for |
| --- | --- | --- | --- |
| Build | Main agent, where you work | via the subagents | Every step |
| Product Assistant | WHY / WHAT | `product/product.md`, `spec.md` | Framing, user stories, acceptance criteria, feedback |
| Planner | HOW | `plan.md` | A technical plan for one Ready story |
| Coder | BUILD / VERIFY | code, tests, `verification.md` | Branch, implement, `npm run verify`, Pull Request |

**First session (Playbook step 1–2).** Put your design-thinking material (interview notes, personas, problem statement) in `docs/design-thinking/`. In Build, ask it to use the Product Assistant to draft `product/product.md` from that material. Answer its questions one at a time. You decide; it drafts.

**Approvals.** Each subagent has its own permissions; the Coder asks before `git push` and `gh pr create`. Read every request before you approve. Approving is your job as engineer.

## 8. Supabase: database, login and migrations

Do this when your first story needs to store data or sign users in. Supabase runs in the cloud; you do not run it locally.

**Step 1 – Create a project**

1. In the [Supabase dashboard](https://supabase.com/dashboard), click **New project**.
2. Name it after your MVP, choose a region in Europe (for example Stockholm or Frankfurt), and generate a database password. Save the password in a password manager.
3. Wait until the project is ready (1–2 minutes).

**Step 2 – Put the public keys in `.env`**

Click **Connect** (or Project Settings → API Keys) and copy the Project URL and the publishable key (called *anon* key in older projects):

```bash
VITE_SUPABASE_URL=https://<project-ref>.supabase.co
VITE_SUPABASE_ANON_KEY=<publishable or anon key>
```

Never put the **service_role** / secret key in `.env` or any `VITE_` variable. It bypasses all security rules.

The publishable key is safe in the browser **only because Row Level Security (RLS) protects your tables**. A table without RLS enabled and policies in place is open to anyone who has the key.

**Step 3 – Link the repo to the project**

```bash
supabase login                               # opens the browser
supabase link --project-ref <project-ref>    # the ref is in the project URL
```

**Step 4 – Apply migrations**

The Coder writes database changes as SQL files in `supabase/migrations/`. They reach the database only when you push them:

```bash
supabase db push      # applies new migrations in filename order
```

Check the result under **Table Editor** and **Authentication → Policies** (RLS) in the dashboard. Pushing to the database is your decision, never the agent's.

**Step 5 – Auth settings (if your MVP has login)**

Under **Authentication → URL Configuration**, set Site URL to `http://localhost:5173` while developing, and add your Vercel URL to Redirect URLs after step 9.

## 9. Vercel: preview and production

Do this before your first user test, so users and the Product Owner can try a real URL. In this harness you deploy by hand from a verified commit; automatic Git deploys stay off.

**Step 1 – Log in and link (once)**

```bash
vercel login
vercel link          # create a new project; accept the detected Vite settings
```

Check the project settings in the Vercel dashboard:

| Setting | Value |
| --- | --- |
| Framework preset | Vite |
| Build command | `npm run build` |
| Output directory | `dist` |
| Node.js version | 24.x |

**Step 2 – Add the environment variables**

Add the same two values as in your `.env`, for Production and Preview:

```bash
vercel env add VITE_SUPABASE_URL
vercel env add VITE_SUPABASE_ANON_KEY
```

**Step 3 – Deploy**

```bash
npm run verify       # always from the exact commit you deploy
vercel               # Preview URL – for the PO to try a Pull Request
vercel --prod        # Production – only after the PR is accepted and merged
```

**Step 4 – Check the live site.** Open the URL, run the critical user journey by hand, and add the URL to Supabase Redirect URLs if you use login. Roll back with `vercel rollback` or by deploying the previous good commit.

## 10. Ready checklist

You are ready to build when every box on day 1 is ticked.

**Day 1**

- [ ] GitHub account (and Student Developer Pack applied for)
- [ ] `git`, `gh`, `node` 24, `opencode` 2 and VS Code installed and showing versions
- [ ] `gh auth status` says you are logged in
- [ ] Your own repo created from the template and cloned
- [ ] `npm ci` and `npx playwright install chromium` completed
- [ ] `npm run dev` shows the app in the browser
- [ ] `npm run verify` passes
- [ ] OpenCode connected with `/connect`, **MiMo-V2.6-Flash** or **Nemotron 3.5** selected in `/models`, and you can see Product Assistant, Planner and Coder
- [ ] Build is the active agent, and you can switch agents with **Shift+Tab** or **Ctrl+X** then **A**

**Before the first data story**

- [ ] Supabase project created, URL and publishable key in `.env`
- [ ] `supabase link` done

**Before the first user test**

- [ ] Vercel project linked, environment variables set
- [ ] Preview URL opens and the critical journey works

## 11. Security rules

- `.env` is git-ignored. Never commit it, and never paste keys into a chat with an agent.
- Only public values go in `VITE_` variables: the Supabase URL and the publishable/anon key. Everything in `VITE_` ends up in the browser.
- Never use the Supabase **service_role** or secret key in the app.
- Protect user data with Row Level Security (RLS) policies, and check them in the dashboard.
- Set a spending limit on any paid model API key.
- Read what an agent asks permission for before you approve it. Agents ask before `git push`, PR commands, destructive git commands and reading `.env`.

## 12. Troubleshooting

Ask Build first, with the prompt in [§4](#connect-opencode-now). Use the table below when OpenCode itself does not work, or when the agent's fix does not solve the problem.

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `command not found` right after installing | Terminal opened before the install | Close and reopen the terminal; on Linux/WSL run `source ~/.bashrc` |
| `node -v` shows 18, 20 or 22 | Old Node.js on the PATH | `nvm install 24 && nvm alias default 24`, or reinstall the LTS from nodejs.org |
| `npm ci` fails with a lockfile error | Repo cloned without `package-lock.json`, or npm too old | Pull the latest template; update npm with `npm install -g npm` |
| E2E tests fail with "Executable doesn't exist" | Playwright browser missing | `npx playwright install chromium` (Linux/WSL: add `--with-deps`) |
| Port 5173 already in use | Another dev server is running | Stop it with Ctrl+C, or close the other terminal |
| Agents missing in OpenCode, or permission errors on start | OpenCode 1 installed, or started outside the repo | Check `opencode --version` is 2.x; run `opencode` from the repo folder |
| An agent says it may not edit a file or run a command | You selected Product Assistant, Planner or Coder directly instead of Build | Press **Shift+Tab** (or **Ctrl+X** then **A**) to switch back to Build, and ask Build to use that agent |
| Model requests stop with a rate-limit error | The free model is busy or limited | Wait a few minutes, or switch between **MiMo-V2.6-Flash** and **Nemotron 3.5** in `/models` |
| Coder says it cannot create the Pull Request | `gh` not logged in | `gh auth login`, then `gh auth setup-git` |
| `git push` asks for a password | Git not using the GitHub CLI login | `gh auth setup-git` |
| App shows no data or a Supabase error | `.env` empty or wrong, or dev server not restarted | Check the two `VITE_` values, then restart `npm run dev` |
| `supabase db push` fails to connect | Project paused, or wrong database password | Resume the project in the dashboard; re-run `supabase link` |
| Vercel page is blank or a 404 on reload | Missing environment variables or SPA routing | Add both `VITE_` variables in Vercel and redeploy; ask the Planner about a `vercel.json` rewrite |
| Very slow installs on Windows | Repo stored under `/mnt/c` in WSL | Move the repo to `~/code` inside Ubuntu |

Still stuck? Email Per Christian Engdal at [pcen@hvl.no](mailto:pcen@hvl.no) with the exact command, the full error text and your operating system.

## Sources

- [OpenCode 2 documentation](https://opencode.ai/v2/docs) and [permissions](https://opencode.ai/v2/docs/permissions)
- [OpenCode: migrate from V1](https://opencode.ai/v2/docs/migrate-v1/)
- [OpenCode Console: workspaces](https://opencode.ai/v2/docs/console/workspaces/) and [members](https://opencode.ai/v2/docs/console/members/)
- [Supabase CLI: getting started](https://supabase.com/docs/guides/local-development/cli/getting-started)
- `README.md`, `AGENTS.md`, `docs/deployment.md` and `.opencode/agents/` in the [MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template) repository
