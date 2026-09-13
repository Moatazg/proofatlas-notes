# ProofAtlas

Notes on <https://www.proofatlas.ai/> — what it is, what it gives you, and how you would actually use it.

**This is not our work.** ProofAtlas is a third-party platform, built and run by someone else. Nothing here is affiliated with it, and none of the mathematics it hosts is ours. This file is only a reader's guide: notes taken from the public site, written down so someone new to it can tell what it is and what to do with it.

Compiled 2026-09-13 from the public site (see [Provenance](#provenance)). All figures are as of that date and the site's counters move, so treat them as a snapshot, not a spec.

---

## What it is

ProofAtlas describes itself as "AI-first formal mathematics." It is a public, structured corpus of in-progress research on open mathematical conjectures, organized so that a person or an AI agent can pick up a specific piece of unfinished work and continue it.

The stated production model is that AI agents "work together on new mathematics, pursuing proof routes in parallel, challenging one another, and turning successful ideas into papers and checked formalizations." The site then "keeps claims, dependencies, useful failures, and open questions connected so others can continue the work." So the corpus is largely machine-generated and machine-curated; the site's own framing of the human role is inspection and continuation.

The collaboration section is in **read-only beta**: "inspect the mathematics now; task, agent, and submission controls remain visible but unavailable."

---

## What it gives you, and what it does not

This is the important distinction, and it is easy to misread from the marketing.

**It gives you the context layer.** The material and its structure: problem statements, partially developed arguments, which routes were already tried and eliminated, which obligations are still open, the definitions and setup, and the dependency graph tying them together. That is the "what to work on, and what is already known" half of the problem.

**It does not give you a way to run the work.** ProofAtlas hosts the mathematics; it does not host the machinery that does mathematics with it. In plain terms, it is a library, not a laboratory.

The missing machinery is usually called a *harness*: the setup that actually runs an AI assistant on a problem. A harness is four things — the AI model itself; the loop that lets it try something, see the result, and try again; the tools it is allowed to use (above all a proof checker, which is a program that mechanically verifies a proof is correct); and the computer those tools run on. ProofAtlas supplies none of these. It also has no way, at present, to send a finished result back.

Their own workflow line is explicit about where the boundary sits:

> "Choose a prepared task → Work it yourself or with an agent → Return what changed"

Read that middle step carefully. "Work it yourself or with an agent" means the working happens somewhere else, on equipment the reader provides. ProofAtlas supplies the problem and its history; everything that turns a problem into a result has to be brought along.

In the read-only beta the loop cannot be closed at all — the claim and submission controls are visible but disabled — so working on a task today means copying the material off the site and into a setup of one's own.

---

## Vocabulary and structure

The site's units nest like this. Getting these straight matters, because the headline number is the one most likely to be misread.

### Workspace
One conjecture or research program. 268 of them as of 2026-09-13. Examples: Tate Conjecture, Abundance Conjecture, Graph Reconstruction Conjecture, Barnette's Conjecture, Birch and Swinnerton-Dyer.

A workspace page carries: problem statement, a plain-language explainer, research status, the argument map and routes, open questions, record corrections, mathematical context and references, and the (currently disabled) contribution interface.

### Investigation line
A line of the retained research corpus. **This is a corpus-size metric — closer to "lines of code" than to "number of problems."** It is not a prompt and not a task. 770,128 total across 268 workspaces, roughly 2,900 per workspace on average; Erdős–Hajnal alone holds 24,107.

Lines are tagged by what kind of material they are:

| Category | Share |
|---|---|
| Argument development | 83% |
| Definitions and setup | 6% |
| Open obligations | 5% |
| Explored or eliminated routes | 3% |
| Computational analysis | 3% |

The 3% of eliminated routes is arguably the most valuable slice, since recorded dead ends are exactly what a fresh agent would otherwise waste its budget rediscovering.

### Route
A distinct proposed path to the result, inside one workspace. Several routes can sit on the same conjecture. Routes are drawn as a dependency graph: intermediate steps as nodes, logical prerequisites as arrows, and status badges on each node ("intermediate", "open", "in progress", "blocked"). A real example of a node and its blocker: "Frobenius scalarizes numerically" is marked intermediate and "depends on missing premise."

### Task
**This is the prompt-shaped unit** — a stated next step plus the mathematical context it inherits from its workspace. The count is not prominently advertised; a search snapshot showed 971 ready tasks against a then-smaller corpus.

A complete example, from the Tate Conjecture workspace:

> "Prove Gate I: every normalized middle block has m-squared independent numerical endomorphisms. **Suggested move:** Formalize the trace-pairing certificate and search for geometric correspondences giving matrix units, while testing every purely tensor-formal argument against arbitrary semisimple representation categories."

Another, from elsewhere on the site:

> "Formalize cell extraction and bounded local defect. Define canonical cells in a lexicographically optimal three-marked decomposition…"

Note the shape: a target, plus a suggested move, plus an explicit warning about which arguments will fail. That is a genuinely well-posed agent prompt, and it is the main thing worth taking from the site.

### Formalization
A Lean-checked statement with recorded declarations and source line counts. Example of the precision level:

> "For every N ≥ 15,552, the proportion of natural numbers n < N that do not fall below their starting value within k ≤ log n accelerated Collatz steps is at most 10,000,000 · N⁻¹ᐟ¹⁰⁰."

---

## Evidence status — how claims are labeled

Every claim and task carries metadata, and the labeling discipline is the site's strongest feature. Each item tracks:

- **Status** — active, open, in progress, blocked, stopped
- **Type** — conjecture, lemma, reduction, open obligation, explored limitation
- **Evidence posture** — peer-reviewed, source-reported, unaudited, narrowed, or failed
- **Dependencies** — e.g. "depends on missing premise", "work reported in progress"
- **Source tracking** — author, year, DOI where available

The site is also explicit about what a Lean check does *not* establish: independent replication, specialist peer review, historical priority, alignment between the prose paper and the formal statement, or that the full conjecture is solved. On the Collatz work it states plainly: "The full Collatz conjecture remains open."

Read the badge before you read the mathematics. "Accepted in ProofAtlas" is an internal status, not external peer review.

---

## The method — and how to build the same thing for your own problem

ProofAtlas publishes no methodology document. There is no `/about/` page, and the papers section explains verification for individual results but not how a workspace is assembled. What follows is read off the artifacts themselves — the same structure recurs on every workspace page examined, so it is clearly a fixed schema rather than a per-problem improvisation.

This matters beyond the site. Their 110-odd workspaces are all named open conjectures in pure mathematics; a problem outside that set gets no workspace. But the schema is not specific to conjectures, and it can be applied to any research problem by hand.

### The schema

**1. A layered problem statement.** Three registers, in order: a one-sentence question in plain language; the same thing in notation; then an exact formal statement with every variable defined and every condition explicit. Yau's Nodal-Set workspace opens with "is the hypersurface measure of its zero set bounded above by a constant times its frequency √λ?" and only then descends to the formal version. The layering is what lets a reader — or an agent — enter at the right level of precision instead of guessing it.

**2. Routes, not a proof.** The problem is not treated as one undifferentiated difficulty. It carries several independent *routes*, each a proposed path to the result, and each broken into discrete steps joined by dependency arrows. Several routes coexist on the same problem; they are not ranked.

**3. Typed steps.** Every node in a route is one of a small number of kinds: theorem candidate, reduction, open obligation, narrowed route, definition. The type tells you what kind of work the node still needs.

**4. Typed edges.** Dependencies are labeled too, not merely drawn: "supports", "depends on missing premise", "proposed". An arrow saying *depends on missing premise* is the single most useful mark in the whole system — it points at a gap and says nobody has filled it.

**5. Status separate from evidence.** Two independent axes, and conflating them is the mistake the schema exists to prevent.
   - *Status* — active, open, in progress, blocked, stopped. Where the work stands.
   - *Evidence posture* — peer-reviewed, source-reported, unaudited, narrowed, failed. What justifies believing it.

   A step can be active and unaudited at once. Source attribution (author, year, DOI) travels with the evidence axis, not the status one.

**6. Failures retained as first-class objects.** Approaches tried and eliminated are kept in the record, tagged as such, at roughly 3% of the corpus. They are not deleted when they fail. This is the part most working notes get wrong, and the part that pays for itself: without it, everyone who arrives — human or machine — re-derives the same dead end.

**7. Tasks with three parts.** Every prepared task is a target, a suggested move, and a constraint. From the Yau workspace, verbatim: target — "Prove bounded-overlap Laurent-corona selection and gauge-coherent amplitude-energy telescoping on surfaces"; move — "Audit annular screening in one fixed isothermal disk, then build divisor-weighted ball growth and prove the parent/child amplitude inequality before summing the surface decomposition." Elsewhere the third part appears as an explicit warning about which arguments will fail. A target alone is a wish; the three parts together are workable.

**8. A verification endpoint named exactly.** Where something is machine-checked, the record names the precise declaration, the audited commit, and the scope of the audit — for example that Lean "checks only the exact displayed endpoint Bondy.bondy_longest_cycle at audited source commit 9e13b044", over a "204-module dependency cone", reporting "no project mathematical axioms or sorry". Not "verified" — *this statement, at this commit, under these foundations*.

**9. A corrections log that separates presentation from substance.** Record corrections read like: "We corrected the cited passages. We removed a duplicate or outdated task or route step… The mathematical claims and their status did not change." Editorial churn is logged where it cannot be mistaken for mathematical movement.

**10. An explicit statement of what the record does not establish.** On the workspace: "The route map and retained computations do not by themselves establish this statement." Sitewide: a Lean check settles neither replication, nor peer review, nor priority, nor whether the formal statement matches the informal conjecture.

### Applying it to a problem of your own

The schema needs no software. It is a way of organizing a directory of notes, and the discipline is in the labels rather than the tooling.

1. **State the problem in all three registers** — question, notation, formal statement. Do this before anything else; most of the ambiguity in a research problem surfaces here.
2. **Enumerate routes separately.** Give each independent approach its own space. Resist collapsing them into one narrative early — the point is to keep alternatives alive and comparable.
3. **Break each route into typed steps and name the dependencies.** Mark every place a step rests on something unproved with the equivalent of *depends on missing premise*. The set of those marks is your actual research agenda.
4. **Label status and evidence separately on every claim,** and attach the source to the evidence label. Never let "we are working on it" and "it is verified" share a field.
5. **Keep a failures file and write to it every time something is ruled out** — what was tried, how far it got, why it stopped. Treat deleting a dead end as data loss.
6. **Write next steps as three-part tasks** — target, suggested move, known constraint — not as a to-do list of topics.
7. **Where a claim is machine-checkable, name the endpoint exactly**: the statement checked, the version checked, and what the check did not cover.
8. **Log presentation changes apart from mathematical ones,** so a reader can tell reorganization from progress.
9. **Say in writing what the record does not establish.** A well-organized set of notes is persuasive out of proportion to its content; the disclaimer is what keeps it honest.

Done this way, the result is what makes a problem workable by someone who did not live through its history — which includes an AI assistant, and includes you in six months.

## Claimed advances (as of 2026-09-13)

Nine, with the site's own evidence labels:

| Area | Claim | Status |
|---|---|---|
| Collatz dynamics | A positive fraction reaches 1 in logarithmic Collatz time | Lean checked; acceptance review open |
| Complex analysis | Sendov's conjecture proof package | Lean checked; acceptance review open |
| Graph drawing | Strong Papadimitriou–Ratajczak manuscript + Lean package | Lean checked; acceptance review open |
| Number theory | Natural-density Collatz descent in logarithmic time | Accepted in ProofAtlas; Lean checked |
| Graph theory | Formalized proof of Bondy's minimum-degree longest-cycle conjecture | Lean checked; acceptance review open |
| Combinatorial game theory | Rectangle-reachable Berlekamp counterexample | Unverified manuscript; adversarial audit attached |
| Graph theory | Jackson Hamilton-decomposition counterexample | Accepted in ProofAtlas; Lean checked |
| Convex geometry | Mixed-area lower bound for Moser's convex worm problem | Lean checked; acceptance review open |
| Number theory | Collatz predecessor lower bounds at exponent 0.90 | Accepted in ProofAtlas; Lean checked |

---

## How to use it

### Reading it

No account, software, or setup is needed to read ProofAtlas. Anyone can open `/collaboration/`, pick a conjecture, and read the whole record.

A useful order:

1. **Pick a workspace** in an area you can judge for yourself.
2. **Read the argument map before the prose.** The diagram of routes shows where the real gap is; the nodes marked "blocked" or "depends on missing premise" are the live questions. The prose around them is elaboration.
3. **Read the eliminated routes next.** These are the approaches already tried and ruled out. It is the part of the record that cannot be reconstructed from scratch, and the main reason the site is worth reading at all.
4. **Check the label on every claim.** Each item is marked with its evidence status. "Lean checked" means a program verified the argument. "Unaudited" or "source-reported" means nobody has. Treat the second kind as a lead, not a fact.
5. **Then read the prepared tasks.** Each is a specific, stated next step. They are the most directly usable thing on the site.

Nothing can currently be submitted back. Claiming a task and sending in a result are disabled in the beta, so anything done with this material stays with the person who did it, for now.

### Handing a task to an AI assistant

A prepared task is written in a form an AI assistant can work with directly. If you want to try one, the practical shape is this.

**What you need on your side.** An AI assistant capable of extended work on a problem, and — this is the part that matters — a *proof checker* it can run, meaning Lean together with its mathematical library, Mathlib. Lean is free and open-source. Without it the assistant can produce an argument that reads correctly and is wrong, and there is no way to tell the difference by reading. With it, a claimed proof is either accepted by the checker or it is not. Do not skip this.

**What to give the assistant.** Copy from the workspace page, by hand:

- the task statement in full, including its "suggested move" and any warning it carries;
- the statement of the conjecture itself;
- the steps the task depends on, read off the route diagram above it;
- the relevant definitions and setup;
- **the eliminated routes**, stated as things not to attempt.

That last item does most of the work. Without it, an assistant will spend its effort rediscovering a failure that is already written down on the page it was copied from.

**What to ask for.** Ask for a proof checked in Lean, not an explanation. An explanation is not a result here. If the assistant cannot produce a checked proof, the useful output is a precise statement of where it got stuck — which is itself the kind of thing the site records as an open obligation.

**How to read what comes back.** The checker's verdict is the only verdict. If Lean accepts the proof, the stated result holds. If it does not, nothing has been established, however convincing the accompanying prose is. Note also what a check does not settle: whether the formal statement really says what the informal conjecture says, whether anyone else got there first, and whether a specialist agrees it is interesting. Those remain human judgments.

### A realistic expectation

The prepared tasks are well posed and the record of failed approaches is genuinely valuable. But the problems are open conjectures, some of them famous ones, and the site's own results are mostly partial: bounds improved, special cases settled, counterexamples found. The site says so plainly — of its Collatz work, "The full Collatz conjecture remains open." Expect to contribute a step, not a solution.

## Caveats

- **Read-only beta.** No claiming, no agent connection, no submission.
- **Counters drift, downward-compatibly.** A search snapshot showed 243 workspaces / 734k lines / 971 tasks and elsewhere "198 research programs," against 268 / 770,128 on the live page. The numbers are live and the site quotes different ones in different places; do not treat any single figure as stable.
- **Largely machine-generated.** The corpus is produced by AI agents. The evidence labels are the safeguard, and they only work if you read them.
- **"Accepted in ProofAtlas" ≠ peer-reviewed.** It is an internal editorial status.
- **No stated eligibility, funding, compute, or data-access terms.** The collaboration page says nothing about who may participate or on what terms.

---

## Maintaining these notes

These notes are a snapshot of a live site, so they go stale. Everything published here lives in one file, `README.md`. Working material — raw page dumps, drafts, anything mid-flight — belongs in `local/`, which is gitignored and never published.

**When to refresh.** The site is in beta and its numbers move. A check every month or two is reasonable; a check is definitely warranted if the beta opens up, since that would change the whole "How to use it" section.

**What to re-check, in order:**

1. **Beta status** — `/collaboration/`. Does it still say task, agent, and submission controls are unavailable? If they have opened, the "Handing a task to an AI assistant" section needs rewriting: the hand-copying step goes away and a real submission path replaces it.
2. **The counters** — the home page and `/collaboration/`. Workspace count, investigation-line count, ready-task count, and the category percentages. These are the fastest-moving figures in the file.
3. **Claimed advances** — `/advances/`. Items get added, and an item's evidence label can change (for instance "acceptance review open" becoming "Accepted in ProofAtlas"). Re-copy the whole table rather than patching rows.
4. **Workspace structure** — one workspace page, e.g. the Tate Conjecture. Confirm the sections, the route representation, and the per-item metadata still match what the Vocabulary section describes.
5. **`/about/`** — currently a 404. If a page appears naming who runs ProofAtlas and how it is funded, that is the single most valuable addition to make, and the Provenance note about the gap should come out.

**When updating:** change the "Compiled" date at the top and the "as of" date on the advances heading. Keep the quotations verbatim — if a phrase on the site has changed, replace the quotation rather than paraphrasing around it. If a figure now contradicts what is written here, replace it and, where the site itself is inconsistent, say so in Caveats as the existing counter-drift note does.

**What not to change:** the statement that this is unaffiliated, and the rule that no claim here has been independently verified. Both remain true no matter how the site evolves.

---

## Provenance

Sources, all fetched 2026-09-13:

- [Collaboration beta](https://www.proofatlas.ai/collaboration/) — programs, the workflow line, beta status
- [ProofAtlas home](https://www.proofatlas.ai/) — counts, category breakdown, unit examples
- [Tate Conjecture workspace](https://www.proofatlas.ai/collaboration/tate-conjecture/) — page structure, route representation, task example, item metadata
- [Research advances](https://www.proofatlas.ai/advances/) — the nine claims and their evidence status
- [Papers, manuscripts, and notes](https://www.proofatlas.ai/research/) — publication surface (not yet reviewed in detail)
- Other workspaces seen but not examined: [Abundance](https://www.proofatlas.ai/collaboration/abundance-conjecture/), [Graph Reconstruction](https://www.proofatlas.ai/collaboration/graph-reconstruction-conjecture/), [Barnette's](https://www.proofatlas.ai/collaboration/barnette-conjecture/)

`/about/` returns 404 — there is no public page naming who runs ProofAtlas, how it is funded, or how the corpus was built. That gap is worth noting.

Everything here is quoted or paraphrased from those pages. Nothing was verified against the mathematics itself, and no claim on the site was independently checked in writing these notes.
