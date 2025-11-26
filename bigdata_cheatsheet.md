# Big Data Cheat Sheet

A compact reference for the essentials of **Big Data**, covering Spark (PySpark + sparklyr), distributed systems, storage formats, parallelization, cluster concepts, and SQL-on-Big-Data engines.

---

# 1. What is Big Data? (very short)

- **Volume** → large datasets  
- **Velocity** → fast incoming data  
- **Variety** → structured, semi-structured, unstructured  
- **Veracity** → data quality issues  
- **Value** → insights extracted

Big Data = data that **cannot be processed efficiently on a single machine**.

---

# 2. Apache Spark Essentials

## Start a Spark session

### Python
```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("myApp").getOrCreate()
```

### R
```r
library(sparklyr)
spark_install()
sc <- spark_connect(master = "local")
```

---

# 3. Loading Data

## Python
```python
df = spark.read.csv("file.csv", header=True, inferSchema=True)
df = spark.read.parquet("file.parquet")
```

## R
```r
df <- spark_read_csv(sc, "df", "file.csv", header=TRUE)
```

---

# 4. Spark DataFrame Operations

## Python
```python
df.printSchema()
df.select("col")
df.filter(df.col > 0)
df.groupBy("cat").count()
df.orderBy("col")
```

## R
```r
df %>% select(col)
df %>% filter(col > 0)
df %>% group_by(cat) %>% summarise(n = n())
df %>% arrange(col)
```

---

# 5. Spark SQL

## Enable SQL
```python
df.createOrReplaceTempView("table")
spark.sql("SELECT * FROM table WHERE value > 100")
```

## R
```r
tbl(sc, "df") %>% filter(value > 100)
```

---

# 6. Storage Formats (Big Data)

| Format | Use Case | Pros | Cons |
|--------|----------|------|------|
| **CSV** | simple | human-readable | slow, big, no schema |
| **JSON** | nested | flexible | heavy, slow |
| **Parquet** | analytics | compressed, columnar | not human-readable |
| **ORC** | Hive/Spark | highly optimized | ecosystem-specific |
| **Avro** | streaming | schema evolution | row-based |

**Recommended:** Parquet for most analytics.

---

# 7. Parallelism Concepts

- **Executor** → worker process  
- **Core** → CPU unit  
- **Partition** → chunk of data  
- **Task** → unit of work  
- **Shuffle** → data reorganization (expensive!)  

Rule of thumb: *More partitions = more parallelism but more overhead*.

---

# 8. MLlib Basics

## Python
```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.classification import LogisticRegression

vec = VectorAssembler(inputCols=["x","y"], outputCol="features")
df2 = vec.transform(df)

model = LogisticRegression(featuresCol="features", labelCol="label")
model.fit(df2)
```

## R
```r
library(sparklyr)
df2 <- sdf_mutate(df, features = c(x, y))
ml_logistic_regression(df2, features = features, label = label)
```

---

# 9. Distributed Computing Patterns

### Map → apply function on each row/block  
### Reduce → aggregate the results  

Spark generalizes this with **transformations** and **actions**.

---

# 10. Transformations vs Actions

| Transformations (lazy) | Actions (trigger computation) |
|-------------------------|-------------------------------|
| select() | show() |
| filter() | count() |
| groupBy() | collect() |
| withColumn() | take() |

---

# 11. Hive / SQL-on-Hadoop

Examples of engines:
- Hive  
- Presto / Trino  
- Impala  
- Spark SQL  

Typical usage:
```sql
SELECT category, COUNT(*)
FROM big_table
GROUP BY category;
```

---

# 12. Performance Tips

- Use **Parquet** instead of CSV  
- Filter early (predicate pushdown)  
- Avoid wide shuffles  
- Cache wisely (`df.cache()`)  
- Repartition based on data size

---

# 13. Spark Cluster Architecture (short)

- **Driver** → coordinator  
- **Executors** → workers  
- **Cluster Manager** → YARN, Kubernetes, Standalone  

Job → Stage → Task hierarchy.

---

# 14. Handy Commands

## Python
```python
df.count()
df.summary().show()
df.toPandas().head()
```

## R
```r
df %>% summarise_all(mean)
df %>% head()
```

---

# Mini Summary

- Use **Spark** for distributed processing  
- Use **Parquet** format  
- Avoid shuffles  
- Prefer SQL or DataFrame API  
- Use MLlib for big ML pipelines  

---
