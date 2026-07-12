# Liquidity Consumption Ratio (LCR)
## Complete Specification
Version 2.0

---

# 1. OBJECTIVE

The purpose of this indicator is to measure how much liquidity has been consumed inside the wick of the latest confirmed pivot.

The script must continuously monitor pivots.

Only ONE pivot is active at a time.

When a new confirmed pivot appears:

- Freeze every value of the previous pivot.
- Store all data in arrays.
- Activate the new pivot.
- Start a new LCR calculation.

No repaint.

Only confirmed pivots are allowed.

---

# 2. PIVOT DETECTION

Inputs:

Pivot Timeframe

Left Bars

Right Bars

Use

request.security()

to detect pivots on the selected timeframe.

Bullish pivot

ta.pivotlow()

Bearish pivot

ta.pivothigh()

Only confirmed pivots.

No repaint.

---

# 3. STORED INFORMATION

Each pivot must store:

Price

Bar index

Time

Bullish / Bearish

Current LCR

Body price

Wick extreme

Confirmation state

Arrays:

pivotPrices

pivotBull

pivotTimes

pivotBarIndex

pivotLCR

pivotConfirmed

---

# 4. ACTIVE REFERENCE PIVOT

Variables

activePivotIndex

activePivotBody

activePivotWick

activeBullPivot

activePivotLCR

When a new pivot appears

Freeze previous pivot

Save LCR

Save state

Activate new pivot

Only one active pivot exists.

---

# 5. BODY / WICK RULES

Bullish Pivot

Body reference

close

Wick reference

low

Wick length

close - low

Bearish Pivot

Body reference

close

Wick reference

high

Wick length

high - close

If wick length <= 0

LCR = 0

Continue calculations only if wick > 0.
