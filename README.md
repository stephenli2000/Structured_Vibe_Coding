**Repository:** https://github.com/aotuai/Structured_Vibe_Coding  
**License:** BSD. Use the Structured Vibe Coding CLI tools at your own risk; PRs welcome. Keep changes tiny, logs clear, and defaults sensible.

![Claude, Gemini, ChatGPT, Codex, Cursor](svc.png)

Author's experience:
> Want to leverage frontier models like Claude Fable 5 without skyrocketing your AI bill?
>
> Using our open-source **Structured Vibe Coding CLI tool and methodology**, we generated 90% of the production code for BrainFrame and VisionCapsules Applications using Claude, Gemini, and ChatGPT.
>
> Over the past 1.5 years, we’ve tackled everything from quick fixes to massive architectural tasks—all on a lean monthly budget of **$20–$200** per developer.
>
> We achieved this without relying on Codex, Cursor, or heavy autonomous agents. Strip away the marketing, and the underlying theory is the same. Whether you call it a skill, a harness, or just good software engineering practice, it all comes down to treating AI as another software engineer. Our tool is designed for veteran software and algorithm engineers with deep experience in Python, web development, C/C++, Linux, and Git who want tight control over their LLMs and budgets.
>
> Open sourced here: https://github.com/aotuai/Structured_Vibe_Coding

- [1. Philosophy: Structured Vibe Coding](#1-philosophy-structured-vibe-coding)
  - [1.1. Who is this for?](#11-who-is-this-for)
- [2. The Structured Blueprint Method](#2-the-structured-blueprint-method)
  - [2.1. Prerequisites](#21-prerequisites)
  - [2.2. Align Requirements Between AI and Human](#22-align-requirements-between-ai-and-human)
    - [2.2.1. Prompt: Reflect the Human-Defined Change Goal and Scope](#221-prompt-reflect-the-human-defined-change-goal-and-scope)
  - [2.3. Co-Design with AI](#23-co-design-with-ai)
    - [2.3.1. Prompt: Generate an AI-Facing Coding Plan and Update Human-Readable Design Documentation](#231-prompt-generate-an-ai-facing-coding-plan-and-update-human-readable-design-documentation)
  - [2.4. Code and Test with Human Verification](#24-code-and-test-with-human-verification)
    - [2.4.1. Prompt: Implement with AI for Human Audit](#241-prompt-implement-with-ai-for-human-audit)
    - [2.4.2. Prompt: Test with Human Verification](#242-prompt-test-with-human-verification)
  - [2.5. Prepare for Regression Testing](#25-prepare-for-regression-testing)
    - [2.5.1. Prompt: Capture Regression Context for AI Audit and Human Verification](#251-prompt-capture-regression-context-for-ai-audit-and-human-verification)
- [3. Vibe Coding Tools](#3-vibe-coding-tools)
  - [3.1. Quick Start](#31-quick-start)
  - [3.2. `concatenate_text_files.py`](#32-concatenate_text_filespy)
  - [3.3. `concatenate_python_files.py`](#33-concatenate_python_filespy)
  - [3.4. `save_commits.py`](#34-save_commitspy)
  - [3.5. `analyze_folder.py`](#35-analyze_folderpy)
- [4. Workflow Examples](#4-workflow-examples)
  - [4.1. Whole-project review](#41-whole-project-review)
  - [4.2. Python bug fix in a small tool](#42-python-bug-fix-in-a-small-tool)
  - [4.3. Focused PR feedback](#43-focused-pr-feedback)
  - [4.4. The Blueprint Method](#44-the-blueprint-method)
- [5. FAQ](#5-faq)

---

# 1. Philosophy: Structured Vibe Coding

Fundamentally, Agentic IDEs (like Cursor or Copilot) introduce a middleman into your workflow. They wrap the underlying foundation models in a black box, relying on hidden system prompts and automated, unpredictable context-gathering.

Structured Vibe Coding explicitly rejects the black box. It operates on two core pillars:

* **Vobe Coding Tooling**: Scripts that allow humans to share exact context with the AI, and for the AI to return complete files back to the human. So the interaction with AI is transparent to the user.
  * **Concatenate scripts:** Package source code, git history, requirements and design documents for dropping into a chat interface
  * **Code retrieval tools:** Retrieve output and dropping back to the repository (To be developed, **PRs welcome**)
* **Structured Blueprint Workflows**: A clear, phase-based framework that forces the AI to collaborate exactly how a junior and senior software engineer would interact:
  * **Requirements Clarification** 
  * **Design Constraint Confirmation** 
  * **Code & Test Verification**

| Feature | Structured Vibe Coding  | Agentic IDEs (Cursor/Copilot) |
| :--- | :--- | :--- |
| **Target User** | **Production Engineering.** CLI-comfortable devs and budget-conscious teams. | **Speed-Focused Devs.** Individuals rapidly prototyping. Output often requires a separate engineering pass for production. |
| **Context Control** | **Absolute.** You build the exact text file using the CLI tools. | **Low.** The IDE guesses what matters via RAG. |
| **Transparency** | **High.** Prompts are version-controlled docs. | **Low.** Hidden system prompts and hidden RAG. |
| **Iteration Speed** | **High Overall**. Reaches production faster by giving human developer context controls to avoid rework. | **Medium Overall**. Frictionless for micro-edits, but automated drafting often leads to context loss and rework. |
| **Cost** | **Predictable.** Fully control by human developer. | **Non Predictable.** Often exceed Monthly IDE subscription tiers. |
| **Vendor Lock-in** | **None.** Works with Claude, GPT, local models. | **High.** Tied to their specific interface/servers. |

For small features, you don't need the Structured Blueprint Method — jump straight to [Vibe Coding Tools](#3-vibe-coding-tools). This toolkit is for software engineers comfortable with the command line.

- **Chat-only workflow:** AI is changing fast — don't lock yourself in to any one model or tool.
- **Requirements, design, and coding:** Have AI confirm each stage before moving to the next.
- **Git and diff:** Communicate with the AI through git history and diffs so it can reason over the exact context you see.

For larger features, asking an AI to design architecture and write code at the same time often leads to hallucinations and context drift. Use **The Blueprint Method**: a structured three-phase methodology that shifts AI from a guessing machine to a precise execution engine.

## 1.1. Who is this for?

- **Minimize cost.** Flat-rate chat subscriptions instead of token-metered agentic tools — predictable spend for solo developers, students, contractors, and small teams.
- **Switch models freely.** The same text bundle drops into any chat — Private AI (self-hosted Llama, DeepSeek, Qwen), Claude, GPT, Gemini — so you pick the best model per task without changing tools.
- **Production Software Engineering.** The Blueprint Method (requirements → design → code, with verification gates) outlives any specific tool or model.

---

# 2. The Structured Blueprint Method

A methodology that gets explicit confirmation at each step: requirements → design → coding. Humans and AI stay aligned without rework or context drift.

## 2.1. Prerequisites

Keep design, requirement, and test documentation in Markdown, following the repository's existing locations and naming conventions.

VS Code, with a few extensions, gives you a Word/Google Docs-like editing experience for Markdown:

- Export Google Docs to HTML ZIP, then convert the HTML into Markdown with Pandoc:
  ```
  pandoc GoogleDocExport.html --wrap=none --markdown-headings=atx -f html-native_divs-native_spans -t gfm-smart --extract-media=. -o CleanedDocExport.md
  ```
- Use Mermaid `sequenceDiagram` for inline interaction sequences.
- Use Mermaid `graph TD` for inline flowcharts and architecture diagrams.
- Use drawio for diagrams, then export to SVG and embed inline in your Markdown.
- Use GitHub Flavored Markdown (GFM) for tables.
- Use the **Markdown All-in-One** extension for section numbering and the Table of Contents.

## 2.2. Align Requirements Between AI and Human

Feed the AI your change request and ask it to audit for edge cases before any code is written.

### 2.2.1. Prompt: Reflect the Human-Defined Change Goal and Scope

1. Place the feature request alongside the project's existing requirement or design documentation. Follow the repository's conventions; if none exist, use a clearly named Markdown document kept with the other feature materials.

2. Package the related source code and document files with the Vibe Coding Tools. For example:

```bash
python3 ./Structured_Vibe_Coding/concatenate_text_files.py my_app/ --recursive
```
This generates `my_app_concat.txt`.

3. Attach `my_app_concat.txt` to the AI chat along with the following prompt:

> Please review the feature request document.
> 
> Audit the feature requests. Stop and ask for clarification if you find any logical gaps, unhandled edge cases, contradictions, or missing data dependencies.
> ### Rules of Engagement
> **The Ground Truth Principle:** Treat the current codebase as the source of truth for existing behavior and implementation. Treat the approved feature request as the source of truth for intended user-visible behavior. Design documentation represents historical intent. If these sources conflict, identify the conflict and ask for clarification instead of silently choosing one.
> 
> **No Code Yet:** Wait for my approval on your audit and answers to any clarifying questions before writing any code or architectural designs. When asking for clarification, please use entirely user-verifiable language—try to avoid referring to underlying codebase structures or architectural terminology.
> 
> ### Update Instructions (Apply only if there are no pending clarifications)
> If there are no pending clarifications, update the provided feature requests, incorporating the following instructions:
> 
> - **Human-Readable Feature Request:** Organize the updated feature request into two levels:
>    - Begin with a very short **UX Goal** that a human can understand in 30–60 seconds. State who needs the feature, what they can accomplish, and the intended user-visible outcome. Do not include corner cases in this short section.
>    - Follow it with **Detailed Feature Requirements** covering all user-visible behavior and UX corner cases established in this thread.
>    - Use entirely user-verifiable language. Avoid code, component names, storage details, architecture, and other implementation terminology.
>    - Give each detailed requirement a stable identifier so design, implementation, tests, and future audits can refer to it.
> - **Incremental Scoping:** Break the feature down into smaller, user-verifiable sub-features that build upon one another. Do not create new requirements for unchanged behavior. Retain references to existing behavior when needed to define user expectations, dependencies, constraints, or regression coverage.
>    - For each proposed sub-feature, justify your choice: Is it genuinely easy to communicate with a human user, and is a wrong assumption quick to fix? If yes, keep it as a combined feature; if no, break it down into sub-features.
> - **Atomicity & Feedback Loops:** When deciding whether to break a feature down further or simply make an assumption, justify your choice using three rules:
>    - *Cost/Token Efficiency:* Would a breakdown save time and token spend overall?
>    - *Correction Ease:* Is the assumption easy to communicate with a human user and quick to fix if wrong?
>    - *Fewer iterations:* Does it require more iterations or fewer iterations? More iterations will cost more human time and often cost more tokens.
> - **Acceptance Criteria:** Define the feature requests using a strict **Given-When-Then** format. Use entirely user-verifiable language—do not refer to underlying codebase structures or architectural terminology.

The initial feature request can be drafted in any format by the user.

## 2.3. Co-Design with AI

Have the AI map out *how* the change request integrates into the codebase by updating the project's existing design documentation and diagrams. You may skip this step for small features.

### 2.3.1. Prompt: Generate an AI-Facing Coding Plan and Update Human-Readable Design Documentation

> Based on the approved feature request and the codebase context, generate or update the project's human-readable design documentation and generate a strict step-by-step coding plan for AI execution. Follow the repository's existing locations and naming conventions; if none exist, choose clear Markdown documents kept with the feature materials. Do not create implementation steps for unchanged behavior. Retain existing implementation details when they are necessary to explain dependencies, invariants, compatibility constraints, or regression risks. Wait for my approval.
>
> The Ground Truth Principle: Treat the current codebase as the source of truth for existing behavior and implementation. Treat the approved feature request as the source of truth for intended user-visible behavior. Design documentation represents historical intent. If these sources conflict, identify the conflict and ask for clarification instead of silently choosing one.
>
> When updating the design documents, edit only what this change affects. Capture architecture changes, API changes, and external constraints — not low-level implementation details, which live in the code.
>
> When architecture diagrams are provided in an editable format, update them without removing the source metadata needed for future human editing.
>
> If the design introduces user-facing impacts that affect the approved feature request, ask for my confirmation.

## 2.4. Code and Test with Human Verification

Feed the AI the generated coding plan and require it to work incrementally.

### 2.4.1. Prompt: Implement with AI for Human Audit

> Execute step by step. Stop and wait for me to verify the tests pass before moving to the next step.
> 
> Write complete, production-ready code with no placeholders. Coding Constraints:
> - Generate the whole file: Always output the complete file content in a zip file if you can, so I can download, unzip and replace my local copy directly.
> - Minimal changes only: Do the absolute minimum required to accomplish the task. Do not expand the scope.
> - Zero cosmetic changes: I manually review every line using diff tools. Do not reformat existing code, change indentation, merge/split lines, or modify/add/remove comments. Leave the surrounding code exactly as you found it to keep the diff clean.
> - Please provide commit text along with your results.
> 
> During implementation, continuously clean up everything introduced for the feature. Before considering the feature complete, remove temporary debugging code and logs, dead code, unused flags or dependencies, obsolete test data, commented-out experiments, superseded requirements, and duplicate documentation. Preserve intentional production diagnostics, compatibility code, and migration logic. Run the relevant tests again after cleanup.
>
> If you believe the change requires an unplanned broader architectural change, ask for my approval first.

### 2.4.2. Prompt: Test with Human Verification

> Here are the test results.
>
> - Crashing? Yes/No
> - Input:
> - The output is incorrect? Yes/No
> - Actual output:
> - Logs attached:
> - Expected output:
> 
> Please review the logic. Add logging if you need me to trace the execution steps.
>

## 2.5. Prepare for Regression Testing

After the change is verified, preserve the engineering details and test coverage needed for future AI-assisted code audits and regression testing. Keep this technical context separate from the human-readable feature request and human verification checklist.

### 2.5.1. Prompt: Capture Regression Context for AI Audit and Human Verification

> Create or update these two standalone documents for this feature:
>
> - **AI Audit and Regression Context**
> - **Human Verification Checklist**
>
> Follow the repository's existing documentation locations and naming conventions. If equivalent feature-specific documents already exist, update them instead of creating duplicates. If no convention exists, choose clear names, keep both documents with the feature materials, and use stable cross-references so future audits can resolve them.
>
> Produce each document separately using its corresponding section below. Do not merge the two documents.
>
> Treat the current codebase and tests as the source of truth for existing behavior and implementation. Treat the approved feature request as the source of truth for intended user-visible behavior. Use final test results and corrections from this thread as supporting evidence. If these sources conflict, record the conflict or ask for clarification instead of silently choosing one. Do not revive rejected ideas or superseded assumptions. Do not invent behavior, tests, or passing results.
>
> #### AI Audit and Regression Context
>
> In the AI Audit and Regression Context, write for an AI auditing any future change that might affect this feature. Include the coding-level details needed to identify impact, such as affected entry points, data flow, state transitions, invariants, validation and error behavior, persistence, dependencies, compatibility constraints, and relevant edge conditions.
>
> Map the implementation and automated or reproducible non-UX tests to the stable identifiers in the approved feature request. Include exact test locations, commands, setup or test-data prerequisites, expected results, and known coverage gaps when available. Clearly distinguish verified facts from assumptions and untested risks.
>
> In the AI Audit and Regression Context, do not repeat user-visible requirements or human-only test instructions. Refer to the approved feature request for intended user-visible behavior and to the Human Verification Checklist for checks that AI cannot perform.
>
> #### Human Verification Checklist
>
> In the Human Verification Checklist, include only checks that require human perception or judgment, such as visual appearance, clarity, interaction feel, accessibility experience, or whether the workflow is understandable.
>
> Minimize human time. Use the fewest necessary scenarios and steps, combine checks into one short workflow when practical, and omit anything that automated tests or AI can verify reliably. For each check, provide only the required setup, action, immediately observable expected result, and a simple pass/fail confirmation. Put the most important and fastest check first. Do not include implementation details, logs, API checks, or lengthy background explanations.
>
> Review and update the following reusable prompt sections in this workflow document only when a lesson from this feature applies broadly to future features. Do not add feature-specific requirements or implementation details to these reusable prompts. Remove missing, outdated, contradictory, or duplicated instructions when necessary:
>
> - **Prompt: Reflect the Human-Defined Change Goal and Scope**
> - **Prompt: Generate an AI-Facing Coding Plan and Update Human-Readable Design Documentation**
> - **Prompt: Implement with AI for Human Audit**
> - **Prompt: Test with Human Verification**
>
> Keep each concern in its owning prompt instead of duplicating it here. Report any remaining coverage gap, untested risk, or required human confirmation explicitly.
>

---

# 3. Vibe Coding Tools

Keep it simple. These small scripts package the **right** text or code so you can drop a file, folder, or file type into a chat-based AI. **No lock-in** to any model or tool.

## 3.1. Quick Start

```bash
python3 -m venv venv && source venv/bin/activate
# run any tool with: python3 <tool>.py [args]
```

1. Run a tool below to generate a **single text file**.
2. Attach or paste that file into your coding chat.
3. Prompt the AI for what you want — review, refactor, tests, bug fix, or a Blueprint Method phase.

---

## 3.2. `concatenate_text_files.py`

*Snapshot all text files in a repo.*

**What:** Recursively collects text-like files (`.py`, `.md`, `.json`, etc.), skips noisy folders (`.git/`, `node_modules/`, `dist/`, etc.), and writes one bundle with a header per file.

**Why:** Share the **whole project context** in chat without zipping.

**Usage:**
```bash
python3 concatenate_text_files.py path/to/project --recursive
# → creates ./<project>.txt containing all included files with headers
```

The script supports `--code-only` and `--py-only` options to reduce the number of files included in the package.

---

## 3.3. `concatenate_python_files.py`

*Bundle a script plus its local imports.*

**What:** Traces local Python imports starting from one or more entry scripts and concatenates those files into a single text artifact.

**Why:** Give AI the **exact Python dependency closure** it needs for reasoning, without external packages.

**Usage:**
```bash
python3 concatenate_python_files.py project_root/ path/to/main.py [another.py ...]
# → writes project_root_concatenated.txt
```

---

## 3.4. `save_commits.py`

*Package the files changed in a commit or range.*

**What:** Finds files changed in one commit (or between two commits) and writes their contents to a single text file with commit headers.

**Why:** Share **focused diffs** for targeted reviews or debugging.

**Usage:**
```bash
# Single commit
python3 save_commits.py --this f9e8d7c

# Range (base..this)
python3 save_commits.py --base a1b2c3d --this f9e8d7c
# → writes <this>.txt or base-this.txt
```

---

## 3.5. `analyze_folder.py`

*Quick repo inventory.*

**What:** Scans a directory, groups files by type, totals sizes, and shows a small table (count, total size, example largest file). No bundle output — just fast insight.

**Why:** Decide **what to package** (and what to ignore) before chatting.

**Usage:**
```bash
python3 analyze_folder.py              # analyze current folder
python3 analyze_folder.py path/to/dir  # analyze a specific directory
```

---

# 4. Workflow Examples

## 4.1. Whole-project review

1. `python3 concatenate_text_files.py ./myapp` → `myapp.txt`
2. Attach `myapp.txt` in chat.
3. Ask: *"Review for structure, add tests for X, and propose a minimal refactor."*

## 4.2. Python bug fix in a small tool

1. `python3 concatenate_python_files.py ./tools ./tools/runner.py` → `tools_concatenated.txt`
2. Attach; ask: *"There's a crash when input is empty. Fix it and add doctests."*

## 4.3. Focused PR feedback

1. `python3 save_commits.py --base abc123 --this def456` → `abc123-def456.txt`
2. Attach; ask: *"Explain risk, edge cases, and missing tests in this change."*

## 4.4. The Blueprint Method

1. `python3 concatenate_text_files.py ./myapp --code-only` → `myapp_code.txt`
2. Attach `myapp_code.txt` along with your `requirement_prompt.md`.
3. Follow the prompts in Section **The Blueprint Method** to design and execute without context drift.

---

# 5. FAQ

**Why text instead of zip?** Text is immediately visible and searchable in chat, avoids unzip friction, and keeps you and the AI tightly in sync.

**Does this include third-party dependencies?** No — these tools focus on your source and local imports. Mention external packages in your prompt or attach the relevant feature request.
