# Week 13 — Session 1: Securing Distributed Systems — Authentication, Authorization and Zero Trust

**Topic:** Security of distributed systems
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

A distributed system exposes many services and many entry points, and an attacker only has to find a single weak one. This session is about controlling who gets in and what they are allowed to do: **authentication** and **authorization** using tokens (JWT, OAuth2, OIDC), the **API gateway** as the central point for auth, **trust between services**, the **Zero Trust** mindset, and the **common categories of vulnerabilities** a design must defend against. The central idea: *many doors, one mindset — never trust, always verify.*

## Why Security Is Harder in Distributed Systems

A monolith basically has one door. A distributed system has many services, APIs and network paths, and each one is a possible way in. The overall security is only as good as the weakest of them, so protection has to be applied consistently everywhere, not only at the outer edge.

## Authentication (AuthN) and Authorization (AuthZ)

These answer two separate questions that are frequently mixed up:

- **Authentication (AuthN)** establishes **WHO you are** — your identity. *Example: signing in with a password, MFA or a certificate.* It answers "Who are you?"
- **Authorization (AuthZ)** determines **WHAT you may do** — your permissions. *Example: role- or attribute-based access control.* It answers "What can you do?"

Authentication comes first; authorization is then applied to every action.

## Tokens: JWT, OAuth2 and OIDC

- **OAuth2** is an authorization framework that allows an application to obtain **delegated access** to resources within limited **scopes**, without ever handling the user's password.
- **OIDC (OpenID Connect)** places an **identity layer** on top of OAuth2, so the application also finds out **who** the user is (through an ID token).
- **JWT (JSON Web Token)** is a **signed, self-contained** token that holds **claims** — the user's identity, their scopes and an expiration time. Because it is signed, services can check it without querying a database.

Together, they allow a user to sign in once and carry a verifiable token to many services.

## The API Gateway as the Central Auth Point

The **API gateway** is the single entry point placed in front of the internal services, which makes it the obvious place to apply security uniformly:

- Validate tokens (signature, expiration, issuer).
- Enforce authorization (scopes, roles, policies).
- Apply rate limiting and throttling.
- Block malicious traffic and provide auditing/observability.

Centralizing these checks means every request is inspected **before** it gets to an internal service.

## Trust Between Services

Internal calls need authentication as well — a service should **not** be trusted only because it sits "inside" the network. Two common techniques are **mTLS (mutual TLS)**, where both sides present certificates, and **short-lived service tokens** limited to what each service is allowed to call. This stops one compromised service from calling everything else without restriction.

## Zero Trust

**Zero Trust** follows the rule *never trust, always verify*:

- Authenticate and authorize **every** request, no matter where it comes from.
- Treat the network as **hostile** — there is no safe "inside".
- Apply **least privilege** — every identity receives only the access it actually needs.
- Keep verifying continuously, based on identity, device and context.

It replaces the old "hard shell, soft center" approach with verification at every step.

## Common Vulnerability Categories to Design Against

- **Broken authentication** — weak sign-in or poor handling of sessions/tokens.
- **Broken access control** — users can get to data or actions they should not have.
- **Injection (SQL / command)** — untrusted input ends up executed as code or queries.
- **Secrets leakage** — keys, passwords or tokens exposed in code, logs or configuration.
- **Insecure dependencies** — vulnerable third-party libraries and base images.

Addressing these in the design from day one costs much less than fixing them after a breach.

## Key Takeaways

- **AuthN** = who you are; **AuthZ** = what you are allowed to do.
- **OAuth2** delegates access, **OIDC** adds identity and **JWT** carries signed claims.
- The **API gateway** centralizes token validation, authorization and rate limiting.
- Authenticate **service-to-service** calls as well (mTLS / short-lived tokens).
- **Zero Trust**: verify every request, assume the network is hostile, apply least privilege.

## Infographic

See [`week13-session1-security-authn-authz-zero-trust.png`](./week13-session1-security-authn-authz-zero-trust.png) in this folder for a visual summary.

## OptiView mapping (Week 13)

OptiView authenticates users through **`opti-auth-api`** (RS256 JWTs) and uses **`opti-api-gateway`** as the public choke point for the API. The worker and workflow services call the domain APIs with a **`SERVICE_TOKEN`** (in develop it is generated by `dev-keys.sh` in `opti-infra` and never committed). The team's Zero Trust direction: the Compose `platform` network does not count as a trust boundary on its own — every hop should carry an identity that can be verified, and secrets live in environment configuration, never in git or in logs.
