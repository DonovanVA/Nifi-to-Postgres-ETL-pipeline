# Building a data lake from scratch

run
```bash
docker-compose up
```
Services can be found here:

Apache NiFi — `http://localhost:8091/nifi/`
Apache NiFi Registry — `http://localhost:18080/nifi-registry/`
pgAdmin — `http://localhost:5050/browser/`

pgAdmin dummy password: `password`

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
![Screenshot 2024-10-13 181814](https://github.com/user-attachments/assets/16f828fd-63fa-4108-b418-51e993ef433e)

For these 2 processors I have made the commands to insert and query each:
1. PutSQL:

![Screenshot 2024-10-13 182236](https://github.com/user-attachments/assets/a4294482-7613-4cf4-9cd8-ddc700aa0daf)

```sql
INSERT INTO demo_table (column1, column2) VALUES ("test1", "test2");
```
2. ExecuteSQL

![Screenshot 2024-10-13 182404](https://github.com/user-attachments/assets/3d6d2179-b803-45b2-bc87-142379b990fd)

```sql
SELECT * FROM demo_table;
```
### 2. Create an ETL Pipeline with Airflow and NiFi
Use NiFi to fetch data from Minio or an external API.
Process the data (e.g., cleaning, transformation) using NiFi processors.

How I did it:
Created a simple txt file generator using generateflowfile and then store it in DB and the nifi_container in `/opt/nifi/nifi-current/output`
![Screenshot 2024-10-13 190844](https://github.com/user-attachments/assets/bf364476-ec40-4644-bf5c-00eecafcadee)

```bash
docker exec -it nifi_container /bin/bash
cd /opt/nifi/nifi-current/output
ls
```

Then check if the data is written to the file specified in the container in `/opt/nifi/nifi-current/output`
![Screenshot 2024-10-13 174010](https://github.com/user-attachments/assets/4fa5a1db-3b8d-44dc-8ff7-09e96af4a389)

