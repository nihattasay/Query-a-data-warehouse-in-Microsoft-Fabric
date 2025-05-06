# Query-a-data-warehouse-in-Microsoft-Fabric
# Microsoft Fabric Data Warehouse Lab: Querying Sample Data

This repository contains a summary of a hands-on lab for querying a data warehouse in [Microsoft Fabric](https://app.fabric.microsoft.com/). The lab demonstrates how to create a workspace, load a sample data warehouse, run SQL queries to gain insights, ensure data consistency, and save views.

## Lab Overview

In this lab, you will:

1. **Create a Workspace**
   - Navigate to Microsoft Fabric and create a new workspace with trial or premium capacity.

2. **Create a Sample Data Warehouse**
   - Create a sample warehouse named `sample-dw` pre-populated with taxi ride data.

3. **Query the Data Warehouse**
   - Run SQL queries to:
     - Aggregate total trips and revenue by month.
     - Analyze average trip duration and distance by weekday.
     - Identify top 10 pickup and dropoff cities.

4. **Verify Data Consistency**
   - Check for trips with unrealistic durations.
   - Delete inconsistent records such as trips with negative duration.

5. **Save a Query as a View**
   - Create a view that filters average trip metrics to the month of January (`vw_JanTrip`).

6. **Clean Up Resources**
   - Delete the workspace after completing the lab to free up resources.

## Example Queries

### Total Revenue by Month
```sql
SELECT 
 D.MonthName, 
 COUNT(*) AS TotalTrips, 
 SUM(T.TotalAmount) AS TotalRevenue 
FROM dbo.Trip AS T
JOIN dbo.[Date] AS D ON T.[DateID]=D.[DateID]
GROUP BY D.MonthName;
```

### Top 10 Pickup Cities
```sql
SELECT TOP 10 
 G.City, 
 COUNT(*) AS TotalTrips 
FROM dbo.Trip AS T
JOIN dbo.Geography AS G ON T.PickupGeographyID=G.GeographyID
GROUP BY G.City
ORDER BY TotalTrips DESC;
```

## View Creation Example

```sql
CREATE VIEW vw_JanTrip AS
SELECT 
 D.DayName, 
 AVG(T.TripDurationSeconds) AS AvgDuration, 
 AVG(T.TripDistanceMiles) AS AvgDistance 
FROM dbo.Trip AS T
JOIN dbo.[Date] AS D ON T.[DateID]=D.[DateID]
WHERE D.Month = 1
GROUP BY D.DayName;
```

---

© Microsoft Learning | For educational purposes only.
