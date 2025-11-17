# Appendix D: Data Type Mapping

The following pages contain tables showing the data type mapping.
## Data Type Mapping between Database Engine 20,
## teradataml DataFrame dtypes, and Python
This table lists the data type mapping between Database Engine 20, teradataml DataFrame dtypes,
and Python.
| Database Engine 20 Data Type | teradataml DataFrame dtypes | Python |
| ---------------------------- | --------------------------- | ------ |
| BIGINT | int | int |
| BLOB(n) | bytes | bytes |
| BYTE(n) | bytes | bytes |
| BYTEINT | int | int |
| CHAR(n) | str | int |
| CLOB(n) | str | str |
| DATE FORMAT 'YY/MM/DD' | datetime.date | datetime |
| DECIMAL(p,q) / Numeric | decimal.decimal | decimal |
| DOUBLE PRECISION | float | decimal |
| FLOAT | float | float |
| INTEGER | int | int |
| INTERVAL DAY(n) | str | str |
| INTERVAL DAY(n) TO MINUTE | str | str |
| INTERVAL DAY(n) TO SECOND(n) | str | str |
| INTERVAL HOUR(n) | str | str |
| INTERVAL MINUTE(n) | str | str |
| INTERVAL MONTH(n) | str | str |
| INTERVAL SECOND(p,q) | str | str |
| INTERVAL YEAR(n) | str | str |
| NUMBER(p,q) | decimal.decimal | decimal |
| PERIOD(DATE) | str | str |
| PERIOD(TIME(n)) | str | str |
| PERIOD(TIME(n) WITH TIME ZONE) | str | str |
| PERIOD(TIMESTAMP(n) WITH TIME ZONE) | str | str |
| REAL | float | float |
| SMALLINT | int | int |
| TIME(n) | datetime.time | datetime |
| TIME(n) WITH TIME ZONE | datetime.time | datetime |
| TIMESTAMP(n) | datetime.datetime | datetime |
| TIMESTAMP(n) WITH TIME ZONE | datetime.datetime | datetime |
| VARBYTE(n) | bytes | bytes |
| VARCHAR(n) | str | str |
| XML | str | str |