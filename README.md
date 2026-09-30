# Amazon Products Sales Analysis | Power BI

A Power BI portfolio project exploring sales performance, product categories, and review volume through interactive visuals and DAX time-intelligence measures.

I built this report to practice the full reporting workflow: importing and preparing data, modeling relationships, writing DAX, and designing an interactive dashboard. I created the **four KPI measures, date table, and all its calculated columns using DAX**.

## Dashboard preview

<img width="1915" height="951" alt="dashboard-overview" src="https://github.com/user-attachments/assets/05308ae6-cd56-48ff-8289-11cd6a670895" />

The report includes product-category and quarter slicers, KPI cards, time-series charts, a category performance matrix, and product rankings.

## Business questions

- How does the report's sales metric perform year to date and quarter to date?
- How does sales activity vary by month and week?
- Which product categories contribute the largest share of YTD sales?
- Which five products lead by YTD sales and review volume?
- How do the results change when filtering by product category or quarter?

## Tools and skills

**Power BI Desktop · Power Query · DAX · Data modeling · Time intelligence · Data visualization**

Work completed includes:

- Importing data and working with file-based sources, including CSV connectivity.
- Cleaning and processing data with Power Query.
- Building the data model and a dedicated DAX date table.
- Using date, text, filter, and calculation functions, including `CALCULATE` and YTD/QTD time intelligence.
- Applying custom sorting to calendar labels.
- Creating KPI cards, charts, slicers, and a conditionally formatted matrix.
- Formatting visuals and working with report navigation.

## Dataset

The supplied workbook, `Amazon_Combined_Data.xlsx`, contains one worksheet named `Amazon_Data`.

| Attribute | Details |
| --- | --- |
| Source records | 89,082 |
| Date coverage | January 3, 2019–December 31, 2022 |
| Product categories | 8 |
| Fields | Product Category, Product Description, Price(Dollar), Number of reviews, Shipment, Order Date |

The dataset covers Audio Video, Camera, Car Accessories, Laptop, Men Clothes, Men Shoes, Mobile & Accessories, and Toys. These describe the supplied source workbook; the dashboard reflects its own transformations, measures, and filter context.

## DAX date table

I created a separate date table in Power BI rather than relying only on the source date column. All the fields shown below were created using DAX:

| Field | Purpose |
| --- | --- |
| Date | Calendar dates for time-based analysis |
| Month Name | Readable month labels |
| Month Number | Chronological month sorting |
| Week | Sunday-start week numbering using `WEEKNUM` with return type 1 |
| Quarter Number | Numeric quarter ordering |
| Quarter | Readable quarter labels, such as Qtr 1 |

<img width="785" height="332" alt="dax-date-table" src="https://github.com/user-attachments/assets/2359cf7b-e1c5-4ce1-97c2-49d4f2408d60" />

This work helped me practice calendar modeling, calculated columns, chronological sorting, and time-intelligence calculations.

The table uses `CALENDAR` from the earliest to the latest Order Date in the source. Month labels use `FORMAT` with `MMM`; month and quarter numbers use `MONTH` and `QUARTER`. Quarter labels concatenate `Qtr ` with the quarter number. The week containing January 1 is week 1.

See the [original date-table and calculated-column expressions](dax/date-table.dax).

## KPI measures written in DAX

```dax
QTD Sales = TOTALQTD(SUM(Amazon_Data[Price(Dollar)]), 'Date Table'[Date])

YTD Products Sold = TOTALYTD(COUNT(Amazon_Data[Product Category]), 'Date Table'[Date])

YTD Reviews = TOTALYTD(SUM(Amazon_Data[Number of reviews]), 'Date Table'[Date])

YTD Sales = TOTALYTD(SUM(Amazon_Data[Price(Dollar)]), 'Date Table'[Date])
```

| Measure | Calculation |
| --- | --- |
| QTD Sales | Sum of `Price(Dollar)` over the quarter-to-date date context |
| YTD Products Sold | Count of nonblank `Product Category` values over the year-to-date date context; this counts records, not distinct products |
| YTD Reviews | Sum of `Number of reviews` over the year-to-date date context |
| YTD Sales | Sum of `Price(Dollar)` over the year-to-date date context |

The measures use calendar-year time intelligence and respond to the report's date and filter context. Their original expressions are also available in [measures.dax](dax/measures.dax).

## KPIs and visuals

| Component | Purpose |
| --- | --- |
| YTD Sales card | Summarize the report's sales metric for the year-to-date period |
| QTD Sales card | Summarize the report's sales metric for the quarter-to-date period |
| YTD Products Sold card | Display the report's product-count metric for the YTD period |
| YTD Reviews card | Display review volume associated with records in the YTD period |
| Sales by Month line/area chart | Compare monthly sales patterns |
| Sales by Week column chart | Explore shorter-term variation |
| Sales by Product Category matrix | Compare YTD sales, QTD sales, and percentage contribution |
| Top 5 Products by YTD Sales | Identify leading products by the sales metric |
| Top 5 Products by YTD Reviews | Identify products with the highest review volume |
| Product Category and Quarter slicers | Explore selected categories and reporting periods |

The monthly and weekly charts are labeled as period sales in the dashboard. They should be distinguished from cumulative YTD measures when interpreting trends.

## Results visible in the dashboard

In the captured report view:

- **YTD Sales:** approximately **$2.18M**; the category matrix shows **$2,177,738**.
- **QTD Sales:** **$811,090**.
- **YTD Products Sold:** **27.75K**, using the report's displayed metric label.
- **YTD Reviews:** approximately **19M**, using the report's displayed metric label.
- **Men Shoes** contributes **43.18%** of YTD sales, followed by **Camera at 22.62%** and **Men Clothes at 16.42%**.
- These three categories together account for approximately **82.22%** of displayed YTD sales, indicating that performance in this report view is concentrated in a few categories.

These are observations from the saved screenshot, not totals for the entire source workbook. Results depend on the report's measure definitions and date/filter context.

## Interpretation and limitations

- This is a learning and portfolio project using an Amazon product dataset; it is not an official Amazon financial report.
- The source contains `Price(Dollar)` but no quantity field. Interpreting summed prices as sales assumes an appropriate record-level meaning; the data alone does not establish realized revenue.
- “YTD Products Sold” counts nonblank Product Category entries. Interpreting this as units sold assumes each counted record represents one unit; the source has no quantity field to verify that assumption.
- Review counts indicate review volume, not ratings or customer satisfaction. The source has no individual review dates, so filtering by Order Date does not establish when reviews were written.
- The source spans multiple years. Year and date context matter when interpreting YTD/QTD results or grouping by month and week.

## What I learned

This project strengthened my ability to turn reporting requirements into a Power BI report, create a reusable date table with DAX, apply time intelligence, and combine summary KPIs with detailed comparisons. It also reinforced the importance of documenting metric definitions so that visual labels accurately describe the source data.

## Repository contents

- `README.md` — project overview, dashboard preview, approach, and observations.
- `assets/dashboard-overview.png` — screenshot of the completed dashboard.
- `assets/dax-date-table.png` — screenshot of the DAX-created date table.
- `dax/date-table.dax` — original DAX for the date table and its five calculated columns.
- `dax/measures.dax` — original DAX for the four KPI measures.
- 'Amazon_Combined_Data.xlsx' - excel spreadsheet containing data. 

The screenshots provide an immediate preview, and the DAX files document the calculation logic. The `.pbix` file is also included in this package.

## Acknowledgments

The supplied problem statement and functionality references informed the project requirements. The Power BI implementation and DAX date-table work described here were completed by me.

https://drive.google.com/drive/folders/1ZYSOUAGpZKqzBTV9curE9F_lviO0kGUK
