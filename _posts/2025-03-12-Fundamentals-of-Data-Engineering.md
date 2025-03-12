## Key lessons from "Fundamentals of Data Engineering" by Joe Reis

The book "Fundamentals of Data Engineering" offers a wealth of knowledge for data professionals seeking to master this complex and evolving field. After studying this comprehensive resource, I've synthesized key lessons that illustrate the essential concepts, technologies, and best practices that form the foundation of modern data engineering. This exploration covers everything from core responsibilities and architectural patterns to storage systems, modeling approaches, and future trends.

---

## The Data Engineer's Role and Responsibilities

Data engineers serve as the architects and maintainers of data infrastructure that powers analytics and machine learning. One of the most critical responsibilities highlighted in the book is ensuring data quality across the entire data engineering lifecycle. This involves performing systematic data-quality tests and verifying conformance to schema expectations, completeness, and precision.

The book emphasizes three fundamental aspects of data quality that engineers must address. First, accuracy ensures data is factually correct without duplicates or inaccurate numeric values. Second, completeness verifies that records contain all necessary information with valid values in required fields. Third, timeliness confirms that data is available when needed for business operations and decision-making. These quality dimensions form the backbone of reliable data systems and directly impact the trustworthiness of downstream analytics.

As data systems grow in complexity, specialized roles have emerged, including the data reliability engineer—a position focused specifically on maintaining the reliability and integrity of data platforms. This specialization highlights the increasing importance organizations place on consistent, dependable data infrastructure.

## Data Architecture: From Batch to Streaming

The book presents a nuanced view of batch versus streaming architectures, noting that while streaming-first might seem appealing, it often introduces additional costs and complexities. For many common use cases like model training and weekly reporting, batch processing remains an excellent approach.

However, modern data systems rarely fit into a simple batch-or-streaming dichotomy. The Lambda architecture, which maintains separate batch and streaming layers, introduces challenges in reconciling code and data across multiple systems. This has led to the emergence of unified approaches like the Dataflow model implemented by Apache Beam, which treats all data as events with different types of windows. In this paradigm, batch data is simply viewed as bounded event streams, allowing engineers to use nearly identical code for both real-time and batch processing.

The book also explores newer architectural paradigms like the data mesh, which inverts centralized data architecture by applying domain-driven design principles. The data mesh consists of four key components: domain-oriented decentralized data ownership, data as a product, self-serve data infrastructure as a platform, and federated computational governance. This approach redistributes responsibilities, with each domain team taking ownership of serving their data to the broader organization.

## Data Storage Systems and Formats

Data storage fundamentals receive significant attention in the book, covering everything from physical storage media to database types and serialization formats.

The text contrasts hard disk drives (HDDs) with solid-state drives (SSDs), noting that while SSDs eliminate mechanical components and offer faster access, they typically cost nearly 10 times more per gigabyte than magnetic drives. This cost differential has made object storage on magnetic disks the leading option for large-scale data storage in data lakes and cloud data warehouses.

Row-oriented databases organize data as rows on disk to optimize for lookups and in-place updates, while columnar databases arrange data into column files to enable efficient compression and fast scans of large data volumes. The book notes that while columnar databases historically performed poorly on joins (leading to recommendations for denormalization), join performance has improved dramatically in recent years.

Modern data storage also features innovative approaches like the data lakehouse, which combines the controls and data structures of warehouses with object storage flexibility. Systems like Databricks' Delta Lake, Apache Hudi, and Apache Iceberg implement this concept, supporting ACID transactions and blurring the lines between traditional data lakes and warehouses.

Serialization formats receive detailed treatment, with the book covering JSON Lines (JSONL) for semi-structured data, Avro for row-oriented serialization, and columnar storage techniques that enable efficient compression by placing similar values together. For nested data, the book describes "shredding," which maps each location in a JSON document schema into a separate column.

## Data Modeling Approaches

The book explores several data modeling methodologies, contrasting traditional normalized approaches with dimensional modeling and newer techniques like Data Vault.

Normalization is presented as a process of organizing database tables to minimize redundancy and dependency issues. The book walks through the first three normal forms, noting that a database is usually considered normalized when it reaches third normal form.

In contrast, the Kimball approach to dimensional modeling emphasizes fact tables and dimensions. Fact tables represent events, are append-only, and reference only dimensions, while dimension tables contain descriptive attributes. The book details different types of slowly changing dimensions (SCDs), including Type 1 (overwriting existing records), Type 2 (maintaining full history by creating new records), and Type 3 (adding new fields for changes).

The Data Vault model consists of three main table types: hubs (storing business keys), links (maintaining relationships among keys), and satellites (representing attributes and context). This approach offers flexibility for evolving data needs while maintaining historical accuracy.

## Orchestration and Workflow Management

Data orchestration emerges as a critical capability distinct from simple scheduling. While tools like cron are aware only of time, orchestration engines incorporate metadata on job dependencies, typically represented as directed acyclic graphs (DAGs).

The book highlights Apache Airflow, introduced by Airbnb in 2014, as a prominent orchestration tool, while also mentioning newer alternatives like Prefect and Dagster that improve DAG portability and testability. However, Airflow has limitations: it relies on non-scalable components (scheduler and backend database) that become bottlenecks, follows a distributed monolith pattern, lacks support for data-native constructs like schema management, and presents challenges for development and testing.

The text emphasizes that data engineers must understand proper code-testing methodologies, including unit, regression, integration, end-to-end, and smoke testing. These practices ensure reliable data pipelines and contribute to overall data quality.

## Cloud Infrastructure for Data Engineering

Cloud infrastructure considerations form a significant portion of the book's content, with particular attention to scaling, cost management, and geographic distribution.

The text emphasizes that scalable systems should handle both scaling up for extreme loads and scaling down when demand decreases to control costs. This elasticity represents one of cloud computing's core advantages for data workloads.

The book recommends adopting FinOps practices early to manage cloud costs effectively, referencing the FinOps Foundation as a starting point. This proactive approach helps organizations avoid unexpectedly high cloud bills as data volumes and processing needs grow.

Geographic distribution of cloud resources receives detailed treatment. The book notes that clouds provide their highest network bandwidth and lowest latency between systems within a zone, suggesting that high-throughput data workloads should run in a single zone for performance and cost reasons. It also explains virtual private clouds (VPCs) and the advantages of multi-region storage options like those offered by Google Cloud Platform, which provide geo-redundancy without complex replication processes.

The shared responsibility model in cloud computing is identified as a critical concept for data engineers to understand, as it delineates which security and operational aspects fall to the cloud provider versus the customer.

## Recovery and Reliability

The book introduces two key metrics for system reliability: Recovery Time Objective (RTO) and Recovery Point Objective (RPO). RTO represents the maximum acceptable time for a service outage, varying based on business impact—from minutes for critical online systems to days for internal reporting. RPO defines the acceptable state after recovery, acknowledging that data is often lost during outages.

Data processing systems handle concurrency differently. PostgreSQL uses row locking, which can degrade performance but ensures consistency. In contrast, Google BigQuery employs a point-in-time full table commit model where read queries access the latest committed snapshot and don't see subsequent changes, avoiding table locks but queuing write operations to maintain consistency.

For historical data access, modern systems offer "time-travel" capabilities with varying retention periods. BigQuery maintains a seven-day history window, while Databricks typically retains data indefinitely until manually vacuumed (which becomes important for controlling storage costs).

## Serving Data and Future Trends

The book explores various approaches to making data accessible for analysis and applications. It introduces the concept of materialized views, which precompute some or all of a view's data in advance to improve query performance. Data virtualization and federated queries allow systems to query data across multiple sources without ingestion, with tools like Trino and Presto leading in this space.

Reverse ETL emerges as a growing practice, described as the process of sending data from analytics systems back to source systems. This approach enables operational analytics, where data drives immediate action rather than just retrospective insights.

Looking to the future, the book identifies several trends shaping data engineering. Data sharing features in platforms like Snowflake, Redshift, and BigQuery allow organizations to exchange data securely. Metrics layers (also called semantic layers or headless BI) provide tools for maintaining and computing business logic, with examples including Looker and dbt.

The book suggests that future data stacks will increasingly be powered by OLAP databases purpose-built for streaming, with systems like Druid, ClickHouse, Rockset, and Firebolt leading this evolution. These technologies will enable more real-time data applications and analytics capabilities.

## Conclusion: The Evolving Landscape of Data Engineering

"Fundamentals of Data Engineering" presents a comprehensive view of this rapidly evolving field, emphasizing that successful data engineers must balance technical depth with business understanding. The book illustrates how the boundaries between traditionally distinct components—data lakes and warehouses, batch and streaming, storage and compute—continue to blur as technologies mature.

The lessons from this book underscore that effective data engineering requires not just mastery of specific tools but a holistic understanding of data architectures, quality practices, and the business contexts in which data systems operate. As the field continues to evolve, the fundamental principles of designing for reliability, scalability, and business value remain constant guides for data engineering excellence.
