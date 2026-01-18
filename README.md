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

```
automation/
│
├── engine/
│   ├── run.php                     → Moteur principal d’automatisation (cron / CLI) (FR)
│   │                               → Main automation engine (cron / CLI) (EN)
│   │
│   ├── billing_rules.php           → Règles de facturation et de tarification dynamique (FR)
│   │                               → Billing rules and dynamic pricing logic (EN)
│   │
│   ├── run_batch.php               → Moteur d’automatisation BATCH pour la facturation clients (FR)
│   │                               → Batch automation engine for client invoicing (EN)
│   │
│   ├── vendor/                     → Dépendances PHP (ex: DomPDF) (FR)
│   │                               → PHP dependencies (e.g. DomPDF) (EN)
│   │
│   ├── alerts.php                  → Gestion des alertes et notifications d’exécution (FR)
│   │                               → Execution alerts and notifications handling (EN)
│   │
│   ├── import_csv.php              → Import et validation des fichiers CSV clients (FR)
│   │                               → Client CSV import and validation handler (EN)
│   │
│   ├── mailer.php                  → Envoi des emails avec facture en pièce jointe (FR)
│   │                               → Email sender with invoice attachment (EN)
│   └── templates/
│       ├── invoice.html.php        → Template PDF de facture (bilingue FR / EN) (FR)
│       │                           → Invoice PDF template (bilingual FR / EN) (EN)
│       │
│       └── invoices_batch.html.php → Facture CLIENTS (batch) (FR)
│                                   → Client Invoices (Batch) (EN)
├── clients/
│   └── client_xxx.php              → Fiche client (seul fichier à modifier par client) (FR)
│                                   → Client configuration file (only file to edit per client) (EN)
├── batch_clients/
│   └── client_xxx.php              → Configuration batch d’un client final (facturation mensuelle) (FR)
│                                   → Batch configuration for an end client (monthly invoicing) (EN)
├── data/
│   ├── logs/
│   │   └── xxx.log                 → Logs d’exécution par client (FR)
│   │                               → Execution logs per client (EN)
│   ├── archive_batch/
│   │   └── xxx.csv                 → CSV client archivé (FR)
│   │                               → Archived client CSV (EN)
│   ├── batch_sent/
│   │   └── xxx.zip                 → Zip envoyés (FR)
│   │                               → Zip sent (EN)
│   ├── usage/
│   │   └── xxx.json                → Suivi d’usage mensuel par client (FR)
│   │                               → Monthly client usage tracking (EN)
│   ├── revenues/
│   │   └── xxx.json                → Recettes cumulées (source comptable interne) (FR)
│   │                               → Cumulative revenues (internal accounting source) (EN)
│   ├── payments/
│   │   └── xxx.json                → Paiements reçus du client (virements, montants réellement encaissés) (FR)
│   │                               → Payments received from the client (bank transfers, actually received amounts) (EN)
│   ├── balance/
│   │   └── xxx.json                → Solde comptable du client (facturé vs payé, statut payé / impayé) (FR)
│   │                               → Client accounting balance (invoiced vs paid, paid / unpaid status) (EN)
│   ├── invoices/
│   │   └── client/                 → Factures de l’activité principale (facturation directe, usage interne) (FR)
│   │                               → Invoices from the main activity (direct invoicing, internal use) (EN)
│   ├── invoices_batch/
│   │   └── client/                 → Factures générées dans le cadre du service batch (clients finaux) (FR)
│   │                               → Invoices generated as part of the batch service (end clients) (EN)
│   ├── inbox_batch/
│   │   └── batch.csv               → Fichier CSV fourni par le client (source de facturation batch) (FR)
│   │                               → Client-provided CSV file (batch invoicing source) (EN)
│   ├── counters/
│   │   └── xxx.json                → Compteur annuel de factures par client (facturation directe) (FR)
│   │                               → Annual invoice counter per client (direct invoicing) (EN)
│   └── counters_batch/
│       └── xxx.json                → Compteur annuel de factures par client (facturation batch) (FR)
│                                   → Annual invoice counter per client (batch invoicing) (EN)
├── docs/
│       └── format_csv.md           → Spécification officielle du format CSV attendu (FR)
│                                   → Official specification of the expected CSV format (EN)
├── tools/
│   ├── balances.php                → Met à jour les soldes clients à partir des recettes et des paiements (FR)
│   │                               → Updates client balances based on revenues and payments (EN)
│   │
│   ├── purge_log.php               → Script de nettoyage (FR)
│   │                               → Cleanup Script (EN)
│   │
│   ├── recettes_year.php           → Script PHP d’export des recettes encaissées sur une année complète (FR)
│   │                               → PHP script to export actually received revenues for a full year (EN)
│   │
│   ├── recettes_month.php          → Script PHP d’export des recettes encaissées sur un mois donné (FR)
│   │                               → PHP script to export actually received revenues for a given month (EN)
│   │
│   └── revenues_csv.php            → Script PHP d’export des recettes vers un fichier CSV (comptabilité) (FR)
│                                   → PHP script to export revenues to a CSV file (accounting) (EN)
├── exports/
│   ├── recettes/
│   │   └── recettes.csv            → CSV mensuels / annuels générés à la demande (FR)
│   │                               → Monthly CSV exports generated on demand / Yearly CSV exports generated on demand (EN)
│   │
│   └── payments/                   → CSV généré à partir des fichiers JSON de paiements reçus (FR)
│       └── payments.csv            → CSV generated from the received payments JSON files (EN)
│
├── downloads/
│   └── *.zip                       → Archives ZIP mensuelles par client, contenant les factures PDF générées automatiquement (FR)
│                                   → Monthly ZIP archives per client, containing automatically generated PDF invoices (EN)
│
├── LICENCE.md                      → Conditions d’utilisation et cadre légal (FR)
├── LICENSE.md                      → Terms of use and legal Framework (EN)
│
├── README_FR.md                    → Documentation générale du système (FR)
└── README.md                       → General system documentation (EN)
```


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
