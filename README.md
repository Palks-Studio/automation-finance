<p align="center">
  <img src="docs/images/Palks_Studio.png" alt="Palks Studio" width="600">
</p>

> 🇬🇧 English | [🇫🇷 Français](./README_FR.md)

# Automation System – Invoicing & Revenue Tracking

This repository contains an **invoice and revenue automation system** designed to operate:  

- without a CMS  
- without external SaaS services  
- without a database  
- without any exposed web interface  

The entire system relies on PHP scripts executed via cron jobs, with a deliberately **simple, readable, and auditable architecture**.

The system provides:  

- automatic PDF invoice generation (FR / EN)  
- automatic invoice delivery by email  
- per-client revenue tracking (JSON)  
- revenue export to CSV (accounting-ready)  
- reliable yearly invoice numbering

It is **not** a certified accounting software and does not replace:  

- a certified accountant  
- a regulated accounting software  
- nor any legal tax or reporting obligations

The data produced by this system is intended for **internal and operational** use only.

---

## What this repository is not

This repository does not provide:  

- a user interface  
- a ready-to-use SaaS product  
- a certified invoicing system  
- an automated payment processing system

It is a **documented technical architecture**,  
designed to illustrate a robust and autonomous automation approach.

---

## Core principles

- One client = one configuration file  
- No sensitive data exposed on the web  
- No dependency on third-party invoicing services  
- Full traceability (logs, invoices, revenues)  
- CLI-only execution (no browser access)

This system is designed to be:  

- robust  
- predictable  
- maintainable over time  
- understandable without advanced knowledge

---

## Project structure

```
automation/
│
├── run_automation.php              → Moteur d’orchestration global de l’automatisation (FR)
│                                   → Global automation orchestration engine (EN)
├── engine/
│   ├── run.php                     → Moteur principal d’automatisation (cron / CLI) (FR)
│   │                               → Main automation engine (cron / CLI) (EN)
│   │ 
│   ├── export_accounting_year.php  → Export annuel des recettes (FR)
│   │                               → Annual revenue export (EN)
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
│   ├── alerts                      → Gestion des alertes et notifications d’exécution (FR)
│   │                               → Execution alerts and notifications handling (EN)
│   │
│   ├── import_cvs.php              → Import et validation des fichiers CSV clients (FR)
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
│   ├── update_balances.php         → Met à jour les soldes clients à partir des recettes et des paiements (FR)
│   │                               → Updates client balances based on revenues and payments (EN)
│   │
│   └── export_revenues_csv.php     → Script PHP d’export des recettes vers un fichier CSV (comptabilité) (FR)
│                                   → PHP script to export revenues to a CSV file (accounting) (EN)
├── exports/
│   └── export_revenues.csv         → Fichier CSV contenant les recettes exportées (données tabulaires) (FR)
│                                   → CSV file containing exported revenues (tabular data) (EN)
├── downloads/
│   └── *.zip                       → Archives ZIP mensuelles par client, contenant les factures PDF générées automatiquement (FR)
│                                   → Monthly ZIP archives per client, containing automatically generated PDF invoices (EN)
│
├── LICENSE.md                      → Conditions d’utilisation et cadre légal (FR)
│                                   → Terms of use and legal Framework (EN)
│
└── README.md                       → Documentation générale du système (FR)
                                    → General system documentation (EN)
```


---

## Global workflow

### 1. Client configuration

Each client is defined in a dedicated file:

```bash
clients/client_xxx.php
```


This file contains:  

- client identity  
- billing information  
- currency  
- language (FR / EN)  
- execution mode (`test` or `live`)

This is the **only file** to edit when adding or adjusting a client.

---

### 2. Automated execution (cron)

The main script is:

```
engine/run.php
```


It is executed via a cron job (daily, monthly, or scheduled as needed).

At each execution:  

- active clients are processed  
- an invoice is generated if applicable  
- the invoice is archived  
- revenues are updated  
- an email is sent to the client  
- execution logs are written

---

### 3. PDF invoicing

- Invoices are generated as PDF files using DomPDF  
- The template supports both French and English  
- The language depends solely on the client configuration  
- Legal mentions (VAT, exemptions, etc.) are handled automatically

Invoices are archived per client in:

```bash
data/invoices/{client_id}/
```


---

### 4. Revenues (accounting source)

For each client, a JSON file is maintained:

```bash
data/revenues/{client_id}.json
```


It contains:

- cumulative total  
- detailed invoice history  
- currency  
- invoice numbers and dates

This file is the **internal accounting source** of the system.

---

### 5. Accounting export (CSV)

An export tool is provided:

```bash
tools/export_revenues_csv.php
```


It generates CSV files usable by:  

- spreadsheets  
- accountants  
- accounting software

CSV files can be deleted and regenerated at any time  
(with no impact on source data).

---

## Real monthly cycle (invoicing & payments)

The system operates on a simple and predictable monthly cycle.

### Before the 15th of the month

Clients make their payment (bank transfer).  
No emails are automatically sent before this date.

### On the 13th or 14th of the month (quick manual check)

Check received payments on the bank account.

Update the files in `data/payments/`:  

- one file per client  
- empty if no payment was received  
- filled if a payment was received

### Accounting update

Run the following script from the command line:

```bash
php tools/update_balances.php
```


This script:  

- compares invoiced amounts vs paid amounts  
- computes the balance  
- automatically sets the status to `paid` or `unpaid`

The results are written to:

```bash
data/balance/{client_id}.json
```


### On the 15th of the month

Invoices are automatically generated.

Emails are sent only if:  

- the client is active  
- `options.auto_send = true`  
- the script is executed on the 15th

No email is sent outside of this date.

---

## Email sending rules

Invoice email delivery is strictly controlled.

All of the following conditions must be met for an email to be sent:  

- the client is active (`active = true`)  
- `options.auto_send = true`  
- the script is executed on the 15th of the month  
- execution is done via CLI (cron)

This rule is deliberately enforced in `mailer.php`  
to prevent any accidental or out-of-cycle email delivery.

---

## Batch invoicing (client service)

The system includes a batch invoicing engine designed for clients
who provide their own billing data via CSV files.

How it works:  

- one CSV file per client  
- one CSV row = one PDF invoice  
- automatic invoice generation  
- monthly grouping into a ZIP archive  
- email delivery of the download link (optional)

Related script:

```bash
engine/run_batch.php
```


Important rules:  

- PDF invoices are always generated  
- sending (ZIP + email) depends only on `options.auto_send`  
- sending is restricted to the 15th of the month  
- one send per client per month (anti-duplicate safeguard)

The expected CSV format is documented in:  

`docs/format_csv.md`

This document is intended for internal use  
or for a supervised technical client.

---

## Security

- The engine refuses execution outside CLI  
- No web endpoint is exposed  
- Data is stored locally on the server  
- No direct browser access is possible

---

## Maintenance & cleanup

- `logs/` and CSV export files can be safely cleaned  
- `invoices/`, `revenues/`, and `counters/` must never be deleted  
- Invoice numbering is automatic and yearly

---

## Project status

Status: **Stable – production-ready**.

This system is used in real conditions,  
has no critical external dependencies,  
and is designed to operate autonomously over the long term.

---

© Palks Studio — see LICENSE.md  
https://palks-studio.com
