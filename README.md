# Fuel Station and Pricing Analysis - Île-de-France

This project focuses on analyzing fuel station data in the Île-de-France region to help "Translogis," a transportation company based in Paris, optimize fuel costs. Using Power BI, DAX, and SQL, we identify the closest fuel stations with the lowest prices to improve cost management for the company's truck fleet. The analysis aims to support better decision-making regarding refueling strategies.

<img width="586" alt="dash fr fuel" src="https://github.com/user-attachments/assets/6adf3c48-9bb1-4978-8dce-9c9efddcc4ce">


## Report Link : 

Here’s the link to view the full interactive report on Power BI: [View Report](https://app.powerbi.com/groups/me/reports/2504a25e-339d-48a6-92ee-dac153b51dd2/c29599463420397b2f5f?experience=power-bi
)


## Table of Contents
- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Tools and Technologies](#tools-and-technologies)
- [Data Cleaning and Transformation](#data-cleaning-and-transformation)
- [Data Analysis and Visualization](#data-analysis-and-visualization)
- [DAX Codes](#dax-codes)
- [SQL Queries](#sql-queries)
- [Conclusion](#conclusion)

## Project Overview
This project aims to identify the most cost-effective fuel stations for the "Translogis" fleet within the Île-de-France region by analyzing fuel prices and distances from key locations. The dashboard created in Power BI allows for interactive data exploration and visualization to facilitate fuel management decisions.

## Objectives
1. Identify fuel stations with the lowest prices for different types of fuel (SP95, SP98, E10, E85).
2. Provide insights into the availability of 24/7 service across fuel stations.
3. Visualize distances from the company's base to various fuel stations and analyze price trends.
4. Support decision-making for optimizing refueling strategies to reduce overall transportation costs.

## Tools and Technologies
- **Power BI**: For creating interactive dashboards and visualizations.
- **DAX (Data Analysis Expressions)**: Used for creating measures and performing calculations.
- **SQL**: For querying, transforming, and validating data.
- **Microsoft Excel**: Used for initial data exploration and cleaning.
- **GitHub**: For version control and project management.

## Data Cleaning and Transformation
- **Data Import**: Fuel pricing data was imported into Power BI after initial processing in SQL.
- **Handling Missing Data**: Removed records with missing fuel prices or incomplete address information.
- **Filtering**: Focused on stations located in Île-de-France with known fuel prices.
- **Distance Calculation**: Calculated the distance from the company's base to each station using SQL spatial functions.

## Data Analysis and Visualization

### Power BI and DAX
- Created measures to analyze the **average fuel prices** and **count of 24/7 stations**.
- Visualized **top 10 closest fuel stations** and their respective prices for different fuel types.
- Added filters for fuel type, price range, and distance to allow dynamic exploration.

### Key Visualizations
1. **Summary Cards**: Display total fuel stations, 24/7 stations, average prices, and distance range.
2. **Map Visualization**: Shows the geographic distribution of fuel stations with color-coded pricing.
3. **Top 10 Closest Stations Table**: Lists the nearest stations with fuel prices and distance details.
4. **24/7 Service Availability Chart**: Illustrates the number of stations that offer 24/7 service.

## DAX Codes

- **Total 24/7 Stations**
   ```DAX
   Total_24_7_Stations = 
   CALCULATE(
       COUNT('fuel_db'[id]),
       'fuel_db'[Automate 24-24] = "oui"
   )

- **Average Price Calculation**
   ```DAX
   Average_Price = 
   VAR SelectedFuelType = SELECTEDVALUE(FuelTypes[FuelType])
   RETURN 
       SWITCH(
           SelectedFuelType,
           "SP95", AVERAGE('fuel_db'[prix_sp95]),
           "SP98", AVERAGE('fuel_db'[prix_sp98]),
           "E10", AVERAGE('fuel_db'[prix_e10]),
           "E85", AVERAGE('fuel_db'[prix_e85]),
           BLANK()
       )

- **Creating a Fuel Types Table**
   ```DAX
  FuelTypes = DATATABLE(
    "FuelType", STRING,
    {
        {"SP95"},
        {"SP98"},
        {"E10"},
        {"E85"}
    }
  )
- **Calculating the Actual Fuel Price Based on Selected Fuel Type**
  ```DAX
  Actual Station Price = 
  VAR SelectedFuelType = SELECTEDVALUE(FuelTypes [FuelType])
  RETURN 
    SWITCH(
        TRUE(),
        SelectedFuelType = "SP95", FIRSTNONBLANK('fuel_db fuel (2)'[prix_sp95], 0),
        SelectedFuelType = "SP98", FIRSTNONBLANK('fuel_db fuel (2)'[prix_sp98], 0),
        SelectedFuelType = "E10", FIRSTNONBLANK('fuel_db fuel (2)'[prix_e10], 0),
        SelectedFuelType = "E85", FIRSTNONBLANK('fuel_db fuel (2)'[Prix E85], 0),
        BLANK()  // Returns blank if no selection is made
    )
## SQL Queries
- **Calclating distance:**
  ```sql
    SELECT
    adresse, 
    ville,
    prix_sp95, 
    latitude, 
    longitude,
    ROUND(
        ST_Distance_Sphere(
            POINT(2.3906766, 48.7413944), 
            POINT(CAST(longitude AS DECIMAL(9, 6)), CAST(latitude AS DECIMAL(9, 6)))
        ) / 1000, 1
    ) AS distance_km
    FROM fuel
    WHERE region='Île-de-France'
    AND prix_sp95 IS NOT NULL
    ORDER BY distance_km;


 ## Conclusion
 
The project offers valuable insights for "Translogis" by identifying fuel stations with optimal pricing and analyzing distance groups to aid in strategic decision-making. By understanding fuel price trends and the availability of 24/7 services, "Translogis" can reduce fuel expenses and optimize its logistics operations.


  
