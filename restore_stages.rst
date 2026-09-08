Restore Stages
==============

When :program:`myloader` start to restore, it needs to execute multiple task and reach milestone.

Schema / Database Level Objects
-------------------------------

Schema Dropping: Runs DROP DATABASE IF EXISTS for each database (if overwrite options :option:`--drop-database <myloader --drop-database>` are enabled).

Schema Creation: Reads <db>-schema-create.sql files and executes CREATE DATABASE.

Base Table Structure (Pre-Data)
-------------------------------

Table Dropping: Executes DROP TABLE IF EXISTS in the target database if overwrite flags are active.

Main Table Creation: Reads <db>.<table_name>-schema.sql files and takes all the standard KEY, INDEX and CONSTRAINTS lines it stripped from the original CREATE TABLE and rewrites them into an ALTER TABLE <table> ADD (KEY/CONSTRAINT)... statements and saves them in different variable and only runs the new CREATE TABLE statements. This builds base table definitions and primary/unique key structures required for data ingestion.

Bulk Data Loading (Data Load)
-----------------------------

Data Insertion: Reads chunked data files (<db>.<table_name>.<chunk>.sql) and inserts rows concurrently using multiple worker threads.

The Secondary Indexes (Post-Table Data Load)
--------------------------------------------

Execution: By default, this reconstructed ALTER TABLE statement is kept in memory and executed immediately after all parallel data chunks for that specific table have finished loading.

Why it's split: Rebuilding secondary indexes at the end via MySQL's "Fast Index Creation" (which sorts the data and builds the index in bulk) is significantly faster than updating the index tree row-by-row during millions of INSERT operations.

Post-Data Objects and Constraints
---------------------------------

Foreign Keys: Executes <db>.<table_name>-schema-post.sql files using ALTER TABLE ... ADD CONSTRAINT to prevent lockups and severe performance degradation during INSERT operations.

View Creation: Reads and executes view definitions (<db>-schema-view.sql or <db>.<view_name>-schema.sql). Deferred until after tables are populated to prevent missing dependency errors.

Trigger Creation: Runs <db>.<table_name>-schema-triggers.sql to build triggers. Postponed to avoid firing triggers unnecessarily during bulk data loading.

Stored Procedures & Functions: Executes CREATE PROCEDURE and CREATE FUNCTION statements from schema metadata.

Event Creation: Executes CREATE EVENT statements defined in metadata files.
