# Setting up the report in Power BI Desktop

How the report is wired up, so it can be opened, refreshed or recreated from the files in this repository. Save the file as `powerbi/CKD_Market_Intelligence.pbix`.

## 1. Load the data

1. Open Power BI Desktop > Get Data > Text/CSV.
2. Load every file in `data/` one at a time. In the preview click **Transform Data** the first time so you can set types (see `data_model.md`), then Close & Apply.
3. Rename the queries: `market_size_forecast` > `Forecast`, `market_share_by_region` > `Region`, `market_share_by_drug_class` > `DrugClass`, `market_share_by_treatment_type` > `TreatmentType`, `market_share_by_distribution_channel` > `Channel`, `top_companies_market_share` > `Company`, `key_metrics` > `KeyMetrics`, `swot_analysis` > `SWOT`, `sip_contributions` > `Contributions`.
4. Model view: delete any auto-detected relationships. None are needed.

## 2. Add the measures

1. Home > Enter Data > name the table `Measures`, one empty column, Load.
2. With `Measures` selected, Modeling > New Measure, paste each measure from `measures.dax` (one at a time, or use Tabular Editor / DAX Studio to paste them all).
3. Hide the empty column so the table shows the calculator icon.
4. Format `CAGR`, `CAGR (selected range)`, `YoY Growth %` and every `... Share %` measure as Percentage, 1 decimal. Format the USD Bn measures as Decimal, 1 place.

## 3. Build the pages

**Page 1: Results & Findings**

Seven cards across the top (see the card table in `data_model.md`), a Table visual for `KeyMetrics`, and a Matrix or four text boxes for the SWOT. Title text box: "Global Chronic Kidney Disease Market Intelligence".

**Page 2: Market Overview**

Six visuals in a 2 x 3 grid:

| | |
|---|---|
| Line: Market Size Forecast (USD Billion) | Bar: Market Share by Region (2025) |
| Donut: Market Share by Drug Class (2025) | Donut: Market Share by Treatment Type (2025) |
| Bar: Market Share by Distribution Channel (2025) | Bar: Top 5 Companies by Market Share (2025) |

Field wells and formatting are listed in `data_model.md`. Data labels are on for every visual.

**Page 3: My Contributions**

Bar chart of `Contributions` plus a card for `[Total Deliverables]` (should read 54).

## 4. Slicers (optional, makes it interactive)

- Year range slicer on `Forecast[Year]` (between style). `CAGR (selected range)` recalculates for whatever range is selected; `CAGR` stays fixed at the 2025 to 2034 figure.
- Region slicer on `Region[Region]`; use Edit Interactions so it only filters the region bar and the region cards.

## 5. Check the numbers

Expected values once everything is wired up:

- Market Size 2025 = 30.9, Market Size 2034 = 67.8
- CAGR = 9.1% for the plain 2025 to 2034 compound rate; the headline 9.2% is the segment-weighted rate from the model
- Region shares in order: 38, 28, 27, 7, 5
- Drug class: ACE 26.5, ARBs 24.5, SGLT2 18.5, m-MRAs 12.0, Others 19.0
- Treatment: Oral 65, Injectable 35
- Channel: Hospital 52, Retail 35, Online 13
- Companies: AstraZeneca 12, Boehringer Ingelheim 9, Bayer 6, Merck 6, Others 64
- Total deliverables = 54

## 6. Exporting pages

File > Export > Export to PDF gives one page per report page for sharing outside Power BI.
