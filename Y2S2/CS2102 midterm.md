```sql
-- false if both are null, true if one is null
x IS DISTINCT FROM y
```

#### § Database Modifications

```sql
-- create / drop Table
CREATE TABLE Students(
  studentId	INTEGER, name VARCHAR(100), birthDate DATE
);
DROP TABLE IF EXISTS TableName CASCADE;
-- insert
INSERT INTO Students values (123, 'Alice', '2000-01-01');
-- delete
DELETE FROM Students WHERE studentId = 123;
-- update
UPDATE Accounts SET balance = balance * 1.02 WHERE accountId = 123;
-- modify schema
ALTER TABLE Students ALTER COLUMN dept DROP DEFAULT;
ALTER TABLE Students DROP COLUMN dept;
ALTER TABLE Students ADD COLUMN faculty VARCHAR(20);
ALTER TABLE Students ADD CONSTRAINT grade FOREIGN KEY (grade) REFERENCES Grades;
```

#### § Constraints

1. Not Null

2. Unique: $\text{null}$ does not violate unique constraint

3. Primary key

4. Foreign Key: `MATCH FULL`: cannot be partially null. Foreign key need not be primary key, but must be unique

5. Check

##### Violations:

1. `NO ACTION`: rejects delete/update, default action
2. `RESTRICT`: similar to `NO ACTION` except constraint checking cannot be deferred
3. `CASCADE`: propagates delete/update to referencing tuples
4. `SET DEFAULT`: updates to some default value
5. `SET NULL`: updates to $\text{null}$

##### Deferrable constraints

Checking of constraints can be deferred to the end of transaction `DEFERRABLE INITIALLY DEFERRED/IMMEDIATE`

```sql
CREATE TABLE Employees(
	eid INTEGER PRIMARY KEY, ename VARCHAR(100), managerId INTEGER,
  CONSTRAINT employees_fk FOREIGN KEY(managerId) REFERENCES Employees
  	DEFERRABLE INITIALLY IMMEDIATE
);
BEGIN
	SET CONSTRAINT employees_fk DEFERRED; -- only for this transaction
	DELETE FROM Employees WHERE eid = 2;
	UPDATE Employees SET managerId = 1 WHERE eid = 3;
COMMIT
```

##### Set Operations

```sql
UNION / INTERSECT / EXCEPT -- eliminate duplicates
UNION ALL / INTERSECT ALL / EXCEPT ALL
```

`INTERSECT` has higher precedence

#### § Multi-Relation Queries

```sql
-- cross join
FROM r1 AS A, r2 AS B -- alias
FROM r1 CROSS JOIN r2
-- inner join
FROM r1 INNER JOIN / JOIN r2 ON r1.a1 = r2.a1
-- natural join
FROM r1 NATURAL JOIN r2
-- left/right outer join
FROM r1 LEFT / RIGHT [OUTER] JOIN r2
-- natural left/right outer join
FROM r1 NATURAL LEFT / RIGHT [OUTER] JOIN r2
```

#### § Subqueries

##### `EXISTS`

Returns $\text{true}$ if output non-empty, otherwise returns $\text{false}$

```sql
WHERE [NOT] EXISTS (SELECT 1 FROM ... WHERE ...)
```

##### `IN`

Returns $\text{false}$ if output empty, otherwise returns $((v=v_1) \lor (v=v_2) \lor \dots \lor (v=v_n))$, subquery must return exactly one column

```sql
WHERE v IN (SELECT a FROM r WHERE ...)
WHERE v IN (v1, v2, ..., vn) -- alternative form
```

##### `ANY/SOME`

Returns $\text{false}$ if output empty, otherwise returns $((v\text{ op }v_1) \lor (v\text{ op }v_2) \lor \dots \lor (v\text{ op }v_n))$, subquery must return exactly one column

```sql
WHERE v op ANY(SELECT a FROM r WHERE ...)
```

##### `ALL`

Returns $\text{true}$ if output empty, otherwise returns $((v\text{ op }v_1) \land (v\text{ op }v_2) \land \dots \land (v\text{ op }v_n))$, subquery must return exactly one column

```sql
WHERE v op ALL(
  SELECT a FROM r
  WHERE a IS NOT NULL -- take care of NULL
  AND ...
)
```

##### Row Constructors

`IN/ANY/ALL` can use subqueries the return more than one column

```sql
WHERE row(a1, a2) IN/op ANY/op ALL(
  SELECT a1, a2 FROM r WHERE ...
)
```

##### Scalar Subqueries

Subquery that returns at most one tuple with one column or `NULL`, can be used as a scalar expression

##### Usage of Subqueries

- `WHERE` clause

- `FROM` clause, must be enclosed in parentheses and assigned alias

	```sql
	FROM r1 NATURAL JOIN (
	  SELECT a1 FROM r2 WHERE ...
	) AS r
	```

- `HAVING` clause

#### § Aggregate Functions

Computes a single value from a set of tuples, used in `SELECT`, `HAVING`, `ORDER BY` or subqueries

- `MIN/MAX/AVG/SUM`: returns the same for empty relation and relation with only `NULL` values
- `COUNT(A)`: returns number of non-null values
- `COUNT(*)`: returns number of rows

#### § `GROUP BY` & `HAVING` Clauses

In $\texttt{GROUP BY} \, a_1, \dots, a_n$, two tuples $t$ and $t'$ belongs to the same group if $\forall i \in [1, n], \, t.a_i \texttt{ IS NOT DISTINCT FROM } t'.a_i$
For each column $A$ in $R$ that appears in `SELECT` or `HAVING`, one of the following must hold:

1. $A$ appears in `GROUP BY`
2. $A$ appears in an aggregated expression
3. Primary key of $R$ appears in `GROUP BY`

#### § Conditional Expressions

##### `CASE`

```sql
CASE [expression]
	WHEN condition_1/value_1 THEN result_1
	...
	WHEN condition_n/value_n THEN result_n
	ELSE result_0
END
```

##### `COALESCE`

`COALESCE(first, second, third)`
Returns first non-null value in arguments, returns `NULL` if all arguments are `NULL`

##### `NULLIF`

`NULLIF(value_1, value_2)`
Returns `NULL` if `value_1` is equal to `value_2`, otherwise returns `value_1`

#### § Evaluation of Queries

```sql
SELECT select-list FROM from-list
WHERE where-condition GROUP BY group-by-list
HAVING having-condition ORDER BY order-by-list
LIMIT ... OFFSET ...
```

1. Compute cross-product in `from-list`
2. Select tuples for `where-condition`
3. Partition using `group-by-list`
4. Select groups with `having-condition`
5. Generate output for attributes in `select-list`
6. Remove duplicate tuples
7. Sort output tuples based on `order-by-list`
8. Remove tuples based on `OFFSET` and `LIMIT`

#### § Miscellaneous

##### Common Table Expressions (CTEs)

```sql
WITH
	R1 AS (Q1),
	...,
	RN AS (Qn)
SELECT / INSERT / UPDATE / DELETE ...
```

##### Views

```sql
CREATE VIEW v(b1, b2) AS -- rename columns
	SELECT ...
```

##### Pattern Matching

`-`: single character
`%`: sequence of 0 or more characters
`SIMILAR TO`: regex

##### Universal Quantification

A pair of `NOT EXISTS` to replace "for all"
