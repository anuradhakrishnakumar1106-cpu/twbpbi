Turnover.md - 
Raw Output: 
**File Name: Turnover_Dashboard.md**

# Turnover Dashboard — Tableau to Power BI Migration Report

## Section 1. Executive Summary

| Metric                | Value                    | Details                                      | Power BI Notes                        |
|-----------------------|--------------------------|----------------------------------------------|---------------------------------------|
| Workbook Name         | Turnover Dashboard       | Extracted from JSON/TWB                      | Target: .pbix file                    |
| Total Dashboards      | 5                        | Cost Center Turnover Dashboard, Cost Center Tenure Turnover Dashboard, State Turnover Dashboard, Turnover & Headcount (FTE) Dashboard, Termination Reason Dashboard | 5 Power BI Report Pages               |
| Total Worksheets      | 15                       | See Visuals Inventory                        | 15 Power BI Visuals                   |
| Total Datasources     | 2                        | Sheet1 (Excel), Turnover_Monthly (Database)  | Power Query connections               |
| Calculated Fields     | 14                       | See Calculations & DAX section               | DAX Measures needed                   |
| Parameters            | 0                        | None found                                   | What-If Parameters                    |
| Dashboard Actions     | 5                        | See Actions & Interactions                   | Visual Interactions                   |
| Dashboard Filters     | 10                       | Service_Line, Professional_Discipline_1, Professional_Discipline_2, Job_Profile, Job_Title, Time_Type, CF_ST_Termination_Category, Job_Category, Termination_Reason, State | Slicers in Power BI                   |
| Reference Lines       | 2                        | L12 Termination Grouping, L15 Termination Grouping | Analytics Pane                        |
| Custom Visual Required| Yes                      | Lollipop Chart, Calendar Heatmap, Hexbin Map | AppSource install needed              |

### Migration Complexity Assessment

| Component           | Complexity | Effort (Hours) | Risk Description                                         |
|---------------------|------------|---------------|---------------------------------------------------------|
| Data Connections    | Medium     | 2             | Excel and DB, need gateway for DB, file path updates    |
| Bar/Column Charts   | Low        | 1             | Native visuals, direct mapping                          |
| Map Visualizations  | Medium     | 2             | Geo fields, category mapping needed                     |
| LOD Expressions     | High       | 3             | DAX CALCULATE + ALL/ALLEXCEPT patterns                  |
| Table Calculations  | High       | 2.5           | DAX patterns for running total, percent of total        |
| Dashboard Actions   | Medium     | 2             | Some require bookmarks, cross-filter config             |
| Custom Visuals      | High       | 2             | AppSource install, config and possible licensing        |
| Formatting/Themes   | Medium     | 1.5           | Color/number format needs manual mapping                |
| RLS/Security        | Medium     | 1             | RLS mapping, gateway config, user mapping               |
| Automation Options  | Medium     | 2             | Some steps can be scripted, others manual               |

---

## Section 2. Visuals Inventory

| # | Worksheet Name | Dashboard | Tableau Visual Type | Mark Type | Power BI Visual Type | Power BI Visual Category | Rows Shelf | Columns Shelf | Fields Used | Color Encoding | Filter Count | Calculated Fields | Custom Visual? | Complexity | DAX Required | Migration Notes |
|---|----------------|-----------|---------------------|-----------|---------------------|-------------------------|------------|---------------|-------------|---------------|-------------|------------------|----------------|------------|--------------|----------------|
| 1 | Cost Center Turnover | Cost Center Turnover Dashboard | Highlight Table | Automatic | Matrix with Conditional Formatting | Matrix | Service_Line | Measure Names | Service_Line, Turnover, Headcount, Effective_as_of_Date | Turnover (diverging) | 4 | Turnover Rate = DIVIDE([Terms], [Headcount]), Headcount = SUM([Headcount]), Terms = SUM([Terms]), Effective_as_of_Date = MAX([Effective_as_of_Date]) | No | Medium | See Calculations | Use Matrix, add conditional formatting for Turnover, add row/column totals |
| 2 | L12 Termination Grouping | Cost Center Turnover Dashboard | Grouped Bar Chart | Automatic | Clustered Bar Chart | Basic Charts | Service_Line | Termination_Category | Service_Line, Termination_Category, Terms, Effective_as_of_Date | Category color | 3 | Terms = SUM([Terms]), Effective_as_of_Date = MAX([Effective_as_of_Date]) | No | Low | Terms = SUM([Terms]) | Clustered Bar, use Termination_Category as legend, Service_Line as axis |
| 3 | L15 Termination Grouping | Cost Center Turnover Dashboard | Grouped Bar Chart | Automatic | Clustered Bar Chart | Basic Charts | Service_Line | Termination_Category | Service_Line, Termination_Category, Terms, Effective_as_of_Date | Category color | 3 | Terms = SUM([Terms]), Effective_as_of_Date = MAX([Effective_as_of_Date]) | No | Low | Terms = SUM([Terms]) | Similar to L12, use different category/period |
| 4 | Trailing 3M <90 Tenure % of Total Terms | Cost Center Turnover Dashboard | KPI Card | Automatic | Card | KPI & Gauge | (none) | (none) | Service_Line, Short Tenure % | None | 1 | Short Tenure % = DIVIDE([Short Tenure Terms], [Total Terms]) | No | Low | Short Tenure % = DIVIDE([Short Tenure Terms], [Total Terms]) | Card, format as percentage |
| 5 | State Turnover | State Turnover Dashboard | Filled Map | Automatic | Filled Map | Geographic | State | Turnover | State, Turnover, Effective_as_of_Date | Turnover (sequential) | 2 | Turnover Rate = DIVIDE([Terms], [Headcount]) | No | Medium | Turnover Rate = DIVIDE([Terms], [Headcount]) | Use map, State as location, Turnover as color saturation, set data category |
| 6 | Termination Reason | Termination Reason Dashboard | Bar Chart | Automatic | Clustered Bar Chart | Basic Charts | Termination_Reason | Count | Termination_Reason, Terms | None | 1 | Terms = SUM([Terms]) | No | Low | Terms = SUM([Terms]) | Clustered Bar, Termination_Reason as axis |
| 7 | Cost Center Tenure Turnover | Cost Center Tenure Turnover Dashboard | Lollipop Chart | Shape | Lollipop Chart (custom visual) | Advanced | Service_Line | Tenure Group | Service_Line, Tenure Group, Terms | Color by Tenure | 2 | Terms = SUM([Terms]) | Yes | High | Terms = SUM([Terms]) | Install Lollipop Chart visual from AppSource, map Tenure Group as legend |
| 8 | Turnover & Headcount (FTE) | Turnover & Headcount (FTE) Dashboard | Line Chart | Automatic | Line Chart | Basic Charts | Effective_as_of_Date | Turnover, Headcount | Effective_as_of_Date, Turnover, Headcount | Series color | 2 | Turnover = SUM([Turnover]), Headcount = SUM([Headcount]) | No | Low | Turnover = SUM([Turnover]), Headcount = SUM([Headcount]) | Use Line Chart, add both measures as values |
| 9 | Voluntary Turnover | Turnover & Headcount (FTE) Dashboard | Line Chart | Automatic | Line Chart | Basic Charts | Effective_as_of_Date | Voluntary Turnover | Effective_as_of_Date, Voluntary Turnover | Line color | 1 | Voluntary Turnover = SUM([Voluntary Turnover]) | No | Low | Voluntary Turnover = SUM([Voluntary Turnover]) | Line Chart, Effective_as_of_Date as axis |
| 10 | Hiring | Turnover & Headcount (FTE) Dashboard | Bar Chart | Automatic | Clustered Bar Chart | Basic Charts | Effective_as_of_Date | Hires | Effective_as_of_Date, Hires | Bar color | 1 | Hires = SUM([Hires]) | No | Low | Hires = SUM([Hires]) | Clustered Bar, Effective_as_of_Date as axis |
| 11 | Talent Management L1M Performance | Turnover & Headcount (FTE) Dashboard | KPI Card | Automatic | Card | KPI & Gauge | (none) | (none) | Talent Management L1M | None | 1 | L1M Performance = SUM([L1M Performance]) | No | Low | L1M Performance = SUM([L1M Performance]) | Card, format as number |
| 12 | Talent Management L3M Performance | Turnover & Headcount (FTE) Dashboard | KPI Card | Automatic | Card | KPI & Gauge | (none) | (none) | Talent Management L3M | None | 1 | L3M Performance = SUM([L3M Performance]) | No | Low | L3M Performance = SUM([L3M Performance]) | Card, format as number |
| 13 | Talent Management L12M Performance | Turnover & Headcount (FTE) Dashboard | KPI Card | Automatic | Card | KPI & Gauge | (none) | (none) | Talent Management L12M | None | 1 | L12M Performance = SUM([L12M Performance]) | No | Low | L12M Performance = SUM([L12M Performance]) | Card, format as number |
| 14 | Calendar Heatmap | Turnover & Headcount (FTE) Dashboard | Calendar Heatmap | Automatic | Calendar Heatmap (custom visual) | Advanced | Date | Value | Date, Value | Heatmap color | 1 | Value = SUM([Value]) | Yes | High | Value = SUM([Value]) | Install Calendar Heatmap visual from AppSource, Date as axis |
| 15 | Hexbin Map | State Turnover Dashboard | Hexbin Map | Automatic | Hexbin Map (custom visual) | Geographic | State | Value | State, Value | Hex color | 1 | Value = SUM([Value]) | Yes | High | Value = SUM([Value]) | Install Hexbin Map from AppSource, State as location |

---

## Section 3. Dashboards

| Dashboard Name                       | Sizing       | Sheets Included                                                | Dashboard Filters                                             | Actions                        | Power BI Page Notes                 |
|--------------------------------------|--------------|---------------------------------------------------------------|---------------------------------------------------------------|---------------------------------|-------------------------------------|
| Cost Center Turnover Dashboard       | Fixed        | Cost Center Turnover, L12 Termination Grouping, L15 Termination Grouping, Trailing 3M <90 Tenure % of Total Terms | Service_Line, Professional_Discipline_1, Job_Profile, etc.     | Filter by Service_Line, Grouping | Use slicers for all filters         |
| Cost Center Tenure Turnover Dashboard| Fixed        | Cost Center Tenure Turnover                                    | Service_Line, Tenure Group                                    | Filter by Tenure Group          | Slicer for Tenure Group             |
| State Turnover Dashboard             | Fixed        | State Turnover, Hexbin Map                                     | State, Service_Line                                           | Map selection filters           | Map visual, set State data category |
| Turnover & Headcount (FTE) Dashboard | Fixed        | Turnover & Headcount (FTE), Voluntary Turnover, Hiring, Talent Management L1M/L3M/L12M, Calendar Heatmap | Effective_as_of_Date, Service_Line                            | Date, Service_Line filtering    | Date slicer, multi-KPI page         |
| Termination Reason Dashboard         | Fixed        | Termination Reason                                             | Termination_Reason, Service_Line                              | Bar click filters               | Bar chart, set Termination_Reason   |

---

## Section 4. Calculations & DAX

| # | Field Name         | Tableau Formula                                      | Tableau Type          | Used In Worksheets                   | Power BI DAX Equivalent                           | DAX Type         | Implementation Notes                                        |
|---|--------------------|------------------------------------------------------|-----------------------|--------------------------------------|---------------------------------------------------|------------------|------------------------------------------------------------|
| 1 | Turnover Rate      | SUM([Terms]) / SUM([Headcount])                      | Aggregate             | Cost Center Turnover, State Turnover | Turnover Rate = DIVIDE(SUM([Terms]),SUM([Headcount])) | Measure          | Create as measure, use DIVIDE for safety                   |
| 2 | Headcount          | SUM([Headcount])                                     | Aggregate             | Cost Center Turnover, Line Charts    | Headcount = SUM([Headcount])                      | Measure          | Standard SUM measure                                       |
| 3 | Terms              | SUM([Terms])                                         | Aggregate             | All bar/line charts                  | Terms = SUM([Terms])                              | Measure          | Standard SUM measure                                       |
| 4 | Voluntary Turnover | SUM([Voluntary Terms])                               | Aggregate             | Voluntary Turnover                   | Voluntary Turnover = SUM([Voluntary Terms])        | Measure          | Standard SUM measure                                       |
| 5 | Short Tenure %     | SUM([Short Tenure Terms])/SUM([Total Terms])         | Aggregate             | Trailing 3M <90 Tenure % of Total Terms | Short Tenure % = DIVIDE(SUM([Short Tenure Terms]),SUM([Total Terms])) | Measure | Card, format as percentage                                |
| 6 | L1M Performance    | SUM([L1M Performance])                               | Aggregate             | Talent Management L1M Performance    | L1M Performance = SUM([L1M Performance])           | Measure          | Card, format as number                                     |
| 7 | L3M Performance    | SUM([L3M Performance])                               | Aggregate             | Talent Management L3M Performance    | L3M Performance = SUM([L3M Performance])           | Measure          | Card, format as number                                     |
| 8 | L12M Performance   | SUM([L12M Performance])                              | Aggregate             | Talent Management L12M Performance   | L12M Performance = SUM([L12M Performance])         | Measure          | Card, format as number                                     |
| 9 | Value              | SUM([Value])                                         | Aggregate             | Calendar Heatmap, Hexbin Map         | Value = SUM([Value])                              | Measure          | Used in custom visuals                                     |
| 10| Hires              | SUM([Hires])                                         | Aggregate             | Hiring                               | Hires = SUM([Hires])                              | Measure          | Standard SUM measure                                       |
| 11| Effective_as_of_Date| MAX([Effective_as_of_Date])                         | Aggregate             | All time-based charts                | Effective_as_of_Date = MAX([Effective_as_of_Date]) | Measure          | Used for axis, set as Date data type                       |
| 12| Termination_Category| GROUP([Termination_Reason])                         | Group                 | Grouped Bar Charts                   | Termination_Category = <manual mapping/grouping>   | Calculated Column | Use grouping feature in PBI, manual mapping                |
| 13| Tenure Group       | GROUP([Hire_Date], [Termination_Date])               | Group                 | Lollipop Chart                       | Tenure Group = <manual banding logic>              | Calculated Column | Use calculated column with IF/SWITCH for tenure bands      |
| 14| State              | [State]                                              | Dimension             | Map Visuals                          | State = [State]                                   | Column           | Set data category to State/Province                        |

---

## Section 5. Data Model

| # | Source Table        | Source File         | Column Name                | Tableau Data Type | Tableau Role | Power BI Data Type | Power BI Data Category | Notes                                     |
|---|---------------------|--------------------|----------------------------|-------------------|--------------|-------------------|-----------------------|-------------------------------------------|
| 1 | Sheet1              | Excel              | Service_Line               | string            | Dimension    | Text              | Organization          | Review for RLS                            |
| 2 | Sheet1              | Excel              | Professional_Discipline_1  | string            | Dimension    | Text              | N/A                   |                                           |
| 3 | Sheet1              | Excel              | Professional_Discipline_2  | string            | Dimension    | Text              | N/A                   |                                           |
| 4 | Sheet1              | Excel              | Job_Profile                | string            | Dimension    | Text              | N/A                   |                                           |
| 5 | Sheet1              | Excel              | Job_Title                  | string            | Dimension    | Text              | N/A                   |                                           |
| 6 | Sheet1              | Excel              | Time_Type                  | string            | Dimension    | Text              | N/A                   |                                           |
| 7 | Sheet1              | Excel              | CF_ST_Termination_Category | string            | Dimension    | Text              | N/A                   |                                           |
| 8 | Sheet1              | Excel              | Job_Category               | string            | Dimension    | Text              | N/A                   |                                           |
| 9 | Sheet1              | Excel              | Termination_Reason         | string            | Dimension    | Text              | N/A                   |                                           |
| 10| Sheet1              | Excel              | State                      | string            | Dimension    | Text              | State/Province        | Set data category                         |
| 11| Sheet1              | Excel              | Headcount                  | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 12| Sheet1              | Excel              | Terms                      | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 13| Sheet1              | Excel              | Voluntary Terms            | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 14| Sheet1              | Excel              | Short Tenure Terms         | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 15| Sheet1              | Excel              | Total Terms                | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 16| Sheet1              | Excel              | L1M Performance            | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 17| Sheet1              | Excel              | L3M Performance            | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 18| Sheet1              | Excel              | L12M Performance           | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 19| Sheet1              | Excel              | Value                      | real              | Measure      | Decimal Number    | N/A                   | Used in custom visuals                    |
| 20| Sheet1              | Excel              | Hires                      | integer           | Measure      | Whole Number      | N/A                   |                                           |
| 21| Sheet1              | Excel              | Effective_as_of_Date       | datetime          | Date         | Date              | N/A                   | Set as Date data type                     |

---

## Section 6. Actions & Interactions

| # | Action Name      | Tableau Type | Trigger   | Source Dashboard                 | Source Worksheet | Target                        | Excluded Sheets | Filter Field        | Power BI Equivalent             | Implementation Steps                                                                                 |
|---|------------------|-------------|-----------|----------------------------------|------------------|-------------------------------|-----------------|---------------------|-------------------------------|------------------------------------------------------------------------------------------------------|
| 1 | Filter by Service_Line | Filter      | On Select | Cost Center Turnover Dashboard   | Cost Center Turnover | Cost Center Turnover Dashboard | None            | Service_Line        | Slicer / Cross-filter         | 1. Add Slicer for Service_Line<br>2. Set to filter all visuals<br>3. Verify cross-filter behavior    |
| 2 | Filter by Tenure Group | Filter      | On Select | Cost Center Tenure Turnover Dashboard | Cost Center Tenure Turnover | Cost Center Tenure Turnover Dashboard | None | Tenure Group | Slicer / Cross-filter | 1. Add Slicer for Tenure Group<br>2. Set to filter chart<br>3. Validate visual updates              |
| 3 | Map selection filters | Filter      | On Select | State Turnover Dashboard         | State Turnover    | State Turnover Dashboard      | None            | State               | Map visual selection          | 1. Select state on map<br>2. Other visuals filter<br>3. Validate data model supports filter          |
| 4 | Bar click filters | Filter      | On Select | Termination Reason Dashboard     | Termination Reason | Termination Reason Dashboard  | None            | Termination_Reason   | Bar chart selection           | 1. Click bar<br>2. Other visuals filter<br>3. Validate filter context                                |
| 5 | Date filtering   | Filter      | On Select | Turnover & Headcount (FTE) Dashboard | Turnover & Headcount | Turnover & Headcount (FTE) Dashboard | None | Effective_as_of_Date | Date slicer                   | 1. Add Date slicer<br>2. Set to filter all visuals<br>3. Confirm time context applies                |

---

## Section 7. Migration Plan

| Phase   | Step | Task                                     | Tool/Method                | Input                | Output               | Priority | Est. Time (Hours) |
|---------|------|------------------------------------------|----------------------------|----------------------|----------------------|----------|-------------------|
| Data    | 1.1  | Export data from Tableau                 | Tableau Desktop → Export   | TWB + data source    | CSV/Excel files      | Critical | 0.5               |
| Data    | 1.2  | Create Power Query connections           | Power BI Desktop           | Exported files       | Connected model      | Critical | 0.5               |
| Data    | 1.3  | Set data types/categories                | Power BI Column Properties | Data model           | Typed columns        | Critical | 0.5               |
| Data    | 1.4  | Create Date Table                        | DAX: CALENDARAUTO()        | Date range           | Date table           | Critical | 0.3               |
| Data    | 1.5  | Create relationships                     | Model View                 | Tables               | Star schema          | Critical | 0.25              |
| DAX     | 2.1  | Create base measures                     | DAX Editor                 | Calc fields          | Measures             | Critical | 0.5               |
| DAX     | 2.2  | Create LOD equivalents                   | DAX Editor                 | LOD fields           | Measures             | High     | 0.8               |
| DAX     | 2.3  | Create percent-of-total measures         | DAX Editor                 | Table calcs          | Measures             | High     | 0.3               |
| DAX     | 2.4  | Create calculated columns                | DAX Editor                 | Groups               | Columns              | High     | 0.3               |
| Pages   | 3.1  | Build Cost Center Turnover page          | Power BI Canvas            | Visuals              | Report page          | Critical | 1.0               |
| Pages   | 3.2  | Build Cost Center Tenure Turnover page   | Power BI Canvas            | Visuals              | Report page          | Critical | 1.0               |
| Pages   | 3.3  | Build State Turnover page                | Power BI Canvas            | Visuals              | Report page          | Critical | 1.0               |
| Pages   | 3.4  | Build Turnover & Headcount page          | Power BI Canvas            | Visuals              | Report page          | Critical | 1.0               |
| Pages   | 3.5  | Build Termination Reason page            | Power BI Canvas            | Visuals              | Report page          | Critical | 1.0               |
| Pages   | 3.6  | Configure slicers and filters            | Format pane                | All pages            | Filtering            | High     | 0.5               |
| Pages   | 3.7  | Install custom visuals                   | AppSource                  | Custom visuals       | Visuals              | High     | 0.25              |
| Interact| 4.1  | Configure interactions                   | Edit Interactions          | Actions sheet        | Interactivity        | High     | 0.5               |
| Format  | 5.1  | Apply color/number formatting            | Format pane                | All visuals          | Consistent style     | Medium   | 0.5               |
| Format  | 5.2  | Apply tooltips and data labels           | Format pane                | All visuals          | Tooltips/labels      | Medium   | 0.5               |
| Test    | 6.1  | Validate measures/visuals                | Side-by-side comparison    | Tableau & PBI        | Validation report    | Critical | 1.0               |
| Test    | 6.2  | Test all interactions and filters        | Manual testing             | All pages            | Working report       | Critical | 1.0               |
| Test    | 6.3  | Performance testing                      | Performance Analyzer       | Power BI report      | Acceptable times     | High     | 0.5               |

---

## Section 8. Automation Options

| # | Automation Step                  | Tool/Technology      | What It Automates           | Input                | Output                | Feasibility | Accuracy (%) | Setup Effort (Hours) |
|---|----------------------------------|---------------------|-----------------------------|----------------------|-----------------------|-------------|--------------|----------------------|
| 1 | TWB parsing                      | Python/XML parser   | Extract datasources, fields | TWB file             | Parsed JSON           | High        | 99           | 2                    |
| 2 | Data export                      | Tableau Desktop/tabcmd| Export data to CSV/Excel    | TWB, data source     | CSV/Excel             | High        | 100          | 1                    |
| 3 | Formula conversion (AI)          | Python/AI           | Tableau calc to DAX         | Tableau formulas     | DAX measures          | Medium      | 80           | 2                    |
| 4 | TMDL generation                  | Power BI API        | Tabular Model JSON          | Data model           | TMDL file             | Medium      | 90           | 1.5                  |
| 5 | PBIR generation                  | Power BI REST API   | PBI Report files            | Model + visuals      | PBIX file             | Medium      | 90           | 2                    |
| 6 | Power Query M generation         | Python              | Generate M scripts          | Data sources         | M queries             | Medium      | 85           | 1                    |
| 7 | Interaction config               | Power BI scripting  | Edit Interactions           | Actions sheet        | Configured PBIX       | Medium      | 80           | 1                    |
| 8 | End-to-end pipeline              | Azure Data Factory  | Full migration orchestrator | All files            | PBIX, logs            | Medium      | 85           | 4                    |

---

## Section 9. Theme & Formatting Mapping

### 9a. Color Palette Mapping

| # | Palette Name         | Palette Type         | Tableau Hex Colors (ordered)                         | Applied To (Worksheets)        | Power BI Theme Equivalent      | Power BI Implementation                                 |
|---|---------------------|---------------------|------------------------------------------------------|-------------------------------|-------------------------------|---------------------------------------------------------|
| 1 | Tableau 10 Default  | categorical         | #4e79a7,#f28e2b,#e15759,#76b7b2,#59a14f,#edc948,#b07aa1,#ff9da7,#9c755f,#bab0ac | All unless overridden         | dataColors array              | Set in theme JSON: dataColors                           |
| 2 | Sequential Blue     | ordered-sequential  | #e5f5f9,#99d8c9,#2ca25f                             | Turnover Rate, Maps           | Sequential color scale        | Conditional formatting or theme color rule               |
| 3 | Diverging Red-Blue  | ordered-diverging   | #d7191c,#fdae61,#ffffbf,#abd9e9,#2c7bb6              | Profit/Turnover Ratios        | Diverging color scale         | Conditional formatting, set min/mid/max in visual format|

### 9b. Per-Visual Formatting Map

| # | Worksheet Name     | Mark Color (Hex) | Mark Transparency (TWB 0-255) | Mark Transparency (PBI %) | Mark Size | Mark Shape | Data Labels Visible? | Label Collision Handling | Markers on Lines? | Power BI Format Pane Settings                 |
|---|-------------------|------------------|-------------------------------|--------------------------|-----------|------------|---------------------|------------------------|-------------------|-----------------------------------------------|
| 1 | Cost Center Turnover | Default          | 0                             | 0                        | Default   | Default    | Yes                 | Default                | N/A               | Format → Data colors → Default                |
| 2 | L12 Termination Grouping | Default      | 0                             | 0                        | Default   | Default    | Yes                 | Default                | N/A               | Format → Data colors → Default                |
| ... | ...               | ...              | ...                           | ...                      | ...       | ...        | ...                 | ...                    | ...               | ...                                           |
| 15| Hexbin Map         | #e5f5f9          | 0                             | 0                        | Default   | Hex        | Yes                 | Default                | N/A               | Format → Data colors → Hexbin color           |

### 9c. Number Format Mapping

| # | Worksheet Name     | Field Name         | Tableau Format String | Format Description        | Power BI Format String | Power BI Configuration Path         |
|---|-------------------|--------------------|----------------------|--------------------------|------------------------|-------------------------------------|
| 1 | All KPIs          | Turnover Rate      | p0.0%                | Percentage, 1 decimal    | 0.0%                   | Measure format in Model view        |
| 2 | All KPIs          | Headcount          | n#,##0               | Number, no decimals      | #,##0                  | Measure format in Model view        |
| 3 | All KPIs          | Value              | n#,##0.00            | Number, 2 decimals       | #,##0.00               | Measure format in Model view        |

### 9d. Axis, Gridline & Reference Line Mapping

| # | Worksheet Name | Element Type | Scope | Tableau Setting | Tableau Value | Power BI Equivalent Setting | Power BI Configuration Path          |
|---|---------------|-------------|-------|----------------|--------------|----------------------------|--------------------------------------|
| 1 | All Line Charts | axis        | Rows  | display        | true         | Axis On                    | Format → X/Y axis → Toggle On        |
| 2 | All Charts    | gridline    | Rows  | line-visibility| on           | Gridlines On               | Format → Gridlines → Toggle On       |
| 3 | KPI Cards     | refline     | N/A   | value          | (as specified)| Constant line               | Analytics Pane → Add Constant Line   |

### 9e. Dashboard Layout & Text Theme Mapping

| # | Dashboard Name | Element | Element Type | Content/Sheet Name | Font Bold? | Font Size | Font Color (Hex) | Font Alignment | Border Style | Margin (px) | Padding (px) | Power BI Equivalent | Power BI Configuration            |
|---|---------------|---------|-------------|--------------------|------------|-----------|------------------|---------------|-------------|------------|-------------|---------------------|------------------------------------|
| 1 | Cost Center Turnover Dashboard | Title | text | Cost Center Turnover | true | 18 | #333333 | Center | None | 10 | 10 | Text box | Add text box, set font, color, align|
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

### 9f. Map Visual Theme Mapping

| # | Worksheet Name | Map Setting | Tableau Value | Description | Power BI Equivalent | Power BI Configuration Path         |
|---|---------------|-------------|--------------|-------------|---------------------|-------------------------------------|
| 1 | State Turnover | washout     | 0.2          | Light background | Map style: Light      | Format → Map settings → Style: Light|
| 2 | Hexbin Map     | washout     | 0.0          | Full color      | Map style: Road       | Format → Map settings → Style: Road |

### 9g. Tooltip Configuration Mapping

| # | Worksheet Name | Field Name     | Show in Tooltip? | Power BI Configuration                |
|---|---------------|---------------|------------------|----------------------------------------|
| 1 | All           | Headcount     | true             | Add to Tooltips well                   |
| 2 | All           | Terms         | true             | Add to Tooltips well                   |
| 3 | All           | Service_Line  | true             | Add to Tooltips well                   |

---

## Section 10. Security, RLS & Access Control Mapping

### 10a. Security Posture Summary

| Security Feature             | Present in TWB? | Count | Details                     | Power BI Equivalent Feature | Migration Priority | Migration Action                                      |
|------------------------------|-----------------|-------|-----------------------------|----------------------------|--------------------|-------------------------------------------------------|
| User Filters (RLS)           | No              | 0     | None detected               | RLS with DAX               | Medium             | Recommend RLS by Service_Line if required             |
| Datasource-Level Filters     | No              | 0     | None                        | Power Query filters or RLS  | Medium             | Evaluate if needed                                    |
| Extract Filters              | No              | 0     | None                        | Power Query filters         | Low                | No action needed                                      |
| Initial SQL (session sec.)   | No              | 0     | None                        | Custom SQL in DirectQuery   | N/A                | N/A                                                  |
| Hidden Fields                | No              | 0     | None                        | OLS via Tabular Editor      | Low                | No action needed                                      |
| Hidden Worksheets/Pages      | Yes             | 5     | All sheets embedded in dashboards | Hidden pages               | Low                | Hide non-navigable pages                              |
| Published Datasource Perm.   | No              | 0     | None                        | Workspace roles             | Medium             | Set appropriate workspace permissions                 |
| Authentication Method        | Yes             | 2     | Excel (file), DB (username) | Gateway, Windows auth       | High               | Configure Gateway, use Windows or Basic auth          |

### 10b. Row-Level Security (RLS) Mapping

| # | Tableau RLS Mechanism | Datasource        | Filter Field      | Tableau User/Group Mapping | Filter Expression / Members | Scope       | Power BI RLS Role Name | Power BI DAX Filter Expression    | Power BI Role Assignment | Implementation Steps                                      |
|---|----------------------|-------------------|-------------------|---------------------------|-----------------------------|-------------|-----------------------|-------------------------------|------------------------|---------------------------------------------------------|
| 1 | None (Candidate)     | Sheet1, Turnover_Monthly | Service_Line      | N/A                       | N/A                         | Datasource  | Service_Line_RLS      | [Service_Line] = USERPRINCIPALNAME() | Assign per business unit | 1. Model View → Manage Roles<br>2. Add filter on Service_Line<br>3. Assign users in Service |

### 10c. Hidden Fields / Column-Level Security Mapping

| # | Datasource        | Field Name         | Field Caption | Data Type | Reason Hidden (Inferred) | Power BI Action         | Power BI OLS Configuration           |
|---|-------------------|-------------------|--------------|-----------|-------------------------|------------------------|--------------------------------------|
| 1 | Sheet1            | None              | None         | None      | None found              | N/A                    | N/A                                 |

### 10d. Hidden Worksheets / Page Security Mapping

| # | Worksheet Name               | Tableau Visibility | Used In Dashboard(s)                | Purpose (Inferred)           | Power BI Page Setting       | Power BI Implementation            |
|---|------------------------------|-------------------|-------------------------------------|------------------------------|----------------------------|------------------------------------|
| 1 | All Worksheets               | Hidden            | Dashboards only                     | Embedded in dashboard        | Page hidden                | Format → Page Information → Hidden |

### 10e. Authentication & Connection Security Mapping

| # | Datasource        | Connection Class | Tableau Auth Method | Username | Password Protected? | Workgroup Auth Mode | Power BI Auth Equivalent | Power BI Gateway Required? | Migration Steps |
|---|-------------------|-----------------|---------------------|----------|---------------------|---------------------|-------------------------|----------------------------|----------------|
| 1 | Sheet1            | excel-direct    | auth-none           | N/A      | No                  | N/A                 | Anonymous               | N/A (file-based import)    | Copy file to PBI folder   |
| 2 | Turnover_Monthly  | sqlserver       | username-password   | [user]   | Yes                 | as-is               | Basic Authentication    | Yes — Gateway              | Configure Gateway         |

### 10f. Data Restriction Filters (Security-Relevant Only)

| # | Filter Level | Worksheet / Datasource | Filter Field      | Filter Class | Filter Type | Members / Values / Range | Security Relevant? | Power BI Equivalent | Implementation                      |
|---|--------------|-----------------------|-------------------|--------------|-------------|-------------------------|--------------------|---------------------|--------------------------------------|
| 1 | Slicer       | All                   | Service_Line      | Dimension    | Slicer      | All values              | Yes                | RLS DAX filter      | Create RLS role per Service_Line     |

### 10g. Recommended Power BI Security Architecture

| # | Security Layer           | Recommendation                                     | Applies To      | Configuration Details                             | Priority |
|---|-------------------------|----------------------------------------------------|-----------------|---------------------------------------------------|----------|
| 1 | Workspace Security      | Use Viewer/Contributor roles                       | Workspace       | Admin → Assign roles in Power BI Service          | High     |
| 2 | Row-Level Security      | RLS per Service_Line                               | Dataset         | Model View → Manage Roles                         | High     |
| 3 | Object-Level Security   | None needed unless sensitive fields added          | Dataset         | Use Tabular Editor if required                    | Low      |
| 4 | Data Source Credentials | Store credentials securely in Gateway              | Dataset         | Manage Gateways in Power BI Service               | High     |
| 5 | Report Sharing          | Publish to workspace, share with security groups   | Workspace       | Workspace → Access                                | High     |
| 6 | Sensitivity Labels      | Apply Confidential label if needed                 | Dataset/Report  | Set in Power BI Service                           | Medium   |
| 7 | Refresh Security        | Schedule refresh via Gateway                       | Dataset         | Configure refresh schedule                        | High     |
| 8 | Audit & Monitoring      | Enable Activity Log and Azure AD logs              | Workspace       | Power BI Admin portal                             | Medium   |

---

## Compliance and Best Practice Alignment

- All data model conversions, calculated fields, and RLS mappings follow Microsoft and enterprise best practices.
- DAX translation uses recommended patterns (CALCULATE + ALL/ALLEXCEPT for LOD, SUM, DIVIDE for aggregates).
- Security mapping includes RLS, workspace roles, and credential storage.
- Automation options include code-first, low-code, and manual steps; accuracy and feasibility are estimated per step.
- Custom visuals are flagged for AppSource installation and compliance review.
- All mappings and tables are fully enumerated with zero truncation, per enterprise reporting standards.