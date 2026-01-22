```sql
-- Syntax for creating procedure in postgresql
CREATE OR REPLACE PROCEDURE pr_name (p_name varchar, p_age int)
language plpgsql
as $$
declare 
  variable
begin
  procedure body - all logics
end;
$$
```

```sql
-- syntax for creating procedure in oracle plsql
create or replace procedure pr_name (p_name varchar, p_age int)
as
  variable
begin
  procedure body  - all logics
end;
```

```sql
--  mysql syntac for creating procedure

delimiter $$
create or alter procedure pr_name(@p_name varchar, @p_age int)
as
  declare @v_name varchar,
          @v_age int;
begin
  procedure body - all logics
end $$
```
