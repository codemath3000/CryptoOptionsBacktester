This is a simple backtester for options trading strategies. I originally designed it for crypto options, but it can also be used for other types of options.

It takes in a series of day-by-day positions on various products (both options and underlying), producing a PnL graph and Sharpe ratio. Options auto-exercise at expiry if appropriate.

Example output for constant +1 position of BTC-USD (underlying):

Sharpe ratio: 8.7188
![PnL graph](BTC-USD.png)

Example output for constant +1 position of BTC-USD June 2025 call option with 100k strike:

Sharpe ratio: 6.4083
![PnL graph](BTC-27JUN25-100000-C.png)

Input CSV format:
| Column name     | Required?            | Data type / format      | Example value          | Meaning & rules                                                                                                                                                   |
| --------------- | -------------------- | ----------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `date`          | **Yes**              | `YYYY-MM-DD` (ISO 8601) | `2025-05-05`           | Trading-day close to which the mark & position apply. One row per instrument per calendar day.                                                                    |
| `instrument_id` | **Yes**              | string                  | `BTC-27JUN25-100000-C` | Unique symbol for the *row itself* (spot or option). For options: `<UL>-<DDMMMYY>-<STRIKE>-<C∕P>`.                                                                |
| `type`          | **Yes**              | `S` ∕ `C` ∕ `P`         | `C`                    | Instrument class: **S**pot, **C**all, **P**ut.                                                                                                                    |
| `underlying`    | **Required for C/P** | string                  | `BTC-USD`              | Spot instrument\_id on which the option settles. Can be blank for spot rows.                                                                                      |
| `strike`        | **Required for C/P** | float                   | `100000`               | Strike price of the option. Leave blank (or `NaN`) for spot rows.                                                                                                 |
| `expiry`        | **Required for C/P** | `YYYY-MM-DD`            | `2025-06-27`           | Option expiry date. Blank / `NaT` for spot rows.                                                                                                                  |
| `price_bid`     | **Yes**              | float                   | `21500.25`             | Best bid *at the daily close* for this instrument.                                                                                                                |
| `price_ask`     | **Yes**              | float                   | `21560.75`             | Best ask *at the daily close* for this instrument.                                                                                                                |
| `position`      | **Yes**              | float (signed)          | `-2`                   | End-of-day holding *after* all trades have settled.<br> • **Positive** = long<br> • **Negative** = short<br>Units: contracts for options, units of coin for spot. |

Note that, for all options at a given date, there must be corresponding row(s) for the underlying asset(s) at that date.
