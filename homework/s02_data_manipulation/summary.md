# Homework 2: Data Manipulation - Concept Summary

This document summarizes the key concepts needed to complete Homework 2.

## Prerequisites

Before starting this homework, you should be comfortable with:
- Session 1 concepts (DataFrames, select, with_columns, expressions)
- Basic Polars syntax

## Key Concepts

### 1. Filtering Rows with `filter()`

```python
# Simple filter
df.filter(pl.col("amount") > 100)

# Multiple conditions with AND (&)
df.filter(
    (pl.col("category") == "Electronics") &
    (pl.col("price") > 500)
)

# Multiple conditions with OR (|)
df.filter(
    (pl.col("status") == "completed") |
    (pl.col("status") == "shipped")
)

# Using is_in() for multiple values
df.filter(pl.col("status").is_in(["completed", "shipped"]))
```

| Pandas | Polars |
|--------|--------|
| `df[df["col"] > 100]` | `df.filter(pl.col("col") > 100)` |
| `df[df["col"].isin([...]]` | `df.filter(pl.col("col").is_in([...]))` |

**Important**: Always wrap conditions in parentheses when using `&` or `|`.

### 2. Conditional Logic: `when().then().otherwise()`

```python
df.with_columns(
    pl.when(pl.col("amount") >= 500)
      .then(pl.lit("Large"))
      .when(pl.col("amount") >= 100)
      .then(pl.lit("Medium"))
      .otherwise(pl.lit("Small"))
      .alias("size_category")
)
```

| Pandas | Polars |
|--------|--------|
| `np.where(cond, true_val, false_val)` | `pl.when(cond).then(true_val).otherwise(false_val)` |
| `np.select(conditions, choices, default)` | Chain multiple `when().then()` |

### 3. Groupby Aggregations

```python
# Basic groupby
df.group_by("category").agg(
    pl.len().alias("count"),
    pl.col("amount").sum().alias("total"),
    pl.col("amount").mean().alias("average")
)

# Multiple grouping columns
df.group_by("category", "region").agg(
    pl.col("sales").sum().alias("total_sales")
)
```

| Pandas | Polars |
|--------|--------|
| `df.groupby("col").agg({"a": "sum"})` | `df.group_by("col").agg(pl.col("a").sum())` |
| `df.groupby("col").size()` | `df.group_by("col").len()` |

### Common Aggregation Functions

| Function | Description |
|----------|-------------|
| `pl.len()` | Count all rows |
| `pl.col("x").sum()` | Sum |
| `pl.col("x").mean()` | Average |
| `pl.col("x").min()` | Minimum |
| `pl.col("x").max()` | Maximum |
| `pl.col("x").std()` | Standard deviation |
| `pl.col("x").n_unique()` | Count unique values |
| `pl.col("x").first()` | First value |
| `pl.col("x").last()` | Last value |

### 4. Handling Missing Data

```python
# Check for nulls
df.null_count()

# Filter null/not null
df.filter(pl.col("col").is_null())
df.filter(pl.col("col").is_not_null())

# Drop rows with nulls
df.drop_nulls()                    # Any column
df.drop_nulls(subset=["col1"])     # Specific columns

# Fill nulls
df.with_columns(
    pl.col("name").fill_null("Unknown"),
    pl.col("value").fill_null(0),
    pl.col("price").fill_null(pl.col("price").mean())  # Fill with mean
)
```

| Pandas | Polars |
|--------|--------|
| `df.isna()` | `pl.col("col").is_null()` |
| `df.dropna()` | `df.drop_nulls()` |
| `df.fillna(value)` | `df.fill_null(value)` or `with_columns(pl.col().fill_null())` |

### 5. Sorting

```python
# Single column
df.sort("amount")                           # Ascending
df.sort("amount", descending=True)          # Descending

# Multiple columns
df.sort(["category", "amount"], descending=[False, True])
```

| Pandas | Polars |
|--------|--------|
| `df.sort_values("col")` | `df.sort("col")` |
| `df.sort_values("col", ascending=False)` | `df.sort("col", descending=True)` |

### 6. Combining DataFrames

#### Concatenation
```python
# Vertical (stack rows)
pl.concat([df1, df2])

# Horizontal (add columns)
pl.concat([df1, df2], how="horizontal")
```

#### Joins
```python
# Left join
df1.join(df2, on="key", how="left")

# Inner join
df1.join(df2, on="key", how="inner")

# Different column names
df1.join(df2, left_on="id", right_on="customer_id", how="left")
```

| Join Type | Description |
|-----------|-------------|
| `inner` | Only matching rows |
| `left` | All from left, matching from right |
| `right` | Matching from left, all from right |
| `full` | All rows from both (outer) |
| `semi` | Left rows with match in right |
| `anti` | Left rows with NO match in right |

| Pandas | Polars |
|--------|--------|
| `pd.concat([df1, df2])` | `pl.concat([df1, df2])` |
| `df1.merge(df2, on="key")` | `df1.join(df2, on="key")` |
| `how="outer"` | `how="full"` |

### 7. String Operations

```python
df.with_columns(
    pl.col("name").str.to_uppercase().alias("upper"),
    pl.col("name").str.to_lowercase().alias("lower"),
    pl.col("text").str.contains("keyword").alias("has_keyword"),
    pl.col("email").str.split("@").list.last().alias("domain")
)
```

| Pandas | Polars |
|--------|--------|
| `df["col"].str.upper()` | `pl.col("col").str.to_uppercase()` |
| `df["col"].str.split(",").str[0]` | `pl.col("col").str.split(",").list.first()` |

## Quick Reference

| Task | Polars Code |
|------|-------------|
| Filter rows | `df.filter(pl.col("x") > 100)` |
| Conditional column | `pl.when(cond).then(val).otherwise(other)` |
| Group and aggregate | `df.group_by("col").agg(pl.col("x").sum())` |
| Check nulls | `df.null_count()` |
| Drop nulls | `df.drop_nulls()` |
| Fill nulls | `df.with_columns(pl.col("x").fill_null(0))` |
| Sort | `df.sort("col", descending=True)` |
| Join | `df1.join(df2, on="key", how="left")` |
| Concat | `pl.concat([df1, df2])` |
