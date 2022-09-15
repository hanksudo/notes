# PostgreSQL Note

## Commands

<https://www.postgresql.org/docs/9.5/static/reference-client.html>

### Show work memory

```bash
SHOW work_mem;
SHOW maintenance_work_mem;
```

### Create Database with UTF-8 encoding

```bash
createdb -E UTF8 -T template0 --locale=en_US.utf8 <dbname> -O owner
```

### Connect to database

```bash
psql
psql -U user
psql -U postgres
psql -U postgres -h locahlost -d db
```

### backup database

```bash
pg_dump db_name > file
```

### restore

```bash
psql db_name < file
```

### backup and restore to another server

```bash
pg_dump db_name -h localhost -p 15432 | psql -h localhost -p 15433
pg_dump db_name --data-only -h localhost -p 15432 | psql -h localhost -p 15433
```

-T exclude table

### backup & recovery with compression

```bash
pg_dump dbname | gzip > filename.gz
gunzip -c filename.gz | psql dbname
```

### Monitoring

```bash
htop -u postgres
```

### Force kill hanging query

```
# check locked
SELECT relation::regclass, * FROM pg_locks WHERE NOT GRANTED;

SELECT * FROM pg_stat_activity WHERE status = 'active';
SELECT pg_terminate_backend(PID);
SELECT pg_cancel_backend(PID);
```

### Dump all databases and reload databases

```bash
pg_dumpall > db.out
psql -f db.out postgres
```

### Database

- `\l` - list all databases

- `\c db_name` - connect to database

- create database

```sql
CREATE DATABASE newdb WITH OWNER demorole ENCODING 'UTF8';
```

- change owner

```sql
ALTER DATABASE h2 OWNER TO h2;
```

- grant access

```sql
GRANT CONNECT ON DATABASE database_name TO username;
```

### Table

- `\dt` - list tables

- list columns on tables

```
\d table_name
```

- copy table to another db

```bash
pg_dump -t table_to_copy source_db | psql target_db
```

#### reassign all tables owner

<https://gist.github.com/hanksudo/f1365f3a764d3c8519cc>

### Permission

- `\z table_name` - Permission of table

#### Grant Permission

```
GRANT SELECT, UPDATE, INSERT ON table_name TO role_name;
GRANT SELECT ON table_name TO ROLE_NAME;
```

multiple tables

```
GRANT SELECT ON ALL TABLES IN SCHEMA public TO username;
```

#### Revoke Permission

```
REVOKE ALL ON table_name FROM role_name;
```

### Role

- `\du` - list roles

- create role

```
CREATE ROLE demorole WITH LOGIN ENCRYPTED PASSWORD 'password' CREATEDB;
CREATE ROLE replicator WITH REPLICATION LOGIN ENCRYPTED PASSWORD 'password';
```

- alter role

```
ALTER ROLE demorole CREATEROLE CREATEDB REPLICATION SUPERUSER;
ALTER ROLE demorole WITH ENCRYPTED PASSWORD '123456';
```

```sql
-- drop role
DROP ROLE demorole;
```

### Index

- list all indexes

```
SELECT * FROM pg_stat_all_indexes WHERE schemaname = 'public' ORDER BY indexrelname;
```

- list index of table

```
\d table_name
```

- create index

```
CREATE INDEX CONCURRENTLY diary_user_idx ON diary(user_id, id);
```

- drop index

```
DROP INDEX diary_user_idx;
```

### Other

- Show config file path

```
SHOW config_file;
```

- Show current setting

```
\set
```

- show the value of a run-time parameter

```
SHOW ALL;
SHOW SERVER_VERSION;
```

### SQLs

- alter foreign key

```sql
ALTER TABLE diary ADD FOREIGN KEY (meter_id) REFERENCES user_meter(id);
```

- list tables name

```sql
SELECT table_name FROM information_schema.tables
WHERE table_schema='public' AND table_type='BASE TABLE';
```

- update datetime

```sql
UPDATE diary SET recorded_at = recorded_at + INTERVAL '17 days'
```

- Distinct ON Multi columns

```sql
SELECT DISTINCT ON(glucose_value, recorded_at) glucose_value, recorded_at FROM diary WHERE user_id = 1 ORDER BY recorded_at;
```

- Regular Expression

```sql
SELECT * FROM product WHERE serial_number !~ '\w{15}';
SELECT * FROM product WHERE serial_number ~ '\w{15}';
```

- Explain Analyze

```sql
EXPLAIN ANALYZE select * from api_log;
```

- Find Largest table in the db

```sql
SELECT relname, relpages FROM pg_class ORDER BY relpages DESC;
```

- Sql function that returns a table with I/O infomation (obtained from pg_statio_user_indexes view) and filtered by current database and "public" schema

```sql
CREATE OR REPLACE FUNCTION indexes_io_for_current_database()
RETURNS TABLE(indexname name, idx_blks_read bigint, idx_blks_hit bigint) AS $$
    SELECT i.indexrelname, i.idx_blks_read, i.idx_blks_hit
    FROM pg_statio_user_indexes AS i
    WHERE i.relname IN (SELECT table_name FROM information_schema.tables WHERE table_schema = 'public');
$$ LANGUAGE SQL;
SELECT * FROM indexes_io_for_current_database() ORDER BY idx_blks_hit DESC LIMIT 5;
```

- Sql function that returns a table with index usage infomation (obtained from pg_stat_user_indexes view) and filtered by current database and "public" schema:

```sql
CREATE OR REPLACE FUNCTION indexes_stat_for_current_database()
RETURNS TABLE(indexname name, idx_scan bigint, idx_tup_read bigint, idx_tup_fetch bigint) AS $$
    SELECT i.indexrelname, i.idx_scan, i.idx_tup_read, i.idx_tup_fetch
    FROM pg_stat_user_indexes AS i
    WHERE i.relname IN (SELECT table_name FROM information_schema.tables WHERE table_schema = 'public');
$$ LANGUAGE SQL;

SELECT * FROM indexes_stat_for_current_database() ORDER BY idx_scan DESC LIMIT 5;
```

- Finding duplicate rows

```sql
SELECT id from (
  SELECT id,
  ROW_NUMBER() OVER(
    PARTITION BY first_name,
                 last_name,
                 email
              ORDER BY id
    ) AS user_row_number
  FROM users
) duplicates
WHERE
duplicates.user_row_number > 1
```

#### list all tables and indexes size

```sql
SELECT TABLE_NAME,
       pg_size_pretty(pg_relation_size(TABLE_NAME)) AS relation_size,
       pg_size_pretty(pg_total_relation_size(TABLE_NAME)) AS total_relation_size,
       pg_size_pretty(pg_indexes_size(TABLE_NAME)) AS indexes_size
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY pg_relation_size(TABLE_NAME) DESC;
```

#### Find unused index

```sql
SELECT relname, indexrelname, idx_scan
FROM   pg_catalog.pg_stat_user_indexes
WHERE  schemaname = 'public';
```

### Extensions

#### pg_stat_statements

add into `/etc/postgresql/9.4/main/postgresql.conf`

```
shared_preload_libraries = 'pg_stat_statements'    # (change requires restart)
pg_stat_statements.max = 10000
pg_stat_statements.track = all
```

```bash
sudo su - postgres
psql
```

```sql
\x
CREATE extension pg_stat_statements;
\dx
SELECT query, calls, total_time, rows, 100.0 * shared_blks_hit /
               nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent
          FROM pg_stat_statements ORDER BY total_time DESC LIMIT 5;
```

## .pgpass

```bash
127.0.0.1:5432:*:username:password
```

- [PostgreSQL: Documentation: 14: 34.16. The Password File](https://www.postgresql.org/docs/current/libpq-pgpass.html)

## compare two databases

```bash
sudo -u postgres pg_dump -f original.sql db_name
sudo -u postgres pg_dump -h 192.168.33.32 -f new.sql db_name
java -jar apgdiff-2.4.jar --ignore-start-with original.sql new.sql
```

## Remote query execution

```bash
psql <REMOTE POSTGRESQL URL> \
-c "copy (
      SELECT *
      FROM users
      WHERE date_trunc('month', created_at) = '2016-01-01'
    ) to stdout" \
| psql <LOCAL POSTGRESQL DATABASE> \
-c "copy users from stdin"
```

## Other References

- [PostgreSQL - Memory](http://michael.otacoo.com/manuals/postgresql/settings/memory/)
- [PostgreSQL: Documentation: 9.4: Transaction Isolation](http://www.postgresql.org/docs/9.4/static/transaction-iso.html)
- [PostgreSQL: Documentation: 9.4: Performance Tips](http://www.postgresql.org/docs/9.4/static/performance-tips.html)
- [PostgreSQL: Documentation: 9.4: Replication](http://www.postgresql.org/docs/9.4/static/runtime-config-replication.html)
- [PostgreSQL: Documentation: 9.4: The Statistics Collector](http://www.postgresql.org/docs/current/static/monitoring-stats.html)
- [Lock Monitoring - PostgreSQL wiki](https://wiki.postgresql.org/wiki/Lock_Monitoring)
- [PgTune](http://pgtune.leopard.in.ua/)
- [Improving PostgreSQL performance on AWS EC2](http://blog.2ndquadrant.com/postgresql_performance_on_ec2/)
- [Number Of Database Connections - PostgreSQL wiki](https://wiki.postgresql.org/wiki/Number_Of_Database_Connections)
- [PgBouncer - lightweight connection pooler for PostgreSQL](https://pgbouncer.github.io/)
- [How much RAM is PostgreSQL using?](http://www.depesz.com/2012/06/09/how-much-ram-is-postgresql-using/)
- [Database Isolation Levels And Their Effects on Performance and Scalability - High Scalability -](http://highscalability.com/blog/2011/2/10/database-isolation-levels-and-their-effects-on-performance-a.html)
- [PostgreSQL: Documentation: 9.4: Continuous Archiving and Point-in-Time Recovery (PITR)](http://www.postgresql.org/docs/9.4/static/continuous-archiving.html)
- [Zero to PostgreSQL streaming replication in 10 mins | Greg Reinacker's Weblog](http://www.rassoc.com/gregr/weblog/2013/02/16/zero-to-postgresql-streaming-replication-in-10-mins/)
- [12-Step Program for Scaling Web Applications on PostgreSQL](http://www.slideshare.net/kigster/12step-program-for-scaling-web-applications-on-postgresql)
- [omniti-labs/pgtreats · GitHub](https://github.com/omniti-labs/pgtreats)

## Trouble shooting

- [Re: How to monitor locks (max_pred_locks_per_transaction)?](http://www.postgresql.org/message-id/CADKuZZCgfVoRJ1UgUkiLNjfOh54-Hq5kkrAsiTBZPKz+pvYwjQ@mail.gmail.com)

```
out of shared memory
HINT:  You might need to increase max_pred_locks_per_transaction.
```

**The total 'SIReadLock' count must be less than max_pred_locks_per_transaction * max_connections.**

```sql
SELECT * FROM pg_locks WHERE mode = 'SIReadLock';
```

```bash
watch -n 3 "psql -c \"SELECT * FROM pg_locks WHERE mode='SIReadLock';\" -L SIReadLock.log"
grep -nEo '([0-9]+) rows' SIReadLock.log | sort -rn | head -n 1
```

**remaining connection slots are reserved for non-replication superuser connections**

```sql
select usename, client_addr, count(*) from pg_stat_activity 
group by client_addr, usename;
```
