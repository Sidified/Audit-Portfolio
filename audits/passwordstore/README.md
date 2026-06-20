# PasswordStore — Security Review
 
**Auditor:** Sid Hary
**Type:** Course / Practice (Cyfrin Updraft Security Curriculum)
**Date:** 15 June 2026 · **Report version:** 1.0
 
---
 
## Overview
 
PasswordStore is a single-user protocol for storing and retrieving a password on-chain. By design, only the owner should be able to set the password and read it back.
 
This review tested whether that privacy-and-access model actually holds. It does not. The core privacy guarantee is impossible to achieve on a public blockchain — anything written to storage is readable by anyone — and the password setter is missing access control entirely, so any address can overwrite it.
 
## Scope
 
|                   |                                            |
| ----------------- | ------------------------------------------ |
| **File in scope** | `./PasswordStore.sol`                      |
| **Commit**        | `7d55682ddc4301a7b13ae9413095feffd9924566` |
| **Method**        | Manual review + Foundry PoCs               |
 
## Findings Summary
 
| Severity      | Count |
| ------------- | ----- |
| High          | 2     |
| Medium        | 0     |
| Low           | 0     |
| Informational | 1     |
| **Total**     | **3** |
 
| ID  | Title                                                                       | Severity      |
| --- | --------------------------------------------------------------------------- | ------------- |
| H-1 | Password stored on-chain is publicly readable from storage                  | High          |
| H-2 | `setPassword` has no access control; any address can overwrite the password | High          |
| I-1 | Incorrect NatSpec `@param` on parameterless `getPassword`                   | Informational |
 
## Full Report
 
📄 **[Full report (PDF)](../../reports/2026-06-15-passwordstore.pdf)** · [Markdown source](report.md)
 
---
 
<sub>This is a practice audit of an intentionally-vulnerable codebase, completed to drill methodology and report-writing: scope definition, manual review, PoC construction, and severity classification using the CodeHawks Impact × Likelihood matrix.</sub>