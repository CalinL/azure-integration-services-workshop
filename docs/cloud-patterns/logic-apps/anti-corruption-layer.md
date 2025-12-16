# Anti-Corruption Layer (ACL) with Azure Logic Apps

The **Anti-Corruption Layer (ACL)** pattern protects a modern system from being tightly coupled to a legacy system (or external partner) by placing a translation/adapter layer in between.

In integration solutions, Logic Apps is often used as the ACL because it can:
- transform schemas,
- mediate protocols,
- normalize data and errors,
- isolate authentication and connectivity details.

---

## When to use

Use ACL when:
- integrating with a legacy/partner system that has unstable contracts or non-ideal semantics,
- you want to shield internal domain models from external data shapes,
- you need protocol mediation (file/SOAP/FTP ↔ HTTP/events).

Avoid ACL when:
- you control both sides and can evolve contracts directly.

---

## Reference architecture

```mermaid
flowchart LR
  New[Modern Services] --> ACL[ACL\n(Logic App)]
  ACL --> Legacy[Legacy / Partner System]

  subgraph ACL Responsibilities
    T[Transform / map schemas]
    P[Protocol mediation]
    V[Validate + normalize errors]
    S[Security boundary]
  end

  ACL --- T
  ACL --- P
  ACL --- V
  ACL --- S
```

### Logic Apps mapping
- Use connectors/adapters to interact with the external system.
- Transform payloads using mapping/transform steps.
- Normalize errors into consistent internal codes/events.
- Emit canonical events/messages into your internal messaging backbone.

---

## Key considerations (workshop depth)

- Define a **canonical model** on the modern side (stable internal contract).
- Version the ACL independently of internal services.
- Keep the ACL focused on translation/mediation, not core business rules.
- Treat ACL as a security boundary (credentials, network access, auditing).

---

## References

- Anti-Corruption Layer pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer
- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Azure Logic Apps documentation: https://learn.microsoft.com/en-us/azure/logic-apps/
