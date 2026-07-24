---
name: Register a hotkey and start mining
description: Register a hotkey on a subnet, serve your axon, and confirm your UID on the metagraph.
api: mcp/bittensor-intents.json
operations: [subnet_registration_cost, burned_register, serve_axon, serve_axon_tls, serve_prometheus, metagraph, uid, neurons]
---

# Register a hotkey and start mining

Claim a slot (UID) on a subnet and publish your axon so validators can reach you.

## Auth
Register with your **coldkey**; the registered **hotkey** then signs mining traffic (btauth/1 signed requests).

## Steps
1. Check the cost with `subnet_registration_cost` for the target `netuid`.
2. **Plan**, then execute `burned_register` (recycle TAO to register) to obtain a UID.
3. Publish connectivity with `serve_axon` (or `serve_axon_tls`); optionally `serve_prometheus` for metrics.
4. Confirm placement: read `uid` for your hotkey and inspect `metagraph` / `neurons` on the subnet.

## Rules
- Registration is rate-limited and immunity-gated per subnet; branch on codes like `AllNetworksInImmunity` from `errors/bittensor-error-codes.yml`.
- Keep the hotkey online for serving but never the coldkey — use `proxy`/`multisig` guardrails (`conventions/bittensor-conventions.yml`).
