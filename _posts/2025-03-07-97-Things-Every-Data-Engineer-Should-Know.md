## "Key lessons from "97 Things Every Data Engineer Should Know" by Tobias Macey"

The evolving landscape of data engineering demands a nuanced understanding of technical principles, architectural patterns, and operational best practices. Drawing from insights in *97 Things Every Data Engineer Should Know* by Tobias Macey, this report synthesizes critical lessons across data storage, infrastructure design, quality assurance, and system scalability. Below, we explore these themes in depth, supported by actionable strategies and real-world applications.

---

### Foundational Principles of Data Storage Optimization

#### Pushdowns and Columnar Efficiency

Modern data systems rely heavily on **pushdown optimizations** to minimize computational overhead. By projecting only necessary columns and applying predicate filters at the storage layer, engineers reduce I/O and CPU costs significantly. For instance, a query engine that skips deserializing irrelevant rows improves throughput in columnar formats like Parquet or ORC. This aligns with AWS’s guidance on optimizing S3 performance through selective scanning.

#### Compression and Encoding Trade-Offs

Balancing compression algorithms (e.g., Zstandard, Snappy) with encoding schemes (e.g., dictionary, run-length) requires evaluating storage costs against query latency. While aggressive compression reduces footprint, it increases CPU utilization during decoding. Tools like Apache Hudi and lakeFS integrate these optimizations while maintaining ACID guarantees, ensuring atomic transactions across distributed systems.

---

### Infrastructure as Code and Deployment Strategies

#### Immutable Infrastructure with Terraform

Adopting infrastructure-as-code (IaC) tools like **Terraform** enables reproducible environments and minimizes configuration drift. Terraform’s declarative approach allows engineers to preview changes before deployment, reducing risks of runtime failures. For example, provisioning a Kinesis-S3-Athena pipeline via IaC ensures consistency across development, staging, and production environments.

#### Avoiding Web Consoles for Critical Workflows

Manual interventions via cloud consoles introduce human error and auditability gaps. Automating resource provisioning through version-controlled Terraform modules ensures compliance with GDPR and other regulatory frameworks. This is particularly critical when handling EU resident data, where audit trails are mandatory.

---

### Data Quality and Validation Frameworks

#### Context-Enriched Business Rules

Static data checks (e.g., non-null constraints) are insufficient for detecting nuanced anomalies. Encoding **domain-specific interactions**—such as verifying that lifetime payments never exceed purchases per customer—provides deeper validation. Tools like Great Expectations enable these checks through customizable Python validators, which can be integrated into CI/CD pipelines.

#### Statistical and Distributional Testing

Validating data distributions (e.g., histograms of transaction amounts) complements schema checks. Automated anomaly detection in pipeline outputs—such as unexpected skews in sensor data—ensures robustness. For example, comparing input/output histograms in a fraud detection system can reveal misconfigured transformations.

---

### Architectural Patterns for Scalable Systems

#### Facade and Decorator Patterns in Pipeline Design

Applying software design patterns to data workflows enhances modularity. The **facade pattern** simplifies interactions with logging frameworks by exposing a unified API to DAGs, while decorators inject cross-cutting concerns like monitoring without cluttering business logic. Decoupling these concerns allows teams to independently upgrade monitoring tools (e.g., Prometheus to Datadog) without disrupting pipelines.

#### Bitemporal Modeling for Temporal Accuracy

Incorporating **bitemporal timestamps** (event time and processing time) enables accurate historical queries. For instance, reprocessing data with late arrivals becomes trivial when storage arrival times are serialized, allowing aggregates to exclude records that arrived after a cutoff. Apache Flink’s watermarking mechanism exemplifies this approach, ensuring event-time correctness in streaming applications.

---

### Stream Processing and Real-Time Analytics

#### Choosing the Right Streaming Framework

Selecting between **microservice-oriented** (Kafka Streams) and **bulk-processing** (Apache Flink) frameworks depends on workload complexity. High-cardinality event streams (e.g., clickstream analytics) benefit from Kafka’s lightweight processing, while Flink excels at windowed aggregations over terabytes of data. AWS’s Kinesis Data Streams and Firehose further illustrate this divide, with Firehose optimizing for batched S3 writes.

#### Handling Small Files in Distributed Storage

Small files degrade performance in block-based storage systems like S3 or HDFS. Engineers must coalesce micro-batches into larger partitions (e.g., hourly instead of per-minute) or use compaction utilities in Delta Lake/Iceberg. AWS’s guidelines recommend file sizes ≥128 MB to maximize throughput and minimize LIST operations.

---

### Metadata Management and Collaboration

#### Centralized Metastores for Data Discovery

Tools like the **Hive Metastore** provide a unified catalog for datasets, enabling consistent access control and schema evolution tracking. Versioned data lakes powered by lakeFS extend this by offering Git-like branching for experiments, ensuring isolation between production and test environments.

#### Embedded Collaboration in Workflows

Reducing friction in team coordination accelerates delivery. Integrating access requests into Slack approvals or embedding dataset documentation in Jupyter notebooks ensures context remains tied to assets. This aligns with Zalando’s data mesh implementation, where domain teams self-serve via standardized APIs.

---

### Versioning and Schema Evolution Strategies

#### Topic-Based Versioning in Event Streams

Adopting **topic-per-schema-version** in Kafka isolates consumers during migrations. For example, a `user-events-v2` topic allows legacy services to continue using `v1` while new consumers adopt `v2`. This mirrors API versioning practices and simplifies rollbacks.

#### Schema Compatibility Modes

Avro’s backward/forward compatibility rules prevent breakages during schema updates. A `FULL` compatibility mode ensures new fields are optional, while `NONE` permits breaking changes for major revisions. Teams must enforce these rules via CI checks in schema registries.

---

### Conclusion: Building Future-Proof Data Systems

The lessons from *97 Things Every Data Engineer Should Know* underscore the importance of **adaptability** and **rigor** in system design. From optimizing storage layouts to enforcing GDPR-compliant pipelines, engineers must balance technical depth with operational pragmatism. Emerging trends like data meshes and bitemporal modeling will dominate next-generation architectures, but foundational principles—such as IaC and automated validation—remain timeless.

To stay ahead, teams should:

1. Institutionalize infrastructure-as-code for all environments.
2. Embed data quality checks at every pipeline stage.
3. Adopt metastore solutions for centralized governance.
4. Prioritize streaming architectures for real-time insights.

By internalizing these strategies, organizations can transform raw data into resilient, scalable assets that drive decision-making.
