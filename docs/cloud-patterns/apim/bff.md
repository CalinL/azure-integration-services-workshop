# Backends for Frontends (BFF) with APIM

The **Backends for Frontends (BFF)** pattern creates a dedicated backend per client experience (for example: mobile vs web), so each client gets an API that matches its needs without forcing a shared backend to constantly compromise.

In Azure Integration architectures, APIM is often the *front door* and the BFF services are implemented using Azure Functions or App Service.

---

## When BFF is a good fit

Use BFF when:
- Mobile and web clients need different payload shapes, pagination, or caching.
- A shared backend becomes a bottleneck due to competing client requirements.
- You want independent release cadences per client type.

Avoid BFF when:
- All clients make the same requests and share the same UX needs.

---

## Reference architecture (APIM + per-client BFF)

```mermaid
flowchart LR
  subgraph Clients
    M[Mobile App]
    W[Web App]
    D[Desktop App]
  end

  APIM[API Management\n(Gateway)]

  subgraph BFFs
    MBFF[Mobile BFF\n(Azure Function)]
    WBFF[Web BFF\n(Azure Function)]
    DBFF[Desktop BFF\n(Azure Function)]
  end

  subgraph Backend
    S1[Microservice A]
    S2[Microservice B]
    S3[Microservice C]
  end

  M --> APIM --> MBFF
  W --> APIM --> WBFF
  D --> APIM --> DBFF

  MBFF --> S1
  MBFF --> S2

  WBFF --> S1
  WBFF --> S2
  WBFF --> S3

  DBFF --> S2
  DBFF --> S3
```

### What APIM does in this design
APIM is the enforcement and governance layer:
- authentication/authorization (for example, validate JWT)
- throttling and quotas
- caching (where appropriate)
- routing to the correct BFF
- centralized observability

### What the BFF does in this design
Each BFF is client-specific:
- aggregates and shapes responses for its client
- implements client-specific caching rules (if needed)
- handles client-specific pagination, filtering, and “screen-friendly” payloads

---

## Practical APIM guidance for BFF

- **Model per-client APIs explicitly**
  - Separate APIs and/or products for Mobile vs Web.
  - Use clear base paths like `/mobile/*` and `/web/*`.

- **Enforce security and segmentation**
  - Different clients often have different auth requirements and risk profiles.
  - Apply policy at the *API* or *product* scope to keep it maintainable.

- **Keep BFFs small**
  - A BFF should not become a “mini-monolith.” If it grows too large, split by bounded context.

- **Observability**
  - Correlate across APIM → BFF → backend with a shared correlation ID.

---

## Workshop exercise ideas

- Start with one shared API behind APIM.
- Add a mobile client requirement (smaller payload, aggressive caching).
- Implement a mobile BFF and keep the web client on a richer BFF.
- Compare:
  - latency (one call vs multiple)
  - blast radius (mobile changes don’t affect web)
  - governance (one gateway, multiple backends)

---

## References

- Backends for Frontends pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends
- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Gatekeeper pattern (often adjacent to BFF with APIM): https://learn.microsoft.com/en-us/azure/architecture/patterns/gatekeeper
- Rate Limiting pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/rate-limiting-pattern
- Gateway Routing pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing
