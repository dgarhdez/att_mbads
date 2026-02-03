# Advanced Tech Track - Polars for Data Analytics (ATT-MBADS)

This repository contains course materials for the Advanced Tech Track focusing on **Polars** for high-performance data analytics.

## Prerequisites

Students should have completed PDA1 and be familiar with:
- Python fundamentals
- Pandas basics (DataFrames, Series, filtering, groupby)
- Data manipulation concepts

## Course Overview

| Session | Topic | Description |
|---------|-------|-------------|
| 1 | Introduction to Polars | Polars basics, Pandas comparison, reading/writing files |
| 2 | Data Manipulation | Filtering, transformations, groupby, joins |
| 3 | Lazy Evaluation | Query optimization, LazyFrames, performance tuning |

## Setup Instructions

### 1. Install uv (if not already installed)

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 2. Clone and Setup

```bash
# Clone the repository
git clone <repository-url>
cd att_mbads

# Create virtual environment and install dependencies
uv sync

# Install dev dependencies (jupyter, etc.)
uv sync --all-extras
```

### 3. Launch Jupyter

```bash
uv run jupyter notebook
```

## Repository Structure

```
att_mbads/
├── class_material/
│   ├── s01_intro_to_polars/
│   ├── s02_data_manipulation/
│   └── s03_lazy_evaluation/
├── homework/
│   ├── s01_polars_basics/
│   ├── s02_data_manipulation/
│   └── s03_lazy_evaluation/
├── pyproject.toml
└── README.md
```

## Why Polars?

Polars is a modern DataFrame library written in Rust that offers:

- **Performance**: Often 10-100x faster than Pandas for large datasets
- **Memory efficiency**: Better memory management through lazy evaluation
- **Modern API**: Expressive and consistent expression-based syntax
- **Parallel execution**: Automatic parallelization of operations
- **Lazy evaluation**: Query optimization before execution

## Resources

- [Polars Documentation](https://docs.pola.rs/)
- [Polars User Guide](https://docs.pola.rs/user-guide/)
- [Polars API Reference](https://docs.pola.rs/api/python/stable/reference/)
