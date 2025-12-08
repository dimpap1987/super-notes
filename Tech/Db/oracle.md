***** CREATE ORACLE USER *****

Ανοίγω cmd και εκτελώ την εντολή:
1)sqlplus system - admin1234
2)Δίνω κωδικό
3)alter session set "_ORACLE_SCRIPT"=true;
4)create user HYBRID identified by 12345;
5)grant all privileges to HYBRID;

- login ? 
    - conn <username>/<pass>


## how to delete user(table) in oracle
- DROP USER HYBRID CASCADE

select @@global.max_connections;


## check sessions
SELECT SID, SERIAL#, USERNAME, STATUS, MACHINE FROM V$SESSION WHERE USERNAME IS NOT NULL;

## How to restart oracle db

you need to be logged in as a oracle user

```bash
su oracle

sqlplus / as sysdba
SQL>  shutdown
SQL> startup
```

## Tablespaces

A tablespace in a relational database is a storage location where the actual data underlying database objects can be kept. It provides a layer of abstraction between the physical data storage and the logical database objects, like tables and indexes.

```sql
SELECT *
FROM DBA_TABLESPACES;
```