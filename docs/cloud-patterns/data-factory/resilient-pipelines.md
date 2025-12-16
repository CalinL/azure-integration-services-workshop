# Resilient Pipelines with Azure Data Factory

Data pipelines fail in predictable ways: source unavailability, transient network errors, throttling, and schema drift. Cloud design patterns help you design explicit resilience.

This guide focuses on two patterns frequently applied to ADF:
- **Retry** (handle transient faults)
- **Scheduler Agent Supervisor** (control and supervise distributed work)

---

## Pattern: Retry

### What it solves
Transient faults are common in distributed systems. Retrying a failed operation after a short delay often succeeds.

### ADF mapping
In ADF, retries are typically configured per activity. Use retry to handle transient issues like:
- temporary source outages,
- throttling,
- intermittent network failures.

Key guidance:
- use bounded retries (don’t retry forever),
- apply delays/backoff to avoid retry storms,
- fail fast for non-transient errors.

---

## Pattern: Scheduler Agent Supervisor

### What it solves
When you have many independent units of work, a scheduler/supervisor coordinates dispatch and monitors progress.

### ADF mapping
ADF can act as the scheduler/supervisor when:
- you dispatch work to external compute (Databricks, Functions, stored procedures),
- you run parameterized pipelines per partition/date/customer.

```mermaid
flowchart LR
  ADF[ADF Pipeline\nSupervisor] --> Jobs[Work items\n(partitions/batches)]
  Jobs --> Exec1[Execution 1]
  Jobs --> Exec2[Execution 2]
  Jobs --> Exec3[Execution 3]
  Exec1 --> ADF
  Exec2 --> ADF
  Exec3 --> ADF
```

---

## References

- Retry pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/retry
- Scheduler Agent Supervisor pattern: https://learn.microsoft.com/en-us/azure/architecture/patterns/scheduler-agent-supervisor
- Cloud design patterns catalog: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Azure Data Factory documentation: https://learn.microsoft.com/en-us/azure/data-factory/
