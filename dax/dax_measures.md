# DAX Measures - Revenue Performance Analytics

This file documents the core DAX measures used in the Power BI dashboard.

---

## Total Revenue
```DAX
Total Revenue = 
SUM ( Fact_OrderItems[price])
```

---

## Orders
```DAX
Orders = 
DISTINCTCOUNT ( Fact_OrderItems[order_id])
```

---

## Average Order Value (AOV)
```DAX
AOV =
DIVIDE ([Total Revenue], [Orders])
```

---

## Running Total Revenue
```DAX
Running Total Revenue = 
CALCULATE (
	[Total Revenue],
	FILTER (
		ALL ( Dim_Date[Date] ),
		Dim_Date[Date] <= MAX ( Dim_Date[Date])
	)
)
```

---

## Revenue Month-over-Month%

Calculates percentage change in revenue compared to the previous month.

```DAX
Revenue MOM% = 
VAR CurrentMonthRevenue =
    [Total Revenue]

VAR PreviousMonthRevenue =
    CALCULATE (
        [Total Revenue],
        DATEADD ( Dim_Date[Date], -1, MONTH )
    )

RETURN
DIVIDE (
    CurrentMonthRevenue - PreviousMonthRevenue,
    PreviousMonthRevenue
)
```


## Revenue Year-over-Year%
```DAX
Revenue YoY% =
DIVIDE (
	[Total Revenue] - [Revenue (Prior Year)],
	[Revenue (Prior Year)]
	)
```
---