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

Continue calculations only if wick > 0.---

# 6. LCR FORMULA

The Liquidity Consumption Ratio (LCR) measures how much of the pivot wick has been consumed by price after the pivot has formed.

Rules:

Bullish Pivot

Body = Close of the pivot candle

Wick Extreme = Low of the pivot candle

Wick Length = Body - Wick Extreme

Current Consumption = Current High - Wick Extreme

LCR = (Current Consumption / Wick Length) × 100

Bearish Pivot

Body = Close of the pivot candle

Wick Extreme = High of the pivot candle

Wick Length = Wick Extreme - Body

Current Consumption = Wick Extreme - Current Low

LCR = (Current Consumption / Wick Length) × 100

Clamp values

Minimum = 0%

Maximum = 100%

Never allow negative values.

Never allow values above 100%.

---

# 7. LCR UPDATE

The active pivot updates every bar.

Only the active pivot updates.

Historical pivots never change.

Historical pivots remain frozen forever.

Every new candle recalculates only:

activePivotLCR

When a new pivot appears:

Freeze previous pivot

Store final LCR

Activate new pivot

Restart calculation

---

# 8. DISPLAY

Display only ONE label.

Always attached to the active pivot.

Label text

LCR

Current percentage

Example

LCR

72%

Bullish labels

Green

Bearish labels

Red

Text

White

---

# 9. LABEL POSITION

Bullish Pivot

Below pivot

Bearish Pivot

Above pivot

Offset

Small fixed offset

Never overlap candle body.

Always remain readable.

---

# 10. DEBUG MODE

Input

Show Debug

If enabled

Show Pivot Index

Show Stored Pivot Count

Show Active Pivot

Show Current LCR

If disabled

Display nothing except LCR label.
