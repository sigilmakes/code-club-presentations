---
title: State of AI
info: |
  AI agents in Scientific Computing

theme: seriph

themeConfig:
  primary: "#2E2D62"
  secondary: "#1E5DF8"
  accent: "#00A788"
  background: "#FFFFFF"
  backgroundDark: "#2E2D62"
  text: "#1A1A1A"
  textLight: "#1A1A1A"

colorSchema: 'light'

class: text-center
layout: cover
hideInToc: true

download: false
---

<style>
@import url('https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap');

:root {
  --slidev-theme-primary: #2E2D62;
  --slidev-theme-secondary: #1E5DF8;
  --slidev-theme-accent: #00A788;
  --slidev-theme-soft: rgba(46, 45, 98, 0.05);
  --slidev-theme-soft-2: rgba(30, 93, 248, 0.05);
  --slidev-theme-border: rgba(46, 45, 98, 0.14);
  --slidev-theme-text: #1f2340;
  --slidev-font-display: 'Instrument Serif', Georgia, serif;
  --slidev-font-body: 'IBM Plex Sans', system-ui, sans-serif;
  --slidev-font-mono: 'IBM Plex Mono', monospace;
}

html,
body,
#app {
  font-family: var(--slidev-font-body);
  color: var(--slidev-theme-text);
}

.slidev-layout {
  font-family: var(--slidev-font-body);
  color: var(--slidev-theme-text);
  background-color: #fcfcfd;
  background-image:
    radial-gradient(circle at top right, rgba(30, 93, 248, 0.07), transparent 24rem),
    radial-gradient(circle at bottom left, rgba(0, 167, 136, 0.05), transparent 22rem);
}

.slidev-layout::before {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  background-image: radial-gradient(circle, rgba(46, 45, 98, 0.045) 0.8px, transparent 0.8px);
  background-size: 26px 26px;
  opacity: 0.28;
}

.slidev-layout.cover {
  background-color: #2E2D62;
  background-image:
    radial-gradient(circle at top, rgba(0, 167, 136, 0.18), transparent 18rem),
    radial-gradient(circle at bottom right, rgba(30, 93, 248, 0.14), transparent 20rem);
  color: #FFFFFF;
}

.slidev-layout.cover::before {
  display: none;
}

h1,
.slidev-layout h1 {
  font-family: var(--slidev-font-display);
  font-weight: 400;
  color: #2E2D62;
  letter-spacing: -0.03em;
  line-height: 0.95;
}

.slidev-layout.cover h1 {
  color: #00A788;
}

h2,
h3,
.slidev-layout h2,
.slidev-layout h3 {
  font-family: var(--slidev-font-body);
  color: #2E2D62;
  letter-spacing: -0.02em;
  font-weight: 600;
}

.slidev-layout p,
.slidev-layout li,
.slidev-layout ol,
.slidev-layout ul,
.slidev-layout blockquote {
  font-family: var(--slidev-font-body);
  color: var(--slidev-theme-text);
  line-height: 1.45;
}

.slidev-layout strong {
  color: #2E2D62;
}

.slidev-layout em {
  color: #1E5DF8;
}

.slidev-layout a {
  color: #1E5DF8;
  text-decoration-color: rgba(30, 93, 248, 0.35);
}

.slidev-layout ul,
.slidev-layout ol {
  margin-top: 0.45rem;
}

.slidev-layout li {
  margin: 0.22rem 0;
}

.slidev-layout ul li::marker,
.slidev-layout ol li::marker {
  color: #00A788;
}

.slidev-layout img {
  border-radius: 18px;
  box-shadow: 0 22px 48px rgba(46, 45, 98, 0.14);
}

.slidev-layout pre,
.slidev-layout .shiki {
  border-radius: 18px;
  border: 1px solid rgba(46, 45, 98, 0.12);
  box-shadow: 0 14px 34px rgba(46, 45, 98, 0.12);
  overflow: hidden;
}

.slidev-layout code {
  font-family: var(--slidev-font-mono);
}

.slidev-layout table {
  width: 100%;
  border-collapse: collapse;
  overflow: hidden;
  border-radius: 16px;
  box-shadow: 0 16px 36px rgba(46, 45, 98, 0.08);
  background: #ffffff;
}

.slidev-layout th {
  background: #2E2D62;
  color: #FFFFFF;
  font-family: var(--slidev-font-mono);
  font-size: 0.78rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
}

.slidev-layout th,
.slidev-layout td {
  padding: 0.62rem 0.82rem;
  border: 1px solid rgba(46, 45, 98, 0.10);
}

.slidev-layout tr:nth-child(even) td {
  background: rgba(30, 93, 248, 0.03);
}

.slidev-layout.center :is(.toc-wrapper, .slidev-toc) {
  background: rgba(255, 255, 255, 0.88);
  border: 1px solid rgba(46, 45, 98, 0.12);
  border-radius: 18px;
  padding: 1.2rem 1.4rem;
  box-shadow: 0 18px 42px rgba(46, 45, 98, 0.10);
  backdrop-filter: blur(10px);
}

.pattern-split {
  display: grid;
  grid-template-columns: minmax(0, 1.05fr) minmax(0, 0.95fr);
  gap: 2rem;
  align-items: center;
  text-align: left;
  margin-top: 0.8rem;
}

.pattern-copy {
  background: linear-gradient(180deg, rgba(255,255,255,0.98), rgba(30, 93, 248, 0.025));
  border: 1px solid var(--slidev-theme-border);
  border-top: 4px solid var(--slidev-theme-accent);
  border-radius: 24px;
  padding: 1.2rem 1.25rem;
  box-shadow: 0 20px 42px rgba(46, 45, 98, 0.10);
}

.pattern-copy h2 {
  margin: 0.25rem 0 0.55rem;
  font-size: 1.35rem;
  line-height: 1.08;
}

.pattern-copy p,
.pattern-copy li {
  font-size: 0.95rem;
  line-height: 1.38;
}

.pattern-copy ul {
  margin: 0.65rem 0 0;
  padding-left: 1.1rem;
}

.pattern-kicker {
  font-size: 0.68rem;
  font-family: var(--slidev-font-mono);
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #1E5DF8;
}

.pattern-visual {
  background: rgba(255, 255, 255, 0.82);
  border: 1px solid rgba(46, 45, 98, 0.12);
  border-radius: 28px;
  padding: 1rem;
  box-shadow: 0 22px 44px rgba(46, 45, 98, 0.10);
}

.pattern-visual svg {
  width: 100%;
  height: 300px;
  display: block;
}

.pattern-visual text {
  font-family: var(--slidev-font-mono);
  font-size: 14px;
  font-weight: 600;
}

@media (max-width: 1100px) {
  .pattern-split {
    grid-template-columns: 1fr;
  }

  .pattern-visual svg {
    height: 220px;
  }
}
</style>

# State of AI

A tour of AI agents for scientific research

<div class="flex justify-center gap-8 mt-4">
  <figure class="text-center">
    <img src="./images/the_memex_cropped.jpg" class="h-48 rounded" alt="The Memex" />
  </figure>
</div>


---
layout: center
hideInToc: true
---

<Toc minDepth="1" maxDepth="1" />


---
layout: statement
---

# What is an agent?


---
hideInToc: true
---

# What is an Agent?

- Simple answer: "an LLM that can do things"
- Better answer: an LLM in a **harness**. The LLM generates commands and the harness executes them
- The harness exposes tools, which are just programs: bash, editors, search, APIs
- *Anything you can do on a computer, an agent can probably also do*

```bash
> "List the files in my folder"
[llm-with-bash]$ ls .
MyFolder/ file.txt
```


---
hideInToc: true
---

# The Loop

The agent alternates between **reasoning** and **acting**

```bash {lines:true}
> "Write a Python script that models exponential growth"
(reasoning)   "I need the formula for exponential growth."
(tool:search)  "exponential growth definition"
  [output]     Wikipedia, BBC Bitesize, etc.
(reasoning)   "I'll use the Wikipedia article."
(tool:http)    GET en.wikipedia.org/wiki/Exponential_growth
  [output]     <article_content>
(reasoning)   "I have the formula. Writing the script."
(tool:write)   Create exponential_growth.py
  [output]     File written.
(response)    "Done. Want me to run or plot it?"
```

**TL;DR: an agent is an LLM with tools in a loop**


---
layout: two-cols-header
hideInToc: true
---

# A Computer for the Computer

::left::

LLMs have limited context windows, no persistent memory. But with tools, an agent can:
  - Write ideas, plans, and learnings to files ("programs")
  - Read those "programs" later
  - Execute these "programs"
  - Save the results

<span style="color: #955438">

## An agent is *a computer for the computer*

</span>

::right::

![Harness](./images/harness.jpeg)


---
layout: statement
---

# What's the point?

---
hideInToc: true
---

# What can agents do?

An AI agent with access to your computer can:

- Understand an entire codebase and write documentation
- Design and implement new features
- Conduct literature reviews and identify new developments relevant to your research
- Profile performance and find optimisations
- Improve code quality, check for bugs, fix security vulnerabilities
- Prepare simulations, run them and then reason about the outputs


---
hideInToc: true
---

# What I've done with agents

In the past 4 months:

- Built a data archival system for cryo-EM datasets (and then ported it to Rust in an afternoon)
- Built a Kubernetes-based web app with nice UI for sandboxed agents in the cloud (in ~2 days)
- Built a recursive-language-model system based on A. Zhang's paper [(Link)](https://arxiv.org/abs/2512.24601)
- Built various CLI/GUI tools+extensions, improved all my code quality, built web apps...
- ...and much much more, with barely any manual coding.

---
hideInToc: true
---

# Agents are not magic

Agents *can* do all these things. They're not magically good at it.

- The output is only as good as what goes in
- Problems aren't solved through one-shot prompting
- You need to teach the agent about the problem
- You need to understand what the agent is doing!!
- **Treat agents as a colleague you need to teach**


---
hideInToc: true
---

# Can an agent help with my research?

## Yes

- It probably knows more than you think (e.g. DFT, common simulation methods)
- If it doesn't know something, it can search for and learn the relevant literature
- It can build itself a "memory" — documents about your specific problem
- If it still doesn't know, **you can teach it**

---
hideInToc: true
---

# Agents can help with the pain of science

- **Legacy codebases:** fix dependency hell of old projects, refactor legacy code, improve testing frameworks, debugging arcane code
- **Optimisation:** GPU porting, language porting, parallelisation etc.
- **Documentation:** automatic documentation of old projects
- **Data management:** clean out files, perform data analysis, generate plots, etc.
- **Research support:** search literature, monitor for new research, discussing ideas
- **Research:** managing slurm jobs, running simulations, proposing experiments


---
layout: statement
---

# Getting started


---
hideInToc: true
---

# How to work with agents

- Before doing any work, ask the agent to **explore your project**
- Small task? Let it run. Big task? Build a plan together. Have a dialogue.
- Review the output. If there's too much, set up a reviewer agent
- Build structure: git history, reports, kanban boards

### Don't overthink it. Just talk to it.


---
hideInToc: true
---

# Things to keep in mind

1) **Stay involved with your work.**
    - Agents can work faster than you, but they are also sloppy.

2) **Write things down**
    - The agent can only help if it can access the information it needs

3) **Focus on one thing at a time**
    - Agents can work on twenty things at once, but can you process that?

4) **Keep things simple**
    - An agent can build a GUI in minutes, great! *Do you need one?*

5) **Use agents to make you better, not to be a slop cannon**

---
layout: statement
hideInToc: true
---

# Agents remove all friction 
## You still need friction to guide you
### *You* need to be the friction

---
hideInToc: true
---

# Getting started

Various options:

- **Claude, Codex, Google Antigravity, Github Copilot, Cursor:** Frontier AI
- **Opencode, Amp, Droid:** Third-party harnesses with multi-model support.
- **Openclaw, Nemoclaw, Hermes:** Personal long-running AI agents that evolve over time
- **Pi:** Minimal but extensible harness. Also an SDK for building your own agent. We use this!


---
hideInToc: true
---

# Local models

"Local" models aren't really there yet, but they do have use:

- **Google Gemma 4 and Qwen 3.5:** Fit on phones, laptops and consumer-grade GPUs.
- **Qwen3-Coder-Next, Qwen3.5 120b, gpt-oss-120b:** Work well with larger VRAM budgets (~96GB)
- **MiniMax M2.7:** Frontier model, but you need at least 128-256GB. Best for heavy unified memory

Small models can be used for automating small tasks, for larger problems you'll need larger models. Experiment with them!

---
hideInToc: true
---

# Helpful Tools
You probably already use some of these

### Software

- **Obsidian:** Notes, kanband boards, knowledge bases etc. Works as an agent's memory
- **Linear / GitHub Issues:** Work tracking
- **Docker:** sandboxes, reproducibility, throwaway environments
- **qmd:** let agents search your documents properly
- **tmux:** interactive CLIs, debuggers, long-running agent sessions

### Other stuff

- **MCP:** connect agents to external services and APIs
- **Skills:** reusable prompts for workflows, using software etc.

### Ask the agent what tools would help! It probably knows

---
layout: statement
---

# Agentic Design Patterns

---
hideInToc: true
---

# Design Patterns
Things to think about:

- **Memory:** agents forget everything between sessions. How do we fix that?
- **Tools:** how does an agent use our software?
- **Parallelism:** how do we run multiple agents at once, and when should we?
- **Scheduling:** how do we get an agent to work continuously?
- ***Security:*** how do we control what the agent can do?

Lots of problems to solve here! But they're largely just traditional engineering.

---
hideInToc: true
---

# Memory

Agents don't remember anything between context windows.

But they can write things down, and then later retrieve that information. This is called *Agentic* RAG (Retrieval Augmented Generation).

- Just give the agent `grep`
- Use vector databases, graph databases, BM25 search etc.
- [qmd](https://github.com/tobi/qmd) is an excellent tool for letting agents search through documents which combines a lot of this

## Important principles

1. **Progressive disclosure:** don't stuff everything into one file. Organise into files, folders etc.
2. **Forgetting:** Prune and consolidate stale information
3. **Maintenance:** Remembering and forgetting needs to be an active, guided process


---
hideInToc: true
---

# Tools & Software
If this interests you, look into MCP, ACP and agent skills.

## Traditional software

- The best thing you can do: **make a CLI and document it**
- Need to talk to the web server? Document the API or make an "MCP server"
- **Prefer text-in / text-out interfaces**

If you want agents to use your software, make a CLI.

## Agentic software
Embedding an agent *inside* your software. Think beyond a chat box:

- Buttons can map to repeatable prompts.
- Agents can program UI in real time!

---
hideInToc: true
---

# Parallelism
Ahmdal's law still applies

- A single agent can do a *lot*, but there is a limit.
- Context windows aren't RAM. As context fills up, the agent's output degrades.
- Splitting tasks among agents mitigates this "context rot".

## Scaling to the task

- A single agent can handle a few thousand lines of code by itself fine
- But parallel agents can work on frontend/backend at the same time
- Agents can play different roles.


---
hideInToc: true
---

# Implementer ↔ Reviewer

<div class="pattern-split">
  <div class="pattern-copy">
    <div class="pattern-kicker">Pattern 1</div>
    <h2>Implementer ↔ Reviewer</h2>
    <p>One agent builds, another checks.</p>
    <ul>
      <li>Agents can miss things if they self-critique, external feedback is better</li>
      <li>Separation of responsibilities is key</li>
      <li>Great default pattern for real work</li>
    </ul>
  </div>

  <div class="pattern-visual">
    <svg viewBox="0 0 420 300" aria-label="Implementer reviewer loop">
      <defs>
        <marker id="pr-arrow-blue" markerWidth="10" markerHeight="10" refX="8" refY="5" orient="auto">
          <path d="M0,0 L10,5 L0,10 z" fill="#1E5DF8" />
        </marker>
        <marker id="pr-arrow-green" markerWidth="10" markerHeight="10" refX="8" refY="5" orient="auto">
          <path d="M0,0 L10,5 L0,10 z" fill="#00A788" />
        </marker>
      </defs>
      <g fill="none" stroke-linecap="round" stroke-linejoin="round">
        <path d="M95 145 C140 70, 250 70, 295 145" stroke="#1E5DF8" stroke-width="6" marker-end="url(#pr-arrow-blue)"/>
        <path d="M295 165 C250 240, 140 240, 95 165" stroke="#00A788" stroke-width="6" marker-end="url(#pr-arrow-green)"/>
      </g>
      <g fill="#ffffff" stroke="#2E2D62" stroke-width="2">
        <rect x="45" y="110" width="96" height="90" rx="24"/>
        <rect x="279" y="110" width="96" height="90" rx="24"/>
      </g>
      <g fill="#2E2D62" text-anchor="middle">
        <text x="93" y="160">Implementer</text>
        <text x="327" y="160">Reviewer</text>
      </g>
      <g fill="#1E5DF8">
        <text x="210" y="72" text-anchor="middle">work</text>
      </g>
      <g fill="#00A788">
        <text x="210" y="248" text-anchor="middle">feedback</text>
      </g>
    </svg>
  </div>
</div>

---
hideInToc: true
---

# Lead ↔ Fan-out

<div class="pattern-split">
  <div class="pattern-copy">
    <div class="pattern-kicker">Pattern 2</div>
    <h2>Lead ↔ Fan-out</h2>
    <p>A lead agent breaks the work down and delegates to workers.</p>
    <ul>
      <li>The lead owns the plan, the workers own their scope</li>
      <li>This works well for independent tasks, but is a nasty trap for tightly coupled work</li>
      <li>Works best for exploring large codebases, web browsing, porting code etc.</li>
    </ul>
  </div>

  <div class="pattern-visual">
    <svg viewBox="0 0 420 300" aria-label="Lead fan out pattern">
      <defs>
        <marker id="lf-arrow-blue" markerWidth="5" markerHeight="5" refX="4.2" refY="2.5" orient="auto">
          <path d="M0,0 L5,2.5 L0,5 z" fill="#1E5DF8" />
        </marker>
        <marker id="lf-arrow-green" markerWidth="5" markerHeight="5" refX="4.2" refY="2.5" orient="auto">
          <path d="M0,0 L5,2.5 L0,5 z" fill="#00A788" />
        </marker>
      </defs>
      <g fill="none" stroke-linecap="round" stroke-linejoin="round">
        <path d="M210 96 L104 182" stroke="#1E5DF8" stroke-width="6" marker-end="url(#lf-arrow-blue)"/>
        <path d="M210 96 L210 182" stroke="#1E5DF8" stroke-width="6" marker-end="url(#lf-arrow-blue)"/>
        <path d="M210 96 L316 182" stroke="#1E5DF8" stroke-width="6" marker-end="url(#lf-arrow-blue)"/>
        <path d="M122 176 Q160 144 194 110" stroke="#00A788" stroke-width="3.5" marker-end="url(#lf-arrow-green)" opacity="0.95"/>
        <path d="M210 176 L210 118" stroke="#00A788" stroke-width="3.5" marker-end="url(#lf-arrow-green)" opacity="0.95"/>
        <path d="M298 176 Q260 144 226 110" stroke="#00A788" stroke-width="3.5" marker-end="url(#lf-arrow-green)" opacity="0.95"/>
      </g>
      <g fill="#ffffff" stroke="#2E2D62" stroke-width="3">
        <rect x="156" y="34" width="108" height="62" rx="24"/>
        <rect x="56" y="182" width="92" height="62" rx="24"/>
        <rect x="164" y="182" width="92" height="62" rx="24"/>
        <rect x="272" y="182" width="92" height="62" rx="24"/>
      </g>
      <g fill="#2E2D62" text-anchor="middle">
        <text x="210" y="65">lead</text>
        <text x="102" y="219">worker A</text>
        <text x="210" y="219">worker B</text>
        <text x="318" y="219">worker C</text>
      </g>
    </svg>
  </div>
</div>

---
hideInToc: true
---

# Team Mesh

<div class="pattern-split">
  <div class="pattern-copy">
    <div class="pattern-kicker">Pattern 3</div>
    <h2>Team Mesh</h2>
    <p>Agents talk to each other, not just to a lead.</p>
    <ul>
      <li>Most flexible but also most chaotic</li>
      <li>Needs proper thread management</li>
      <li>Use when the problem genuinely requires collaboration, not just division of labour</li>
    </ul>
  </div>

  <div class="pattern-visual">
    <svg viewBox="0 0 420 300" aria-label="Team mesh pattern">
      <g fill="none" stroke-linecap="round" stroke-linejoin="round">
        <g stroke="#2E2D62" stroke-width="3" opacity="0.28">
          <line x1="210" y1="42" x2="319" y2="122"/>
          <line x1="319" y1="122" x2="278" y2="248"/>
          <line x1="278" y1="248" x2="142" y2="248"/>
          <line x1="142" y1="248" x2="101" y2="122"/>
          <line x1="101" y1="122" x2="210" y2="42"/>
        </g>
        <g stroke="#1E5DF8" stroke-width="6">
          <line x1="210" y1="42" x2="278" y2="248"/>
          <line x1="278" y1="248" x2="101" y2="122"/>
          <line x1="101" y1="122" x2="319" y2="122"/>
          <line x1="319" y1="122" x2="142" y2="248"/>
          <line x1="142" y1="248" x2="210" y2="42"/>
        </g>
      </g>
      <g fill="#2E2D62" stroke="#00A788" stroke-width="4">
        <circle cx="210" cy="42" r="22"/>
        <circle cx="319" cy="122" r="22"/>
        <circle cx="278" cy="248" r="22"/>
        <circle cx="142" cy="248" r="22"/>
        <circle cx="101" cy="122" r="22"/>
      </g>
      <g fill="#ffffff" text-anchor="middle" dominant-baseline="middle">
        <text x="210" y="42">A</text>
        <text x="319" y="122">B</text>
        <text x="278" y="248">C</text>
        <text x="142" y="248">D</text>
        <text x="101" y="122">E</text>
      </g>
    </svg>
  </div>
</div>

---
hideInToc: true
---

# Security and Reliability

## Headline dangers:

- Prompt injection!
- Misalignment! 
- Agent = RCE!

These are real dangers, and require using reliably safe LLMs.

## More mundane problems:

- Preventing API keys, ssh keys etc. entering context or being pushed to github 
- Preventing agents blowing up prod
- Preventing agents from running 5 compilers and melting your CPU


---
hideInToc: true
---

# Sandbox as a tool

## Bad approaches:
- Tell the agent "please do not read .env" (They will)
- Prevent the read tool from reading ssh keys (The agent will now use `cat`)

## Good approaches:
1) Put the agent in a sandbox.
    - Works, but it can still read `~/.ssh`, `.env` in the container.
    - Agent needs its own filesystem, accounts etc
2) Put the *tools* in a sandbox.
    - Agent can see your files, but can never see secrets.

## *Decouple the agent's brain from its hands*

---
layout: statement
---

# Working together


---
hideInToc: true
---

# Obvious Concerns

- **Security & data sovereignty**
- **Governance**
- **Infrastructure**
- **Reproducibility**
- **Climate Impact**

We don't have the answers to all of these! But we can try :)

---
hideInToc: true
---

# Agentic AI Interest Group

- We're launching a group for people who want to use agents seriously.
- We can discuss tools, workflows, and various ideas about what does/doesn't work.
- Bring your research problems, ideas and criticisms :D
- Possibility for periodic talks, training etc.

## Please join if you're interested!

---
hideInToc: true
---

# Other Work

- We'll be spending the next few months evaulating various different agent frameworks/tools/etc
- Will also be running workshops/seminars etc. to help people get started.

If this sounds interesting:
- Think about how you can use agents in your work, and please let us know your ideas.
- Not sure how an agent can help? Confused by agents? Get in touch!

---
hideInToc: true
---

# Thanks for listening

### Get started
- Pick one annoying task and try an agent on it! Claude Code, Codex, Opencode, or Pi are all good starting points
- Not sure where to begin? That's what the interest group is for :)

### Get in touch
- Join the **Agentic AI Interest Group** — we'll be running workshops, evaluating tools, and sharing what we learn
- Have ideas, constraints, or concerns? Bring them. Especially the concerns.

## What questions do you have?
