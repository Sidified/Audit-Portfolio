# Smart Contract Security Portfolio — Sid Hary

> Independent smart contract security researcher. Solidity + Foundry, focused on EVM protocol security through hands-on auditing.

**Contact:** [X / Twitter](https://x.com/Sid_Hary_) · [GitHub](https://github.com/Sidified) · CodeHawks: _profile coming soon_ · Sherlock: _profile coming soon_

---

## About

I'm working toward professional smart contract security auditing. This repository collects my audit work — written reports, findings, and the process behind each review.

To keep things honest about what each engagement demonstrates:

- **Course / Practice** — audits of intentionally-vulnerable codebases from the Cyfrin security curriculum, done to drill methodology and report-writing.
- **Contest** — competitive audits on public platforms (CodeHawks, Sherlock).
- **Private** — independent reviews of live or open-source code.

Every audit here — practice or not — is treated as a real engagement: defined scope, manual review first, tooling second, and a full written report with proof-of-concept tests for each confirmed finding.

---

## Audits

| Date    | Project       | Type   | Findings     | Report                                                                     |
| ------- | ------------- | ------ | ------------ | -------------------------------------------------------------------------- |
| 2026-06 | PasswordStore | Course | 2H · 0M · 0L | [PDF](reports/2026-06-passwordstore.pdf) · [details](audits/passwordstore) |

<sub>Severity key — **H** High · **M** Medium · **L** Low · **I** Informational · **G** Gas</sub>


---

## Methodology

1. **Scope & context** — read the docs, map the system, write down the trust assumptions and invariants before touching the code.
2. **Manual review** — line-by-line, attacker mindset, against the invariants from step 1.
3. **Static analysis** — Slither and Aderyn to sweep for known patterns, *after* manual review so the tools don't anchor my thinking.
4. **Proof of concept** — a Foundry test reproducing each confirmed finding.
5. **Reporting** — severity by Impact × Likelihood, with concrete mitigations.

---

## Toolkit

`Foundry` · `Solidity` · `Slither` · `Aderyn` · manual review

---

<sub>Open to audit contest collaboration and security research opportunities. Reach out on X.</sub>