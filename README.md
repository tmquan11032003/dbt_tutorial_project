# DBT Databricks Tutorial Project

Project học **dbt (data build tool)** với **Databricks** làm warehouse, xây dựng theo kiến trúc **Medallion (Bronze → Silver → Gold)**.

## Mục tiêu học

- Xây dựng data pipeline hoàn chỉnh với dbt: sources → models → tests → snapshots
- Thực hành Medallion Architecture: raw → cleaned → business-ready
- Áp dụng data quality tests (unique, not_null, accepted_values, custom generic tests)
- Snapshot SCD Type 2 cho slowly-changing dimensions

## Tech stack

- **dbt-core** 1.12 + **dbt-databricks** adapter 1.10
- **Databricks** (SQL Warehouse)
- **Python** 3.12, quản lý dependency bằng `uv`

## Cấu trúc thư mục

\`\`\`
dbt_project_tutorial/
├── models/
│   ├── source/       # Source declarations (sources.yml)
│   ├── bronze/       # Raw layer - copy 1:1 từ source
│   ├── silver/       # Cleaned + joined
│   └── gold/         # Business-ready aggregates
├── snapshots/        # SCD Type 2 tracking
├── seeds/            # CSV lookup data
├── tests/            # Singular + generic tests
├── macros/           # Reusable Jinja macros
└── analyses/         # Ad-hoc SQL exploration
\`\`\`

## Setup

### 1. Yêu cầu
- Python 3.12
- Databricks workspace + SQL warehouse
- Databricks Personal Access Token (PAT) với scope `sql`

### 2. Cài đặt

\`\`\`bash
# Clone repo
git clone <repo-url>
cd DBT_Project

# Tạo virtual env & cài deps
uv sync

# Setup env vars
cp .env.example .env
# Sửa .env, điền DBT_DATABRICKS_TOKEN của bạn
\`\`\`

### 3. Cấu hình dbt profile

Tạo file `~/.dbt/profiles.yml`:

\`\`\`yaml
dbt_project_tutorial:
  target: dev
  outputs:
    dev:
      type: databricks
      host: <your-workspace>.cloud.databricks.com
      http_path: /sql/1.0/warehouses/<warehouse-id>
      token: "{{ env_var('DBT_DATABRICKS_TOKEN') }}"
      catalog: dbt_tutorial_dev
      schema: default
\`\`\`

### 4. Chạy

\`\`\`bash
cd dbt_project_tutorial
dbt debug        # verify connection
dbt deps         # cài dbt packages (nếu có)
dbt build        # chạy toàn bộ: seed → models → tests → snapshots
\`\`\`

## Learning notes

Project này là một phần của lộ trình học AIDE (Data Engineering) — lesson: Transformation Layer với dbt.
