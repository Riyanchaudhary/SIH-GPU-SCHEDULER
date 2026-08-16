# Contributing to SIH-GPU-SCHEDULER

Thank you for contributing to this project!

## Getting Started

1. Fork the repository (if you are not a direct collaborator).
2. Clone the repository:

```bash
git clone https://github.com/Riyanchaudhary/SIH-GPU-SCHEDULER.git
cd SIH-GPU-SCHEDULER
```

## Adding Your Work

Please add your files only to the folder assigned for your contribution.

Example:

```text
SIH-GPU-SCHEDULER/
├── Dataset/
├── Backend/
├── Frontend/
└── Documentation/
```

If you are working on the frontend, place your files inside the `Frontend/` folder. Do not modify unrelated folders unless necessary.

## Updating Your Local Repository

Before starting work:

```bash
git pull origin main
```

## Committing Your Changes

Stage your changes:

```bash
git add .
```

Create a commit:

```bash
git commit -m "Added feature description"
```

Examples:

```bash
git commit -m "Added GPU scheduling algorithm"
git commit -m "Updated frontend dashboard"
git commit -m "Added project documentation"
```

## Pushing Your Changes

Push to GitHub:

```bash
git push origin main
```

## Good Practices

* Keep commits small and meaningful.
* Use clear commit messages.
* Do not delete other contributors' work.
* Test your code before pushing.
* Pull the latest changes before starting new work.

## Need Help?

If you face any issues with Git, GitHub, or the project structure, contact the project maintainers before making major changes.
# GPU Cluster Scheduler - Supabase Setup Guide

This guide explains how team members can connect to the shared Supabase database and fetch data into Python.

---

## 1. Install Required Libraries

Run:

```bash
pip install pandas sqlalchemy psycopg2-binary
```

---

## 2. Get Supabase Database Credentials

1. Open Supabase Dashboard
2. Go to:

```
Project Settings
    ↓
Database
    ↓
Connection String
```

Copy the PostgreSQL connection string.

Example:

```text
postgresql://postgres:PASSWORD@db.xxxxx.supabase.co:5432/postgres
```

---

## 3. Create a Python File

Create:

```text
test_connection.py
```

---

## 4. Connect to Supabase

```python
from sqlalchemy import create_engine

DATABASE_URL = (
    "postgresql://postgres:"
    "YOUR_PASSWORD"
    "@db.xxxxx.supabase.co:5432/postgres"
)

engine = create_engine(DATABASE_URL)

print("Connected Successfully!")
```

---

## 5. Fetch Jobs Table

```python
import pandas as pd
from sqlalchemy import create_engine

DATABASE_URL = "YOUR_CONNECTION_STRING"

engine = create_engine(DATABASE_URL)

jobs_df = pd.read_sql(
    "SELECT * FROM jobs",
    engine
)

print(jobs_df.head())
```

---

## 6. Fetch Specific Columns

```python
query = """
SELECT
    job_id,
    user,
    duration,
    plan_mem,
    plan_gpu,
    task_name,
    gpu_type
FROM jobs
"""

jobs_df = pd.read_sql(query, engine)

print(jobs_df.head())
```

---

## 7. Check Number of Jobs

```python
count_df = pd.read_sql(
    "SELECT COUNT(*) FROM jobs",
    engine
)

print(count_df)
```

---

## 8. Example for Forecasting Model

```python
import pandas as pd

query = """
SELECT
    plan_mem,
    plan_gpu,
    task_name,
    gpu_type,
    duration
FROM jobs
"""

df = pd.read_sql(query, engine)

print(df.shape)
```

Features:

```python
X = df[
    [
        "plan_mem",
        "plan_gpu",
        "task_name",
        "gpu_type"
    ]
]
```

Target:

```python
y = df["duration"]
```

---

## 9. Current Jobs Table Schema

| Column | Description |
|----------|----------|
| job_id | Primary Key |
| job_name | Alibaba Job Identifier |
| user | User/Tenant ID |
| start_time | Job Arrival Time |
| duration | Runtime |
| plan_mem | Requested Memory (GB) |
| plan_gpu | Requested GPU Percentage |
| task_name | Framework / Task Type |
| gpu_type | GPU Model |

---

## 10. Security Note

DO NOT commit:

```python
DATABASE_URL
PASSWORD
SUPABASE_KEYS
```

to GitHub.

Instead create:

```text
.env
```

Example:

```env
DATABASE_URL=postgresql://postgres:password@db.xxx.supabase.co:5432/postgres
```

Python:

```python
from dotenv import load_dotenv
import os

load_dotenv()

DATABASE_URL = os.getenv("DATABASE_URL")
```

---

## Quick Test

Run:

```python
jobs_df = pd.read_sql(
    "SELECT * FROM jobs LIMIT 5",
    engine
)

print(jobs_df)
```

If rows are printed, the connection is working successfully.
