<p align="center">
  <img src="docs/images/Palks_Studio.png" alt="Palks Studio" width="600">
</p>

> 🇫🇷 Français | [🇬🇧 English](./README.md)

# Système d’automatisation – Facturation & Recettes

Ce dépôt contient un système d’automatisation de facturation conçu pour fonctionner :  

- sans CMS  
- sans SaaS externe  
- sans base de données  
- sans interface web exposée

L’ensemble repose sur des scripts PHP exécutés via cron, avec une architecture volontairement simple, lisible et auditable.

Le système permet :  

- la génération automatique de factures PDF (FR / EN)  
- l’envoi automatique des factures par email  
- le suivi des recettes par client (JSON)  
- l’export des recettes au format CSV (comptable)  
- une numérotation annuelle fiable des factures.

Il ne constitue **pas** un logiciel de comptabilité certifié et ne remplace pas :  

- un expert-comptable  
- un logiciel comptable réglementé  
- ni les obligations fiscales et déclaratives légales

Les données produites par ce système sont destinées à un usage **interne et opérationnel**.

---

## Ce que ce dépôt n’est pas

Ce dépôt ne fournit pas :  

- une interface utilisateur  
- un logiciel SaaS prêt à l’emploi  
- un outil de facturation certifié  
- un système de paiement automatisé

Il s’agit d’une **architecture technique documentée**,  
conçue pour illustrer une approche d’automatisation robuste et autonome.

---

## Principes clés

- Un client = un fichier de configuration  
- Aucune donnée sensible exposée sur le web  
- Aucune dépendance à un service tiers de facturation  
- Traçabilité complète (logs, factures, recettes)  
- Exécution exclusivement en ligne de commande (CLI)

Ce système est conçu pour être :  

- robuste  
- prévisible  
- maintenable dans le temps  
- compréhensible sans connaissance avancée

---

## Structure du projet

```
automation/
│
├── run_automation.php              → Moteur d’orchestration global de l’automatisation (FR)
│                                   → Global automation orchestration engine (EN)
├── engine/
│   ├── run.php                     → Moteur principal d’automatisation (cron / CLI) (FR)
│   │                               → Main automation engine (cron / CLI) (EN)
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

## Fonctionnement global

### 1. Configuration client

Chaque client est défini dans un fichier dédié :

```bash
clients/client_xxx.php
```


- l’identité du client  
- les informations de facturation  
- la devise  
- la langue (FR / EN)  
- le mode (`test` ou `live`)

C’est le seul fichier à modifier pour ajouter ou ajuster un client.

---

### 2. Exécution automatique (cron)

Le script principal est :

```
engine/run.php
```


Il est exécuté via une tâche cron (exemple quotidien ou mensuel).

À chaque exécution :  

- les clients actifs sont parcourus  
- une facture est générée si applicable  
- la facture est archivée  
- les recettes sont mises à jour  
- un email est envoyé au client  
- les logs sont écrits

---

### 3. Facturation PDF

- Les factures sont générées au format PDF via DomPDF  
- Le template est bilingue FR / EN  
- La langue dépend uniquement de la configuration du client  
- Les mentions légales (TVA, exonération, etc.) sont gérées automatiquement

Les factures sont archivées par client dans :

```bash
data/invoices/{client_id}/
```


---

### 4. Recettes (source comptable)

Pour chaque client, un fichier JSON est maintenu :

```bash
data/revenues/{client_id}.json
```


Il contient :  

- le total cumulé  
- l’historique détaillé des factures émises  
- la devise  
- les dates et numéros de facture

Ce fichier est la source comptable interne du système.

---

### 5. Export comptable (CSV)

Un outil d’export est fourni :

```bash
tools/export_revenues_csv.php
```


Il permet de générer des fichiers CSV exploitables par :  

- un tableur  
- un expert-comptable  
- un logiciel de comptabilité

Les CSV peuvent être supprimés et régénérés à tout moment
(sans impact sur les données sources).

---

## Cycle mensuel réel (facturation & paiements)

Le système fonctionne selon un cycle mensuel simple et prévisible.

### Avant le 15 du mois

- Les clients effectuent leur paiement (virement bancaire).  
- Aucun email n’est envoyé automatiquement avant cette date.

### Le 13 ou 14 du mois (contrôle manuel rapide)

- Vérifier les paiements reçus sur le compte bancaire.

- Mettre à jour les fichiers dans `data/payments/` :  

  - un fichier par client  
  - vide si aucun paiement  
  - rempli si paiement reçu

### Mise à jour comptable

Lancer le script suivant en ligne de commande :

```bash
php tools/update_balances.php
```

Ce script :  

- compare les montants facturés vs payés  
- calcule le solde  
- définit automatiquement le statut`paid` ou `unpaid`

Les résultats sont écrits dans :

```bash
data/balance/{client_id}.json
```


### Le 15 du mois

Les factures sont générées automatiquement

Les emails sont envoyés UNIQUEMENT si :  

- le client est actif  
- `options.auto_send = true`  
- le script est exécuté le 15

Aucun envoi n’a lieu en dehors de cette date.

---

## Règles d’envoi des emails

L’envoi des factures par email est strictement contrôlé.

Conditions nécessaires pour qu’un email parte :  

- le client est actif (`active = true`)  
- l’option `options.auto_send = true`  
- le script est exécuté le 15 du mois  
- l’exécution se fait en CLI (cron)

Cette règle est volontairement codée dans `mailer.php`  
afin d’éviter tout envoi accidentel ou hors cycle.

---

## Facturation batch (service clients)

Le système inclut un moteur de facturation batch destiné aux clients  
qui transmettent leurs propres données de facturation via CSV.

Fonctionnement :  

- un fichier CSV par client  
- une ligne CSV = une facture PDF  
- génération automatique des factures  
- regroupement mensuel en archive ZIP  
- envoi du lien par email (optionnel)

Script concerné :

```bash
engine/run_batch.php
```


Règles importantes :  

- les PDF sont toujours générés  
- l’envoi (ZIP + email) dépend uniquement de `options.auto_send`  
- l’envoi est limité au 15 du mois  
- un envoi par client et par mois (anti-double envoi)

La spécification du format CSV attendu est documentée dans :  

`docs/format_csv.md`

Ce document est destiné à un usage interne ou à un client technique encadré.

---

## Sécurité

- Le moteur refuse toute exécution hors CLI  
- Aucun endpoint web n’est exposé  
- Les données sont stockées localement sur le serveur  
- Aucun accès direct n’est prévu depuis un navigateur

---

## Nettoyage et maintenance

- Les dossiers `logs/` et les exports CSV peuvent être nettoyés sans risque  
- Les dossiers `invoices/`, `revenues/` et `counters/` ne doivent jamais être supprimés  
- La numérotation des factures est automatique et annuelle

---

## État du projet

Statut : Stable – prêt pour une utilisation en production.

Le système est utilisé en conditions réelles,  
sans dépendance externe critique  
et conçu pour fonctionner de manière autonome sur le long terme

---

© Palks Studio — voir LICENSE.md  
- https://palks-studio.com
