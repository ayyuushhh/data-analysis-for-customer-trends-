# 🛍️ Customer Shopping Behavior Analysis

A complete end-to-end data analytics project that analyzes customer shopping behavior using **Python (Pandas)**, **PostgreSQL / MySQL / SQL Server**, and **Power BI**.

---

## 📁 Project Structure

```
📦 Customer-Shopping-Behavior
├── 📓 Customer_Shopping_Behavior_Analysis.ipynb  # Python EDA + Data Cleaning
├── 📊 customer_behavior_dashboard.pbix           # Power BI Dashboard
├── 📄 customer_shopping_behavior.csv             # Raw Dataset
└── 📝 README.md
```

---

## 🎯 Objective

To analyze customer purchasing patterns, segment customers by age and income, identify discount/promo usage trends, and visualize key business insights through an interactive Power BI dashboard.

---

## 🔄 Project Workflow

```
Raw CSV Data
     ↓
Python (Pandas) — Data Cleaning & Feature Engineering
     ↓
Database (PostgreSQL / MySQL / SQL Server) — Data Storage
     ↓
Power BI — Interactive Dashboard & Visualization
```

---

## 🐍 Python Notebook — What Was Done

### 1. Data Loading
```python
import pandas as pd
df = pd.read_csv('customer_shopping_behavior.csv')
```

### 2. Data Cleaning
- Checked for null/missing values using `df.isnull().sum()`
- Imputed missing `Review Rating` with **median rating per category**
```python
df['Review Rating'] = df.groupby('Category')['Review Rating'] \
                        .transform(lambda x: x.fillna(x.median()))
```

### 3. Column Renaming (Snake Case)
```python
df.columns = df.columns.str.lower().str.replace(' ', '_')
df = df.rename(columns={'purchase_amount_(usd)': 'purchase_amount'})
```

### 4. Feature Engineering

**Age Group Column**
```python
labels = ['Young Adult', 'Adult', 'Middle-aged', 'Senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)
```

**Purchase Frequency in Days**
```python
frequency_mapping = {
    'Weekly': 7, 'Fortnightly': 14, 'Bi-Weekly': 14,
    'Monthly': 30, 'Quarterly': 90,
    'Every 3 Months': 90, 'Annually': 365
}
df['purchase_frequency_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

**Dropped Redundant Column**
```python
# promo_code_used == discount_applied (100% match)
df = df.drop('promo_code_used', axis=1)
```

---

## 🗄️ Database Integration

Data was exported to SQL databases using **SQLAlchemy**.

### PostgreSQL
```python
from sqlalchemy import create_engine
engine = create_engine("postgresql+psycopg2://user:password@localhost:5432/customer_behavior")
df.to_sql('customer', engine, if_exists='replace', index=False)
```

### MySQL
```python
engine = create_engine("mysql+pymysql://user:password@localhost:3306/customer_behavior")
df.to_sql('customer', engine, if_exists='replace', index=False)
```

### MS SQL Server
```python
engine = create_engine("mssql+pyodbc://user:password@localhost,1433/customer_behavior?driver=ODBC+Driver+17+for+SQL+Server")
df.to_sql('customer', engine, if_exists='replace', index=False)
```

---

## 📊 Power BI Dashboard

The `.pbix` file contains an interactive dashboard with:

- 📈 **Sales trends** by category and season
- 👥 **Customer segmentation** by age group
- 💰 **Purchase amount** distribution
- 🎟️ **Discount & promo code** usage analysis
- 🔁 **Purchase frequency** patterns
- 🌍 **Regional** buying behavior

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python (Pandas) | Data cleaning & feature engineering |
| Jupyter Notebook | EDA & analysis |
| PostgreSQL | Primary database |
| MySQL | Alternative database |
| MS SQL Server | Enterprise database option |
| SQLAlchemy | Python ↔ Database connector |
| Power BI | Dashboard & visualization |

---

## ⚙️ Setup & Installation

### Step 1 — Clone the repo
```bash
git clone https://github.com/yourusername/customer-shopping-behavior.git
cd customer-shopping-behavior
```

### Step 2 — Install Python dependencies
```bash
pip install pandas sqlalchemy psycopg2-binary pymysql pyodbc
```

### Step 3 — Run the Notebook
```bash
jupyter notebook Customer_Shopping_Behavior_Analysis.ipynb
```

### Step 4 — Setup Database
- Create a database named `customer_behavior` in your preferred SQL tool
- Update credentials in the notebook
- Run the database connection cells

### Step 5 — Open Power BI
- Open `customer_behavior_dashboard.pbix` in Power BI Desktop
- Refresh data source if needed

---

## 📂 Dataset Columns

| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier |
| `age` | Customer age |
| `age_group` | Derived — Young Adult / Adult / Middle-aged / Senior |
| `category` | Product category |
| `purchase_amount` | Purchase value in USD |
| `review_rating` | Customer review score |
| `frequency_of_purchases` | How often customer buys |
| `purchase_frequency_days` | Derived — frequency in days |
| `discount_applied` | Whether discount was used |
| `season` | Season of purchase |

---

## 💡 Key Insights

- Missing `review_rating` values were filled using **category-wise median** — preserving data distribution
- `promo_code_used` and `discount_applied` were **100% identical** — one was dropped to avoid redundancy
- Customers were **segmented into 4 age groups** using quartile-based binning
- Purchase frequency was **converted to numeric days** for easier analysis

---

## 👤 Author

**Ayush Jha**
- 📧 Ayushjhajnt@gmail.com
- 🔗 [LinkedIn](https://linkedin.com/in/yourprofile)
- 🐙 [GitHub](https://github.com/yourusername)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
