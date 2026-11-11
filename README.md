# Banking

The common banking vocabulary -- bank accounts, their balances and transactions, the counterparties money is sent to, the transfers that send it and the statements that record it -- distilled from the APIs that already exist, open-banking standards included. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Banking, Open Banking, Consumer Banking, Banking as a Service, Neobank, Digital Banking, Core Banking in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| bank_account | 136 | 128 | name 76%, type 73%, currency 63%, status 62%, id 58%, account_number 57%, balance 51% | accounts 123, account 18, bank-accounts 5, bankaccounts 4 |
| balance | 74 | 70 |  | balances 49, balance 37 |
| transaction | 118 | 110 | id 89%, amount 87%, direction 74%, description 71%, status 67%, posted_at 56%, account_id 56% | transactions 116, transaction 12, banktransactions 1 |
| beneficiary | 43 | 41 | id 58%, name 54% | beneficiaries 19, payees 8, counterparties 8, external-accounts 6 |
| transfer | 73 | 71 | amount 78%, status 75%, created_at 58%, description 56%, id 52% | payments 55, transfers 21, payment 9, transfer 8 |
| statement | 49 | 44 | id 59% | statements 40, statement 8, account_statements 1 |

Cohort: 315 providers with harvested OpenAPI, 211 crosswalks. Of the providers exposing a bank_account: 49% create, 89% list, 80% fetch, 32% update, 18% delete. Generated 2026-11-11 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- Open-banking standards dominate banking APIs, and measured naively they ARE the vocabulary: fifty Australian banks implement the Consumer Data Right and some thirty UK providers the Open Banking UK standard. Counted per bank, a beneficiary's 'core' was CDR's own field names. Counted once per standard, the market's own agreement shows.
- A bank account agrees on name, type, status, currency, id, account number and balance -- once the open-banking standards count once, a proprietary bank account and a standardised one share the shape.
- A transaction agrees on id, amount, status, direction and description -- direction (credit or debit) is core, which is not true of a payment in the payments context.
- Balances agree on nothing outright: account, available and currency all sit just under half. Open-banking standards split a balance into typed entries; proprietary APIs carry available and current side by side.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [arazzo.yml](arazzo.yml) -- reference workflows composed only of the shared operations (compositions, not measurements).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- A standard counts once. The 50 CDR banks are one voice (`au-cdr-banking`) and the UK Open Banking implementers another (`open-banking-uk`), identified by the standards' own paths. Every bank keeps its own crosswalk, marked with the standard it implements. Revolut, Barclays and Zopa also publish proprietary APIs and are counted on their own.
- Money moving between accounts is a `transfer` at `/transfers`, though banks say `payments` (64 providers, against 29): `/payments` is the payments context's, and an open-banking payment initiation is an account-to-account transfer.
- A counterparty is a `beneficiary` at `/beneficiaries`: beneficiaries lead (19) and the word collides with nothing.
- A bank account is `/accounts`, the market's word (123 providers), because inside banking it means only one thing. Accounting uses `/ledger-accounts` to stay clear of it.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
