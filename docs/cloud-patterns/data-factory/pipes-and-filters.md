# Pipes and Filters with Azure Data Factory

The **Pipes and Filters** pattern breaks processing into discrete steps (filters) connected by a pipeline (pipes). In data integration, each transformation/activity is a filter, and orchestration is the pipe.

Azure Data Factory (ADF) pipelines align well with this model:
- each activity performs a specific task (copy, transform, validate),
- the pipeline defines sequencing, branching, and dependencies.

---

## Reference architecture

```mermaid
flowchart LR
  Src[Source] --> ADF[ADF Pipeline]

  subgraph ADF Pipeline
    A1[Filter 1: Ingest/Copy]
    A2[Filter 2: Validate]
    A3[Filter 3: Transform]
    A4[Filter 4: Publish]
  end

  ADF --> A1 --> A2 --> A3 --> A4 --> Sink[Destination]
```

---

## Key considerations (L200–L300)

- **Small, composable steps**: keep each activity focused and reusable.
- **Contracts between stages**: define file/table schemas and naming conventions.
- **Failure isolation**: decide what should retry vs what should fail fast and alert.
- **Idempotency**: design pipeline steps so re-runs don’t corrupt results.

---

## References

- Pipes and Filters pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters
- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Azure Data Factory documentation: https://learn.microsoft.com/en-us/azure/data-factory/
