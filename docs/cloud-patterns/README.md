# Cloud Design Patterns for Azure Integration Services

This section maps **Microsoft Azure cloud design patterns** to the services covered in this workshop:
- Azure API Management (APIM)
- Azure Logic Apps
- Azure Service Bus
- Azure Event Grid
- Azure Functions
- Azure Data Factory

The goal is to help you recognize *when* a pattern applies, *what problem it solves*, and *how it commonly shows up* in Azure Integration solutions.

## Pattern Guides by Service

| Integration service | Guide |
|---|---|
| Azure API Management (APIM) | [APIM patterns](apim/README.md) |
| Azure Logic Apps | [Logic Apps patterns](logic-apps/README.md) |
| Azure Service Bus | [Service Bus patterns](service-bus/README.md) |
| Azure Event Grid | [Event Grid patterns](event-grid/README.md) |
| Azure Functions | [Functions patterns](functions/README.md) |
| Azure Data Factory | [Data Factory patterns](data-factory/README.md) |

## How to Use These Guides in an L200–L300 Workshop

A practical flow that works well in instructor-led delivery:
1. Pick a **use case** (sync API integration, async messaging, eventing, workflow orchestration, data pipelines).
2. Identify the **primary Integration service**.
3. Apply 2–4 **patterns** to address reliability, security, and scale.
4. Validate with:
   - failure modes (retries, circuit breakers, dead-lettering)
   - throughput and backpressure (load leveling, competing consumers)
   - integration boundaries (anti-corruption layer, strangler fig)

## References

- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
