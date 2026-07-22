---
model: gemini-3.1-pro-high
temperature: 0.1
top_p: 0.1
max_output_tokens: 16384
---

# Rule 02: Idempotency Standards

## Overview
All automation applied to the Linux environment must be strictly idempotent to ensure system stability and predictability.

## Directives
1. **State Checks**: Before applying any change, check the current system state. If the target state is already met, exit cleanly without making changes.
2. **Safe Retries**: Tasks must be safe to run multiple times in succession without causing unintended side-effects or errors.
3. **Atomic Operations**: Where possible, file writes and state changes must be atomic (e.g., write to temporary file, then `mv`).
4. **No Implicit Dependencies**: Do not rely on ephemeral state that may change between executions.
