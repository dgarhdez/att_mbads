# Homework 3: Lazy Evaluation - Concept Summary

This document summarizes the key concepts needed to complete Homework 3.

## Prerequisites

Before starting this homework, you should be comfortable with:
- Session 1 & 2 concepts (DataFrames, filtering, groupby, joins)
- Understanding of data pipelines

## Key Concepts

### 1. Eager vs Lazy Execution

| Mode | Description | When Executed |
|------|-------------|---------------|
| **Eager** | Operations execute immediately | Each line runs as called |
| **Lazy** | Operations are recorded, not executed | When `.collect()` is called |

**Benefits of Lazy Evaluation:**
- Automatic query optimization
- Predicate pushdown (filters applied early)
- Projection pushdown (only needed columns read)
- Memory efficiency
- Better performance for complex queries

### 2. Creating LazyFrames

```python
# Method 1: Scan a file (recommended for large files)
lf = pl.scan_csv("file.csv")
lf = pl.scan_parquet("file.parquet")

# Method 2: Convert DataFrame to lazy
lf = df.lazy()
```

| Eager | Lazy | Description |
|-------|------|-------------|
| `pl.read_csv()` | `pl.scan_csv()` | CSV files |
| `pl.read_parquet()` | `pl.scan_parquet()` | Parquet files |

**Note**: Pandas has no equivalent to `scan_*`. It always reads files immediately.

### 3. Executing Lazy Queries: `collect()`

```python
# Build the query (nothing executes yet)
lf = (
    pl.scan_csv("data.csv")
    .filter(pl.col("amount") > 100)
    .select("id", "amount", "category")
    .group_by("category")
    .agg(pl.col("amount").sum())
)

# Execute the query
result = lf.collect()  # Returns a DataFrame
```

**Important**: Call `collect()` only once at the end, not multiple times.

### 4. Inspecting Query Plans: `explain()`

```python
# View the optimized query plan
print(lf.explain())

# View the unoptimized plan (for comparison)
print(lf.explain(optimized=False))
```

Key elements in query plans:
- `Csv SCAN`: Reading from CSV
- `SELECTION`: Filter conditions
- `PROJECT`: Columns being selected
- `AGGREGATE`: Groupby operations
- `SORT`: Sorting operations

### 5. Query Optimizations

#### Predicate Pushdown
Filters are pushed to the data source, reducing data read:

```python
# Both filters get pushed to the scan
lf = (
    pl.scan_csv("data.csv")
    .filter(pl.col("category") == "Electronics")
    .select("id", "amount")
    .filter(pl.col("amount") > 500)
)
```

#### Projection Pushdown
Only required columns are read from disk:

```python
# Only "id" and "amount" columns will be read
lf = (
    pl.scan_csv("data.csv")  # File has 20 columns
    .select("id", "amount")   # Only 2 needed
)
```

### 6. Performance Benchmarking

```python
import time

def benchmark(func, n_runs=3):
    times = []
    for _ in range(n_runs):
        start = time.time()
        result = func()
        times.append(time.time() - start)
    return sum(times) / len(times)

# Compare eager vs lazy
eager_time = benchmark(lambda: pl.read_csv("data.csv").filter(...).collect())
lazy_time = benchmark(lambda: pl.scan_csv("data.csv").filter(...).collect())

print(f"Eager: {eager_time:.3f}s")
print(f"Lazy: {lazy_time:.3f}s")
print(f"Speedup: {eager_time/lazy_time:.1f}x")
```

### 7. Streaming for Large Datasets

```python
# Process data in chunks (for larger-than-memory datasets)
result = (
    pl.scan_csv("huge_file.csv")
    .filter(pl.col("amount") > 0)
    .group_by("category")
    .agg(pl.col("amount").sum())
    .collect(streaming=True)  # Enable streaming
)
```

Use streaming when:
- Dataset is larger than available memory
- You want to limit memory usage
- Processing very large files

### 8. Window Functions with `over()`

Window functions calculate values across groups without reducing rows:

```python
df.with_columns(
    # Average per group
    pl.col("amount").mean().over("category").alias("category_avg"),

    # Rank within group
    pl.col("amount").rank().over("category").alias("rank_in_category"),

    # Count per group
    pl.len().over("category").alias("group_count"),

    # Difference from group average
    (pl.col("amount") - pl.col("amount").mean().over("category")).alias("diff_from_avg")
)
```

| Pandas | Polars |
|--------|--------|
| `df.groupby("g")["x"].transform("mean")` | `pl.col("x").mean().over("g")` |
| `df.groupby("g")["x"].rank()` | `pl.col("x").rank().over("g")` |

### 9. Best Practices

**DO:**
- Use `scan_*` for large files
- Filter early in the pipeline
- Select only needed columns
- Use `explain()` to understand query plans
- Use Parquet instead of CSV
- Call `collect()` once at the end

**DON'T:**
- Call `collect()` multiple times
- Mix eager and lazy unnecessarily
- Use Python `apply()` functions (slow)
- Iterate row by row

### Good Query Structure

```python
result = (
    pl.scan_csv("data.csv")
    # 1. Filter early (predicate pushdown)
    .filter(pl.col("status") == "active")
    .filter(pl.col("amount") > 0)
    # 2. Select only needed columns (projection pushdown)
    .select("id", "amount", "category")
    # 3. Transform
    .with_columns(
        (pl.col("amount") * 1.1).alias("amount_adjusted")
    )
    # 4. Aggregate
    .group_by("category")
    .agg(pl.col("amount").sum().alias("total"))
    # 5. Sort
    .sort("total", descending=True)
    # 6. Execute once
    .collect()
)
```

## Quick Reference

| Task | Polars Code |
|------|-------------|
| Create LazyFrame | `pl.scan_csv("file.csv")` |
| Convert to lazy | `df.lazy()` |
| Execute query | `lf.collect()` |
| View plan | `lf.explain()` |
| Streaming | `lf.collect(streaming=True)` |
| Window function | `pl.col("x").mean().over("group")` |

## Comparison: Pandas vs Polars Lazy

| Feature | Pandas | Polars Lazy |
|---------|--------|-------------|
| Query optimization | None | Automatic |
| Predicate pushdown | Not available | Yes |
| Projection pushdown | Not available | Yes |
| View query plan | Not available | `explain()` |
| Streaming | Manual (chunksize) | `collect(streaming=True)` |
| Memory efficiency | Store all intermediates | Optimized |
