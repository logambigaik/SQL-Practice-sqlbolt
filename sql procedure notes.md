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
  declare v_name varchar,
          v_age int;
begin
  procedure body - all logics
end $$
```

```sql
DROP TABLE IF EXISTS product;

CREATE TABLE product (
    product_code VARCHAR(20) PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    quantity_remaining INT NOT NULL,
    quantity_sold INT NOT NULL
);

INSERT INTO product (product_code, product_name, price, quantity_remaining, quantity_sold)
VALUES('P001', 'Laptop', 75000.00, 10, 5),
('P002', 'Mobile Phone', 30000.00, 25, 15),
('P003', 'Office Chair', 4500.00, 40, 10);

DROP TABLE IF EXISTS sales;

CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    product_code VARCHAR(20) NOT NULL,
    quantity_ordered INT NOT NULL,
    sale_date DATE NOT NULL,
    total_price NUMERIC(10,2),

    CONSTRAINT fk_product_code
        FOREIGN KEY (product_code)
        REFERENCES product(product_code)
);

INSERT INTO sales 
(product_code, quantity_ordered, sale_date, total_price)
VALUES
('P001', 2, '2024-01-10', 150000.00),
('P002', 3, '2024-01-12', 90000.00),
('P003', 5, '2024-01-15', 22500.00);

SELECT * FROM product, sales;



CREATE OR REPLACE PROCEDURE PR_BUY_PRODUCTS()
LANGUAGE PLPGSQL
AS $$
DECLARE 
  V_PRODUCT_CODE VARCHAR(20);
  V_PRICE float;
BEGIN
  SELECT product_code, price
  INTO V_PRODUCT_CODE, V_PRICE
  FROM product
  where product_name='Laptop';
  
  INSERT INTO SALES (sale_date, product_code, quantity_ordered, total_price)
  VALUES(current_date,V_PRODUCT_CODE,1,(V_PRICE *1));
  
  UPDATE product
  SET quantity_remaining = (quantity_remaining -1)
      ,quantity_sold = (quantity_sold +1)
  WHERE product_code = V_PRODUCT_CODE;
  
  RAISE NOTICE 'PRODUCT SOLD!';

END
$$;

CALL PR_BUY_PRODUCTS();

SELECT * FROM product  where product_name='Laptop';

```  




