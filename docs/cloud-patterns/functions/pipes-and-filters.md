# Pipes and Filters with Azure Functions

The **Pipes and Filters** pattern decomposes processing into a sequence of independent steps (“filters”), connected by a transport (“pipes”). Each step transforms or enriches data and passes it forward.

Azure Functions is often used to implement filters, with messaging (Service Bus/Event Hubs/Storage queues) or HTTP as the pipe.

---

## Reference architecture

```mermaid
flowchart LR
  In[Input] --> P1[Pipe 1\n(queue/topic)]
  P1 --> F1[Filter 1\nFunction]
  F1 --> P2[Pipe 2]
  P2 --> F2[Filter 2\nFunction]
  F2 --> P3[Pipe 3]
  P3 --> F3[Filter 3\nFunction]
  F3 --> Out[Output]
```

---

## Key considerations (L200–L300)

- **Independence**: each filter should be deployable and scalable independently.
- **Error handling**: decide whether to stop the pipeline or route failures to a side channel.
- **Message contracts**: define stable schemas between stages.
- **Observability**: propagate correlation IDs across all stages.

---

## References

- Pipes and Filters pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters
- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Azure Functions documentation: https://learn.microsoft.com/en-us/azure/azure-functions/
