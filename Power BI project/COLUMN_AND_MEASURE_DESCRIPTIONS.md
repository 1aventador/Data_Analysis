# Column and Measure Descriptions for Business Users

## Territory Lookup Table

| Column | Description |
|--------|-------------|
| SalesTerritoryKey | Unique identifier for each sales territory |
| Region | Sales region or territory name (e.g., Southwest, Northeast) |
| Country | Country where the territory operates (e.g., United States, Canada) |
| Continent | Continent where the territory is located (e.g., North America, Europe) |

## Product Categories Lookup Table

| Column | Description |
|--------|-------------|
| ProductCategoryKey | Unique identifier for each product category |
| CategoryName | Product category name (e.g., Bikes, Accessories, Clothing) |

## Product Subcategories Lookup Table

| Column | Description |
|--------|-------------|
| ProductSubcategoryKey | Unique identifier for each product subcategory |
| SubcategoryName | Subcategory name (e.g., Road Bikes, Mountain Bikes, Helmets) |
| ProductCategoryKey | Foreign key linking to parent category (hidden from users) |

## Product Lookup Table

| Column | Description |
|--------|-------------|
| ProductKey | Unique identifier for each product |
| ProductSubcategoryKey | Foreign key linking to product subcategory (hidden from users) |
| ProductSKU | Stock Keeping Unit - unique code for inventory tracking |
| SKU Type | Product type extracted from SKU (e.g., RFR, RCA, HES) |
| ProductName | Descriptive name of the product |
| ModelName | Model name of the product (e.g., Mountain-100, Road-750) |
| ProductDescription | Detailed description of product features and specifications |
| ProductColor | Color of the product (e.g., Red, Black, Silver) |
| ProductSize | Size of the product (e.g., S, M, L, XL) |
| ProductStyle | Style classification of the product (e.g., Unisex, Women's, Men's) |
| ProductCost | Manufacturing or acquisition cost per unit |
| ProductPrice | Retail selling price per unit |
| Discount Price | Price Point | Calculated category based on retail price (Low: <$100, Mid-Range: $100-$500, High: >$500) |
| SKU Category | First part of SKU code used for product classification |

## Customer Lookup Table

| Column | Description |
|--------|-------------|
| CustomerKey | Unique identifier for each customer |
| Prefix | Salutation prefix (Mr., Mrs., Ms., Dr., etc.) |
| FirstName | Customer's first name |
| LastName | Customer's last name |
| BirthDate | Customer's date of birth |
| MaritalStatus | Marital status (Single, Married, Divorced, Widowed) |
| Gender | Gender (M/F or Male/Female) |
| EmailAddress | Customer's email address for contact |
| AnnualIncome | Total annual household income in dollars |
| TotalChildren | Number of children in the household |
| EducationLevel | Highest education level (High School, Bachelors, Graduate, etc.) |
| Occupation | Customer's primary occupation or job title |
| HomeOwner | Whether customer owns or rents their home (Yes/No) |
| Full Name | Customer's complete full name from source data |
| Domain Name | Email domain extracted from email address |
| Is Parent? | Calculated field: Yes if TotalChildren > 0, else No |
| Customer Priority | Calculated field: "Priority" if Income > $100K AND has children, else "Standard" |
| Income Level | Calculated income bracket (Low: <$50K, Average: $50-100K, High: $100-150K, Very High: >$150K) |
| Education Category | Calculated education grouping (High School, Undergrad, Graduate) |
| Customer Full Name (CC) | Complete formatted name combining Prefix, FirstName, LastName |
| Birth Year | Extracted year from birth date |

## Calendar Lookup Table

| Column | Description |
|--------|-------------|
| Date | Calendar date (key field) - every date has a row in this dimension |
| Day Name | Name of the day of week (Monday, Tuesday, etc.) |
| Start of Week | First day (Monday) of the week containing this date |
| Start of Month | First day of the month containing this date |
| Start of Quarter | First day of the quarter containing this date |
| Month Name | Full month name (January, February, etc.) |
| Month Number | Month as number (1-12) |
| Start of Year | January 1st of the year containing this date |
| Year | Calendar year (e.g., 2020, 2021) |
| Month Number (DAX) | Month calculated as text using SWITCH function |
| Month Short | 3-letter abbreviation of month (JAN, FEB, etc.) |
| Day of Week | Day of week as number (1=Monday, 7=Sunday) |
| Weekend | Categorization (Weekend or Weekday) |

## Sales Data Table (Fact Table)

| Column | Description |
|--------|-------------|
| OrderDate | Date when the order was placed (hidden from end users) |
| StockDate | Date when inventory was recorded (hidden from end users) |
| OrderNumber | Unique order identifier (e.g., SO1234567) |
| ProductKey | Foreign key to Product Lookup table (hidden from users) |
| CustomerKey | Foreign key to Customer Lookup table (hidden from users) |
| TerritoryKey | Foreign key to Territory Lookup table (hidden from users) |
| OrderLineItem | Line number within an order (1st item, 2nd item, etc.) |
| OrderQuantity | Quantity of items ordered in this line item |
| Quantity Type | Calculated field: "Single Item" if Qty=1, else "Multiple Items" |

## Returns Data Table (Fact Table)

| Column | Description |
|--------|-------------|
| ReturnDate | Date when product was returned (hidden from end users) |
| TerritoryKey | Territory associated with the return (hidden from users) |
| ProductKey | Product being returned (hidden from users) |
| ReturnQuantity | Number of units returned |

## Measure Table - All Measures

### Base Sales Metrics

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Quantity Sold | Total number of items sold across all orders. Sum of all order quantities. | Use in KPI cards, trends, or comparisons with returned quantity |
| Quantity Returned | Total number of items returned by customers. Sum of all return quantities. | Compare with Quantity Sold to understand return rate; higher returns may indicate quality issues |
| Total Orders | Count of unique orders placed. Each order number counted once. | Use to track order volume and sales activity |
| Total Customers | Count of unique customers who made purchases. Each customer counted once. | Indicates customer base size and market reach |
| Total Returns | Count of individual return transactions. | Monitor return activity and customer satisfaction |
| Return Rate | Percentage of items returned vs sold. Formula: Returned / Sold | Watch for increases indicating product/service quality issues. Target: <5% |

### Revenue & Cost Metrics

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Total Revenue | Total sales revenue from all orders. Calculated as Quantity × Price. | Primary KPI for sales performance; compare against target and prior year |
| Total Cost | Total cost of goods sold. Calculated as Quantity × Product Cost. | Essential for profitability analysis; track against revenue |
| Total Profit | Gross profit earned. Formula: Revenue - Cost. | Core financial metric; use to assess business profitability |
| Average Retail Price | Average selling price across all products in selection. | Understand pricing trends; identify high/low price point products |
| Overall Average Price | Overall average price ignoring all filters. Baseline for comparison. | Use as reference point for regional or period-specific pricing analysis |
| Average Revenue per Customer | Revenue divided by customer count. How much each customer spends on average. | Indicates customer value; use for segmentation and targeting strategies |
| Average Order Value | Total revenue divided by number of orders. Average $ per order. | Track to understand purchase patterns; increases suggest upselling success |
| Revenue per Unit | Revenue divided by items sold. Average price per item sold. | Useful for understanding effective selling price including mix effects |
| Cost per Unit | Total cost divided by items sold. Average cost per item. | Important for margin analysis and pricing decisions |

### Profitability Analysis

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Gross Margin % | Profit as percentage of revenue. Formula: (Revenue-Cost)/Revenue | Target industry benchmark: 40-60% for retail; monitor monthly trends |
| Profit Margin % | Net profit as percentage of revenue. Formula: Profit/Revenue | Bottom line profitability; target varies by business model (typically 5-15%) |

### Order Analysis

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Bulk Orders | Count of orders with quantity > 1 (multiple items). | Track bulk purchasing behavior; useful for B2B analysis |
| Weekend Orders | Count of orders placed on Saturdays and Sundays. | Understand weekend vs weekday purchase patterns for staffing |
| High Ticket Orders | Count of orders containing items above average price. | Identify luxury/premium product demand; useful for inventory planning |
| All Orders | Total orders across all contexts, ignoring filters. | Baseline comparison for filtered results |
| % of All Orders | Current selection as percentage of total orders. | See what portion of business a segment represents |

### Product-Specific Analysis

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Bike Sales | Quantity of bikes sold. | Track bike category performance separately from other products |
| Bike Returns | Number of bike units returned. | Monitor bike quality and customer satisfaction |
| Bike Return Rate | Return percentage for bikes only. | Compare against overall return rate; high rates suggest product issues |
| Products Sold Count | Number of unique products sold (distinct products). | Understand product mix breadth; more variety suggests customer choice |
| Average Revenue per Product | Total revenue divided by number of unique products sold. | Indicates average product value in sales mix |
| Return Rate by Category | Return percentage by product category. | Identify categories with quality or satisfaction issues |
| High Return Products | Count of products with return rate > 10%. | Flag problem products for quality review |

### Return Analysis

| Measure | Description | How to Use |
|---------|-------------|-----------|
| All Returns | Total returns across all contexts. Baseline comparison. | Use as denominator for return percentage calculations |
| % of All Returns | Current return count as percentage of total returns. | Understand proportion of returns by segment |

### Time Intelligence Metrics

| Measure | Description | How to Use |
|---------|-------------|-----------|
| YTD Revenue | Revenue from January 1st to selected date in calendar year. | Track annual progress; compare with target |
| Previous Month Revenue | Revenue from prior calendar month. | Month-over-month comparison baseline |
| Previous Month Orders | Order count from prior month. | Compare order trends month-over-month |
| Previous Month Returns | Return count from prior month. | Monitor return trends and seasonal patterns |
| Previous Month Profit | Profit from prior month. | Profitability comparison over time |
| Year-over-Year Revenue | Revenue from same date period last year. | True annual comparison accounting for seasonality |
| Same Period Last Year Revenue | Revenue from exactly 365 days ago. | Precise annual comparison for trend analysis |
| 10-Day Rolling Revenue | Revenue from last 10 days including today. | See short-term trend smoothing |
| 90-Day Rolling Profit | Profit from last 90 days. | Quarterly view of profitability trends |

### Growth & Performance Metrics

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Sales Growth % | Month-over-month revenue change as percentage. | Monitor momentum; positive = growth, negative = decline |
| Year-over-Year Growth % | Annual revenue growth percentage. | Key strategic metric; compare against industry benchmarks |
| Order Frequency | Orders per customer on average. | Indicates customer engagement; higher = more loyal |
| Customer Repeat Purchase Rate | Percentage of customers making multiple purchases. | Loyalty indicator; target: >30% for healthy business |
| Territory Performance Index | Territory revenue vs average territory revenue. | >1.0 = above average, <1.0 = below average territory |

### Customer Analytics

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Top 10% Customer Revenue | Revenue from top 10% of customers by value. | Understand revenue concentration; 80/20 rule often applies |
| Top 10% Customer % of Total | Top customers' revenue as % of total revenue. | If >50%, risk of customer concentration |

### Engagement & Status

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Days Since Last Order | Number of days since most recent order. | Identify inactive customers; 30+ days = at-risk customer |

### Target Comparison Metrics

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Revenue Target | Target revenue set at 110% of previous month. | Compare actual vs target; shows if business is growing |
| Order Target | Target orders at 110% of previous month. | Growth target for order volume |
| Profit Target | Target profit at 110% of previous month. | Profitability growth target |
| Revenue vs Target | Actual revenue minus target. Shows gap in dollars. | Positive = exceeding target, Negative = falling short |
| Revenue vs Target % | Revenue gap as percentage of target. | Shows % under/over performance |
| Profit vs Target | Actual profit minus target (in dollars). | Profitability performance vs goal |
| Profit vs Target % | Profit gap as percentage of target. | Shows % profit under/over performance |
| YTD % of Target | Year-to-date progress toward annual target. | >100% on target, <100% behind pace |

### Fiscal Year Time Intelligence (July-June)

| Measure | Description | How to Use |
|---------|-------------|-----------|
| Fiscal Year Start Month | Constant value = 7 (July). Documents fiscal year start. | Reference only; confirms July-June fiscal period |
| Current Fiscal Year | Calculates current fiscal year based on today's date. | Context for reports; changes automatically on July 1st |
| Fiscal Year (Selected Date) | Calculates fiscal year for any selected date in filters. | Works with date slicers to show correct fiscal year |
| Fiscal Year Start Date | Start date of current fiscal year (July 1st of prior year). | Reference point for YTD calculations |
| Fiscal Year End Date | End date of fiscal year (June 30th). | Reference point for FY-end analysis |
| Total Sales YTD (Fiscal) | Revenue from July 1st to selected date (fiscal YTD). | Primary fiscal year KPI; tracks annual progress |
| Total Sales PY (Fiscal) | Revenue from same period last fiscal year. | Enables true year-over-year comparison |
| YoY % Growth (Fiscal) | Year-over-year growth percentage (fiscal basis). | Key growth metric for leadership review |
| YoY Δ (Fiscal) | Year-over-year change in absolute dollars. | Shows real impact of growth |
| Previous Month Sales | Sales from prior calendar month. | Month-over-month comparison and trends |

---

## Notes for Business Users

- **Fiscal Year**: This model uses a fiscal year that runs July 1 - June 30 (not calendar year)
- **Hidden Columns**: Some technical columns are hidden (showing only *Key fields and dates) - these are foreign keys needed for lookups
- **Format Display**: Currency shows $ by default; percentages show as 0.00%; Numbers use thousands separators
- **Filters**: Most measures work correctly with date filters, territory filters, product filters, and customer filters
- **Priority**: Focus on Revenue, Profit %, Growth %, and Customer Count as primary KPIs

## Contact for Questions
For questions about measure definitions, calculations, or business rules, consult the Power BI model documentation or contact the Analytics team.
