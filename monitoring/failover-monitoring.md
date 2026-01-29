# Failover monitoring strategies for Azure Cosmos DB

When operating globally distributed Azure Cosmos DB accounts, it’s critical to monitor for failover events. Failovers can be either:
1. **Service‑managed (automatic)** due to regional outages
2. **Forced (manual)** via user‑initiated actions

## Quick summary of alerting options

Here’s a **clean comparison table** you can drop directly into a doc or slide, categorizing the **available alert options for Cosmos DB failovers**, split by type and intent.

***

### Cosmos DB Failover Alert Options – Comparison Table

| Alert Type                                           | Signal Source                                           | Detects Automatic (Service‑Managed) Failover | Detects Forced (Manual) Failover | Scope                           | Reliability for Failover Detection | Primary Use Case                                      | Notes / Limitations                                                                |
| ---------------------------------------------------- | ------------------------------------------------------- | -------------------------------------------- | -------------------------------- | ------------------------------- | ---------------------------------- | ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Azure Service Health Alerts**                      | Service Health (Incidents / Advisories)                 | ✅ *Indirect*                                 | ❌                                | Subscription / Region / Service | **Low–Medium**                     | Platform‑declared outages, executive & customer comms | Does **not guarantee** a failover occurred; some failovers happen without a Service Health incident [\[Cosmos DB...G/CSS Sync | Meeting\]](https://teams.microsoft.com/l/meeting/details?eventId=AAMkADg5Yzk5YWI1LTk5ZmItNGQyZi1iYTg5LTkzYTQzYmIzOTY5NgFRAAgI3lL-3FbAAEYAAAAApD6QZWe5tkmKauhrrj5csgcADqW2AT2c-UerFB7cOBr6_wAAAAABDQAADqW2AT2c-UerFB7cOBr6_wAEw8R3DAAAEA%3d%3d) |
| **Activity Log – Platform Event**                    | Azure Activity Log (`Region Failed Over (Platform)`)    | ✅                                            | ✅                                | Cosmos DB account               | **High**                           | **Primary failover detection & paging**               | Most authoritative signal that a failover actually happened; requires alert rule setup [\[MSFT - Ope...r activity | Meeting\]](https://teams.microsoft.com/l/meeting/details?eventId=AAMkADg5Yzk5YWI1LTk5ZmItNGQyZi1iYTg5LTkzYTQzYmIzOTY5NgBGAAAAAACkPpBlZ7m2SYpq6GuuPlyyBwAOpbYBPZz9R6sUHtw4Gvr7AAAAAAENAAAOpbYBPZz9R6sUHtw4Gvr7AARKIvBeAAA%3d)                                |
| **Azure Monitor Logs (Diagnostics + Log Analytics)** | Cosmos DB control‑plane diagnostics                     | ✅                                            | ✅                                | Cosmos DB account               | **High (Post‑event)**              | RCA, audits, timeline reconstruction                  | Not ideal for real‑time paging; requires diagnostics enabled [\[MSFT Open...over drill | Meeting\]](https://teams.microsoft.com/l/meeting/details?eventId=AAMkADg5Yzk5YWI1LTk5ZmItNGQyZi1iYTg5LTkzYTQzYmIzOTY5NgBGAAAAAACkPpBlZ7m2SYpq6GuuPlyyBwAOpbYBPZz9R6sUHtw4Gvr7AAAAAAENAAAOpbYBPZz9R6sUHtw4Gvr7AARNXu72AAA%3d)                                                          |
| **Metrics‑Based Alerts**                             | Azure Monitor metrics (latency, errors, RU spikes)      | ❌                                            | ❌                                | Account / container / region    | **Low**                            | Detect customer impact symptoms                       | Indicates degradation, **not** a failover event itself [\[MSFT Open...over drill | Meeting\]](https://teams.microsoft.com/l/meeting/details?eventId=AAMkADg5Yzk5YWI1LTk5ZmItNGQyZi1iYTg5LTkzYTQzYmIzOTY5NgBGAAAAAACkPpBlZ7m2SYpq6GuuPlyyBwAOpbYBPZz9R6sUHtw4Gvr7AAAAAAENAAAOpbYBPZz9R6sUHtw4Gvr7AARNXu72AAA%3d)                                                                |
| **Client / SDK Detection**                           | Application telemetry (write endpoint changes, retries) | ✅ *Observed*                                 | ✅ *Observed*                     | Application                     | **Low–Medium**                     | Supplemental signal from app perspective              | Non‑authoritative; app‑dependent; varies by SDK behavior [\[MSFT - Ope...r activity | Meeting\]](https://teams.microsoft.com/l/meeting/details?eventId=AAMkADg5Yzk5YWI1LTk5ZmItNGQyZi1iYTg5LTkzYTQzYmIzOTY5NgBGAAAAAACkPpBlZ7m2SYpq6GuuPlyyBwAOpbYBPZz9R6sUHtw4Gvr7AAAAAAENAAAOpbYBPZz9R6sUHtw4Gvr7AARKIvBeAAA%3d)                                                              |

***

### Recommended Operational Pattern (1‑liner)

*   **Use Activity Log alerts as the source of truth for failover detection**
*   **Use Service Health alerts for outage context and external communications**
*   **Use Metrics & Logs to understand customer impact and perform RCA**

If you want, I can:

*   Convert this into a **single leadership slide**
*   Mark which alerts should page **ACE On‑Call vs PG On‑Call**
*   Add **recommended alert severity & wiring (Sev‑2 vs Sev‑1)**


***

## 1. Azure Service Health alerts (Subscription‑level)

**What they capture**

*   **Service‑managed (automatic) regional failovers**
*   Only when Azure **declares a service incident or outage**

**How it works**

*   Azure Service Health emits notifications tied to:
    *   **Service Incidents**
    *   **Planned Maintenance**
    *   **Health Advisories**
*   Alerts are scoped at the **subscription + service + region** level

**Strengths**

*   Authoritative signal that Microsoft has acknowledged a regional issue
*   Useful for **post‑incident correlation** and customer communications
*   Zero configuration beyond standard Service Health alert rules

**Limitations (important)**

*   **Does NOT guarantee failover detection**
    *   Automatic failover may occur **before** or **without** a Service Health incident
*   No signal for:
    *   **Forced (manual) failovers**
    *   **Partial degradations**
*   No account‑ or region‑specific failover details

✅ Best for: **Broad outage awareness**, **exec comms**, **incident confirmation**  
❌ Not sufficient for: **Operational detection of actual failovers**

 [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/azure/reliability/reliability-cosmos-db-nosql), [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/azure/cosmos-db/monitor)

***

## 2. Azure Activity Log (Control‑Plane) alerts : **Most reliable**

**What they capture**

*   **Service‑managed failovers**
*   **Forced (manual) failovers**
*   **Change in write region**
*   **Region marked offline / online**

**How it works**

*   Cosmos DB emits **control‑plane events** into the Azure Activity Log
*   A specific platform signal exists:
    *   **“Region Failed Over (Platform)”**
*   Alerts can be created in Azure Monitor directly on these events

**Strengths**

*   **Explicit failover signal**
*   Accounts for **both automatic and manual failovers**
*   Near real‑time
*   Objectively authoritative for “a failover happened”

**Limitations**

*   Requires:
    *   Diagnostic settings / Activity Log retention
    *   Explicit alert rule setup
*   Doesn’t convey *why* the failover occurred (only that it did)

✅ Best for: **Primary detection mechanism**, **on‑call alerting**, **audit trails**  
✅ Recommended default for Cosmos DB production accounts

 [\[stackoverflow.com\]](https://stackoverflow.com/questions/61879775/cosmos-db-failover-alert)

***

## 3. Azure Monitor Logs (Log Analytics) : Correlative / advanced

**What they can capture**

*   Evidence of failover behavior via:
    *   Control‑plane diagnostics
    *   Region metadata changes
*   Can be queried historically

**How it works**

*   Enable **Cosmos DB diagnostic logs**
*   Query logs for:
    *   Region role changes
    *   Account configuration transitions

**Strengths**

*   Flexible
*   Useful for **RCA and post‑mortem timelines**
*   Can power analytics or dashboards

**Limitations**

*   **Not ideal for real‑time alerting**
*   Requires explicit log ingestion and queries
*   More operational overhead

✅ Best for: **Forensics, RCA, analytics**  
❌ Not recommended as first‑signal alert

 [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/azure/cosmos-db/create-alerts), [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/azure/cosmos-db/monitor)

***

## 4. Metric‑based alerts (Indirect signal only)

**What they capture**

*   Symptoms caused *by* failover:
    *   Error spikes
    *   Latency increase
    *   Traffic rerouting

**What they do NOT capture**

*   The failover event itself
*   Write‑region change confirmation

**Value**

*   Early indication of instability
*   Good for **customer‑impact detection**

✅ Best for: **Impact detection**  
❌ Not failover‑aware

 [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/azure/cosmos-db/create-alerts)

***

## 5. Client‑side detection (SDK‑based) : *Supplemental*

**What it captures**

*   Write endpoint changes observed by the SDK
*   SDK retry / regional routing behavior

**Limitations**

*   Application‑dependent
*   Not service‑authoritative
*   Not suitable as primary signal

✅ Useful as **additional context only**

 [\[stackoverflow.com\]](https://stackoverflow.com/questions/61879775/cosmos-db-failover-alert)

***

## Recommended alert strategy (concise)

| Layer                | Purpose                         | Recommended |
| -------------------- | ------------------------------- | ----------- |
| **Activity Log**     | Detect that a failover occurred | ✅ Yes       |
| **Service Health**   | Confirm platform outage         | ✅ Yes       |
| **Metrics**          | Detect customer impact          | ✅ Yes       |
| **Logs**             | RCA & audit                     | ✅ Optional  |
| **Client detection** | Supplemental telemetry          | ⚠️ Optional |

***
