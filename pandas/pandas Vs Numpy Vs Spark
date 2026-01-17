# 📘 End‑to‑End Data Analysis Cheat Sheet

**Pandas → NumPy → PySpark (Cleaning, Analysis, Windows, Joins)**

---

## 🔹 1. Pandas — Core Operations

### Inspect & Understand Data

```python
df.head()
df.tail()
df.info()
df.describe()
df.shape
```

**Meaning:** Understand structure, datatypes, nulls, ranges

---

### Select / Filter

```python
df[['col1','col2']]
df.loc[df['Age'] > 60]
df[df['Gender'] == 'Female']
```

**Meaning:** Row / column filtering

---

### GroupBy & Aggregate

```python
df.groupby('Hospital')['Billing Amount'].mean()
df.groupby('Doctor').agg({'Billing Amount':'sum','Name':'count'})
```

**Meaning:** Summarize data by categories

---

### Apply & Lambda

```python
df['flag'] = df.apply(lambda r: r['Age'] > 60, axis=1)
```

**Meaning:** Row‑wise custom logic

---

### Merge / Concat

```python
pd.merge(df1, df2, on='id', how='left')
pd.concat([df1, df2], axis=0)
```

**Meaning:** Combine datasets

---

## 🔹 2. NumPy — Vectorized Logic

### np.where

```python
np.where(df['Age'] > 60, 'Senior', 'Adult')
```

**Meaning:** If‑else logic at scale

---

### np.select

```python
conds = [df['Age'] > 60, df['Age'] > 40]
vals = ['Senior', 'Mid']
np.select(conds, vals, default='Young')
```

**Meaning:** Multiple conditional logic

---

### Boolean Masks

```python
df[df['price'] > df['price'].mean()]
```

**Meaning:** Fast filtering

---

## 🔹 3. PySpark — DataFrame Basics

### Read & Inspect

```python
spark.read.csv(path, header=True, inferSchema=True)
df.printSchema()
df.show(5)
```

**Meaning:** Load & inspect large data

---

### Select / Filter

```python
df.select('Name','Age')
df.filter(col('Age') > 60)
```

**Meaning:** Column & row selection

---

### withColumn

```python
df.withColumn('Age_plus_1', col('Age') + 1)
```

**Meaning:** Add / transform columns

---

### Regex Cleaning

```python
regexp_replace(col('Name'), '[^a-zA-Z ]', '')
```

**Meaning:** Clean text data

---

## 🔹 4. PySpark — GroupBy & Aggregation

```python
df.groupBy('Hospital') \
  .agg(
      F.count('*').alias('cnt'),
      F.avg('Billing Amount').alias('avg_bill')
  )
```

**Meaning:** Aggregate metrics

---

### HAVING‑like Filter

```python
df.groupBy('Hospital') \
  .agg(F.avg('Billing Amount').alias('avg_bill')) \
  .filter(col('avg_bill') > 50000)
```

---

## 🔹 5. PySpark — Window Functions

### Window Spec

```python
window = Window.partitionBy('Hospital').orderBy('Date of Admission')
```

---

### rank / row_number

```python
F.rank().over(window)
F.row_number().over(window)
```

**Meaning:** Ranking within groups

---

### lag / lead

```python
F.lag('Billing Amount').over(window)
F.lead('Billing Amount').over(window)
```

**Meaning:** Previous / next row values

---

### Running Total / Moving Avg

```python
F.sum('Billing Amount').over(window)
window.rowsBetween(-2,0)
```

---

## 🔹 6. PySpark — Joins

### Basic Join

```python
df1.join(df2, df1.id == df2.id, 'left')
```

### Join Types

* `inner` → matched only
* `left` → all left rows
* `right` → all right rows
* `outer` → everything

---

### Alias & Column Conflicts

```python
df1.alias('a').join(df2.alias('b'), col('a.id')==col('b.id'))
```

---

### Join + Aggregate

```python
df1.join(df2,'Name','left') \
   .groupBy('Hospital') \
   .agg(F.sum('Billing Amount'))
```

---

## 🔹 7. Execution Rules (Important)

### Order Matters

```
JOIN → FILTER → GROUPBY → ORDERBY → LIMIT → SHOW
```

### Action vs Transformation

* **Action:** show(), count(), collect()
* **Transformation:** filter(), withColumn(), groupBy(), join()

---

## 🔹 8. Key Mental Models

* `groupBy` → collapses rows
* `window` → preserves rows
* `lag/lead` → change detection
* `rank` vs `row_number` → ties vs strict count
* Missing join match → NULL (not blank)

---

✅ **You are now ready for the consolidated quiz**
