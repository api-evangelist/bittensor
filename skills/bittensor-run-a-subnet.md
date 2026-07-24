---
name: Create and tune a subnet
description: Register a new subnet, set its identity and hyperparameters, and manage emissions as the subnet owner.
api: mcp/bittensor-intents.json
operations: [subnet_registration_cost, register_subnet, set_subnet_identity, set_hyperparameter, set_mechanism_count, set_subnet_emission_enabled, subnet_hyperparameters, subnet_identity]
---

# Create and tune a subnet

Stand up a new subnet and administer it as owner. These are **owner/root** operations — treat them as safety-critical (`agentic-access/bittensor-agentic-access.yml`).

## Auth
Sign with the subnet-owner **coldkey**. Owner/root controls warrant human-in-the-loop confirmation.

## Steps
1. Read `subnet_registration_cost`, then **plan** and execute `register_subnet` to obtain a `netuid`.
2. Brand it with `set_subnet_identity`; read back with `subnet_identity`.
3. Tune economics/behavior with `set_hyperparameter` (owner-settable) and `set_mechanism_count`; verify via `subnet_hyperparameters`.
4. Control TAO emission with `set_subnet_emission_enabled` (root-gated).

## Rules
- Owner actions are blocked during the weights window (`AdminActionProhibitedDuringWeightsWindow`) and other guardrails — branch on `errors/bittensor-error-codes.yml`.
- Always `--dry-run`/`plan` first; attach a `Policy` restricting `allowed_netuids` to your subnet.
