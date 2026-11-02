# posture-sec

We audit the configuration that public chains already expose, because almost nobody reads it.

One rule shapes everything here: a tool that inspects your accounts must not be able to hurt them. Every project in this org is read-only and secret-free by construction, not by convention.

## Projects

| Project | What it does | State |
| --- | --- | --- |
| [Gauge](https://github.com/posture-sec/guage) | Reads a Stellar account's signers, thresholds, flags and home domain from Horizon, compares them with a policy file you declare, and exits nonzero when the two disagree. Go CLI, binary `stellar-gauge`. | Unaudited, functional alpha. [Documentation site](https://posture-sec.github.io/guage/) live since 2026-11-02. |

Gauge is the only code repository published in this org today. Airlock, a Python scanner for Stellar and Soroban repositories, is the sibling project: it shares conventions with Gauge and no code, and it has no public home here yet.

## How the work runs

- **The roadmap is the contract.** Each repository carries a `ROADMAP.md`, and CI refuses to merge a change that does not update it in the same pull request. Task IDs, a changelog row and a progress table move together.
- **Claims are traceable or marked.** Every number in a README, on the docs site and on this page is read from the code or from a named run against a named network and ledger. What has not been verified says so.
- **Findings are posture, never safety.** Nothing here will call an account secure. The chain shows signers, thresholds and flags; a read-only tool cannot know your intent, so it reports what it read and how far it looked.
- **Failure is loud.** An unreachable endpoint, an oversized or malformed response, an unknown config key, a rule that panics: all of them exit nonzero. Exit 0 means the run completed and every rule it evaluated agreed with the policy.

## What has been observed

Read on 2026-11-02:

- OpenSSF Scorecard for Gauge: **6.8** overall. `Pinned-Dependencies`, `Token-Permissions`, `Dangerous-Workflow`, `Security-Policy`, `Dependency-Update-Tool`, `Vulnerabilities`, `Binary-Artifacts`, `Fuzzing`, `License` and `CI-Tests` score 10; `Branch-Protection` scores 4; `Code-Review`, `Maintained`, `Contributors`, `SAST` and `CII-Best-Practices` score 0. [Full result](https://scorecard.dev/viewer/?uri=github.com/posture-sec/guage).
- Gauge has been run against testnet only, which is its own verification policy. Five recorded runs, the latest at ledger 5517199 on 2026-11-02: one WARN finding, exit 1.
- Readiness in Gauge's roadmap: 38 of 70 tasks across Phases 0 to 6, 1 of 10 gates green. v0.1.0 is Phases 0 to 4 and v0.2.0 is Phases 5 and 6. Neither tag exists yet.
- One maintainer and no external contributions yet, which is why `Contributors` and `Code-Review` score 0. Those are honest numbers, not good ones.

## Reporting a problem

[Gauge's SECURITY.md](https://github.com/posture-sec/guage/blob/main/SECURITY.md) states what the code is, what it is not, and how to disclose a finding. Read its status section first: it tells you whether what you found is a bug or a limitation that is already written down.
