# 🚀 Big Data Analysis using PySpark 
Internship Task-1 | CodTech

## 📖 Project Overview
This project focuses on performing **Big Data Analysis** on a large dataset using **PySpark**  to demonstrate scalability. The dataset used is **COVID-19 Data** from OWID (Our World in Data).

## 📂 Dataset
- **Source:** OWID COVID-19 dataset  
- **Format:** CSV  
- **Size:** Large-scale dataset  
- **Columns:** Date, Country, New Cases, New Deaths, Total Cases, etc.

## 🔥 Technologies Used
- **Python**  
- **PySpark**  
- **Matplotlib & Seaborn** (for visualization)  
- **Google Colab** (for execution)

## 📊 Analysis Performed
✔ **Data Cleaning & Preprocessing**  
✔ **Exploratory Data Analysis (EDA)**  
✔ **Big Data Processing using PySpark**  
✔ **Data Aggregation & Grouping**  
✔ **COVID-19 Trend Analysis by Country**  
✔ **Visualizations (Bar Charts, Line Charts, Scatter Plots)**  

## 🖥 Setup & Installation
 1️⃣ Install Dependencies  

pip install pyspark dask pandas matplotlib seaborn


2️⃣ Clone This Repository  
git clone https://github.com/manjarisaxena/big-data-analysis.git
cd big-data-analysis


 3️⃣ Run the Script  
Run in **Google Colab** or **Jupyter Notebook**:  
python
!python analysis.py

📌 Results & Insights
- Top 10 countries with highest COVID-19 cases & deaths  
- Daily new cases trend for India  
- Relationship between cases & deaths using scatter plot  
- COVID-19 growth patterns over time  

 📜 Deliverables
📂 **Jupyter Notebook / Python Script**  
📊 **Visualizations & Reports**  
📁 **Processed Data Outputs**  

## 📜 Code Overview
import zipfile
import os
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, sum, to_date

# Initialize Spark Session
spark = SparkSession.builder.appName("COVIDDataAnalysis").getOrCreate()

# Load the dataset
csv_file = "/content/owid-covid-data.csv"
df = spark.read.csv(csv_file, header=True, inferSchema=True)

# Data Exploration
df.printSchema()
print(f"Total Rows: {df.count()}")

# Data Cleaning
df_clean = df.filter(col("new_cases").isNotNull())
df_clean = df_clean.withColumn("date", to_date(col("date")))

# Analysis: Total Cases per Country
df_cases = df_clean.groupBy("location").agg(sum("new_cases").alias("total_cases"))
df_cases_pd = df_cases.toPandas()

# Analysis: Total Deaths per Country
df_deaths = df_clean.groupBy("location").agg(sum("new_deaths").alias("total_deaths"))
df_deaths_pd = df_deaths.toPandas()

# Visualization: Total Cases per Country
top_10_cases = df_cases_pd.sort_values(by="total_cases", ascending=False).head(10)
plt.figure(figsize=(12, 6))
sns.barplot(x="total_cases", y="location", data=top_10_cases, palette="Blues_r")
plt.xlabel("Total Cases (in Millions)")
plt.ylabel("Country")
plt.title("Top 10 Countries with Highest COVID Cases")
plt.show()

# Visualization: Total Deaths per Country
top_10_deaths = df_deaths_pd.sort_values(by="total_deaths", ascending=False).head(10)
plt.figure(figsize=(12, 6))
sns.barplot(x="total_deaths", y="location", data=top_10_deaths, palette="Reds_r")
plt.xlabel("Total Deaths")
plt.ylabel("Country")
plt.title("Top 10 Countries with Highest COVID Deaths")
plt.show()

# Visualization: COVID Cases Over Time for India
df_india = df_clean.filter(df_clean.location == "India").select("date", "new_cases").toPandas()
plt.figure(figsize=(12, 6))
plt.plot(df_india["date"], df_india["new_cases"], color='blue', label="Daily Cases")
plt.xlabel("Date")
plt.ylabel("New Cases")
plt.title("Daily COVID Cases Trend in India")
plt.xticks(rotation=45)
plt.legend()
plt.show()

# Save results
df_cases.write.csv("/content/covid_cases_per_country.csv", header=True)
df_deaths.write.csv("/content/covid_deaths_per_country.csv", header=True)

print("Analysis complete! Data saved.")

 🤝 Contributing
Feel free to contribute by improving data insights or adding new visualizations.

---



