# Flutterwave API Surface Notes

This document summarizes integration areas to help keep skill content accurate and version-aware.

## Base URLs

- v3: https://api.flutterwave.com/v3
- v4 sandbox reference examples: https://developersandbox-api.flutterwave.com
- v4 docs and reference index: https://developer.flutterwave.com/docs and https://developer.flutterwave.com/reference

## Auth

- v3: Bearer secret key in Authorization header
- v4: Bearer auth in current reference examples; treat v4 as evolving and validate production auth requirements before launch

## Verified v4 Resource Families

The current v4 reference index exposes these major integration families:

- Customers (list, create, retrieve, update, search)
- Charges (list, create, retrieve, update)
- Payment Methods (list, create, retrieve)
- Mobile Networks (retrieve)
- Banks (retrieve banks, retrieve branches, account lookup)
- Wallets (account lookup, balances, statements)
- Transfers (create/list/retrieve/update/retry)
- Transfer Recipients and Senders
- Transfer Rates
- Settlement (list/retrieve)
- Chargebacks (list/create/retrieve/update)
- Refunds (list/create/retrieve)
- Orders (list/create/retrieve/update)
- Virtual Accounts (list/create/retrieve/update)

## Cross-Cutting Patterns

- Always include a deterministic tx_ref for idempotency and reconciliation.
- Verify all payments server-side before fulfillment.
- Use webhook + poll fallback for resilient state updates.
- Store Flutterwave IDs and your own internal IDs for joins.

## Skill Families

- Setup and testing
- Payment collection channels
- Transactions and refunds
- Payouts and beneficiaries
- Subaccounts and plans
- Virtual cards and accounts
- Bills and verification
- Settlements and disputes

## Versioning Guidance for Agents

- Keep route paths version-specific.
- Do not mix request body fields across versions.
- Keep a per-skill "v4 delta" block.
- Prefer typed wrappers and narrow response parsing.

## Webhook Security Notes

- Verify `flutterwave-signature` using HMAC-SHA256 with your dashboard webhook secret hash.
- Return HTTP 200 quickly; non-200 and redirects are treated as webhook failure.
- Timeout is 60 seconds.
- Retries can be enabled; docs indicate retry attempts at fixed intervals when delivery fails.
- Keep handlers idempotent and always re-verify transaction amount/currency/tx_ref through API before fulfillment.
