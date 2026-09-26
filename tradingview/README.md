# Sn1P3r Sideway and Zone (TradingView · Pine Script v6)

File: [`Sn1P3r_Sideway_and_Zone.pine`](Sn1P3r_Sideway_and_Zone.pine)

## How to test it on TradingView

1. Open a chart (built for **XAUUSD 5m · OANDA**, but works on any intraday symbol).
2. **Pine Editor** → **Open** → **New blank indicator** → select everything and delete it.
3. Paste the whole content of `Sn1P3r_Sideway_and_Zone.pine` → **Save** → **Add to chart**.
4. Open the indicator **Settings** to see the inputs (🧩 MODULES, 📐 ZONE ·…, 📊 GRID ·…).

## What is on the chart

| Element | What it is |
|---|---|
| Grey box + 2 grey lines | Confirmed sideways structure and its high/low levels. The box keeps growing until price breaks out. A wick outside the range without a confirmed break widens it, and the zones move with it. The latest structure extends right, and older ones stop where the next one starts. A new structure can only start after the previous one has broken out. |
| `BUY ZONE n` / `SELL ZONE n` | Trading-zone ladder built from the structure (see the formula below). |
| Yellow ▲ `⚡ 50` | ADX crossed the peak level during working hours. |
| `SCALP OK` | Zone touched during working hours, with ADX flat and ADR spent. |
| `1/3ADR±`, `5ADR±` | Day open ± ADR/3 and ± ADR, drawn from the day start to the last bar + offset. |
| Top-right table | Status, ADX, Session, 5ADR, Today moved, Used (bar), Left to ADR, Grid rule, Read. |
| Bottom-centre table | Previous daily ranges (Day -1 on the left; hover a cell to see which day), then `5ADR = …`. The ranges are recorded from the chart's own bars. If the history is short (e.g. 1m), missing days show `—` and the ADR averages the days available. |
| Teal and orange vertical lines, `GRID ZONE` | GRID ON and GRID OFF, with the planned grid window as a box. |
| Grey background | Outside working hours (06:00–17:00 UTC+7). |

## Zone ladder formula

`R = structure high − structure low`, `S = Zone spacing (× range)`, `H = Added-zone height (× range)`

- BUY ZONE 2k−1: top = high − S·k·R, bottom = top − H·R
- SELL ZONE 2k: bottom = high + S·k·R, top = bottom + H·R

With the defaults (S = 2, H = 0.5), BUY 1 is 1R under the low, SELL 2 is 2R over the high,
BUY 3 is 3R under the low and SELL 4 is 4R over the high. When price confirms a break beyond the
outermost zone of a side, the next zone is added one spacing further out (BUY 5, SELL 6, …).

Checked against the original indicator on XAUUSD 5m and 1m: all 76 zone prices are identical,
including the auto-added BUY ZONE 5/7/9/11 and the last digit on half-tick values.
For example, with high 4281.970 and low 4271.135 (R = 10.835), the ladder gives
BUY ZONE 1 = 4254.883 - 4260.300, SELL ZONE 2 = 4303.640 - 4309.058 and
SELL ZONE 4 = 4325.310 - 4330.728.

## Grid logic

- **Grid rule** (dashboard row) is the ADR threshold only: today's range ≥ *Minimum % of 5ADR consumed* → `✅ open`, otherwise `❌ shut`.
- **GRID ON** requires the Grid rule to be open, the session to be open (and not in the final *No New Grid* minutes), and no danger breach when *Close an ACTIVE grid…* is enabled.
  With **GRID ON needs ADX peak → flat** (📊 GRID · ADX, default ON), it also waits until ADX has crossed the peak level (50) and then fallen below the flat level (30) within *Peak Expiry* minutes.
  With that option OFF, the grid opens as soon as the rule passes, at most once each time the conditions become true.
- **GRID OFF** happens when the grid window (90 min) ends, the session closes, the Grid rule shuts, or the danger level is hit (if enabled).

## Alerts

- **Any alert() function call**: uses the `Alert() · …` toggles in the settings. These messages include prices and the disclaimer.
- **Individual conditions**: GRID ON, GRID OFF, ADR gate opened, ADX peak, Sideways zone confirmed, Sideways breakout, Zone touched, Outer zone broken.

*Automated technical signals, not financial advice.*
