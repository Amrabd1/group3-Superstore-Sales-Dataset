# group3-Superstore-Sales-Dataset
import pandas as pd
df = pd.read_csv("Superstore Sales Dataset.csv")
df.info()
df.info()
df.describe()
# Convert date columns to datetime format for time-based analysis
df["Order Date"] = pd.to_datetime(df["Order Date"], format="%d/%m/%Y")
df["Ship Date"] = pd.to_datetime(df["Ship Date"], format="%d/%m/%Y")

# Extract year and month for trend analysis
df["Year"] = df["Order Date"].dt.year
df["Month"] = df["Order Date"].dt.month

# Extract order fulfillment time
df["Delivery Time (Days)"] = (df["Ship Date"] - df["Order Date"]).dt.days

# Now, I'll perform various analyses based on the given objectives. Let's start with sales performance analysis.
sales_performance = df.groupby(["Year", "Month"])["Sales"].sum().reset_index()
sales_performance
# Extract Year and Month from Order Date
df["Order Date"] = pd.to_datetime(df["Order Date"])  # Ensure correct format
df["Year"] = df["Order Date"].dt.year
df["Month"] = df["Order Date"].dt.month

# Aggregate sales data by Year and Month
sales_performance = df.groupby(["Year", "Month"])["Sales"].sum().reset_index()
import matplotlib.pyplot as plt
import seaborn as sns

# Set the figure size and style
plt.figure(figsize=(12, 6))
sns.set_style("whitegrid")

# Plot sales trends over time
sns.lineplot(data=sales_performance, x="Month", y="Sales", hue="Year", marker="o", palette="tab10")

# Customize the plot
plt.title("Monthly Sales Trends Over the Years", fontsize=14)
plt.xlabel("Month", fontsize=12)
plt.ylabel("Total Sales ($)", fontsize=12)
plt.xticks(range(1, 13), ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"])
plt.legend(title="Year")
plt.show()

#Top 10 Best-Selling Products
top_products = df.groupby("Product Name")["Sales"].sum().nlargest(10)
plt.figure(figsize=(10, 6))
sns.barplot(y=top_products.index, x=top_products.values, palette="viridis")
plt.title("Top 10 Best-Selling Products", fontsize=14)
plt.xlabel("Total Sales ($)", fontsize=12)
plt.ylabel("Product Name", fontsize=12)
plt.show()

#Sales Performance by Region
region_sales = df.groupby("Region")["Sales"].sum().sort_values(ascending=False)
plt.figure(figsize=(8, 5))
sns.barplot(y=region_sales.index, x=region_sales.values, palette="coolwarm")
plt.title("Sales Performance by Region", fontsize=14)
plt.xlabel("Total Sales ($)", fontsize=12)
plt.ylabel("Region", fontsize=12)
plt.show()

#  Most Profitable Categories
plt.figure(figsize=(8, 5))
sns.barplot(y=profit_per_category.index, x=profit_per_category.values, palette="coolwarm")
plt.title("Total Sales by Category", fontsize=14)
plt.xlabel("Total Sales ($)", fontsize=12)
plt.ylabel("Category", fontsize=12)
plt.show()

# Calculate Order Fulfillment Time (Days between Order Date and Ship Date)
df["Fulfillment Time (Days)"] = (df["Ship Date"] - df["Order Date"]).dt.days

# 1. Evaluate Order Fulfillment Times
avg_fulfillment_time = df["Fulfillment Time (Days)"].mean()
fulfillment_distribution = df["Fulfillment Time (Days)"].value_counts().sort_index()

# 2. Identify Supply Chain Bottlenecks - Check delays (Orders taking longer than average)
delayed_orders = df[df["Fulfillment Time (Days)"] > avg_fulfillment_time]

# 3. Optimize Shipping Modes - Compare average fulfillment time per shipping mode
shipping_mode_analysis = df.groupby("Ship Mode")["Fulfillment Time (Days)"].mean().sort_values()

# Display calculated metrics
avg_fulfillment_time, delayed_orders.shape[0], shipping_mode_analysis

