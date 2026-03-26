EMS Calls vs Units Power BI Dashboard
Overview

This Power BI project demonstrates minute-by-minute visualization of emergency medical services (EMS) activity, comparing call volume to available ambulance units.

The goal is to replicate a real-world scenario using mock data, highlighting the benefits of Power BI over traditional Excel analysis (which becomes slow and cumbersome with large datasets).


 Tables:
     Calls
        Columns: Calls, Start Time, End Time
        209 mock calls in a single day
        Call duration averages 75 minutes
    Units
        Units, Start Time, End Time
        49 mock ambulance units
        Each unit works 12-hour shifts staggered throughout the day
     TimeTable
        Generated in Power BI using DAX
        Minute-level granularity (00:00 to 23:59)
        Used as the X-axis for all time-based measures
Measures:
    Active Calls
        Active Calls =
        VAR CurrentMinute = MAX(TimeTable[MinuteNumber])
        RETURN
        CALCULATE(COUNTROWS(Calls),
        Calls[Start Time] <= (DATE(2026,1,1) + TIME(0,0,0) + CurrentMinute/1440),
        Calls[End Time] > (DATE(2026,1,1) + TIME(0,0,0) + CurrentMinute/1440))
    Active Units
        Active Units =
        VAR CurrentMinute = MAX(TimeTable[MinuteNumber])
        RETURN
        CALCULATE(COUNTROWS(Units),
        Units[Start Time] <= (DATE(2026,1,1) + TIME(0,0,0) + CurrentMinute/1440),
        Units[End Time] > (DATE(2026,1,1) + TIME(0,0,0) + CurrentMinute/1440))
    
Utilization % (optional)
Unit Utilization % = DIVIDE([Active Calls], [Active Units])

Visualization:
    Stacked Line chart
        X-axis: TimeTable[MinuteNumber] (numeric, continuous)
        Y-axis: Active Calls and Active Units
    Minute labels displayed using Minute or MinuteLabel column
    Shows peak periods and system load in real time
    Highlights periods where calls approach or exceed available units
Features:
    Uses mock data to simulate realistic EMS operations
    Handles 1 day, 209 calls, 49 units efficiently
    Demonstrates Power BI advantages over Excel:
    No row explosion (Excel would require expanding calls per minute)
    Fast computation using DAX measures
    Flexible minute-level visualizations

File Contents:
    chart demo.pbix — Power BI Desktop file
Optional:
    volume data.xlsx — source data table for calls and units info
    README.md — project documentation
How to Use:
Open EMS_Calls_vs_Units.pbix in Power BI Desktop
Verify tables (Calls, Units, TimeTable) are loaded correctly
Refresh the report to see the line chart update dynamically
Adjust data or shift patterns as needed to simulate other scenarios
Notes
All data is mocked for demonstration purposes
Time-based calculations use minute granularity for accuracy
No relationships between tables are required; measures calculate concurrency directly

Author

Joshua Sherrell
