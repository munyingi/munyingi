### Hi, I'm Samwel

Data engineer in Helsinki. I build pipelines end to end in Python, SQL and dbt: ingestion, tested models, CI and a dashboard people can actually use. Open to data engineering and analytics engineering roles in Finland.

Before data engineering I spent three years in fleet operations at TIER and Dott, inspecting and repairing e-scooters and recording the inspection. That work taught me how messy real operational data is, and it shapes how I build: check the source, test the output, make reruns safe.

Each project below starts from a real failure mode a data team runs into, then proves the fix with tests.

---

### Projects at a glance

| Project | The problem | Proof it works |
|---|---|---|
| [**Pelivahti**](https://github.com/munyingi/pelivahti) | Settle bets in real time, catch harmful play within seconds, report tax that can be proven | Crash test: restarted stream output is identical to a clean run |
| [**Revenue reconciliation**](https://github.com/munyingi/revenue-reconciliation) | CFO and sales dashboard disagree by €52,024 | Every euro labelled, unexplained gap **€0.00**, enforced by dbt tests |
| [**Late data and backfills**](https://github.com/munyingi/meter-feed-backfill) | Partner feed down 3 days, weekly report 41% too low | Backfill matches the partner's files to the kWh, however many times it reruns |
| [**GDPR deletion requests**](https://github.com/munyingi/gdpr-deletion-workflow) | "Delete all my data" when copies live in raw files, backups and SCD2 history | 0 personal data left, even after a backup restore drill |
| [**E-commerce CDC**](https://github.com/munyingi/ecommerce-cdc-pipeline) | Stream every database change into a warehouse without duplicates | Full replay from offset zero: zero duplicates, revenue reconciled to the cent |
| [**Helsinki City Bike**](https://github.com/munyingi/helsinki-citybike-pipeline) | Five seasons of public trip data with hidden duplicates | Found 1.9M duplicate trips; totals match the city's figures |
| [**Finnish corporate tax trends**](https://github.com/munyingi/Bussiness-operations) | Vero tax data whose schema changes every year, joined to the PRH register | Every header mapping stored; build fails if a year loses a column |

---

### Featured

#### [Pelivahti: real-time betting settlement and responsible gaming](https://github.com/munyingi/pelivahti) · [4-page case study](https://github.com/munyingi/pelivahti/blob/main/docs/Pelivahti_case_study.pdf)

The data platform behind a regulated betting and casino operator, on free tools only. About 79,000 synthetic events, deliberately damaged with duplicates, late arrivals and invalid messages.

- Event-time stream processing with watermarks and tumbling, sliding and session windows
- Exactly-once results without Kafka transactions: results, offsets and checkpoint commit in one transaction, proven by a crash-and-restart test
- Append-only wallet ledger with reversals when match results are corrected
- Versioned JSON Schema data contracts, with invalid messages sent to a dead-letter queue
- Delta Lake bronze layer with MERGE and time travel, dbt on DuckDB with 46 data tests
- The stream ledger and a batch rebuild must agree to the cent, or the build fails
- Dagster orchestration, Redpanda (Kafka) integration test in CI

`Python` `Kafka/Redpanda` `Delta Lake` `dbt` `DuckDB` `Dagster` `GitHub Actions`

#### [Revenue reconciliation: why the CFO and the dashboard disagree](https://github.com/munyingi/revenue-reconciliation) · [live report](https://munyingi.github.io/revenue-reconciliation/) · [app](https://revenue-reconciliation.streamlit.app)

Three source systems record the same money with different clocks, currencies and definitions. I rebuilt both numbers and matched every order to its payments and refunds.

- Seven labelled differences: failed payments, capture lag, a timezone shift across the DST change, duplicate webhooks, FX, and two kinds of refund timing
- A bridge from €1,207,466.29 to €1,259,490.39 where the unexplained remainder is exactly €0.00
- dbt tests fail the build if any line is unlabelled

`Snowflake` `dbt` `SQL` `Streamlit` `GitHub Pages`

#### [Late data and backfills: when a partner feed goes down](https://github.com/munyingi/meter-feed-backfill) · [3-page case study](https://github.com/munyingi/meter-feed-backfill/blob/main/docs/meter-feed-backfill.pdf)

A replay of a real kind of incident. The naive "just reload the week" fix would have swung the report from 41% too low to 59% too high.

- Date partitions with atomic delete-and-insert, so a rerun replaces a day instead of adding to it
- A backfill command that rebuilds only the affected window in dbt, with a before and after diff per day
- A freshness check that catches gaps in the middle of the timeline, not just stale data
- CI replays the whole incident and fails if a single kWh is off

`Python` `DuckDB` `dbt` `pytest` `GitHub Actions`

#### [GDPR deletion requests](https://github.com/munyingi/gdpr-deletion-workflow) · [live demo](https://gdpr-deletion-workflow.streamlit.app/) · [16-page report](https://github.com/munyingi/gdpr-deletion-workflow/blob/main/docs/GDPR_Deletion_Workflow_Report.pdf)

An Article 17 erasure workflow. Give it a customer ID and it finds every copy of that person's data, including old emails that only survive in SCD Type 2 history.

- Anonymises every SCD2 version in place instead of breaking history
- A suppression list of keyed hashes, so a backup restore cannot bring the person back
- Hash-chained audit log that records what was removed without storing the personal data

`Python` `DuckDB` `dbt snapshots` `Streamlit` `pytest`

<details>
<summary><b>More projects</b></summary>

#### [E-commerce CDC pipeline](https://github.com/munyingi/ecommerce-cdc-pipeline) · [live dashboard](https://ecommerce-cdc-pipeline.streamlit.app)
Debezium reads the PostgreSQL write-ahead log and streams every change through Kafka. Offsets are committed only after the warehouse write succeeds. dbt star schema with SCD2 dimensions and 55 tests.
`PostgreSQL` `Debezium` `Kafka` `dbt` `Docker` `Streamlit`

#### [Helsinki City Bike pipeline](https://github.com/munyingi/helsinki-citybike-pipeline) · [live dashboard](https://helsinki-citybikes.streamlit.app)
Five seasons of HSL trips and FMI weather loaded into Snowflake with idempotent `COPY INTO`. 12.5 million valid trips, 24 dbt tests, and the same project runs on DuckDB in CI.
`Snowflake` `dbt` `DuckDB` `GitHub Actions` `Streamlit`

#### [Finnish corporate tax and company register trends](https://github.com/munyingi/Bussiness-operations) · [live dashboard](https://bussiness-operations.streamlit.app)
Vero corporate tax open data joined to the PRH Trade Register on the Business ID. Handles schema drift between years, validates Business ID check digits before joining, and keeps original and corrected figures side by side.
`Python` `DuckDB` `dbt` `Streamlit`

</details>

---

### How I build

- **Test the data, not just the code.** Every model has dbt tests, and results are reconciled against an outside source where one exists.
- **Make reruns safe.** Loads are idempotent, so a failed or repeated run never creates duplicates.
- **Prove failure handling.** Crash tests, incident replays and restore drills, not just happy-path runs.
- **CI on every push.** Pipelines rebuild and test automatically in GitHub Actions.
- **Watch the cost.** Free tiers, small warehouses and auto-suspend from day one.
- **Document the decisions.** Each repo has a README with architecture, trade-offs and screenshots, and the bigger ones have a written case study.

---

### Tools

- **Languages:** Python, SQL, Bash
- **Data:** dbt, Snowflake, PostgreSQL, DuckDB, Delta Lake
- **Streaming:** Apache Kafka, Redpanda, Debezium
- **Orchestration and platform:** Dagster, Docker, GitHub Actions, Git
- **Apps:** Streamlit, pandas, Altair

**Currently:** preparing for Microsoft Fabric Data Engineer (DP-700).

---

### Contact

[LinkedIn](https://www.linkedin.com/in/samwel-munyingi-352a88161) · samwelmunyingi@gmail.com
