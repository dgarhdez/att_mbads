# Context: att_mbads Repository (Advanced Tech Track - Polars)

## Overview

This repository contains course materials for the **Advanced Tech Track (ATT)** focusing on Polars for data analytics in the MBADS program. It was created following the same structure and conventions as `pda_mbads`.

## Target Audience

Students who have completed PDA1 and are familiar with:
- Python fundamentals
- Pandas basics (DataFrames, filtering, groupby)
- Data manipulation concepts

## Repository Structure

```
att_mbads/
├── pyproject.toml              # uv-managed dependencies
├── README.md                   # Setup instructions
├── CLAUDE.md                   # AI assistant guidelines
├── syllabus.md                 # Course syllabus with session/homework details
├── .gitignore
├── context.md                  # This file
│
├── class_material/
│   ├── s01_intro_to_polars/
│   │   ├── s01_intro_to_polars.ipynb
│   │   └── data/
│   │       ├── employees.csv          # 100 rows
│   │       ├── employees.parquet      # Generated from CSV
│   │       └── employees_sample.csv   # Generated subset
│   │
│   ├── s02_data_manipulation/
│   │   ├── s02_data_manipulation.ipynb
│   │   └── data/
│   │       ├── ecommerce_orders.csv   # 5,000 rows
│   │       └── customers.csv          # 500 rows
│   │
│   └── s03_lazy_evaluation/
│       ├── s03_lazy_evaluation.ipynb
│       └── data/
│           └── large_transactions.csv  # 100,000 rows
│
└── homework/
    ├── s01_polars_basics/
    │   ├── s01_polars_basics_homework.ipynb
    │   ├── s01_polars_basics_homework_solved.ipynb
    │   ├── summary.md                 # Concept summary for homework
    │   └── spotify_tracks.csv          # 5,000 rows
    │
    ├── s02_data_manipulation/
    │   ├── s02_data_manipulation_homework.ipynb
    │   ├── s02_data_manipulation_homework_solved.ipynb
    │   ├── summary.md                 # Concept summary for homework
    │   └── retail_sales.csv            # 10,000 rows
    │
    └── s03_lazy_evaluation/
        ├── s03_lazy_evaluation_homework.ipynb
        ├── s03_lazy_evaluation_homework_solved.ipynb
        ├── summary.md                 # Concept summary for homework
        └── financial_transactions.csv   # 100,000 rows
```

## Course Content

### Session 1: Introduction to Polars
**File:** `class_material/s01_intro_to_polars/s01_intro_to_polars.ipynb`

Topics covered:
1. What is Polars and why use it (Rust-based, performance)
2. Creating DataFrames and Series
   - Pandas comparison for Series (name argument order)
3. Reading/writing files (CSV, Parquet)
   - 3.3 Understanding Parquet Format (columnar vs row storage)
   - 3.4 Performance Comparison (Pandas vs Polars, CSV vs Parquet benchmarks)
4. Basic data inspection (shape, columns, dtypes, head, describe)
5. Column selection with `select()` and `pl.col()`
   - 5.2 Pattern-based selection with Pandas comparison
6. Introduction to the Expression API
   - 6.1 Basic Expressions (concept, reusability, mental model)
   - 6.2 Arithmetic Expressions (+, -, *, /, //, %)
   - 6.3 Aggregation Expressions (mean, sum, min, max, etc.)
   - 6.4 String Expressions (.str namespace) - **Enhanced with detailed comparison**
     - What is the .str namespace
     - Similarities between Pandas and Polars .str
     - Key differences (execution model, method names, return types)
     - Why Polars uses different names (len_chars vs len, Unicode handling)
   - 6.5 Boolean and Comparison Expressions (==, >, &, |, ~)
7. Creating new columns with `with_columns()`
8. Key differences from Pandas

**Key principle:** Every Polars concept includes a Pandas comparison table.

### Session 2: Data Manipulation
**File:** `class_material/s02_data_manipulation/s02_data_manipulation.ipynb`

Topics:
1. Filtering rows with `filter()` and boolean expressions
   - Pandas comparison table
2. Creating columns with `with_columns()` and `alias()`
   - Pandas comparison (assignment vs with_columns, immutability)
3. Conditional logic: `pl.when().then().otherwise()`
   - Pandas comparison (np.where, np.select)
4. Groupby and aggregations with `group_by().agg()`
5. Missing data: `fill_null()`, `drop_nulls()`, `is_null()`
   - Pandas comparison table
6. Sorting with `sort()`
   - Pandas comparison (descending vs ascending parameter)
7. Combining DataFrames: `pl.concat()`, `join()`
   - Pandas comparison for concatenation (how vs axis)
   - Pandas comparison for joins (join vs merge, full vs outer)
8. String operations: `str.to_lowercase()`, `str.contains()`
   - Pandas comparison table (split indexing differences)

### Session 3: Lazy Evaluation
**File:** `class_material/s03_lazy_evaluation/s03_lazy_evaluation.ipynb`

Topics:
1. Eager vs lazy execution explained
   - Pandas comparison (execution models table)
2. Creating LazyFrames: `pl.scan_csv()`, `df.lazy()`
   - Pandas comparison (no equivalent to scan_*)
3. Executing queries: `collect()`
4. Inspecting query plans: `explain()`
   - Pandas comparison (not available in Pandas)
5. Query optimizations: predicate pushdown, projection pushdown
6. Streaming for large datasets: `collect(streaming=True)`
   - Pandas comparison (chunksize vs streaming)
7. Performance benchmarking: Pandas vs Polars eager vs Polars lazy
8. Best practices for query optimization
9. Window functions with `over()`
   - Pandas comparison (transform vs over)

## Homework Assignments

### Homework 1: Polars Basics
- **Dataset:** `spotify_tracks.csv` (~5,000 rows)
- **Summary:** `homework/s01_polars_basics/summary.md`
- **Exercises:** Load CSV, inspect structure, select columns, create new columns, aggregations, save to Parquet
- **Points:** 100 + 10 bonus

### Homework 2: Data Manipulation
- **Dataset:** `retail_sales.csv` (~10,000 rows)
- **Summary:** `homework/s02_data_manipulation/summary.md`
- **Exercises:** Filter, transform, groupby aggregations, handle nulls, sorting, joins
- **Points:** 100 + 10 bonus

### Homework 3: Lazy Evaluation
- **Dataset:** `financial_transactions.csv` (~100,000 rows)
- **Summary:** `homework/s03_lazy_evaluation/summary.md`
- **Exercises:** Build lazy queries, inspect plans, benchmark eager vs lazy, optimize queries, window functions, streaming
- **Points:** 100 + 15 bonus

## Datasets

All datasets are synthetic, generated with seeded random data for reproducibility.

| Dataset | Rows | Location | Description |
|---------|------|----------|-------------|
| employees.csv | 100 | s01 data/ | Employee records (id, name, email, dept, position, salary, hire_date, is_active) |
| ecommerce_orders.csv | 5,000 | s02 data/ | E-commerce orders |
| customers.csv | 500 | s02 data/ | Customer data for joins |
| large_transactions.csv | 100,000 | s03 data/ | Financial transactions for lazy eval demos |
| spotify_tracks.csv | 5,000 | homework s01/ | Music tracks data |
| retail_sales.csv | 10,000 | homework s02/ | Retail sales records |
| financial_transactions.csv | 100,000 | homework s03/ | Transactions for performance exercises |

## Dependencies

From `pyproject.toml`:

```toml
[project]
name = "att-mbads"
version = "0.1.0"
description = "Advanced Tech Track - Polars for Data Analytics"
requires-python = ">=3.11"
dependencies = [
    "polars>=1.0.0",
    "pandas>=2.0.0",
    "pyarrow>=14.0.0",
    "plotly>=5.0.0",
    "python-dotenv>=1.0.0",
]

[tool.uv]
dev-dependencies = [
    "jupyter>=1.0.0",
    "ipykernel>=6.0.0",
    "notebook>=7.0.0",
]
```

## CLAUDE.md Guidelines

Key content principles from CLAUDE.md:

1. **MANDATORY Pandas comparison**: For EVERY new Polars concept, show equivalent Pandas code using:
   - Side-by-side code blocks
   - Comparison tables
   - "Pandas Comparison" markdown headers

2. **Progressive complexity**: Start simple, build to advanced

3. **Performance focus**: Include benchmarks (Pandas vs Polars eager vs Polars lazy)

4. **Expression API emphasis**: Explain what expressions are and how they differ from Pandas
   - What an expression is (a description of a computation, not the result)
   - How expressions differ from Pandas' direct column access
   - Why expressions enable optimization

5. **Practical datasets**: Use realistic data from e-commerce, finance, music, retail domains

## Git Repository

- **Remote:** https://github.com/dgarhdez/att_mbads.git
- **Main branch:** main

### Recent Commits

```
3ece79a Enhance .str namespace explanation and add course syllabus
28ccb7d Add missing Pandas comparisons to notebooks and create homework summaries
2e1f5f9 Initial commit: Advanced Tech Track - Polars course materials
```

## Recent Work Summary

1. Created entire repository structure from scratch
2. Generated all synthetic datasets
3. Created all 3 session notebooks with comprehensive content
4. Created 6 homework notebooks (3 unsolved + 3 solved)
5. Added Parquet format explanation and CSV vs Parquet benchmarks to Session 1
6. Enhanced Expression API section (6.1-6.5) with detailed examples
7. Added Pandas comparison tables for all expression types:
   - Arithmetic operations
   - Aggregation functions
   - String operations (10 methods)
   - Boolean/comparison operations (13 operations)
8. Fixed section numbering issues
9. Updated CLAUDE.md to mandate Pandas comparisons
10. **Added missing Pandas comparisons to all notebooks:**
    - s01: Series creation, pattern-based selection
    - s02: with_columns, sorting, concatenation, joins, string operations
    - s03: execution models, file reading, query plans, streaming
11. **Created summary.md for each homework folder** with key concepts and quick reference tables
12. **Enhanced section 6.4** with detailed .str namespace explanation (similarities, differences, naming rationale)
13. **Created syllabus.md** with complete course overview, session descriptions, homework details, and assessment summary

## Notes

- The `employees.parquet` file is generated when running the Session 1 notebook
- Performance benchmarks may vary based on system; larger datasets show more dramatic differences
- All notebooks follow the naming convention: `s##_topic_name.ipynb`
- Solved homework versions use `_solved` suffix
- Each homework folder contains a `summary.md` with concepts needed for that assignment
