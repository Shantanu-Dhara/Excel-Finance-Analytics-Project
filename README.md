# AtliQ Hardware – Finance Analytics

> Codebasics Data Analytics Bootcamp · Excel project
> **Author:** Shantanu Dhara

## 1. Problem Statement

### Business Overview
AtliQ Hardware is a fast-growing company that sells computer hardware (PCs, mice, keyboards, printers, storage and networking products). Its business model is similar to Dell or HP. AtliQ sells to **customers** (stores and online platforms), and those customers sell to **consumers** like you and me.

**Customer types (platform)**
- **Brick & Mortar**: physical stores such as Croma and Best Buy.
- **E-Commerce**: online marketplaces such as Amazon and Flipkart, plus AtliQ's own e-store.

**Sales channels (business model)**
| Channel | How AtliQ sells | Examples |
|---|---|---|
| **Retailer** | Sells to retail chains and marketplaces, which sell on to consumers | Croma, Best Buy, Amazon, Flipkart |
| **Direct** | Sells straight to consumers through its own stores | AtliQ e-Store, AtliQ Exclusive |
| **Distributor** | Sells through a middleman where direct sales aren't possible | Markets such as China and South Korea |

### Problem to Solve
AtliQ's expansion into **Latin America (LATAM)** ended in heavy losses because decisions were based on surveys and intuition. Management now wants **data-backed decisions**, and Dell, a much larger competitor with a big analytics team, is growing fast. Sales alone do not show whether AtliQ is actually making money, so the finance team needs profitability views built from net sales and product costs. This project builds two Excel reports:

1. **P&L by market** for FY2021: net sales, COGS, gross margin and GM % for every market.
2. **GM % by sub-zone and quarter** for FY2019, FY2020 and FY2021, to see where margins are moving.

## 2. Data
| Table | Type | Description |
|---|---|---|
| `fact_sales_monthly` | Fact | `date, product_code, customer_code, Qty, net_sales_amount` (from Sep 2018) |
| `fact_sales_monthly_with_cost` | Fact | The same columns plus `freight_cost` and `manufacturing_cost` |
| `dim_customer` | Dimension | customer, market, platform, channel |
| `dim_product` | Dimension | division, segment, category, product, variant |
| `dim_market` | Dimension | market → sub_zone → region |
| `ns_targets_2021` | Target | Monthly net-sales target per market, Sep 2020 – Aug 2021 |

The fact files are too large for GitHub, so only 30-row samples are included.

## 3. Approach
1. **Load and model**: Excel Data Model (Power Pivot). Join the cost fact table to `dim_customer` on `customer_code`, then to `dim_market` on `market`.
2. **Clean**: the `NA` sub-zone for Canada and USA is stored as `nan`. Also fix encoding (`Elkj�p`) and the `Philiphines` typo.
3. **Build the P&L measures**:
   - Net Sales = sum of `net_sales_amount`
   - COGS = sum of `manufacturing_cost` + `freight_cost`
   - Gross Margin = Net Sales − COGS
   - GM % = Gross Margin ÷ Net Sales
4. **Report 1**: P&L by market, with region, sub-zone and FY slicers.
5. **Report 2**: GM % by sub-zone across Q1–Q4, one block per FY.

## 4. Results

### Report 1: P&L by market, FY2021 (USD)
| | Net Sales | COGS | Gross Margin | GM % |
|---|---|---|---|---|
| **All markets** (sum of rows) | **598.9M** | **380.6M** | **218.3M** | **36.4%** |
| India | 161.26M | 109.7M | 51.6M | 32.0% |
| USA | 87.78M | 55.3M | 32.5M | 37.0% |
| South Korea | 48.97M | 31.4M | 17.6M | 35.9% |
| Canada | 35.06M | 21.7M | 13.4M | 38.2% |
| United Kingdom | 34.15M | 18.7M | 15.4M | 45.1% |

- **Highest GM %:** Newzealand 48.2%, Japan 46.5%, United Kingdom 45.1%, France 43.2%, Poland 42.6%.
- **Lowest GM %:** Germany 26.2%, Norway 29.5%, Austria 30.1%, Italy 30.1%, India 32.0%.

### Report 2: GM % by sub-zone (grand total for the year)
| Sub-zone | FY2019 | FY2020 | FY2021 |
|---|---|---|---|
| ANZ | 42.6% | 42.8% | 38.3% |
| India | 42.4% | 32.2% | 32.0% |
| NA (Canada, USA) | 35.4% | 39.8% | 37.3% |
| NE | 36.7% | 37.8% | 38.3% |
| ROA | 44.4% | 38.2% | 38.3% |
| SE | 44.2% | 37.9% | 38.5% |

## 5. Insights
- **AtliQ earned about 36% gross margin in FY2021**, roughly $218M on $599M of net sales.
- **India is the biggest market but a low-margin one.** It has 27% of net sales and 24% of gross margin, at 32.0% GM.
- **India's margin fell by about 10 points in one step**, from 42.4% in FY2019 to 32.2% in FY2020, and has stayed there. A drop at a year boundary points to a cost or pricing change, not gradual erosion.
- **ROA and SE also lost about 6 points of GM** in FY2020, and ANZ lost about 4.5 points in FY2021.
- **NE is the only sub-zone with steadily rising margin** (36.7% → 37.8% → 38.3%).
- **Quarters within a year are very stable**, usually within 1–2 points, so the margin shifts are year-level changes and not seasonal.
- **Germany's 26.2% GM is the weakest**, and Norway, Austria and Italy are also below 31%.
- **Sales and profit rankings differ.** The UK has 5.7% of net sales but 7.1% of gross margin, while Germany has 2.0% of net sales but 1.4% of gross margin.
- **Concentration risk:** India and the USA together bring 42% of net sales and 38% of gross margin.

## 6. Recommendations
1. **Investigate India's margin first.** Review pricing, discounts, manufacturing cost and freight cost since FY2020. Bringing India up to the company average of 36% would add about $7M of gross margin on FY2021 sales (illustrative).
2. **Grow the high-margin markets**: Newzealand, Japan, the UK, France and Poland.
3. **Fix the weak European markets**: Germany, Norway, Austria and Italy. Check whether freight, product mix or low prices are the cause.
4. **Drill into ROA, SE and ANZ** by product, customer and channel to find what drove the margin drop.
5. **Track GM % by sub-zone every quarter** as a standard report, and check expected margin before entering any new market (the LATAM lesson).
6. **Reduce dependence on India and the USA** by growing other markets without sacrificing margin.

## 7. Folder structure
```
atliq-finance-analytics/
├── README.md
├── datasets/
│   ├── raw/               # dim_customer, dim_market, dim_product, ns_targets_2021 (CSV)
│   └── sample/            # 30-row samples of the two fact tables (full files are too large)
├── reports/               # report1_pnl_by_market_fy2021.pdf, report2_gm_pct_by_subzone_quarter.pdf
├── solution/              # assignment_solution.xlsx
└── business_problem/      # problem_statement.pdf
```

## 8. Tools
Microsoft Excel: Data Model / Power Pivot, PivotTables, slicers and measures

## 9. Notes
- All values are in USD and come from your two report PDFs.
- The fiscal year runs **September to August**. FY2021 net sales of $598.9M match the 2021 total in the sales analytics project, and the targets file covers Sep 2020 – Aug 2021. Quarters are read as Q1 = Sep–Nov.
- **COGS = manufacturing cost + freight cost.** This is inferred from the cost file columns, so confirm it against your workbook.
- Report 1 totals are the sum of the rounded market rows, so they can differ slightly from the workbook's grand total.
- `ns_targets_2021` is provided but is not used in these two reports. It can support a target-vs-actual follow-up.
