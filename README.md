# AEraLogIn — Agent Execution, Runtime Authorization & Logging Infrastructure


> [!IMPORTANT]
> **Frozen public reference implementation.** This repository preserves a historical source snapshot and is **not** the current AEraLogIn development branch or a production-security certification. Some behavior, tests and dependency versions reflect older architecture. Do not deploy it with real credentials or rely on it for sensitive tasks without independent security review. The complete historical Git content and external integrations have not yet been independently cleared by this documentation update.
>
> **Current project direction:** [Public development roadmap](https://github.com/AEraLogIn/.github/blob/main/ROADMAP.md) · [Organization](https://github.com/AEraLogIn) · [Security policy](SECURITY.md)

AEraLogIn is an open-source authorization and interoperability layer for autonomous agents, their execution runtimes, and agent-to-agent communication.

> **A2A-first architecture. Human Identity is an ownership and bootstrap layer — not the center of execution authorization.**

AEraLogIn separates six concerns:

**Human ownership → Agent identity → Runtime identity → Authorization → A2A communication → Execution evidence**

## The core problem

A credential can identify an account without proving which runtime actually performed an action.

| Question | AEra layer |
|---|---|
| Who owns this agent? | Human / Owner Identity |
| Which software agent is this? | Agent Identity |
| Which runtime is actually executing? | Runtime Identity |
| Is this runtime allowed to act? | Runtime Authorization |
| Can another agent communicate with it? | A2A Gateway |
| What happened during execution? | Logging / Evidence |

## A2A-first architecture

~~~text
Human / Owner
     │ owns / enrolls / governs
     ▼
Agent Identity
     │
     ▼
Runtime Identity
     │
     ▼
Runtime Authorization
     │
     ▼
A2A Gateway
     │
     ├──────── AEra Agent Runtime
     │
     └──────── External A2A Peer
     │
     ▼
Execution / Response
     │
     ▼
Logging / Evidence
~~~

The important boundary is not the human login. It is the transition from **identity to authorized execution**.

## Human Identity

Human authentication remains supported, but it is deliberately secondary to the agent execution architecture.

Human identity is used for ownership, agent enrollment, lifecycle administration, key rotation/revocation authorization and optional human-facing authentication.

A human credential is **not** an agent runtime credential and is never treated as proof that a particular runtime executed an action.

## Agent Identity

An agent has its own cryptographic identity and lifecycle, including stable identity, Ed25519 keys, key identifiers, challenge-response verification, rotation and revocation.

Agent identity is distinct from human sessions and external A2A peer credentials.

## Runtime Identity

The runtime is the execution boundary.

The private runtime key is generated and retained by the runtime. AEraLogIn receives the public key and binds it to the registered agent/runtime relationship.

This allows the system to distinguish:

~~~text
an account has access
        from
this registered runtime actually acted
~~~

That distinction is central to AEraLogIn.

## Runtime Authorization

Authentication is not authorization.

AEraLogIn establishes explicit authorization boundaries around the target agent, runtime, capability/skill, external peer, credential scope, lifecycle state and revocation state.

A valid identity alone does not grant unrestricted execution rights.

## A2A Interoperability

AEraLogIn does not replace the A2A protocol. It provides an authorization and trust boundary around A2A communication.

The public A2A surface includes:

- `/.well-known/agent-card.json`
- `POST /api/a2a`
- JSON-RPC based A2A messaging
- capability discovery
- scoped peer credentials where configured
- replay protection
- gateway authorization

External agents remain external peers. Reaching the gateway does not automatically make a caller an AEra agent or grant it permissions.

## Logging and Evidence

The **LogIn** identity is intentionally retained through the concept of **Logging Infrastructure**:

> **AEraLogIn = Agent Execution, Runtime Authorization & Logging Infrastructure**

Logging is part of the evidence boundary around authorization and execution. It is not itself authorization, and logs must not be treated as cryptographic proof merely because an event was recorded.

## Project scope

AEraLogIn is an agent authorization layer, runtime identity layer, A2A interoperability gateway, capability/credential boundary and security reference implementation for verifiable agent execution.

AEraLogIn is not an LLM, agent framework, model provider, MCP replacement, A2A replacement or general-purpose human login product.

## Development direction

Future development priorities are:

1. Runtime Authorization
2. A2A interoperability
3. Agent/runtime cryptographic binding
4. Capability and credential semantics
5. Verifiable execution and response provenance
6. Security testing and negative proofs
7. Auditability and operational evidence
8. Human identity only where it supports ownership and governance

Human Login remains supported, but it is no longer the architectural center of gravity.

## Repository status

This repository preserves a historical, A2A-oriented public reference. Its source code, historical roadmap and test instructions describe that snapshot, not the current private development status. For up-to-date public priorities and status distinctions, use the [organization roadmap](https://github.com/AEraLogIn/.github/blob/main/ROADMAP.md).

## Testing

For local historical reference testing, the documented command is (results have **not** been independently rerun as part of this documentation update):

~~~bash
python -m pytest tests/ -q
~~~

The security lab contains additional negative-proof and interoperability scenarios.

## License

AEraLogIn is licensed under the Apache License 2.0. See [LICENSE](LICENSE).

---

**AEraLogIn — Agent Execution, Runtime Authorization & Logging Infrastructure.**

**Identity establishes the actor. Authorization establishes the boundary. A2A establishes interoperability. Logging preserves the evidence.**