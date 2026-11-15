### Hi, I'm Samwel

Data engineer in Helsinki working in Python, SQL and dbt. I build pipelines end to end: ingestion, tested models, CI and a dashboard people can actually use. Open to data engineering and analytics engineering roles in Finland.

Before data engineering I spent three years in fleet operations at TIER and Dott, inspecting and repairing e-scooters and recording the inspection. That work taught me how messy real operational data is, and it shapes how I build: check the source, test the output, make reruns safe.

---

### Featured projects

#### [Helsinki City Bike Pipeline](https://github.com/munyingi/helsinki-citybike-pipeline) · [live dashboard](https://helsinki-citybikes.streamlit.app)

Batch ELT on five seasons (2021 to 2025) of HSL trip data and FMI weather.

- Python ingestion into Snowflake with `PUT` and `COPY INTO`, so reruns never duplicate rows
- dbt staging, fact, dimension and aggregate models: 12.5 million valid trips, 24 data tests
- Found 1.9 million duplicate trips in the 2021 source files and an empty duration column that would have dropped about 200,000 valid trips
- Yearly totals match the city's published figures within a few percent
- The same dbt project runs on DuckDB in CI, so every push is tested for free

`Python` `Snowflake` `dbt` `DuckDB` `GitHub Actions` `Streamlit`

#### [E-commerce CDC Pipeline](https://github.com/munyingi/ecommerce-cdc-pipeline) · [live dashboard](https://ecommerce-cdc-pipeline.streamlit.app)

Change data capture from an operational database into a tested analytics warehouse.

- Debezium reads the PostgreSQL write-ahead log and streams every insert, update and delete through Kafka, all in Docker Compose
- Python loader commits Kafka offsets only after the warehouse write succeeds: a full replay from offset zero produced zero duplicates
- dbt star schema with SCD Type 2 customer and product dimensions, so each order joins to the city and price valid when it was placed
- 55 dbt tests, including no overlapping SCD2 windows and revenue reconciled to the cent
- Five-page dashboard: revenue, cohort retention, RFM segments, delivery SLAs and pipeline freshness

`PostgreSQL` `Debezium` `Kafka` `dbt` `Docker` `Streamlit`

#### [Fast Track to dbt](https://github.com/munyingi/fast-track-to-dbt-2025)

dbt Labs workshop project: refactoring with `ref()`, a reusable Jinja macro, data tests, model documentation and a feature-branch pull request workflow.

`dbt` `SQL` `Jinja` `Git`

---

### How I build

- **Test the data, not just the code.** Every model has dbt tests, and results are checked against an outside source where one exists.
- **Make reruns safe.** Loads are idempotent, so a failed or repeated run never creates duplicates.
- **CI on every push.** Pipelines rebuild and test automatically in GitHub Actions.
- **Watch the cost.** Small warehouses, auto-suspend and resource monitors from day one.
- **Document it.** Each repo has a README with architecture, decisions and screenshots.

---

### Tools

- **Languages:** Python, SQL, Bash
- **Data:** dbt, Snowflake, PostgreSQL, DuckDB, Apache Kafka, Debezium
- **Platform:** Docker, GitHub Actions, Git
- **Apps:** Streamlit, pandas, Altair

**Currently learning:** Microsoft Fabric, and adding orchestration to the CDC pipeline.

---

### Contact

[LinkedIn](https://www.linkedin.com/in/samwel-munyingi-352a88161) · samwelmunyingi@gmail.com
