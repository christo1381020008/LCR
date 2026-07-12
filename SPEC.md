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
---

# 11. PIVOT LIFE CYCLE

Each pivot follows the same lifecycle.

STEP 1

A pivot is confirmed.

STEP 2

It becomes the Active Pivot.

STEP 3

LCR starts at 0%.

STEP 4

LCR updates on every new candle.

STEP 5

When LCR reaches 100%

The pivot becomes CONSUMED.

STEP 6

The pivot is frozen forever.

STEP 7

Wait for the next confirmed pivot.

Only one Active Pivot exists.

Only one pivot can update.

Every previous pivot is immutable.

---

# 12. CONSUMED PIVOTS

When LCR reaches 100%

Store

Final LCR

Final Time

Final Bar

Consumed = True

The pivot never changes again.

The label disappears.

The script waits for the next confirmed pivot.

---

# 13. NEW PIVOT

If a new pivot is confirmed before the current Active Pivot reaches 100% LCR:

- Freeze the current Active Pivot at its current LCR value.
- Store its frozen LCR.
- Mark the pivot as Frozen.
- Activate the newly confirmed pivot.
- The new pivot becomes the Active Pivot.

If the new Active Pivot later reaches 100%:

- Mark it as Consumed.
- Remove its LCR label.
- Reactivate the previous Frozen Pivot.
- Resume its LCR calculation from its last frozen value.
- Continue updating until it reaches 100% or another pivot becomes active.

A pivot always owns one single LCR value during its entire lifetime.
---

# 14. DATA STORED

Each pivot stores

Pivot Price

Pivot Time

Pivot Index

Bullish / Bearish

Body Price

Wick Price

Current LCR

Final LCR

Consumed

Replaced

Confirmation Bar

---

# 15. HISTORY

Maximum stored pivots

User Input

Default

500

If maximum is exceeded

Delete the oldest pivot.

Never delete the active pivot.

---

# 16. REPAINT

No repaint allowed.

Only confirmed pivots.

No future information.

No recalculation of historical pivots.

Historical values never change.

Historical labels never move.

---

# 17. ALERTS

Alert

New Pivot

Alert

LCR reaches 25%

Alert

LCR reaches 50%

Alert

LCR reaches 75%

Alert

LCR reaches 100%

Each alert fires only once.

---

# 18. FAILSAFE

If wick length equals zero

Skip calculations.

If pivot arrays become inconsistent

Ignore the pivot.

Never generate runtime errors.

Always continue execution.
---

# 19. REFERENCE PIVOT SELECTION

The indicator must always maintain one Active Reference Pivot.

The Active Reference Pivot is the most recent confirmed pivot that has not yet reached 100% LCR.

When a newer pivot is confirmed:

- Freeze the previous Active Pivot.
- Store its current LCR.
- Activate the new pivot.

When the Active Pivot reaches 100%:

- Mark it as Consumed.
- Remove its label.
- Search backwards for the latest Frozen Pivot that has not yet reached 100%.
- If one exists, reactivate it.
- Resume its LCR from its stored frozen value.
- Continue updating normally.

If no valid Frozen Pivot exists:

The indicator waits until a new pivot is confirmed.

At every moment, only ONE pivot can be Active.

All other pivots are either:

• Frozen
---

# 20. USER SETTINGS

The user must be able to configure every important parameter.

Inputs

• Pivot Timeframe

• Pivot Left Bars

• Pivot Right Bars

• Maximum Stored Pivots

• Maximum Displayed LCR Labels

• Bullish Label Color

• Bearish Label Color

• Text Color

• Text Size

• Label Position

Below the wick

Above the wick

Left of the wick

Right of the wick

• Bold Text

Yes / No

• Decimal Precision

0

1

2

Default = 0

---

# 21. LABEL RULES

Each pivot owns only ONE LCR label.

A label always belongs to its pivot.

A label never changes pivot.

A label only displays

LCR xx%

Example

LCR 72%

No other information is displayed.

No arrows.

No icons.

No trend indication.

No Buy / Sell.

The LCR value is the only information.

---

# 22. LABEL REMOVAL

If LCR reaches 100%

Delete the label immediately.

The pivot remains stored.

The LCR value remains stored internally.

Only the visual label disappears.

The pivot can still become Active again if required by the Active Pivot Selection rules.

---

# 23. PERFORMANCE

The script must remain lightweight.

Avoid unnecessary loops.

Never scan more pivots than required.

Delete old pivots when Maximum Stored Pivots is exceeded.

Use arrays efficiently.

Never create duplicated labels.

Never create duplicated pivots.
or

• Consumed

Never allow two Active pivots simultaneously.
---

# 24. LCR CALCULATION RULES

The LCR always measures the consumption of the wick belonging to the Active Reference Pivot.

Bullish Pivot

0% = Body Low

100% = Wick Low

Bearish Pivot

0% = Body High

100% = Wick High

The LCR is calculated only inside the wick.

The body is never included.

The calculation must always remain between:

0%

and

100%.

---

# 25. FROZEN PIVOTS

When a pivot loses its Active status because a newer pivot is confirmed:

Freeze the current LCR.

Do not update it anymore.

Keep the label visible.

Store the frozen value.

---

# 26. REACTIVATED PIVOTS

If the Active Pivot reaches 100%:

- Mark it as Consumed.
- Delete its LCR label.
- Stop updating it forever.

Then:

Search backwards for the most recent Frozen Pivot.

If its LCR is still below 100%:

- Reactivate it.
- Resume its LCR from its last frozen value.
- Continue updating normally.

If no Frozen Pivot is available:

Wait until a new confirmed pivot is created.

---

# 27. CONSUMED PIVOTS

A Consumed Pivot is a pivot whose LCR has reached exactly 100%.

A Consumed Pivot:

- Can never receive updates again.
- Can never become the Active Pivot again.
- Keeps its stored data for historical purposes.
- Never displays its LCR label again.
---

# 28. DISPLAY LIMIT

Input

Maximum Displayed Labels

Range

1 → 100

Default

10

If the number of visible labels exceeds the limit

Hide the oldest visible labels.

Never hide the Active Pivot label.

---

# 29. FINAL DESIGN RULES

LCR must remain a measurement instrument.

Never generate trading signals.

Never suggest entries.

Never suggest exits.

Never predict market direction.

Never interpret liquidity.

Only measure liquidity consumption.---

# 30. PIVOT STATES

Each pivot can only be in one of four states.

WAITING

The pivot has just been confirmed.

ACTIVE

The pivot is currently measured.

FROZEN

The pivot lost its Active status because a newer pivot became Active.

Its LCR is frozen.

It may become Active again later.

CONSUMED

The pivot reached 100%.

It is permanently finished.

It can never become Active again.
---

# 31. MULTI TIMEFRAME

The user must be able to choose the timeframe used to detect pivots.

Supported values:

• 1 Minute
• 3 Minutes
• 5 Minutes
• 15 Minutes
• 30 Minutes
• 1 Hour
• 4 Hours
• Daily
• Weekly
• Monthly

Pivot detection must always use request.security().

Changing the Pivot Timeframe must immediately rebuild the pivot database.

No historical pivot from the previous timeframe may remain.

---

# 32. PIVOT CONFIRMATION

Only confirmed pivots are valid.

A pivot does not exist before confirmation.

The script must never anticipate a pivot.

No repaint is allowed.

---

# 33. LCR MEMORY

Each pivot owns only one LCR value.

This value exists during the entire lifetime of the pivot.

The LCR may evolve.

The LCR may freeze.

The LCR may resume.

The LCR may finally become Consumed.

A second LCR must never be created for the same pivot.

---

# 34. LABEL STYLE

The user may customize:

• Text Color

• Bullish Label Color

• Bearish Label Color

• Text Size

• Bold

• Position

Available positions

Below

Above

Left

Right

Default

Below for Bullish

Above for Bearish

---

# 35. LABEL CONTENT

The label must only display:

LCR XX%

Examples

LCR 12%

LCR 48%

LCR 81%

No decimals by default.

User may enable

1 decimal

or

2 decimals.

No other text is allowed.

---

# 36. LABEL BEHAVIOUR

The label follows the Active Pivot.

Frozen labels remain fixed.

Consumed labels disappear.

Labels never overlap intentionally.

Labels never change pivot.

Each pivot owns one label only.---

# 37. EXAMPLES OF EXPECTED BEHAVIOUR

## Example 1

A Bullish Pivot A is confirmed.

Pivot A becomes the Active Pivot.

LCR starts at 0%.

Price moves into the wick.

LCR increases.

LCR reaches 42%.

A new Bullish Pivot B is confirmed.

Pivot A becomes Frozen.

Pivot A stores:

- Frozen LCR = 42%

Pivot B becomes Active.

Pivot B starts with LCR = 0%.

---

## Example 2

Pivot B reaches 83%.

Price continues.

Pivot B reaches exactly 100%.

Pivot B becomes Consumed.

Its label disappears.

The indicator searches for the latest Frozen Pivot.

Pivot A is found.

Pivot A becomes Active again.

Its LCR resumes from 42%.

The calculation continues normally.

---

## Example 3

Pivot A finally reaches 100%.

Pivot A becomes Consumed.

Its label disappears.

No Frozen Pivot exists anymore.

The indicator waits for the next confirmed pivot.

---

## Example 4

A pivot reaches 100%.

This pivot is permanently finished.

It can never become Active again.

Its stored values remain available only for historical purposes.

---

## Example 5

A Frozen Pivot never loses its stored LCR.

It always resumes from its last frozen value.

It never restarts from zero.

---

# 38. FINAL PRINCIPLES

LCR is a measurement instrument.

It is NOT a trading strategy.

It is NOT a prediction tool.

It does NOT generate signals.

It does NOT identify entries.

It does NOT identify exits.

It does NOT identify trend direction.

Its only responsibility is to measure liquidity consumption inside the wick of confirmed pivots.

The trader remains entirely responsible for interpreting the information provided by LCR.

---

# 39. DEFINITION OF DONE

The project is considered complete only if:

- The script compiles without errors.
- No repaint exists.
- The LCR is always between 0% and 100%.
- Frozen pivots resume correctly.
- Consumed pivots never reactivate.
- Only one Active Pivot exists.
- Every pivot owns exactly one LCR.
- The indicator respects every rule defined in this specification.

No additional functionality shall be added unless the specification itself is updated.
