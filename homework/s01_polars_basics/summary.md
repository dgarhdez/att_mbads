# Homework 1: Polars Basics - Concept Summary

This document summarizes the key concepts needed to complete Homework 1.

## Prerequisites

Before starting this homework, you should be comfortable with:
- Python fundamentals (variables, functions, data types)
- Pandas basics (DataFrames, Series)

## Key Concepts

### 1. Loading Data

```python
import polars as pl

# Read a CSV file
df = pl.read_csv("file.csv")
```

| Pandas | Polars |
|--------|--------|
| `pd.read_csv()` | `pl.read_csv()` |

### 2. Data Inspection

```python
df.shape          # (rows, columns)
df.columns        # Column names
df.schema         # Column name -> dtype mapping
df.head(n)        # First n rows
df.describe()     # Summary statistics
```

| Operation | Pandas | Polars |
|-----------|--------|--------|
| Shape | `df.shape` | `df.shape` |
| Columns | `df.columns` | `df.columns` |
| Data types | `df.dtypes` | `df.schema` |
| First rows | `df.head()` | `df.head()` |

### 3. Selecting Columns with `select()`

```python
# Select specific columns
df.select("col1", "col2")

# Using expressions
df.select(pl.col("col1"), pl.col("col2"))

# Select by data type
df.select(pl.col(pl.Int64))      # Integer columns
df.select(pl.col(pl.Float64))    # Float columns
df.select(pl.col(pl.String))     # String columns
```

| Pandas | Polars |
|--------|--------|
| `df[["col1", "col2"]]` | `df.select("col1", "col2")` |
| `df.select_dtypes(include=['int64'])` | `df.select(pl.col(pl.Int64))` |

### 4. Creating New Columns with `with_columns()`

```python
df.with_columns(
    (pl.col("a") + pl.col("b")).alias("sum"),
    (pl.col("c") / 1000).alias("c_scaled"),
    pl.col("name").str.to_uppercase().alias("name_upper")
)
```

| Pandas | Polars |
|--------|--------|
| `df["new"] = df["a"] + df["b"]` | `df.with_columns((pl.col("a") + pl.col("b")).alias("new"))` |

### 5. The Expression API

Expressions are descriptions of computations, not results:

```python
# This creates an expression, not data
expr = pl.col("salary") * 1.1

# The expression executes when used in select() or with_columns()
df.with_columns(expr.alias("salary_raised"))
```

### 6. Aggregations

```python
df.select(
    pl.col("value").mean().alias("avg"),
    pl.col("value").sum().alias("total"),
    pl.col("value").min().alias("minimum"),
    pl.col("value").max().alias("maximum"),
    pl.col("category").n_unique().alias("unique_count")
)
```

| Operation | Pandas | Polars |
|-----------|--------|--------|
| Mean | `df["col"].mean()` | `pl.col("col").mean()` |
| Sum | `df["col"].sum()` | `pl.col("col").sum()` |
| Unique count | `df["col"].nunique()` | `pl.col("col").n_unique()` |

### 7. String Operations

```python
pl.col("text").str.to_uppercase()    # UPPERCASE
pl.col("text").str.to_lowercase()    # lowercase
pl.col("text").str.len_chars()       # Length
pl.col("text").str.contains("x")     # Contains substring
```

| Pandas | Polars |
|--------|--------|
| `df["col"].str.upper()` | `pl.col("col").str.to_uppercase()` |
| `df["col"].str.lower()` | `pl.col("col").str.to_lowercase()` |
| `df["col"].str.len()` | `pl.col("col").str.len_chars()` |

### 8. Saving to Parquet

```python
# Write to Parquet
df.write_parquet("output.parquet")

# Read from Parquet
df = pl.read_parquet("output.parquet")
```

| Pandas | Polars |
|--------|--------|
| `df.to_parquet("file.parquet")` | `df.write_parquet("file.parquet")` |
| `pd.read_parquet("file.parquet")` | `pl.read_parquet("file.parquet")` |

## Bonus: Conditional Logic (Preview of Session 2)

```python
pl.when(condition)
  .then(value_if_true)
  .otherwise(value_if_false)
```

Example:
```python
df.with_columns(
    pl.when(pl.col("energy") >= 0.8)
      .then(pl.lit("High Energy"))
      .when(pl.col("energy") >= 0.5)
      .then(pl.lit("Medium Energy"))
      .otherwise(pl.lit("Low Energy"))
      .alias("energy_category")
)
```

## Quick Reference

| Task | Polars Code |
|------|-------------|
| Load CSV | `pl.read_csv("file.csv")` |
| Get shape | `df.shape` |
| Select columns | `df.select("col1", "col2")` |
| Add column | `df.with_columns(expr.alias("name"))` |
| Aggregations | `df.select(pl.col("x").mean())` |
| Save Parquet | `df.write_parquet("file.parquet")` |
