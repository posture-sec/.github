# The posture issue contract (PI-1)

Every repository under this organization is a tool that refuses things, and every issue it
opens is a work order. A work order has to let someone who has never seen the repository start
the work, finish it, and prove it without asking a question, and it has to stop that same person
from doing anything the repository would refuse.

That is the whole job of this format. It exists so an issue can be picked up cold: by a
contributor in a scoped program, by a stranger, or by the owner six weeks from now with no
memory of writing it.

## Two rules the format is built on

**The roadmap is the source of truth; an issue copies a row, it never replaces it.** Every
task-shaped issue names its `ROADMAP.md` row and quotes that row's `Done when:` clause. If the
issue and the file ever disagree, the file is right and the issue is stale, and fixing the issue
means changing the file first, in the same pull request.

**A claim names the run that produced it or says it is local.** Some proof only exists on a
runner: a workflow log, a scheduled run, a release artifact. A contributor cannot always produce
that, so every issue's verification section separates what a person can run at a desk from what
needs the hosted half, and says who can close the hosted half.

## The shape

One marker line, one one-sentence brief, then nine numbered sections. Always this order. Always
all nine present.

```markdown
<!-- posture-issue: <repo>/<task-id> | contract: PI-1 | kind: task | wave: Medium -->

> Restate the change as one sentence of intent, in the words you would say out loud to someone
> picking this up: what stops being true, or what starts working, when this is done.

## 1 · Why this exists
## 2 · Done when
## 3 · Where the work lands
## 4 · Read before writing
## 5 · Rules that bind this issue
## 6 · How to prove it
## 7 · Out of scope
## 8 · Handoff
## 9 · When to stop and ask
```

The numbering is a sequence, not decoration. Sections 1 and 2 are what to understand, 3 and 4 are
what to find, 5 and 7 are what to respect, 6 is what to run, and 8 and 9 are what to do at the
end and in the middle when it goes wrong. A contributor who reads them in order never needs to
jump back.

### The marker

The first line is an HTML comment so it is invisible on the page and greppable from the API:

```
<!-- posture-issue: stellar-posture-watch/9.1 | contract: PI-1 | kind: task | wave: High -->
```

- `<repo>` is the repository name, so a copied issue still says where it belongs.
- `<task-id>` is the `ROADMAP.md` row id, or `report` when the issue is not roadmap-backed.
- `contract: PI-1` is the version. A script that audits backlog shape reads this, not the heading
  text.
- `kind:` is `task` (a roadmap row), `gate` (a readiness claim that closes on an observed run),
  `owner` (an act only the account owner can take), or `report` (something is wrong).
- `wave:` is the tier when the work is shaped for someone outside the organization. Omit it
  otherwise. It records the shape of the work, not a promise of payment; the tier entered in the
  program dashboard is what sets a value, and a label alone lands an issue in the lowest tier.

### 1 · Why this exists

Two to five sentences. The problem as it exists in the world: who is affected, what happens
today because of it, why this repository is the place it gets fixed, and why now. If the task was
born from a failure, name the failure and its run. If it was born from a decision, name the
decision id.

This is the section that cannot be generated from a title. Repeating the title here is the most
common way to write an empty issue.

### 2 · Done when

One unchecked box per observable condition, taken from the row's `Done when:` clause, split at
semicolons. Then the box that keeps the roadmap honest:

```markdown
- [ ] `ROADMAP.md` task 3.4 is ticked in this pull request, with a changelog row stamped from
      `date -u` and the progress table regenerated
```

Close the section with the staleness line, verbatim:

```markdown
Source of truth: `ROADMAP.md` section 5, task 3.4. If this issue and that row disagree, the row
is right and this issue is stale.
```

For `kind: gate` and `kind: owner` rows, the boxes name the observation rather than a code
change, and section 2 says so plainly: nobody should close a gate with a diff.

### 3 · Where the work lands

The paths, each with one line about what changes there. Every existing path must be real: check
the file before you cite it, because a wrong path costs a contributor more time than a missing
one. New files get their intended path and the reason they are new.

For a repository whose layout is documented, cite the document that governs placement rather than
guessing (Gauge has `project_structure.md`, the action has `ARCHITECTURE.md` section 2).

### 4 · Read before writing

Three to six ordered items: the documents, sections and fixtures that carry the constraints, and
the setup a desk needs (toolchain version, pinned linters, how to install them). Point at
sections, not whole files. Reading six documents before starting is cheaper than writing the
seventh.

### 5 · Rules that bind this issue

The two to five hard rules from the repository's own rule table that *this* change can break,
each with the thing that enforces it. This is not a copy of the table. The point is to name the
specific ways this work goes wrong: a renderer change and a workflow change have different ways
to leak a report body, and only one of them has an exit code to lose.

Include, always, the rule that no check may be weakened, skipped or deleted to make something
pass, and say what to do instead: fix the test and explain why in the pull request.

### 6 · How to prove it

The commands, in the order to run them, in fenced blocks with the observable that means success
after each one. Then two lines:

- **Local half:** what a person at a desk can produce, and where to paste it (the pull request's
  testing section, verbatim output, no paraphrase).
- **Hosted half:** the workflow and job name whose log carries the rest, who can trigger it, and
  what to write if they cannot.

Anything this issue cannot prove gets said as a limit, not left unstated. A green desk run is
evidence about a desk.

### 7 · Out of scope

The adjacent temptations, named. What the issue does not authorize: the refactor it makes
possible but does not want, the dependency it might seem to need, the second output format that
would be easy to add here, the protected path it brushes against.

One line belongs in almost every issue in this organization: a new module, a version bump of a
pinned action, or a change to a refusal in a validator needs the owner's agreement before the
work, not in the pull request after it.

### 8 · Handoff

The branch name to use (`<type>/<task-id>-<slug>` from `main`), the pull request template's
sections that this issue fills, and the three things a reviewer will check first for this
particular change. For a protected path, say who reviews it.

### 9 · When to stop and ask

The blockers specific to this issue: a dependency row that is not done, a decision still Open in
the log, two documents that disagree, an `[Unverified]` claim this work depends on, or a hard rule
that the task appears to require breaking. Then how to ask: comment on the issue and say which
rule or which row blocks you, and stop. Do not open a pull request that works around a hard rule
and let the review catch it.

## Length and writing

Aim between 1200 and 2500 characters. Below that, a section is almost always a placeholder; above
it, the issue is doing a tutorial that belongs in a document.

Same conventions as the rest of the organization, because an issue is part of the public record:
plain language, no em dashes, UTC timestamps with the source named, prose wrapped at 100 columns,
and output describes posture. Nothing here calls an account secure or safe, and a clean run is
never written as a claim that an account cannot be misused.

## What does not carry the contract

- **Issue bodies a schedule writes.** The tracking issue in `stellar-posture-watch` and the drift
  issue in `stellar-account-fixtures` are artifacts: their bodies come from `render_report.py`
  over a live report, and the state marker inside them is what the next run believes it already
  saw. They keep their own format, and every one of them says so in its first paragraph.
- **Pull requests.** Those carry the repository's pull request template, which asks for what
  changed and what was run; the contract asks what the work requires. A pull request that closes
  a PI-1 issue quotes that issue's section 2 and section 6 rather than rewriting them.
- **Security reports.** Those go to a private advisory, and the repository's issue form sends them
  there instead of opening a public one.

## Reusing it

Each repository adds a task issue form under `.github/ISSUE_TEMPLATE/task.yml` whose body field
is prefilled with this skeleton, and its `CONTRIBUTING.md` points here for the definitions and
keeps the repository-specific text: which paths, which rules, which commands. The generator in a
repository that publishes its backlog from `ROADMAP.md` emits these sections, so a regenerated
issue and a hand-written one have the same shape.
