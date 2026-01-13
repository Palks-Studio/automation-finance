<p align="center">
  <img src="docs/images/Palks_Studio.png" alt="Palks Studio" width="600">
</p>

> 🇬🇧 English | [🇫🇷 Français](./README_FR.md)

# Palks Studio — Automation System
**Financial automation built for rigor, traceability, and longevity**

This README documents design principles and system architecture.  
It intentionally avoids operational procedures and sensitive details.

---

## Overview

This repository presents a financial automation system designed to handle:

- invoice generation (single & batch)
- revenue tracking
- payment reconciliation
- client balances
- accounting-ready exports

The system is deterministic, auditable, and explicit by design.

It operates:

- without a database
- without a CMS
- without a SaaS dependency
- without any exposed web interface

All executions run server-side, via CLI scripts and cron, with a strict separation of responsibilities.

This project is not a product, not a SaaS, and not a plug-and-play tool.  
It documents a production-grade approach to financial automation.

---

## Project structure

The full project structure is intentionally not documented line by line in this README.

To understand the organization of the system, please refer to the example directory:

```
public_version/example_structure/
```


This directory reflects the real architecture and responsibilities of the system  
without exposing operational or sensitive details.

The actual repository follows the same principles and layout.

---

## What this repository is (and is not)

### This repository is
- a documented architecture for financial automation
- a system designed to be predictable and auditable
- an example of strict separation between billing, payments, and accounting
- a real-world system used in production

### This repository is not
- a certified accounting software
- a ready-to-use invoicing tool
- a payment processing system
- a web application or API service

The outputs produced by this system are intended for internal operational use and for integration with standard accounting workflows.

---

## Core design principles

This system follows a small set of non-negotiable principles:

- **No magic**  
  Every operation is explicit and traceable.

- **No silent processing**  
  Errors stop execution. They are logged and surfaced.

- **No implicit correction**  
  Invalid inputs are rejected, not “fixed”.

- **Files are proofs**  
  Generated artifacts are considered immutable evidence, not disposable outputs.

- **Strict separation of responsibilities**  
  Billing, payments, balances, receipts, and exports are handled independently.

- **CLI-only execution**  
  No web exposure, no background ambiguity.

These principles favor predictability over convenience and clarity over speed.

---

## System architecture (high-level)

The system is composed of independent layers, each with a single responsibility:

- **Billing engines**
  - direct invoicing
  - batch invoicing (CSV-driven)

- **Business rules**
  - centralized pricing and billing logic
  - single source of truth

- **Alerting layer**
  - blocking vs informational alerts
  - explicit execution feedback

- **Payment layer**
  - manual payment records
  - deliberately decoupled from billing

- **Balance reconciliation**
  - computed state (invoiced vs paid)
  - paid / unpaid detection

- **Export layer**
  - accounting-ready CSV outputs
  - reproducible at any time

No layer mutates another implicitly.

---

## Project structure (conceptual view)

The directory layout mirrors the system’s responsibilities:

engine/ → execution engines & business logic
clients/ → client configuration (one file per client)
batch_clients/ → batch client definitions
data/ → immutable operational data (logs, invoices, balances)
docs/ → internal specifications (e.g. CSV format)
tools/ → reconciliation and export utilities
exports/ → generated accounting artifacts
downloads/ → packaged invoice archives


Each directory exists for one reason only.  
Cross-responsibility coupling is intentionally avoided.

---

## Execution model

The system runs on a closed, repeatable cycle:

1. **Generation phase**  
   Invoices are generated based on explicit rules and configurations.

2. **Payment phase**  
   Payments are recorded independently, without automation or assumptions.

3. **Reconciliation phase**  
   Invoiced amounts are compared against received payments.

4. **Consolidation phase**  
   Client balances are computed and statuses updated.

5. **Export phase**  
   Accounting-ready artifacts are produced on demand.

At no point does the system infer or guess missing information.

---

## Batch invoicing model

In batch mode:

- one client provides one CSV file
- one CSV line equals one invoice
- validation is strict and structural
- the entire batch stops on the first error
- raw inputs are archived before consumption

This model favors data integrity over partial success.

---

## Integrity & safeguards

Several mechanisms are enforced across the system:

- anti-duplicate protections
- annual sequential counters
- immutable archives
- explicit execution flags
- categorized alerts
- exhaustive logging

A failed execution is considered safer than a partial one.

---

## Security posture

- CLI-only execution
- no exposed endpoints
- no browser access
- no external API dependency for core operations
- data stored locally on the server

Security is achieved through absence of surface, not complexity.

---

## Maintenance & longevity

The system is designed to:

- be understandable without its original author
- be auditable months or years later
- degrade loudly rather than silently
- integrate cleanly with standard accounting processes

This repository documents an engineering approach, not a shortcut.

---

## Project status

Status: Stable — used in real production conditions.

The system has been designed to operate autonomously,  
with a strong emphasis on rigor, traceability, and long-term maintainability.

---

© Palks Studio — see LICENSE.md  
https://palks-studio.com
