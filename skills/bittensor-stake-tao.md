---
name: Stake TAO to a validator
description: Read a subnet's price and your balance, quote a stake, then add or move stake on the Bittensor chain using the plan/execute model.
api: mcp/bittensor-intents.json
operations: [balance, alpha_price, quote_stake, add_stake, add_stake_limit, move_stake, remove_stake]
---

# Stake TAO to a validator

Back a validator hotkey on a subnet with TAO. Every write is a wallet-signed extrinsic; preview with `plan`/`--dry-run` before submitting.

## Auth
Sign with your **coldkey** (funds) — see `authentication/bittensor-authentication.yml`. Keep the coldkey offline and use a `proxy` for routine work where possible.

## Steps
1. Read holdings with `balance` and current price with `alpha_price` (TAO per alpha) for the target `netuid`.
2. Estimate the outcome with `quote_stake` (price impact + swap fee).
3. **Plan** the stake: run the intent through `plan(...)` / `btcli stake add --dry-run` to see `fee`, `effects`, `warnings`, and the `ok` Policy check.
4. Execute `add_stake` (or `add_stake_limit` to cap price impact). Use `move_stake` to rebalance across subnets, `remove_stake` to exit.

## Rules
- Attach a `Policy` with `max_spend_tao` / `allowed_netuids`; unbounded-cost ops are blocked until caps are raised (`conventions/bittensor-conventions.yml`).
- On failure, branch on the semantic error `code` (e.g. `insufficient_balance`, `insufficient_liquidity`) — see `errors/bittensor-error-codes.yml`. No idempotency key exists; ordering is nonce-based.
