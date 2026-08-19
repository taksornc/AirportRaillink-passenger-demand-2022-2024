#  Airport Rail Link Passenger Analytics Dashboard (2022 - 2024)

An end-to-end Business Intelligence project analyzing passenger volume trends and station usage behavior for the Airport Rail Link system in Bangkok, Thailand. Built with Power BI, Power Query, and DAX.

---

##  Project Overview
This project transforms raw passenger transport data into an interactive Executive Dashboard to help transit managers and commercial strategists:
1. Monitor system-wide passenger trends and seasonality patterns.
2. Evaluate performance across stations and travel types (Arrival vs. Departure).
3. Identify high-value stations to optimize commercial space monetization (e.g., Advertising Space Rates).

---

##  Data Architecture & Star Schema Model
To ensure optimal query performance and scalability, the dataset was structured into a **Star Schema** data model:

* **Fact Table:**
  * `FactPassenger` (Passenger Count, DateID, StationID, TravelTypeID)
* **Dimension Tables:**
  * `DimDate` (Date, Year, Month, Day Name, Day Type)
  * `DimStation` (Station Name)
  * `DimTravelType` (Arrival / Departure)

###  ETL & Data Cleaning (Power Query)
* Removed duplicate records and cleaned text spaces using `Text.Trim` and `Text.Clean`.
* Created surrogate key `DateID` (Format: `YYYYMMDD`) to establish a 1-to-Many relationship with `DimDate`.
* Converted data types to integer/text appropriate for optimal storage.

---
## insight
Dynamic Top 3 Ranking, improving analytical flexibility and model organization.
Built a 2-page interactive Power BI dashboard featuring KPI cards, time-series analysis, weekday/weekend segmentation, filters, and station-level drill-through.
Generated business insights by identifying a recurring April seasonal decline, recommending targeted promotional campaigns during low-demand periods.
Discovered that the Top 3 stations (Suvarnabhumi, Phaya Thai, and Makkasan) accounted for 57.03% of total passenger volume (8 stations, 2024), supporting data-driven premium advertising pricing strategies.

##  DAX Measures & Optimization
Key measures calculated using DAX include:

```dax
// Total Passengers
ปริมาณผู้โดยสารรวม = SUM(FactPassenger[Passenger])

// YoY Growth %
อัตราการเติบโตเทียบปีก่อน (%) = 
VAR CurrentYearValue = [ปริมาณผู้โดยสารรวม]
VAR PreviousYearValue = CALCULATE([ปริมาณผู้โดยสารรวม], SAMEPERIODLASTYEAR(DimDate[Date]))
RETURN
DIVIDE(CurrentYearValue - PreviousYearValue, PreviousYearValue, 0)

// Station Market Share %
ส่วนแบ่งสถานี (%) = 
DIVIDE(
    [ปริมาณผู้โดยสารรวม], 
    CALCULATE([ปริมาณผู้โดยสารรวม], ALL(DimStation))
)



