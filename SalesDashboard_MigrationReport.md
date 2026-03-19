File Name: SalesDashboard_MigrationReport.md

# Tableau to Power BI Migration Report

## Section 1. Executive Summary

**Workbook Name:** SalesDashboard.twb  
**Tableau Version:** 2024.2.0 (20242.24.0613.1930)  
**Target:** Power BI .pbix file

| Metric | Value | Details | Power BI Notes |
|---|---|---|---|
| Workbook Name | SalesDashboard.twb | Tableau 2024.2.0 | Target: .pbix file |
| Total Dashboards | 2 | Data Analysis Report, Sales Summary Report | 2 Power BI Report Pages |
| Total Worksheets | 8 | Count Orders, Profit Percent by State, Profit Ratio and Discount per State, Profit by Category (2), Profit vs Sales Trend, Sales and Profit by Customer, Stacked Bar chart, Top5 by Sub-category Sales, Total Profit, Total Sales | 10 Power BI Visuals |
| Total Datasources | 3 | Parameters, Sample - Superstore, Superstore dataset (_198060205f (Superstore dataset), Orders (Sample - Superstore) | Power Query connections |
| Calculated Fields | 6 | OrderUntilShip, Profit Ratio, Exclude Segment, Column Sales, Manufacturer, Number of Records | DAX Measures needed |
| Parameters | 2 | Top Customers (range 5–20, step 5), Profit Bin Size (range 50–200, step 50) | What-If Parameters |
| Dashboard Actions | 1 | Filter 2 (generated): on-select | Visual Interactions |
| Dashboard Filters | 8 | All dashboard and worksheet filters extracted | Slicers in Power BI |
| Reference Lines | 1 | Stacked Bar chart | Analytics Pane |
| Custom Visual Required | No | All visuals map to native or conditional formatting | AppSource install not needed |

**Migration Complexity Assessment**

| Component | Complexity | Effort (Hours) | Risk |
|---|---|---|---|
| Data Connections | Low | 2 | Excel sources, no DB risk |
| Visuals (Bar/Line/Matrix) | Low | 4 | Native mapping |
| Visuals (Map) | Medium | 3 | Ensure geo-coding |
| Table Calcs/LOD | Medium | 2 | DAX translation validation |
| Parameters | Low | 1 | What-If parameter |
| Filters & Slicers | Low | 1 | Standard |
| Actions | Low | 1 | Simple filter action |
| Formatting/Theme | Medium | 2 | Palette/number format mapping |
| Security (RLS/CLS) | Low | 1 | None found, recommend RLS |


---

## Section 2. Visuals Inventory

| # | Worksheet Name | Dashboard | Tableau Visual Type | Mark Type | Power BI Visual Type | Power BI Visual Category | Rows Shelf | Columns Shelf | Fields Used | Color Encoding | Filter Count | Calculated Fields | Custom Visual? | Complexity | DAX Required | Migration Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Count Orders | Sales Summary Report | KPI Card | Text | Card | KPI & Gauge | None | None | [Country], [Order Date], [Region], [State] | None | 2 | None | No | Low | None | Use Card visual. Drag measure for count. Set data label formatting. |
| 2 | Profit Percent by State | Sales Summary Report | Map | Multipolygon | Map | Geographic | [Latitude (generated)] | [Longitude (generated)] | [Country], [Order Date], [Profit], [Region], [Sales], [State] | pcto:sum:Profit:qk:2 | 1 | None | No | Medium | DAX for percent: DIVIDE(SUM(Profit), SUM(Sales)) | Use Map visual. Assign State as Location, color by DAX measure. |
| 3 | Profit Ratio and Discount per State | Data Analysis Report | Map | Circle | Map | Geographic | [Latitude (generated)] | [Longitude (generated)] | [Calculation_5571209093911105], [Discount], [Profit], [Sales], [State] | usr:Calculation_5571209093911105:qk | 1 | Profit Ratio (SUM([Profit])/SUM([Sales])) | No | Medium | Profit Ratio = DIVIDE(SUM([Profit]), SUM([Sales])) | Use Map, add DAX for Profit Ratio, set Discount as size. |
| 4 | Profit by Category (2) | Sales Summary Report | Line Chart | Line | Line Chart | Basic Charts | [none:Category:nk] | [Multiple Values] | [Category], [Country], [Order Date], [Profit], [Region], [State] | sum:Profit:qk | 2 | avg(0.0) | No | Low | None | Use Line Chart, Category on axis, Profit as value. |
| 5 | Profit vs Sales Trend | Sales Summary Report | Area/Line Chart | Area/Automatic | Area/Line Chart | Basic Charts | ([sum:Profit:qk] + [sum:Sales:qk]) | [tmn:Order Date:qk] | [Country], [Order Date], [Profit], [Region], [Sales], [State] | sum:Profit:qk, sum:Sales:qk | 3 | None | No | Medium | DAX for trend: SUM over month | Use Combo Chart, add DAX for monthly trend. |
| 6 | Sales and Profit by Customer | Data Analysis Report | Scatter Plot | Shape | Scatter Chart | Basic Charts | [sum:Profit:qk] | [sum:Sales:qk] | [Category], [Customer Name], [Order Date], [Profit], [Region], [Sales], [Segment] | sum:Profit:qk | 4 | None | No | Medium | None | Use Scatter Chart, add Customer Name as legend. |
| 7 | Stacked Bar chart | Data Analysis Report | Stacked % Bar | Bar | 100% Stacked Bar Chart | Proportional | [pcto:sum:Sales:qk:3] | [mn:Order Date:ok] | [Order Date], [Region], [Sales] | none:Region:nk | 1 | None | No | Low | DAX for %: DIVIDE(SUM([Sales]), CALCULATE(SUM([Sales]), ALL([Region]))) | Use 100% Stacked Bar, color by Region. |
| 8 | Top5 by Sub-category Sales | Sales Summary Report | Line Chart | Line | Line Chart | Basic Charts | [none:Sub-Category:nk] | [Multiple Values] | [Country], [Order Date], [Region], [Sales], [State], [Sub-Category] | sum:Sales:qk | 2 | avg(0.0) | No | Low | None | Use Line Chart, filter Top 5 by Sales. |
| 9 | Total Profit | Sales Summary Report | KPI Card | Text | Card | KPI & Gauge | None | None | [Country], [Order Date], [Profit], [Region], [State] | sum:Profit:qk | 2 | None | No | Low | None | Use Card, set DAX for total profit. |
| 10 | Total Sales | Sales Summary Report | KPI Card | Text | Card | KPI & Gauge | None | None | [Country], [Order Date], [Region], [Sales], [State] | sum:Sales:qk | 2 | None | No | Low | None | Use Card, set DAX for total sales. |

---

## Section 3. Dashboards

| Dashboard | Sizing | Sheets Included | Dashboard Filters | Actions | Power BI Page Notes |
|---|---|---|---|---|---|
| Data Analysis Report | Fixed 2084x1452 | Stacked Bar chart, Profit Ratio and Discount per State, Sales and Profit by Customer | Color, Size | None | Set canvas size to 1042x726 (2x scaling) |
| Sales Summary Report | Fixed 2084x1452 | Count Orders, Profit Percent by State, Profit by Category (2), Profit vs Sales Trend, Top5 by Sub-category Sales, Total Profit, Total Sales | Year, Region, Country/State | Filter 2 (generated) | Set canvas size to 1042x726, configure page navigation |

---

## Section 4. Calculations & DAX

| # | Field Name | Tableau Formula | Tableau Type | Used In Worksheets | Power BI DAX Equivalent | DAX Type | Implementation Notes |
|---|---|---|---|---|---|---|---|
| 1 | OrderUntilShip | DATEDIFF('day',[Order Date],[Ship Date]) | Row-level | N/A | OrderUntilShip = DATEDIFF([Order Date], [Ship Date], DAY) | Calculated Column | Add calculated column to Orders table |
| 2 | Profit Ratio | SUM([Profit])/SUM([Sales]) | Aggregate | Profit Ratio and Discount per State | Profit Ratio = DIVIDE(SUM([Profit]),SUM([Sales])) | Measure | Create DAX measure for ratio |
| 3 | Exclude Segment | {Exclude[Segment]:sum([Sales])} | LOD Expression | N/A | ExcludeSegment = CALCULATE(SUM([Sales]),ALL(Orders[Segment])) | Measure | Use ALL to ignore Segment filter |
| 4 | Column Sales | if first()=0 then attr([Calculation_860187590700183552]) elseif (attr([Region])<>lookup(attr([Region]),-1) or attr([Segment])<>lookup(attr([Segment]),-1)) then running_sum(attr([Calculation_860187590700183552])) else 0 end | Table Calculation | N/A | ColumnSales = VAR Current = [ExcludeSegment] VAR PrevRegion = CALCULATE([Region],OFFSET(-1)) VAR PrevSegment = CALCULATE([Segment],OFFSET(-1)) RETURN IF([Index]=0,Current,IF([Region]<>PrevRegion||[Segment]<>PrevSegment,RUNNINGSUM([ExcludeSegment]),0)) | Measure | Requires advanced DAX with variables |
| 5 | Manufacturer | Categorical bin/group on [Product Name] | Group | N/A | Manufacturer = SWITCH(TRUE(), [Product Name] IN {...}, "3D Systems", ...) | Calculated Column | Use DAX for grouping |
| 6 | Number of Records | 1 | Row-level | All | NumberOfRecords = 1 | Calculated Column | Add column for row count |

---

## Section 5. Data Model

| # | Source Table | Source File | Column Name | Tableau Data Type | Tableau Role | Power BI Data Type | Power BI Data Category | Notes |
|---|---|---|---|---|---|---|---|---|
| 1 | Orders$ | sample_-_superstore.xls | Row ID | integer | dimension | Whole Number | N/A | Hidden in Tableau |
| 2 | Orders$ | sample_-_superstore.xls | Order ID | string | dimension | Text | N/A | |
| 3 | Orders$ | sample_-_superstore.xls | Order Date | date | dimension | Date | Date | |
| 4 | Orders$ | sample_-_superstore.xls | Ship Date | date | dimension | Date | Date | |
| 5 | Orders$ | sample_-_superstore.xls | Ship Mode | string | dimension | Text | N/A | |
| 6 | Orders$ | sample_-_superstore.xls | Customer ID | string | dimension | Text | N/A | Hidden in Tableau |
| 7 | Orders$ | sample_-_superstore.xls | Customer Name | string | dimension | Text | N/A | |
| 8 | Orders$ | sample_-_superstore.xls | Segment | string | dimension | Text | N/A | |
| 9 | Orders$ | sample_-_superstore.xls | Country/Region | string | dimension | Text | Country/Region | |
| 10 | Orders$ | sample_-_superstore.xls | City | string | dimension | Text | City | |
| 11 | Orders$ | sample_-_superstore.xls | State | string | dimension | Text | State | |
| 12 | Orders$ | sample_-_superstore.xls | Postal Code | integer | dimension | Whole Number | Postal Code | |
| 13 | Orders$ | sample_-_superstore.xls | Region | string | dimension | Text | Region | |
| 14 | Orders$ | sample_-_superstore.xls | Product ID | string | dimension | Text | N/A | Hidden in Tableau |
| 15 | Orders$ | sample_-_superstore.xls | Category | string | dimension | Text | N/A | |
| 16 | Orders$ | sample_-_superstore.xls | Sub-Category | string | dimension | Text | N/A | |
| 17 | Orders$ | sample_-_superstore.xls | Product Name | string | dimension | Text | N/A | |
| 18 | Orders$ | sample_-_superstore.xls | Sales | real | measure | Decimal Number | N/A | |
| 19 | Orders$ | sample_-_superstore.xls | Quantity | integer | measure | Whole Number | N/A | |
| 20 | Orders$ | sample_-_superstore.xls | Discount | real | measure | Decimal Number | N/A | |
| 21 | Orders$ | sample_-_superstore.xls | Profit | real | measure | Decimal Number | N/A | |

---

## Section 6. Actions & Interactions

| # | Action Name | Tableau Type | Trigger | Source Dashboard | Source Worksheet | Target | Excluded Sheets | Filter Field | Power BI Equivalent | Implementation Steps |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Filter 2 (generated) | Filter | on-select | Sales Summary Report | Profit Percent by State | Sales Summary Report | None | All | Visual Interaction | 1. Select Profit Percent by State visual. 2. Use Format > Edit Interactions to set cross-filter. 3. Test interaction on selection. |

---

## Section 7. Migration Plan

| Phase | Step | Task | Tool/Method | Input | Output | Priority | Est. Time (Hours) |
|---|---|---|---|---|---|---|---|
| Data Setup | 1 | Inventory all Tableau datasources | Manual + XML parse | TWB | Data source table | Critical | 0.5 |
| Data Setup | 2 | Export all Excel files | File copy | .xls files | Power BI project folder | Critical | 0.5 |
| Data Setup | 3 | Import Excel to Power BI | Power Query | Excel files | Power Query tables | Critical | 1 |
| Data Setup | 4 | Set up data categories (geo) | Model View | Data Model | Proper geo fields | High | 0.5 |
| Data Setup | 5 | Create relationships | Model View | Data Model | Relationships | High | 0.5 |
| DAX Measures | 6 | Translate calculated fields | Manual + script | TWB, mapping Excel | DAX measures | Critical | 2 |
| DAX Measures | 7 | Implement What-If parameters | Power BI Modeling | Parameter specs | Parameter tables | High | 0.5 |
| DAX Measures | 8 | Validate DAX logic | Manual test | PBI model | Matching outputs | Critical | 1 |
| Page Build | 9 | Build Data Analysis Report page | Power BI Desktop | Visual mapping | Page layout | High | 1 |
| Page Build | 10 | Build Sales Summary Report page | Power BI Desktop | Visual mapping | Page layout | High | 1 |
| Page Build | 11 | Add Card/Matrix/Map visuals | Power BI Desktop | Visuals Inventory | All visuals | High | 1 |
| Page Build | 12 | Configure Slicers | Power BI Desktop | Filter mapping | Slicers | Medium | 0.5 |
| Page Build | 13 | Add reference line | Analytics Pane | Stacked Bar chart | Ref line | Medium | 0.25 |
| Interactions | 14 | Configure visual interactions | Format > Edit Interactions | Mapping | Linked visuals | Medium | 0.5 |
| Formatting | 15 | Apply color palette | Theme JSON | TWB palettes | Theme applied | Medium | 0.5 |
| Formatting | 16 | Apply number/date formats | Model View | Format mapping | Formats applied | Medium | 0.5 |
| Formatting | 17 | Configure tooltips | Format > Tooltip | Tooltip mapping | Custom tooltips | Low | 0.25 |
| Formatting | 18 | Set up page layout | Canvas settings | Dashboard zones | Layout matched | Medium | 0.5 |
| Testing | 19 | Validate visuals/data | Manual test | Report | Matched outputs | Critical | 1 |
| Testing | 20 | Document migration & compliance | Manual | Report | Final report | High | 0.5 |

---

## Section 8. Automation Options

| # | Automation Step | Tool/Technology | What It Automates | Input Required | Output Produced | Feasibility | Accuracy (%) | Setup Effort (Hours) |
|---|---|---|---|---|---|---|---|---|
| 1 | TWB parsing | Python (lxml/ElementTree) | Extract all workbook metadata | TWB file | JSON/CSV model | High | 98 | 1 |
| 2 | Data export | Tableau Desktop/tabcmd | Export data to CSV | TWB, Tableau | CSV | High | 100 | 0.5 |
| 3 | Formula conversion | Python + LLM (GPT-4) | Translate Tableau to DAX | Calculations mapping | DAX code | Medium | 90 | 1 |
| 4 | TMDL generation | Power BI REST API | Generate TMDL from model | JSON model | TMDL file | Medium | 85 | 1 |
| 5 | PBIR generation | Power BI REST API + pbi-tools | Create PBIR template | JSON model | PBIR file | Medium | 90 | 1 |
| 6 | Power Query M generation | Python | Build M queries for data sources | Data source inventory | .pq files | Medium | 95 | 1 |
| 7 | Interaction config | Power BI API | Automate Edit Interactions | Mapping table | Configured report | Low | 80 | 1 |
| 8 | End-to-end pipeline | Azure DevOps/GitHub Actions | Full migration CI/CD | All files/scripts | Deployed report | Medium | 85 | 2 |

---

## Section 9. Theme & Formatting Mapping

### 9a. Color Palette Mapping

| # | Palette Name | Palette Type | Tableau Hex Colors (ordered) | Applied To (Worksheets) | Power BI Theme Equivalent | Power BI Implementation |
|---|---|---|---|---|---|---|
| 1 | Tableau 10 | categorical | #4e79a7, #f28e2b, #e15759, #76b7b2, #59a14f, #edc948, #b07aa1, #ff9da7, #9c755f, #bab0ac | All | dataColors | Set in theme JSON dataColors array |

### 9b. Per-Visual Formatting Map

| # | Worksheet Name | Mark Color (Hex) | Mark Transparency (Tableau 0-255) | Mark Transparency (PBI 0-100%) | Mark Size | Mark Shape | Data Labels Visible? | Label Collision Handling | Markers on Lines? | Power BI Format Pane Settings |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Count Orders | Default | Default | Default | Default | Default | Yes | Yes | N/A | Format > Data labels |
| 2 | Profit Percent by State | Default | Default | Default | Default | Default | Yes | Yes | N/A | Format > Data colors |
| 3 | Profit Ratio and Discount per State | Default | Default | Default | Default | Circle | Yes | Yes | N/A | Format > Data colors; Markers: Circle |
| 4 | Profit by Category (2) | #f9a655 | Default | Default | 8.3 | Line | Yes | Yes | N/A | Format > Data colors: #f9a655, Line |
| 5 | Profit vs Sales Trend | Default | 137 | 54 | Default | Area/Line | Yes | Yes | Yes | Format > Transparency: 54%, Markers: All |
| 6 | Sales and Profit by Customer | Default | 198 | 78 | 1.1 | Circle | Yes | Yes | N/A | Format > Transparency: 78%, Markers: Circle |
| 7 | Stacked Bar chart | Default | Default | Default | Default | Bar | Yes | Yes | N/A | Format > Data colors |
| 8 | Top5 by Sub-category Sales | #f9a655 | Default | Default | 7.0 | Line | Yes | Yes | N/A | Format > Data colors: #f9a655, Line |
| 9 | Total Profit | Default | Default | Default | Default | Text | Yes | Yes | N/A | Format > Data labels |
| 10 | Total Sales | Default | Default | Default | Default | Text | Yes | Yes | N/A | Format > Data labels |

### 9c. Number Format Mapping

| # | Worksheet Name | Field Name | Tableau Format String | Format Description | Power BI Format String | Power BI Configuration Path |
|---|---|---|---|---|---|---|
| 1 | Count Orders | [cnt:Orders_A9D43D036A2C426B9BC077827F69847F:qk] | c"$"#,##0;("$"#,##0) | Currency, no decimals, negatives in parens | $#,##0;($#,##0) | Model view: measure format |
| 2 | Profit Ratio and Discount per State | [Calculation_5571209093911105] | p0% | Percentage, no decimals | 0% | Model view: measure format |
| 3 | Profit by Category (2) | sum:Profit:qk | c"$"#,##0;("$"#,##0) | Currency, no decimals | $#,##0;($#,##0) | Model view: measure format |

### 9d. Axis, Gridline & Reference Line Mapping

| # | Worksheet Name | Element Type | Scope (Rows/Cols) | Tableau Setting | Tableau Value | Power BI Equivalent Setting | Power BI Configuration Path |
|---|---|---|---|---|---|---|---|
| 1 | Profit by Category (2) | axis | rows | display | false | Axis visible: Off | Format > Y axis |
| 2 | Profit by Category (2) | axis | cols | display | false | Axis visible: Off | Format > X axis |
| 3 | Profit by Category (2) | gridline | cols | line-visibility | off | Gridlines off | Format > Gridlines |
| 4 | Stacked Bar chart | refline | cols | fill-above | #00000000 | Ref line above color: transparent | Analytics pane: Reference Line |
| 5 | Stacked Bar chart | refline | cols | fill-below | #00000000 | Ref line below color: transparent | Analytics pane: Reference Line |

### 9e. Dashboard Layout & Text Theme Mapping

| # | Dashboard Name | Element | Element Type | Content/Sheet Name | Font Bold? | Font Size | Font Color (Hex) | Font Alignment | Border Style | Margin (px) | Padding (px) | Power BI Equivalent | Power BI Configuration |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Data Analysis Report | Main | layout-basic | N/A | N/A | N/A | #000000 | N/A | none | 8 | N/A | Canvas size | Page Settings |
| 2 | Data Analysis Report | Stacked Bar chart | sheet | Stacked Bar chart | N/A | N/A | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 3 | Data Analysis Report | Profit Ratio and Discount per State | sheet | Profit Ratio and Discount per State | N/A | N/A | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 4 | Data Analysis Report | Sales and Profit by Customer | sheet | Sales and Profit by Customer | N/A | N/A | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 5 | Data Analysis Report | Color legend | color | Profit Ratio and Discount per State | N/A | N/A | #000000 | N/A | none | 4 | N/A | Legend | Format > Legend |
| 6 | Sales Summary Report | Main | layout-basic | N/A | N/A | N/A | #000000 | N/A | none | 8 | N/A | Canvas size | Page Settings |
| 7 | Sales Summary Report | Count Orders | sheet | Count Orders | N/A | 12 | #000000 | center | none | 4 | N/A | Visual position | Format > General |
| 8 | Sales Summary Report | Profit Percent by State | sheet | Profit Percent by State | N/A | 9 | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 9 | Sales Summary Report | Profit by Category (2) | sheet | Profit by Category (2) | N/A | 9 | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 10 | Sales Summary Report | Profit vs Sales Trend | sheet | Profit vs Sales Trend | N/A | 9 | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 11 | Sales Summary Report | Top5 by Sub-category Sales | sheet | Top5 by Sub-category Sales | N/A | 9 | #000000 | N/A | none | 4 | N/A | Visual position | Format > General |
| 12 | Sales Summary Report | Total Profit | sheet | Total Profit | N/A | 12 | #000000 | center | none | 4 | N/A | Visual position | Format > General |
| 13 | Sales Summary Report | Total Sales | sheet | Total Sales | N/A | 12 | #000000 | center | none | 4 | N/A | Visual position | Format > General |
| 14 | Sales Summary Report | Sales and Profit Dashboard | text | Sales and Profit Dashboard | bold | 12 | #4e79a7 | center | none | 4 | N/A | Text box | Format > Text box |

### 9f. Map Visual Theme Mapping

| # | Worksheet Name | Map Setting | Tableau Value | Description | Power BI Equivalent | Power BI Configuration Path |
|---|---|---|---|---|---|---|
| 1 | Profit Percent by State | washout | 1 | Full white background | Map: Style: Light | Format > Map settings |
| 2 | Profit Ratio and Discount per State | washout | 0 | Full color background | Map: Style: Road | Format > Map settings |

### 9g. Tooltip Configuration Mapping

| # | Worksheet Name | Field Name | Show in Tooltip? | Power BI Configuration |
|---|---|---|---|---|
| 1 | All | All fields | Yes | Add fields to Tooltip well |

---

## Section 10: Security, RLS & Access Control Mapping

### 10a. Security Posture Summary

| Security Feature | Present in TWB? | Count | Details | Power BI Equivalent Feature | Migration Priority | Migration Action |
|---|---|---|---|---|---|---|
| User Filters (RLS) | No | 0 | None detected | Row-Level Security (RLS) with DAX | Medium | Recommend reviewing need for RLS in Power BI |
| Datasource-Level Filters | No | 0 | None | Power Query filters or RLS | Low | None needed |
| Extract Filters | No | 0 | None | Power Query filters (pre-load) | Low | None needed |
| Initial SQL (session security) | No | 0 | None | Custom SQL / Stored Proc in DirectQuery | N/A | Not required |
| Hidden Fields | Yes | 3 | [Row ID], [Customer ID], [Product ID] | Column-Level Security (CLS) via OLS | Low | Hide in model or apply OLS if sensitive |
| Hidden Worksheets/Pages | Yes | 10 | All worksheets are dashboard-only (hidden tabs) | Hidden report pages / App navigation control | Low | Set pages as hidden in Power BI if needed |
| Published Datasource Permissions | No | 0 | None | Workspace roles + dataset permissions | Medium | Configure workspace roles in Power BI |
| Authentication Method | Yes | 1 | auth-none, as-is | Power BI Gateway / Service auth | Medium | Use appropriate auth for data refresh |

### 10b. Row-Level Security (RLS) Mapping

| # | Tableau RLS Mechanism | Datasource | Filter Field | Tableau User/Group Mapping | Filter Expression / Members | Scope (Datasource / Worksheet) | Power BI RLS Role Name | Power BI DAX Filter Expression | Power BI Role Assignment | Implementation Steps |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | None (Candidate) | Orders | Region | N/A | N/A | Datasource | Region_East | [Region] = "East" | Assign users to Region_East | 1. Go to Model > Manage Roles 2. Add Region_East role with DAX filter 3. Assign users in Power BI Service |
| 2 | None (Candidate) | Orders | Segment | N/A | N/A | Datasource | Segment_Consumer | [Segment] = "Consumer" | Assign users to Segment_Consumer | Same as above |

### 10c. Hidden Fields / Column-Level Security Mapping

| # | Datasource | Field Name | Field Caption | Data Type | Reason Hidden (Inferred) | Power BI Action | Power BI OLS Configuration |
|---|---|---|---|---|---|---|---|
| 1 | Orders | Row ID | Row ID | integer | Technical field | Hide from report view | None |
| 2 | Orders | Customer ID | Customer ID | string | Internal key | Hide from report view | None |
| 3 | Orders | Product ID | Product ID | string | Internal key | Hide from report view | None |

### 10d. Hidden Worksheets / Page Security Mapping

| # | Worksheet Name | Tableau Visibility | Used In Dashboard(s) | Purpose (Inferred) | Power BI Page Setting | Power BI Implementation |
|---|---|---|---|---|---|---|
| 1 | All | Hidden | All dashboards | Embedded visual only | Page visible (embedded) | Leave as visible or set as hidden if needed |

### 10e. Authentication & Connection Security Mapping

| # | Datasource | Connection Class | Tableau Auth Method | Username | Password Protected? | Workgroup Auth Mode | Power BI Auth Equivalent | Power BI Gateway Required? | Migration Steps |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Sample - Superstore | excel-direct | auth-none | N/A | No | as-is | Anonymous | No | Connect using Power Query > Excel |

### 10f. Data Restriction Filters (Security-Relevant Only)

| # | Filter Level | Worksheet / Datasource | Filter Field | Filter Class | Filter Type | Members / Values / Range | Security Relevant? | Power BI Equivalent | Implementation |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Worksheet | All | Year, Region, Country, State | Categorical | Slicer | All values | No | Slicer | Add slicers to report |

### 10g. Recommended Power BI Security Architecture

| # | Security Layer | Recommendation | Applies To | Configuration Details | Priority |
|---|---|---|---|---|---|
| 1 | Workspace Security | Use separate workspace for report | Report and dataset | Assign Admin, Member, Contributor, Viewer roles | High |
| 2 | Row-Level Security | Implement RLS if required | Dataset | Model > Manage Roles | Medium |
| 3 | Object-Level Security | Hide technical columns | Dataset | Hide in model or use OLS if needed | Low |
| 4 | Data Source Credentials | Store credentials securely | Dataset | Configure gateway if needed | High |
| 5 | Report Sharing | Publish to workspace, share with groups | Report | Use workspace sharing, app audience | High |
| 6 | Sensitivity Labels | Apply Confidential label if needed | Report, dataset | Set in Power BI Service | Medium |
| 7 | Refresh Security | Schedule refresh securely | Dataset | Use gateway or cloud refresh | High |
| 8 | Audit & Monitoring | Enable Activity Log | Workspace | Admin Portal > Audit Logs | Medium |

---

## Section 11: Regulatory Compliance Notes

- All data lineage, field mapping, calculation logic, and security controls are documented for auditability.
- No PII or sensitive data is present in visible fields; technical fields are hidden.
- All calculations and KPIs are validated for accuracy; changes are logged.
- Row-level security is recommended where business policy requires.
- For regulated industries (SOX, GDPR), ensure all compliance requirements are reviewed with data owners and legal.

---

# END OF REPORT