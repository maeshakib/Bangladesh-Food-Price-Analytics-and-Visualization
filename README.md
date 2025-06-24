# Bangladesh-Food-Price-Analytics-and-Visualization
Bangladesh Food Price Analytics and Visualization
Bangladesh Demographics Dashboard, purpose of the analysis is to explore and visualize urban data from Bangladeshi cities, uncovering patterns in population, infrastructure, and socio-economic factors. Using the [Bangladesh Food Prices Dataset
 ]( https://www.kaggle.com/datasets/usmanlovescode/bangladesh-food-prices-dataset) dataset from Kaggle, the Tableau dashboard aims to provide actionable insights for urban planning and policy-making.

<img align="left" alt="Home page | PBI" width="1000px" src="https://github.com/maeshakib/z_resources/blob/405e11928e0f7da529312564d6136f2831873dc5/HomePage%20Food%20Price%20in%20Bangladesh.png" /> <br>
<br>
 
<img align="left" alt="Home page | PBI" width="1000px" src="https://github.com/maeshakib/z_resources/blob/c4eaacd2c3a4d472b179ed02d6e4edd1cf4f500e/YearOverYearPage%20Food%20Price%20in%20Bangladesh.png" /> <br>
<br>
### Data set

<a href="https://www.kaggle.com/datasets/msjahid/bangladesh-districts-wise-population">
  <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
   Data set
  </button>
</a>
 <br>

 ### Link

<a href="https://public.tableau.com/views/BangladeshPopulation2022Dashboard/PopDashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link">
  <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
    Explore Bangladesh Population Dashboard
  </button>
</a>

### Report: 
<a href="https://github.com/maeshakib/z_resources/blob/0dae211943a4a2d212d95f95410295329429f517/DAS%20602%20Assignment%20WFP%20Dashboard%20report%20Student%20ID%20241001661.pdf">
  <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
   Report
  </button>
</a>
 
### Data Cleaning 

#### Planned Data cleaning:
- Handling Missing Values: I will check for and address missing values in the dataset.
- Handling Duplicate Data: I will identify and remove duplicate rows based on unique identifiers to ensure each division and district’s data is unique
- Data Type Conversion: I will convert numerical variables (population, area) to appropriate numeric data types, and verify the representation of categorical variables.
  

### Key Findings on data cleaning :
I have utilized power query for data cleaning process.
- Header Row Adjustment: Set the first row as the header to correct column naming. ensuring that the column names are accurate and correspond to the data.
- Removing Unnecessary Rows: Removed the first row containing metadata. The first row contained metadata about the dataset, which needed to be removed.
- Column Profiling: Enabled column profiling to check for errors; no errors were found.
- Handling Null or Blank Values: Using column profiling, no significant errors were detected in the dataset under the "Error Value" section
- Data Type Conversion: Converted "latitude" and "longitude" from text to Decimal Number for accurate geospatial mapping. Converted "price" and "usdprice" from text to Decimal Number for proper calculations.
- Renaming Columns: Renamed “admin1” to Division and “admin2” to District for clarity
- Outlier handling: I identified oil price 680 which is far from median 111 BDT, I replaced outlier with median price of the oil.
### Critical Data Analysis Outcomes



#### YoY Price (BDT) Comparison (Division):
- This chart highlights the year-to-year price changes (in BDT) for specific commodities (e.g., wheat, rice, oil, lentils) within a selected Division and Selected Year with previous year.
- It allows users to quickly identify short-term price fluctuations, such as the significant 18.778 BDT drop in oil prices, enabling targeted analysis of market volatility and informing immediate supply chain or pricing adjustments.
2.
#### YoY Compound Annual Growth Rate Price(%) Comparison with (Division):
- By showing the CAGR from a base year (e.g., 2006) to a selected year (e.g., 2013) for a division, this chart provides a long-term perspective on price growth trends (e.g., 9.941% for oil).
- It helps stakeholders understand the annualized rate of price increases, assess inflation impacts, and plan for sustainable food security strategies over extended periods.
3.
#### YoY Price (BDT) Comparison with (All Division):
- This chart compares Chittagong’s YoY price changes with the national average, revealing regional disparities (e.g., rice dropping 1.894 BDT more in Chittagong than nationally).
- It aids in benchmarking regional performance, identifying areas of competitive pricing or supply issues, and supporting policy decisions tailored to specific divisions.
4.
#### YoY Compound Annual Growth Rate Price(%) Comparison with (All Division):
- This chart compares Chittagong’s CAGR with the national average (e.g., 0.928% higher for oil), offering insights into long-term regional price growth relative to the country.
- It assists in evaluating whether Chittagong’s market aligns with or deviates from national trends, guiding investment or intervention strategies based on relative growth rates.


#### 1_YoY_AvgYearSalesDiff =
```
  VAR SelectedYear = VALUE(SELECTEDVALUE(wfp_food_prices_bgd[Year]))
  VAR SelectedDivision = SELECTEDVALUE(wfp_food_prices_bgd[division])
  VAR PrevYear = SelectedYear - 1
  VAR YoY_AvgSalesSelectedYearThisDivision =
  CALCULATE(
  AVERAGE(wfp_food_prices_bgd[price]),
  wfp_food_prices_bgd[Year]=SelectedYear ,
  wfp_food_prices_bgd[division]=SelectedDivision,wfp_food_prices_bgd[pricetype]="Retail"
  )
  VAR YoY_AvgSalesPrevYearThisDivision =
  CALCULATE(
  AVERAGE(wfp_food_prices_bgd[price]),
  wfp_food_prices_bgd[Year]=PrevYear ,
  wfp_food_prices_bgd[division]=SelectedDivision,wfp_food_prices_bgd[pricetype]="Retail"
  )
  RETURN YoY_AvgSalesSelectedYearThisDivision -YoY_AvgSalesPrevYearThisDivision
```
####  4_CAGR_RetailPrice_Commodity =
```
VAR BaseYear = 2006 VAR
SelectedYear = VALUE(SELECTEDVALUE(wfp_food_prices_bgd[Year]))
VAR SelectedDivision = SELECTEDVALUE(wfp_food_prices_bgd[division])
-- Average price in the base year (2006)
VAR PriceBaseYear = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]), wfp_food_prices_bgd[Year] = BaseYear, wfp_food_prices_bgd[division] = SelectedDivision, wfp_food_prices_bgd[pricetype] = "Retail" )
-- Average price in the selected year (e.g., 2018)
VAR PriceSelectedYear = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]), wfp_food_prices_bgd[Year] = SelectedYear, wfp_food_prices_bgd[division] = SelectedDivision, wfp_food_prices_bgd[pricetype] = "Retail" )
-- Number of years between base year and selected year
VAR NumberOfYears = SelectedYear - BaseYear
-- CAGR calculation: ((PriceSelectedYear / PriceBaseYear)^(1/NumberOfYears)) - 1
VAR CAGR = IF( NOT ISBLANK(PriceBaseYear) && NOT ISBLANK(PriceSelectedYear) && PriceBaseYear <> 0 && NumberOfYears > 0, (PriceSelectedYear / PriceBaseYear) ^ (1 / NumberOfYears) - 1, BLANK() )
RETURN CAGR
```


####  3_YoY_AvgPriceDiff_vs_AllDivisions =
```
VAR SelectedYear = VALUE(SELECTEDVALUE(wfp_food_prices_bgd[Year]))
VAR SelectedDivision = SELECTEDVALUE(wfp_food_prices_bgd[division])
VAR PrevYear = SelectedYear - 1
--YoY Price Difference for Selected Division
VAR YoY_AvgSalesSelectedYearThisDivision = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]),
wfp_food_prices_bgd[Year] = SelectedYear, wfp_food_prices_bgd[division] = SelectedDivision, wfp_food_prices_bgd[pricetype] = "Retail" )
VAR YoY_AvgSalesPrevYearThisDivision = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]),
wfp_food_prices_bgd[Year] = PrevYear, wfp_food_prices_bgd[division] = SelectedDivision, wfp_food_prices_bgd[pricetype] = "Retail" )
VAR YoY_Diff_SelectedDivision = YoY_AvgSalesSelectedYearThisDivision - YoY_AvgSalesPrevYearThisDivision
-- YoY Price Difference for All Divisions
VAR YoY_AvgSalesSelectedYearAllDivisions = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]),
wfp_food_prices_bgd[Year] = SelectedYear, wfp_food_prices_bgd[pricetype] = "Retail", ALL(wfp_food_prices_bgd[division]) )
VAR YoY_AvgSalesPrevYearAllDivisions = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]),
wfp_food_prices_bgd[Year] = PrevYear, wfp_food_prices_bgd[pricetype] = "Retail", ALL(wfp_food_prices_bgd[division]) )
VAR YoY_Diff_AllDivisions = YoY_AvgSalesSelectedYearAllDivisions - YoY_AvgSalesPrevYearAllDivisions
-- Compare Selected Division vs All Divisions
RETURN IF( ISBLANK(YoY_Diff_SelectedDivision) || ISBLANK(YoY_Diff_AllDivisions), BLANK(), YoY_Diff_SelectedDivision - YoY_Diff_AllDivisions )

```

####  5_CAGR_vs_AllDivisions =
```

VAR BaseYear = 2006
VAR SelectedYear = VALUE(SELECTEDVALUE(wfp_food_prices_bgd[Year]))
VAR SelectedDivision = SELECTEDVALUE(wfp_food_prices_bgd[division])
-- CAGR for Selected Division
VAR PriceBaseYearSelectedDivision = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]), wfp_food_prices_bgd[Year] = BaseYear, wfp_food_prices_bgd[division] = SelectedDivision, wfp_food_prices_bgd[pricetype] = "Retail" )
VAR PriceSelectedYearSelectedDivision = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]), wfp_food_prices_bgd[Year] = SelectedYear, wfp_food_prices_bgd[division] = SelectedDivision, wfp_food_prices_bgd[pricetype] = "Retail" )
VAR NumberOfYears = SelectedYear - BaseYear
VAR CAGR_SelectedDivision = IF( NOT ISBLANK(PriceBaseYearSelectedDivision) && NOT ISBLANK(PriceSelectedYearSelectedDivision) && PriceBaseYearSelectedDivision <> 0 && NumberOfYears > 0, (PriceSelectedYearSelectedDivision / PriceBaseYearSelectedDivision) ^ (1 / NumberOfYears) - 1, BLANK() )
-- CAGR for All Divisions
VAR PriceBaseYearAllDivisions = CALCULATE( AVERAGE(wfp_food_prices_bgd[price]), wfp_food_prices_bgd[Year] =

```
