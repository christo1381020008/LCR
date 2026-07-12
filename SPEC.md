# LCR – Liquidity Consumption Ratio

Version : 1.0

Author : ChrisAvd

Status : SPECIFICATION FROZEN

Motto

> Measure first. Interpret later.

---

# 1. PROJECT DESCRIPTION

LCR (Liquidity Consumption Ratio) is a liquidity measurement tool.

It measures the percentage of wick consumption from a reference pivot until another pivot becomes the active reference or until the wick has been completely consumed.

LCR NEVER generates trading signals.

It NEVER gives Buy, Sell, Bullish, Bearish or Smart Money decisions.

Its only purpose is to objectively measure liquidity consumption.

---

# 2. PROJECT PHILOSOPHY

LCR is NOT an indicator.

LCR is a measurement instrument.

Its role is comparable to a thermometer.

It measures.

The trader interprets.

The indicator never makes trading decisions.

---

# 3. ABSOLUTE RULES

The following features are permanently forbidden.

• Buy signals

• Sell signals

• CHoCH detection

• MSS detection

• Order Blocks

• Fair Value Gaps

• BOS

• Market Structure

• AI prediction

• Trend prediction

LCR must always remain a measurement tool.

---

# 4. PIVOT DETECTION

The user chooses

• Pivot Timeframe

Examples

Daily

4H

1H

15m

etc.

The user also chooses

Pivot Left

Pivot Right

Example

Left = 3

Right = 3

A pivot is considered valid ONLY when TradingView confirms it.

No anticipation is allowed.

---

# 5. REFERENCE PIVOT

Only ONE reference pivot exists at any moment.

Whenever a newer pivot is confirmed

The newest pivot becomes the active reference.

The previous reference becomes frozen.

Its LCR value remains displayed.
