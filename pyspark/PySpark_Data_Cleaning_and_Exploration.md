#pspark learnings
# PySpark Data Cleaning & Exploration – Cheat Sheet

> **Purpose**: Practical, commonly-used PySpark operations for reading, exploring, and cleaning data.
>
> **Audience**: Beginner → Intermediate (with SQL / Pandas background)
>
> **Dataset example**: Medical / Airline data

---

## 1. Spark Setup

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("DataCleaning") \
    .getOrCreate()
```

**Meaning**: Entry point to Spark. Required before any Spark operation.

---

## 2. Reading Data

### Read CSV

```python
df = spark.read.csv(
    "data.csv",
    header=True,
    inferSchema=True
)
```

* `header=True` → first row is column names
* `inferSchema=True` → Spark guesses data types

---

## 3. Basic Exploration (Equivalent to df.info())

### View data

```python
df.show(5)
```

### Schema & data types

```python
df.printSchema()
```

### Row count

```python
df.count()
```

---

## 4. Column Selection

```python
df.select("Name", "Age", "Gender")
```

**Meaning**: Choose required columns only (memory & performance friendly).

---

## 5. Filtering Rows (WHERE)

```python
from pyspark.sql.functions import col

df.filter(col("Age") > 60)
```

**Meaning**: Row-level filtering (SQL WHERE).

---

## 6. Creating / Updating Columns

### withColumn + when (IF / ELSE)

```python
from pyspark.sql.functions import when

df = df.withColumn(
    "senior_flag",
    when(col("Age") >= 60, "Yes").otherwise("No")
)
```

**Mental map**:

* SQL → CASE WHEN
* NumPy → np.where

---

## 7. Renaming & Dropping Columns

### Rename

```python
df = df.withColumnRenamed("Billing Amount", "billing_amount")
```

### Drop

```python
df = df.drop("Room Number")
```

---

## 8. Null Handling

### Check nulls

```python
df.select([col(c).isNull().alias(c) for c in df.columns]).show()
```

### Drop rows with nulls

```python
df.dropna()
df.dropna(subset=["Age", "billing_amount"])
```

### Fill nulls

```python
df = df.fillna({"Gender": "Unknown"})
df = df.fillna(0, subset=["billing_amount"])
```

---

## 9. Duplicate Handling

### Remove exact duplicates

```python
df.distinct()
```

### Remove duplicates using keys

```python
df.dropDuplicates(["Name", "Date of Admission"])
```

---

## 10. Sorting & Sampling

### Sort

```python
df.orderBy(col("billing_amount").desc())
```

### Limit rows

```python
df.limit(10)
```

---

## 11. Aggregation & Grouping

```python
from pyspark.sql.functions import avg, count

df.groupBy("Medical Condition") \
  .agg(
      avg("billing_amount").alias("avg_bill"),
      count("Name").alias("patient_count")
  )
```

**Meaning**: GROUP BY + aggregations.

---

## 12. Date Handling

### Convert string to date

```python
from pyspark.sql.functions import to_date

df = df.withColumn(
    "Date of Admission",
    to_date(col("Date of Admission"), "yyyy-MM-dd")
)
```

### Date difference

```python
from pyspark.sql.functions import datediff

df = df.withColumn(
    "length_of_stay",
    datediff(col("Discharge Date"), col("Date of Admission"))
)
```

---

## 13. Text Cleaning Functions (VERY COMMON)

### Trim spaces

```python
from pyspark.sql.functions import trim

df = df.withColumn("Doctor", trim(col("Doctor")))
```

### Case formatting

```python
from pyspark.sql.functions import lower, upper, initcap

lower(col("Name"))
upper(col("Name"))
initcap(col("Name"))
```

---

## 14. Regex Cleaning (Key Tool)

### Remove extra spaces

```python
regexp_replace(col("Name"), "\\s+", " ")
```

### Remove numbers

```python
regexp_replace(col("Name"), "[0-9]", "")
```

### Remove special characters

```python
regexp_replace(col("Name"), "[^A-Za-z ]", "")
```

### Remove prefixes (Dr., Mr., Ms.)

```python
regexp_replace(col("Doctor"), "^(Dr\\.|Mr\\.|Ms\\.)\\s*", "")
```

### Extract numeric values

```python
from pyspark.sql.functions import regexp_extract

regexp_extract(col("Room Number"), "[0-9]+", 0)
```

---

## 15. SQL-like Expressions (Quick Transformations)

```python
df.selectExpr(
    "Name",
    "billing_amount",
    "billing_amount * 1.1 as inflated_bill"
)
```

---

## 16. Core Rules to Remember

* Spark is **lazy** → executes only on actions (`show`, `count`)
* DataFrames are **immutable** → every change returns a new DataFrame
* Prefer **column expressions** over Python loops
* Avoid `collect()` on large data

---

## 17. 90% Useful Cleaning Functions (Must Know)

```python
select()
filter()
withColumn()
withColumnRenamed()
drop()
dropna() / fillna()
dropDuplicates()
trim()
lower() / upper() / initcap()
regexp_replace()
to_date()
```

---


