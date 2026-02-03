# Advanced Tech Track: Polars for Data Analytics

## Course Syllabus

### Course Overview

This course introduces **Polars**, a high-performance DataFrame library, to students already familiar with Pandas. The focus is on practical data analysis skills, emphasizing performance optimization and the transition from Pandas to Polars.

**Target Audience**: Students who have completed PDA1 and are comfortable with Python fundamentals and Pandas basics.

**Prerequisites**:
- Python fundamentals (variables, functions, data types)
- Pandas basics (DataFrames, Series, filtering, groupby)
- Data manipulation concepts

---

## Session 1: Introduction to Polars

### Learning Objectives

By the end of this session, students will be able to:
1. Understand what Polars is and why it offers performance advantages
2. Create DataFrames and Series in Polars
3. Read and write various file formats (CSV, Parquet)
4. Perform basic data inspection
5. Select and transform columns using the expression API

### Topics Covered

| Topic | Description |
|-------|-------------|
| **What is Polars?** | Rust-based library, performance benefits, comparison with Pandas |
| **DataFrames & Series** | Creating data structures, naming conventions |
| **File I/O** | Reading/writing CSV and Parquet, format comparison |
| **Parquet Format** | Columnar storage, compression, performance benefits |
| **Data Inspection** | shape, columns, schema, head, tail, describe |
| **Column Selection** | `select()`, `pl.col()`, pattern matching, dtype selection |
| **Expression API** | What expressions are, how they differ from Pandas |
| **Expression Types** | Arithmetic, aggregation, string (`.str`), boolean expressions |
| **Creating Columns** | `with_columns()` and `alias()` |

### Key Concepts

- **Expressions vs Direct Access**: Polars uses expressions (recipes) rather than immediate data access
- **The `.str` Namespace**: String operations in Polars vs Pandas
- **Immutability**: Polars operations return new DataFrames
- **No Index**: Polars doesn't use row indices like Pandas

### Pandas Comparisons Covered

- DataFrame/Series creation
- File reading/writing
- Data inspection methods
- Column selection patterns
- Arithmetic, string, and boolean operations

---

## Homework 1: Polars Basics

**Dataset**: `spotify_tracks.csv` (~5,000 rows of music track data)

### Exercises

| Exercise | Points | Skills Tested |
|----------|--------|---------------|
| Load CSV data | 10 | `pl.read_csv()` |
| Inspect data structure | 10 | `shape`, `columns`, `schema` |
| Select specific columns | 15 | `select()` |
| Create calculated columns | 20 | `with_columns()`, arithmetic expressions |
| Compute aggregations | 20 | `mean()`, `max()`, `min()`, `n_unique()` |
| Select by data type | 10 | `pl.col(pl.Int64)`, `pl.col(pl.Float64)` |
| Save to Parquet | 15 | `write_parquet()`, `read_parquet()` |
| **Bonus**: Conditional column | 10 | `when().then().otherwise()` (preview of Session 2) |

**Total**: 100 points + 10 bonus

---

## Session 2: Data Manipulation and Aggregations

### Learning Objectives

By the end of this session, students will be able to:
1. Filter rows using boolean expressions
2. Create columns with conditional logic
3. Perform groupby aggregations
4. Handle missing data
5. Join and concatenate DataFrames

### Topics Covered

| Topic | Description |
|-------|-------------|
| **Filtering** | `filter()`, single/multiple conditions, `is_in()` |
| **String Filters** | `contains()`, `starts_with()`, `ends_with()` |
| **Conditional Logic** | `when().then().otherwise()` chains |
| **Groupby Aggregations** | `group_by().agg()`, multiple aggregations |
| **Aggregation Functions** | sum, mean, min, max, count, n_unique, first, last |
| **Missing Data** | `null_count()`, `is_null()`, `drop_nulls()`, `fill_null()` |
| **Sorting** | `sort()` with single/multiple columns |
| **Concatenation** | `pl.concat()` vertical and horizontal |
| **Joins** | `join()` with inner, left, right, full, semi, anti |
| **String Operations** | Advanced `.str` methods, splitting, extraction |

### Key Concepts

- **Filter Early**: Apply filters before expensive operations
- **Null vs NaN**: Polars uses `null` consistently (not NaN)
- **Join Types**: Understanding when to use semi/anti joins
- **Immutable Aggregations**: Groupby returns new DataFrames

### Pandas Comparisons Covered

- Filtering syntax (`df[condition]` vs `df.filter()`)
- Conditional logic (`np.where`/`np.select` vs `when().then()`)
- Groupby syntax differences
- Missing data handling (`isna()` vs `is_null()`)
- Join methods (`merge()` vs `join()`)
- Concatenation (`axis=1` vs `how="horizontal"`)

---

## Homework 2: Data Manipulation

**Dataset**: `retail_sales.csv` (~10,000 rows of retail transaction data)

### Exercises

| Exercise | Points | Skills Tested |
|----------|--------|---------------|
| Load and inspect data | 10 | Basic inspection, null detection |
| Filter with conditions | 15 | `filter()`, `is_in()`, compound conditions |
| Create calculated columns | 15 | `with_columns()`, `when().then().otherwise()` |
| Groupby aggregations | 20 | `group_by().agg()`, multiple metrics |
| Handle missing data | 15 | `drop_nulls()`, `fill_null()` |
| Sorting and Top-N | 10 | `sort()`, `head()` |
| Advanced analysis | 15 | Multi-level groupby, percentage calculations |
| **Bonus**: Store report | 10 | Comprehensive analysis combining all skills |

**Total**: 100 points + 10 bonus

---

## Session 3: Lazy Evaluation and Query Optimization

### Learning Objectives

By the end of this session, students will be able to:
1. Understand the difference between eager and lazy execution
2. Create and work with LazyFrames
3. Inspect and understand query plans
4. Apply query optimization techniques
5. Use streaming for large datasets
6. Implement window functions

### Topics Covered

| Topic | Description |
|-------|-------------|
| **Eager vs Lazy** | Execution models, benefits of lazy evaluation |
| **LazyFrames** | `scan_csv()`, `scan_parquet()`, `df.lazy()` |
| **Execution** | `collect()`, when to execute |
| **Query Plans** | `explain()`, reading optimized vs unoptimized plans |
| **Predicate Pushdown** | Filters pushed to data source |
| **Projection Pushdown** | Only needed columns read |
| **Common Subexpression Elimination** | Avoiding redundant computation |
| **Performance Benchmarking** | Comparing Pandas vs Polars eager vs lazy |
| **Streaming** | `collect(streaming=True)` for large datasets |
| **Window Functions** | `over()` for group-wise calculations |
| **Best Practices** | Query structure, optimization tips |

### Key Concepts

- **Deferred Execution**: Operations recorded, not executed until `collect()`
- **Query Optimization**: Polars automatically optimizes query plans
- **Pushdown Optimizations**: Filters and projections applied at scan time
- **Memory Efficiency**: Lazy evaluation reduces intermediate storage

### Pandas Comparisons Covered

- Execution models (Pandas has no native lazy evaluation)
- File reading (`read_*` vs `scan_*`)
- Query inspection (not available in Pandas)
- Chunked processing (`chunksize` vs `streaming=True`)
- Window functions (`transform()` vs `over()`)

---

## Homework 3: Lazy Evaluation and Query Optimization

**Dataset**: `financial_transactions.csv` (~100,000 rows of transaction data)

### Exercises

| Exercise | Points | Skills Tested |
|----------|--------|---------------|
| Create LazyFrame | 10 | `scan_csv()`, schema inspection |
| Predicate pushdown | 15 | `filter()`, `explain()` |
| Projection pushdown | 15 | `select()`, query plan analysis |
| Performance benchmarking | 20 | Timing eager vs lazy execution |
| Query optimization | 20 | Rewriting inefficient queries |
| Window functions | 10 | `over()`, ranking, deviations |
| Streaming execution | 10 | `collect(streaming=True)` |
| **Bonus**: Complex pipeline | 15 | Full analysis combining all concepts |

**Total**: 100 points + 15 bonus

---

## Course Materials

### File Structure

```
att_mbads/
├── class_material/
│   ├── s01_intro_to_polars/
│   │   ├── s01_intro_to_polars.ipynb
│   │   └── data/employees.csv
│   ├── s02_data_manipulation/
│   │   ├── s02_data_manipulation.ipynb
│   │   └── data/ecommerce_orders.csv, customers.csv
│   └── s03_lazy_evaluation/
│       ├── s03_lazy_evaluation.ipynb
│       └── data/large_transactions.csv
│
└── homework/
    ├── s01_polars_basics/
    │   ├── s01_polars_basics_homework.ipynb
    │   ├── s01_polars_basics_homework_solved.ipynb
    │   ├── summary.md
    │   └── spotify_tracks.csv
    ├── s02_data_manipulation/
    │   ├── s02_data_manipulation_homework.ipynb
    │   ├── s02_data_manipulation_homework_solved.ipynb
    │   ├── summary.md
    │   └── retail_sales.csv
    └── s03_lazy_evaluation/
        ├── s03_lazy_evaluation_homework.ipynb
        ├── s03_lazy_evaluation_homework_solved.ipynb
        ├── summary.md
        └── financial_transactions.csv
```

### Resources

- [Polars Documentation](https://docs.pola.rs/)
- [Polars User Guide](https://docs.pola.rs/user-guide/)
- [Polars API Reference](https://docs.pola.rs/api/python/stable/reference/)
- [Polars GitHub](https://github.com/pola-rs/polars)

---

## Assessment Summary

| Assignment | Points | Dataset Size | Key Skills |
|------------|--------|--------------|------------|
| Homework 1 | 100 + 10 bonus | 5,000 rows | Basics, expressions, file I/O |
| Homework 2 | 100 + 10 bonus | 10,000 rows | Filtering, groupby, joins |
| Homework 3 | 100 + 15 bonus | 100,000 rows | Lazy evaluation, optimization |

**Total Course Points**: 300 + 35 bonus
