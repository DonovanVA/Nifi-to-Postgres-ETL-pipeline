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

postgres connection is done via:

host: mypostgres
username: postgres
password: postgres
jdbc:postgresql://mypostgres:5432/postgres

minio username:
minio_admin
minio password:
minio_password

## Tasks:
### 1. Create an ETL Pipeline with Airflow and NiFi
Use NiFi to fetch data from Minio or an external API.
Process the data (e.g., cleaning, transformation) using NiFi processors.

How I did it:

```bash
docker exec -it nifi_container /bin/bash
cd /opt/nifi/nifi-current/output
ls
```

## 2. CI/CD,Database
- Showcase Read and write using the processed data in the Postgres database.
- commit it to the NiFi Registry.

How I did it:
I have create a dummy table before hand using pgadmin query tool
```sql
CREATE TABLE IF NOT EXISTS demo_table (
    id SERIAL PRIMARY KEY,
    column1 TEXT,
    column2 TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

For these 2 processors I have made the commands to insert and query each:
1. PutSQL:
```sql
INSERT INTO demo_table (column1, column2) VALUES ("test1", "test2");
```
2. ExecuteSQL
```sql
SELECT * FROM demo_table;
```
