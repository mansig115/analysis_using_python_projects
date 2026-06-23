# analysis_using_python_projects
I have attached all python projects.
# ==============================
# 1. IMPORT LIBRARIES
# ==============================
import pandas as pd
import matplotlib.pyplot as plt

# Load dataset (Supply Chain dataset)
df = pd.read_csv(r'D:\My Learnings\Supply Chain Analysis\Supply Chain Analysis for Beauty Products\supply_chain_data.csv')
# One-line: Load dataset into pandas DataFrame

# ==============================
# 2. BASIC DATA UNDERSTANDING
# ==============================
print(df.head())        # One-line: Preview first 5 rows
print(df.shape)         # One-line: Check rows & columns count
print(df.columns)       # One-line: Get all column names
print(df.info())        # One-line: Data types + null values overview
print(df.describe())    # One-line: Statistical summary of numeric columns
print(df.dtypes)        # One-line: Data types of each column

# ==============================
# 3. DATA CLEANING
# ==============================

# Standardize text format
df['Product type'] = df['Product type'].str.capitalize()
# One-line: Normalize product type values

# Round monetary columns
money_cols = ['Price', 'Revenue generated', 'Manufacturing costs', 'Shipping costs', 'Costs']
df[money_cols] = df[money_cols].round(2)
# One-line: Round financial values for better readability

# Missing values check
print(df.isnull().sum())  
# One-line: Check missing values in dataset

# Duplicate check
print(df.duplicated().sum())
# One-line: Count duplicate rows

# ==============================
# 4. EXPLORATORY DATA ANALYSIS (EDA)
# ==============================

# Product-wise revenue analysis
product_revenue = df.groupby('Product type')['Revenue generated'].mean().sort_values(ascending=False)
print(product_revenue)
# One-line: Average revenue by product category

product_sales = df.groupby('Product type')['Number of products sold'].sum().sort_values(ascending=False)
print(product_sales)
# One-line: Total sales by product category

# ==============================
# 5. SUPPLIER ANALYSIS
# ==============================

supplier_analysis = (
    df.groupby('Supplier name')
      .agg({
          'Defect rates': 'mean',
          'Supplier Lead time': 'mean',
          'Costs': 'mean',
          'Revenue generated': 'sum'
      })
      .round(2)
      .sort_values('Revenue generated', ascending=False)
)

print(supplier_analysis)
# One-line: Supplier performance comparison

# ==============================
# 6. LOGISTICS ANALYSIS
# ==============================

print(df.groupby('Shipping carriers')[['Shipping costs', 'Shipping times']].mean().round(2))
# One-line: Carrier-wise shipping performance

print(df.groupby('Transportation modes')[['Shipping costs', 'Shipping times']].mean().round(2))
# One-line: Transport mode efficiency comparison

# ==============================
# 7. QUALITY ANALYSIS
# ==============================

print(df['Inspection results'].value_counts())
# One-line: Distribution of inspection results

quality_distribution = df.groupby('Supplier name')['Inspection results'].value_counts(normalize=True).unstack().fillna(0).round(2)
print(quality_distribution)
# One-line: Supplier-wise quality distribution (percentage)

# ==============================
# 8. CORRELATION ANALYSIS
# ==============================

corr = df[['Price','Revenue generated','Defect rates','Shipping costs','Costs']].corr().round(2)
print(corr)
# One-line: Relationship between numeric variables

# ==============================
# 9. VISUALIZATION
# ==============================

df.groupby('Supplier name')['Defect rates'].mean().sort_values(ascending=False).plot(kind='bar')
plt.title('Average Defect Rate by Supplier')
plt.ylabel('Defect Rate')
plt.xticks(rotation=45)
plt.show()
# One-line: Visualize supplier defect rates
