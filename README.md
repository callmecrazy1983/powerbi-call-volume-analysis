# powerbi-call-volume-analysis
# Call Volume vs Resource Utilization (Power BI)

## Problem
Analyze active calls per minute vs trucks in service.

## Approach
- DAX interval overlap logic
- Time table via GENERATESERIES
- Unified fact table with Type (Call, Truck)

## Key Measures
Active Count :=
VAR CurrentMinute = MAX(TimeTable[MinuteValue])
RETURN
CALCULATE(
    COUNTROWS(FactIntervals),
    FactIntervals[StartMinute] <= CurrentMinute,
    FactIntervals[EndMinute] >= CurrentMinute
)

## Visualization
- X-axis: Minute
- Lines: Calls vs Trucks

## Notes
- No row expansion (memory efficient)
- Scales without large data growth
