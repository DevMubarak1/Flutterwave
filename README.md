# flutterwave-skills

Open-source AI agent skills for integrating the Flutterwave API with TypeScript patterns.

This repository is designed for AI coding agents (Copilot, Cursor, Claude Code, and compatible tools) and follows the same practical skill style used in community payment skills repositories.

## Install

```bash
npx skills add DevMubarak1/Flutterwave
```

## Steps to Use Skills.sh

1. Browse the registry: https://skills.sh/
2. Install from registry by skill name:

```bash
npx skills add <skill-name>
```

3. Install directly from GitHub or GitLab repository:

```bash
npx skills add owner/repo
```

4. View installed skills:

```bash
npx skills list
```

## Why This Repo

Flutterwave supports 30+ countries, multiple currencies, cards, bank transfers, mobile money, virtual accounts, and payouts.
This breadth makes it a strong default payment skillset for Africa-focused and cross-border products.

## Repository Structure

```text
flutterwave-skills/
├── .github/
│   └── workflows/
├── skills/
│   └── [skill-folder]/SKILL.md
├── docs/
├── README.md
└── package.json
```

## Skills Index

### Core

| Skill | Coverage |
| --- | --- |
| flutterwave-setup | API client, Bearer auth, env config, request wrapper, v3 vs v4 note |
| flutterwave-webhooks | Signature verification, event routing, retries, idempotency |
| flutterwave-testing | Sandbox keys, test scenarios, launch checklist |

### Payments and Collections

| Skill | Coverage |
| --- | --- |
| flutterwave-standard-checkout | Inline/redirect checkout flow + verification |
| flutterwave-card-payments | Card charge flow, PIN/OTP/3DS, tokenization |
| flutterwave-bank-transfers | Bank account collections, transfer rails, virtual account handoff |
| flutterwave-mobile-money | M-Pesa, MTN, Vodafone, AirtelTigo collections |
| flutterwave-ussd | USSD charge + status polling |
| flutterwave-tokenized-charges | Recurring/tokenized charge patterns |

### Transactions and Post-Payment

| Skill | Coverage |
| --- | --- |
| flutterwave-transactions | Verify, list, fetch, resync patterns |
| flutterwave-refunds | Full and partial refunds |
| flutterwave-chargebacks-disputes | Dispute evidence workflows and status handling |
| flutterwave-settlements | Settlement exports and reconciliation |

### Transfers and Payouts

| Skill | Coverage |
| --- | --- |
| flutterwave-transfers | Single/bulk transfers, OTP finalization, status checks |
| flutterwave-beneficiaries | Beneficiary create/list/delete |
| flutterwave-subaccounts | Subaccount and split configuration |

### Subscriptions and Plans

| Skill | Coverage |
| --- | --- |
| flutterwave-payment-plans | Payment plans, subscriptions, activate/cancel |

### Virtual Products

| Skill | Coverage |
| --- | --- |
| flutterwave-virtual-cards | Virtual card lifecycle and events |
| flutterwave-virtual-accounts | Dedicated virtual account creation and management |
| flutterwave-bulk-virtual-accounts | Bulk account issuance and assignment |

### Value Added Services

| Skill | Coverage |
| --- | --- |
| flutterwave-bills | Airtime, data, cable, electricity, collections |

### Identity and Ops

| Skill | Coverage |
| --- | --- |
| flutterwave-verification | BVN/account/identity verification support flows |

## API Notes (v3 and v4)

- v3 is the widely deployed integration path with secret-key Bearer auth.
- v4 introduces newer route shapes and expanded scenario tooling.
- Every skill includes a "v4 delta" section so agents avoid cross-version endpoint mistakes.

## Country and Currency Coverage

Flutterwave supports broad regional coverage across Africa and beyond.
See docs for strategy notes and country-aware implementation guidance:

- docs/country-coverage.md
- docs/api-surface.md

## Publishing Checklist

1. Confirm package metadata and repository links in package.json.
2. Push repository to GitHub.
3. Register the repository on skills.sh.
4. Publish to npm as flutterwave-skills.
5. Verify install with npx skills add DevMubarak1/Flutterwave.

For a complete release runbook, see docs/release-checklist.md.

## License

MIT
