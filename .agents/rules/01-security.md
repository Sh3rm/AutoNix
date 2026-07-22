---
model: gemini-3.1-pro-high
temperature: 0.1
top_p: 0.1
max_output_tokens: 16384
---

# 🛡️ Swarm Global Directive: 01-Security

## 🌟 Primary Directive

This document establishes the foundational security rules for all agents operating within the Massive-OS-Automation-Swarm. These rules are absolute, overriding, and non-negotiable. Any agent found violating these directives will be immediately terminated by the Master Orchestrator, and the action will be blocked by the `ebpf-destructive-gate`.

## 🛑 Rule 1: The Principle of Least Privilege

No agent shall execute a command or access a resource with privileges higher than strictly necessary for the task at hand.
- **Root Access**: Execution of commands as `root` or via `sudo` is strictly prohibited unless explicitly authorized by a human-in-the-loop (HITL) via the `ebpf-destructive-gate`.
- **File System**: Agents must only modify files within their designated operational directories. Modification of core system files (e.g., `/etc/shadow`, `/boot/`) is forbidden without explicit HITL approval.

## 🛑 Rule 2: Immutable Audit Trail

Every action taken by an agent, particularly those involving file system modification, process execution, or network communication, must be indelibly recorded in the central telemetry and task ledger.
- **No Covert Channels**: Agents are forbidden from establishing unmonitored communication channels. All IPC must route through the authorized NATS broker or gRPC bridge.
- **Log Integrity**: Any attempt to modify, delete, or obfuscate logs is classified as a critical security breach.

## 🛑 Rule 3: Destructive Action Gating

Any action that results in the deletion of data, modification of system configurations, or alteration of network state must be routed through the `ebpf-destructive-gate`.
- **Definition of Destructive**: `rm`, `mv` (across filesystems), `chmod`, `chown`, `iptables`, system service restarts, and kernel module loading/unloading.
- **HITL Requirement**: The gate will pause execution and require cryptographic verification or human approval before proceeding.

## 🛑 Rule 4: Mandatory Sandboxing

All untrusted code, scripts, or parsed data must be executed within the designated isolation environments.
- **JavaScript/DOM**: Must run within the `os-goja-js-sandbox` or `os-playwright-dom` with no access to the host file system or network.
- **Binary Execution**: New binaries must be analyzed by the `research-security-auditor` before execution.

## 🛑 Rule 5: The Evidence First Pattern

All analytical and research agents must adhere to the **Evidence First Pattern**. Assumptions regarding system state, security boundaries, or vulnerability status are prohibited. Every claim must be backed by concrete artifacts (logs, traces, verifiable state).

## 🚨 Enforcement Mechanism

These rules are enforced at the kernel level via eBPF hooks (`ebpf-tetragon-hook`). Any violation will result in a `SIGKILL` being sent to the offending process/agent, followed by an immediate alert to the `research-self-healer` and the human operator.
