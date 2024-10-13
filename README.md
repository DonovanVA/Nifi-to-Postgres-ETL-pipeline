# Nifi to Postgres ETL pipeline
A simple Nifi-Postgres-ETL pipeline I created to demonstrate my understanding of data pipelines, Run:
```bash
docker-compose up
```
Services can be found here:

- Apache NiFi — `http://localhost:8091/nifi/`
- Apache NiFi Registry — `http://localhost:18080/nifi-registry/`
- pgAdmin — `http://localhost:5050/browser/`
- Airflow - `http://localhost:8085/admin/`
* The pgadmin dummy password is `password`

postgres connection is done via:

- host: `mypostgres`
- username: `postgres`
- password: `postgres`
- URL: `jdbc:postgresql://mypostgres:5432/postgres`

![Screenshot 2024-10-13 181531](https://github.com/user-attachments/assets/d9dd751d-8db5-46e3-83c8-34e1e9c766d9)


## Tasks:

### 1. NIFI-Database connection
- Showcase Read and write using the processed data in the Postgres database.

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

Preconfigure the DB connection pool in nifi
(In Database driver connection remember to add `/opt/nifi/nifi-current/jdbc/postgresql-42.7.4.jar`):
![Screenshot 2024-10-13 192426](https://github.com/user-attachments/assets/d1961720-2bf5-4946-a57b-153969750024)


For these 2 processors I have made the commands to insert and query each:
1. PutSQL:

![Screenshot 2024-10-13 182236](https://github.com/user-attachments/assets/a4294482-7613-4cf4-9cd8-ddc700aa0daf)

```sql
INSERT INTO demo_table (column1, column2) VALUES ('test1', 'test2');
```
2. ExecuteSQL

![Screenshot 2024-10-13 182404](https://github.com/user-attachments/assets/3d6d2179-b803-45b2-bc87-142379b990fd)

```sql
SELECT * FROM demo_table;
```
### 2. Create an ETL Pipeline with NiFi and CI/CD workflows
Use NiFi to fetch data from Minio or an external API.
Process the data (e.g., cleaning, transformation) using NiFi processors.

How I did it:
Create a new process group, then I made the following workflow
Created a simple txt file generator using generateflowfile and then store it in Database and the `nifi_container` in `/opt/nifi/nifi-current/output`
![Screenshot 2024-10-13 190844](https://github.com/user-attachments/assets/bf364476-ec40-4644-bf5c-00eecafcadee)

To check for the written file in the container:
```bash
docker exec -it nifi_container /bin/bash
cd /opt/nifi/nifi-current/output
ls
```
![Screenshot 2024-10-13 174010](https://github.com/user-attachments/assets/4fa5a1db-3b8d-44dc-8ff7-09e96af4a389)

Begin versioning (CI/CD) the workflow:
Created a bucket in Nifi registry (`settings`-> `New Bucket`), then in nifi I added a nifi registry client connection (3 bars top right ->`controller settings` -> `+`)

In Nifi Registry:
![Screenshot 2024-10-13 194603](https://github.com/user-attachments/assets/a87679de-6cac-44ef-b412-469c3e7d2253)

In Nifi:
![Screenshot 2024-10-13 192020](https://github.com/user-attachments/assets/dba4cb34-6f41-4281-89ce-84acfc787cf7)

Right click on the process group -> `start version control`, select the client if version control is successful, a green arrow would appear
![Screenshot 2024-10-13 192150](https://github.com/user-attachments/assets/c447a3c8-6fea-4ea2-bbdc-53376e8f8d4d)

The registry will then be updated with the new process in the bucket
![Screenshot 2024-10-13 194551](https://github.com/user-attachments/assets/7c5962d7-47e2-4f1a-bc82-acc897e0489f)

### 3. Airflow monitoring
Monitor NIFI using Apache-airflow DAGs

Install requirements,
```bash
pip install -r requirements.txt
```

then in line 19 of `/airflow/dags/nifi.py` change the process id to yours

Create a new monitoring connection in airflow UI `http://localhost:8085/admin/`
![Screenshot 2024-10-13 193359](https://github.com/user-attachments/assets/2e49d4f1-7d2b-4df9-8cd9-cb164122a1ba)
you will then be able to see the DAG in the airflow UI
