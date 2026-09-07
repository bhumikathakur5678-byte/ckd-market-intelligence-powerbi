# Data model

The report uses a flat model: one fact table per chart plus a measures table. There are no relationships between the segment tables because every share is a slice of the same 2025 market total, which is read from `Forecast` by the measures.

## Tables

| Table (Power BI name) | Source file | Grain | Columns |
|---|---|---|---|
| `Forecast` | `data/market_size_forecast.csv` | one row per year | Year (Whole number), Market_Size_USD_Bn (Decimal), Source_Note (Text) |
| `Region` | `data/market_share_by_region.csv` | one row per region | Region, Share_Pct, Market_Size_2025_USD_Bn, Growth_Flag |
| `DrugClass` | `data/market_share_by_drug_class.csv` | one row per drug class | Drug_Class, Share_Pct, Market_Size_2025_USD_Bn, Growth_Flag |
| `TreatmentType` | `data/market_share_by_treatment_type.csv` | one row per route | Treatment_Type, Share_Pct, Market_Size_2025_USD_Bn |
| `Channel` | `data/market_share_by_distribution_channel.csv` | one row per channel | Distribution_Channel, Share_Pct, Market_Size_2025_USD_Bn |
| `Company` | `data/top_companies_market_share.csv` | one row per company | Company, Share_Pct, Market_Size_2025_USD_Bn, Rank |
| `KeyMetrics` | `data/key_metrics.csv` | one row per KPI | Metric, Value |
| `SWOT` | `data/swot_analysis.csv` | one row per point | Category, Point |
| `Contributions` | `data/sip_contributions.csv` | one row per activity | Contribution, Count |
| `Measures` | (Enter Data, blank) | n/a | holds the DAX in `measures.dax` |

`Market_Size_2025_USD_Bn` in the segment tables is Share_Pct / 100 x 30.9 and is kept as a column so the CSVs are usable outside Power BI. Inside the report the equivalent measures (`Region Value (USD Bn)` etc.) recompute it.

## Power Query steps (same for every CSV)

1. Get Data > Text/CSV > pick the file > Transform Data.
2. Confirm first row is used as headers.
3. Set types: Year > Whole Number, all `Share_Pct` / `_USD_Bn` / `Count` > Decimal Number, everything else > Text.
4. Rename the query to the table name above.
5. Close & Apply.

Optional: put the repo path in a parameter (`DataFolder`) and build each source as `Csv.Document(File.Contents(DataFolder & "market_size_forecast.csv"))` so the report refreshes from any clone.

## Visual mapping (page: Market Overview)

| Poster chart | Visual | Axis / Legend | Values | Formatting |
|---|---|---|---|---|
| Market Size Forecast (USD Billion) | Line chart | X: Forecast[Year] | Forecast[Market_Size_USD_Bn] | Data labels on, markers on, Y axis 0 to 80, category axis as categorical |
| Market Share by Region (2025) | Clustered bar chart | Y: Region[Region] | [Region Share %] | Sort descending, data labels as %, X axis 0 to 40% |
| Market Share by Drug Class (2025) | Donut chart | Legend: DrugClass[Drug_Class] | [Drug Class Share %] | Detail labels: percent of total, 1 decimal |
| Market Share by Treatment Type (2025) | Donut chart | Legend: TreatmentType[Treatment_Type] | [Treatment Type Share %] | Two colours (dark blue / steel blue) |
| Market Share by Distribution Channel (2025) | Clustered bar chart | Y: Channel[Distribution_Channel] | [Channel Share %] | Sort descending, X axis 0 to 60% |
| Top 5 Companies by Market Share (2025) | Clustered bar chart | Y: Company[Company] | [Company Share %] | Sort by Company[Rank] ascending, "Others" last, X axis 0 to 80% |

## Visual mapping (page: Results & Findings)

| Element | Visual | Field |
|---|---|---|
| Estimated market size 2025 | Card | [Market Size 2025 (USD Bn)] |
| Forecast market size 2034 | Card | [Market Size 2034 (USD Bn)] |
| CAGR 2026 to 2034 | Card | [CAGR] formatted as 0.0% |
| Largest regional market | Card | [Largest Region] |
| Fastest-growing region | Card | [Fastest Growing Region] |
| Leading drug class | Card | [Leading Drug Class] |
| Fastest-growing drug class | Card | [Fastest Growing Drug Class] |
| Findings table | Table visual | KeyMetrics[Metric], KeyMetrics[Value] |
| SWOT | Matrix | Rows: SWOT[Category], Values: SWOT[Point] (or four text boxes) |

## Page: My Contributions

| Element | Visual | Field |
|---|---|---|
| Contributions during SIP | Clustered bar chart | Y: Contributions[Contribution], X: Contributions[Count], sort descending, X axis 0 to 25 |
| Total deliverables | Card | [Total Deliverables] |

## Theme

`theme.json` in this folder carries the poster palette. Import it via View > Themes > Browse for themes.

- Primary bar / line: `#1F3A93`
- Donut series: `#1F3A93`, `#2E7D32`, `#E64A19`, `#8E1B5B`, `#9E9E9E`
- Section header plum: `#6B1E4E`
- Treatment donut: `#1F3A93`, `#5B8DB8`
- Contributions bars: `#2C1E5B`
