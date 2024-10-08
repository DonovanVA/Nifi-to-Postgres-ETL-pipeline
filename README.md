# Building a data lake from scratch

run
```bash
docker-compose up
```
Services can be found here:

Apache NiFi — http://localhost:8091/nifi/
Apache NiFi Registry — http://localhost:18080/nifi-registry/
Apache Airflow — http://localhost:8085/admin/
pgAdmin — http://localhost:5050/browser/
minIO — http://localhost:9000/

pgAdmin dummy password:
password

## Tasks:
### 1. Create an ETL Pipeline with Airflow and NiFi
Goal: Build an end-to-end pipeline to extract data from a source (e.g., an API or a file in Minio), transform it, and load it into a Postgres database.
Steps:
Use NiFi to fetch data from Minio or an external API.
Process the data (e.g., cleaning, transformation) using NiFi processors.
Store the processed data in the Postgres database.
Schedule the ETL pipeline using Airflow with DAGs to orchestrate the NiFi job.

### 2. Set Up Data Versioning with NiFi Registry
Goal: Showcase how to track and manage different versions of your NiFi dataflows.
Steps:
Create a simple NiFi dataflow and commit it to the NiFi Registry.
Update the dataflow (e.g., by adding or modifying processors).
Push the new version to the NiFi Registry and demonstrate how to roll back to a previous version if necessary.

### 3. Data Lake Storage and Retrieval using Minio
Goal: Demonstrate the usage of object storage for storing and retrieving large datasets.
Steps:
Upload a large dataset (e.g., CSV, JSON files) to Minio.
Write a script or a NiFi dataflow to retrieve and process the data from Minio.
Store the processed data in Postgres or another external system.

### 4. Data Monitoring and Alerts with Airflow
Goal: Set up monitoring and alerting for the ETL pipeline.
Steps:
Use Airflow's monitoring capabilities (such as task status, retries, and failure handlers).
Set up custom notifications (e.g., via email or a messaging service like Slack) for failed tasks.
Use health checks to ensure that all services (Postgres, NiFi, Minio, etc.) are up and running.

### 5. Backup and Restore Postgres Database
Goal: Demonstrate how to back up and restore the Postgres database as part of a disaster recovery plan.
Steps:
Use pg_dump to back up the Postgres database to Minio.
Simulate a database failure by deleting some data.
Restore the database from the backup stored in Minio.

### 6. Data Consistency Validation
Goal: Validate the consistency and correctness of the data loaded into Postgres after it is processed.
Steps:
Create validation checks (e.g., using SQL queries) to ensure data integrity after each step of the ETL pipeline.
Automate these checks in Airflow to run after each pipeline execution.

### 7. NiFi Cluster Setup
Goal: Showcase high availability and load balancing with a NiFi cluster.
Steps:
Set up multiple NiFi nodes and configure clustering with Zookeeper.
Distribute the data processing workload across the cluster.
Monitor the performance and ensure fault tolerance.

### 8. Automate Dataflow Deployment using Airflow
Goal: Automate the deployment of NiFi dataflows using Airflow.
Steps:
Create a DAG in Airflow that automatically uploads new dataflows to NiFi using the NiFi API.
Schedule deployments and automate updates to dataflows across environments (e.g., dev to prod).
