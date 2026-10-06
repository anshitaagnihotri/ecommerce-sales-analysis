# E-Commerce Sales & Customer Behavior Analysis using Python

Exploratory Data Analysis (EDA) of the UCI Online Retail dataset: sales performance, customer purchasing behaviour, product demand and geographical distribution of customers.

## Problem Statement

An online retail company wants to understand its:

- Sales performance
- Customer purchasing behaviour
- Product demand
- Geographical distribution of customers

## Dataset

- **UCI Online Retail dataset:** https://archive.ics.uci.edu/dataset/352/online+retail
- File: `data/Online Retail.xlsx` (541,909 rows, 8 columns)
- Period: 1 December 2010 to 9 December 2011
- Columns: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country

## Tools

Python, Pandas, NumPy, Matplotlib, Seaborn, Jupyter Notebook (VS Code).

## How to Run

1. Clone the repo: `git clone https://github.com/anshitaagnihotri/ecommerce-sales-analysis.git`
2. Install libraries: `pip install -r requirements.txt`
3. Open `ecommerce_eda.ipynb` in VS Code or Jupyter and run all cells. The notebook reads `data/Online Retail.xlsx` (loading takes 1 to 2 minutes).

## Project Structure

```
ecommerce-sales-analysis/
├── data/
│   └── Online Retail.xlsx
├── ecommerce_eda.ipynb
├── requirements.txt
└── README.md
```

## Data Cleaning

| Problem found | Count | Decision |
|---|---|---|
| Exact duplicate rows | 5,268 | Removed |
| Cancelled invoices (InvoiceNo starts with "C") | 9,288 rows | Moved to a separate table, kept for return analysis |
| Zero or negative price or quantity | 2,517 price rows, 10,624 negative quantity rows | Removed (bad entries and adjustments) |
| Non-product entries (postage, fees, manual adjustments) | StockCode not starting with 5 digits | Removed |
| Missing Description | 1,454 | Removed |
| Missing CustomerID | 135,080 (about 25%) | Rows kept (real sales); only rows with CustomerID used for customer analysis |
| Two invoices (541431 and 581483) with 74,215 and 80,995 units | 2 | Removed, because each was cancelled later (about 245,000 revenue) |

Other large orders were kept because they can be genuine wholesale sales.

**Final dataset:** about 522,500 rows, 4,333 customers (with ID), 19,771 invoices, 3,899 products, 38 countries, total revenue 10,001,167.

New columns created: `TotalAmount` (Quantity x UnitPrice), `Year`, `Month`, `YearMonth`, `DayOfWeek`, `Hour`, `HasCustomer`.

## Analysis Performed

- **Univariate:** histograms of Quantity, UnitPrice and TotalAmount; box plots for outliers
- **Bivariate:** Orders vs Total Spent, Unit Price vs Quantity (scatter plots)
- **Multivariate:** correlation heatmap at customer level
- **Time series:** monthly revenue trend and monthly units of top products
- **Customer and product analysis:** top countries, products and customers (bar charts); orders by day and hour (count plots)

## Key Insights

1. **Revenue is seasonal.** Monthly revenue stays at 5 to 7.7 lakh from January to August 2011, then rises sharply to a peak in November 2011 (1,452,113), about 2.9 times the weakest full month (February 2011, 507,783). December 2011 looks low only because the data ends on 9 December.
2. **The business depends heavily on the United Kingdom.** The UK gives 84.8% of revenue; the other 37 countries together give 15.2%.
3. **A small group of customers drives revenue.** The top 20% of customers (with CustomerID) give 73.9% of revenue, close to the 80/20 rule.
4. **Top products by revenue and by units are different.** Regency Cakestand 3 Tier leads in revenue, while World War 2 Gliders Asstd Designs (54,951 units), Jumbo Bag Red Retrospot (48,371) and White Hanging Heart T-Light Holder (37,872) lead in units.
5. **Order value is skewed.** Average order value is 505.85 but the median is 302.20, so a few very large orders pull the average up.
6. **About 14.81% of invoices are cancelled** (3,836 of 25,900).
7. **Orders follow office hours.** Thursday is the busiest day (4,209 orders), Sunday the weakest (2,203) and there are no Saturday orders. Orders peak at 12:00 (3,204 orders).
8. **Price and quantity move in opposite directions.** Bulk orders are mostly for cheap products and expensive items sell in small quantities.
9. **Customer spend is linked to items bought** (correlation 0.92) and, more weakly, to the number of orders (0.58).
10. **Demand is changing.** White Hanging Heart T-Light Holder (-24.7%), Pack of 72 Retrospot Cake Cases (-20.9%) and Assorted Colours Silk Fan (-19.4%) are declining, while Jumbo Bag Pink Polkadot (+79.6%) and Red Harmonica in Box (+90.2%) are growing. Popcorn Holder and Rabbit Night Light look like late launches, so their growth percentages are not meaningful.

## Business Recommendations

- Prepare stock and marketing for September to November, when demand rises most.
- Reduce dependence on the UK with campaigns and shipping offers in Netherlands, EIRE, Germany and France.
- Give loyalty offers to the top customers, since they bring most of the revenue.
- Investigate the high cancellation rate (stock, shipping or entry errors).
- Send promotions on Tuesday to Thursday during office hours.
- Promote or bundle products with falling demand and keep stock of rising products.

## Limitations

- The data covers only one year (December 2010 to 9 December 2011), so seasonality cannot be confirmed across years.
- About 25% of rows have no CustomerID, so customer insights cover only the remaining customers.
- The reason for the November peak (festive season) is an assumption and is not proved by the data.
- There are no Saturday orders in the dataset, which may be a data collection issue.
