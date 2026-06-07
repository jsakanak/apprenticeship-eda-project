## Sales Measures

```dax
% Slots With Adjustments = VAR ActiveSlots = DISTINCTCOUNT(FactInventoryAdjustments_cleaned[location_id])
VAR TotalSlots = CALCULATE(COUNTROWS(PrimeSlots_cleaned), ALL(PrimeSlots_cleaned))
RETURN
    FORMAT(DIVIDE(ActiveSlots, TotalSlots), "0.0%") & " of slots active"
```

```dax
Item Count = DISTINCTCOUNT(FactInventoryAdjustments_cleaned[item_id])
```

```dax
Negative Adjustments = CALCULATE(COUNTROWS(FactInventoryAdjustments_cleaned), FactInventoryAdjustments_cleaned[subpack_qty] < 0)
```

```dax
Net Adjustment Rate = DIVIDE(
    CALCULATE(COUNTROWS(FactInventoryAdjustments_cleaned), FactInventoryAdjustments_cleaned[subpack_qty] > 0),
    COUNTROWS(FactInventoryAdjustments_cleaned)
)
```

```dax
Positive Adjustments = CALCULATE(COUNTROWS(FactInventoryAdjustments_cleaned), FactInventoryAdjustments_cleaned[subpack_qty] > 0)
```

```dax
Selected Date Range = VAR MinDate = FORMAT(MIN(FactInventoryAdjustments_cleaned[created_timestamp]), "DD MMM YYYY")
VAR MaxDate = FORMAT(MAX(FactInventoryAdjustments_cleaned[created_timestamp]), "DD MMM YYYY")
RETURN MinDate & " – " & MaxDate
```

```dax
Slot Utilisation % = DIVIDE(DISTINCTCOUNT(FactInventoryAdjustments_cleaned[location_id]), CALCULATE(COUNTROWS(PrimeSlots_cleaned), ALL(PrimeSlots_cleaned)))
```

```dax
Total Cost = 
SUMX(
    FactInventoryAdjustments_cleaned,
    FactInventoryAdjustments_cleaned[unit_qty] * RELATED(DimItemL1_cleaned[Unit Price])
)
```

```dax
Total Cost - Negative = CALCULATE(
    SUMX(
        FactInventoryAdjustments_cleaned,
        FactInventoryAdjustments_cleaned[unit_qty] * RELATED(DimItemL1_cleaned[Unit Price])
    ),
    FactInventoryAdjustments_cleaned[unit_qty] < 0
)
```