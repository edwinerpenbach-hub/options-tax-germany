# Options Tax Tracker (Germany)

A desktop tool for German taxpayers who sell (write) stock options through Interactive Brokers or CapTrader. It calculates the tax corrections needed because **German tax law treats short options differently than US/IBKR reporting**.

## The Problem

Under German tax law (*Zuflussprinzip* / *Vereinnahmung der Stillhalterpr&auml;mie*), the premium from selling an option is taxable **when the position is opened**, not when it is closed. Interactive Brokers reports realized gains only when positions are closed, which does not match the German tax treatment.

This creates two discrepancies each tax year:

| Scenario | IBKR reports | German tax requires |
|---|---|---|
| Option sold in 2025, still open on Dec 31 | Nothing (not yet realized) | Taxable in 2025 |
| Option sold in 2024, closed in 2025 | Realized gain/loss in 2025 | Already taxed in 2024, correction needed |

This tool calculates both corrections so you can adjust the IBKR Activity Statement for your German tax return (*Anlage KAP*).

## Download

[**Download options_tracker.exe**](https://github.com/edwinerpenbach-hub/options-tax-germany/releases/latest/download/options_tracker.exe) (Windows, no installation required)

## Features

- **Tax Report Generator** -- Calculates Step 1 (add) and Step 2 (subtract) corrections per tax year, with EUR conversion
- **Portfolio Dashboard** -- Overview of open positions, premium, P/L, and upcoming expirations
- **Trade Management** -- Sell new options, close positions (FIFO), view all trades
- **Import/Export** -- Import trades from CSV (IBKR Activity Statement), export to CSV/Excel
- **ECB Exchange Rates** -- Download EUR/USD rates from the ECB and auto-fill missing rates
- **Dark-themed PyQt6 GUI** -- Modern desktop interface, standalone `.exe` -- no Python installation required

## Screenshot

![Screenshot](Screenshot.png)

## Usage

### 1. Export activity statements

For the initial import, export all Activity Statements from Interactive Brokers since opening your account, if possible. This ensures historical positions are correctly tracked for tax corrections.

**Note:** IBKR limits Activity Statement exports to a maximum of one year at a time. If you have trades spanning multiple years, export yearly and import them all in step 2.

### 2. Import your trades

- **From CSV**: Go to *Import CSV*, select your IBKR Activity Statement CSV export, and click *Import*. You can import multiple yearly exports to build your complete trade history.
- **Manual entry**: Use *Sell New Option* to add individual trades

### 3. Update EUR/USD rates

Go to *Update EUR Rates* and click *Download from ECB*. This fetches historical EUR/USD rates from the European Central Bank and updates all trades.

### 4. Generate tax report

Go to *Tax Report*, enter the tax year, and click *Generate Report*. The tool produces:

- **Step 1**: Options sold during the year that are still open at year-end. **Add** this amount to the IBKR realized figure.
- **Step 2**: Options opened in a prior year that were closed during this year. **Subtract** this amount (it was already taxed).

The report is also exported as `Steuer_YYYY.xlsx` with detailed breakdowns.

### 5. Apply to your tax return

```
Taxable amount = IBKR "Realized" (from Activity Statement)
                 + Step 1 total (EUR)
                 - Step 2 total (EUR)
```

Report this adjusted amount in your *Anlage KAP*.

### Close Unmatched Options

The *Close Unmatched Options* function finds expired options that have no matching closing trade. This can happen when:

- A ticker symbol changes (e.g., a company merger or rebranding)
- The underlying stock has a split or corporate action that changes the option name
- The closing trade in IBKR has a slightly different symbol format than the opening trade

When you run this function, it:
1. Finds all open positions where the expiry date has passed
2. Marks them as closed using the opening date as the close date, and the opening price as the close price
3. Exports the list of closed options to Excel for your records

This ensures your database accurately reflects that these positions are no longer open, even though the closing trade couldn't be automatically matched.

## Disclaimer

This tool is provided for informational and educational purposes only. It is **not tax advice**. The calculations are based on my understanding of German tax law for short options (*Stillhaltergesch&auml;fte*). Always verify the results with a qualified tax advisor (*Steuerberater*) before filing your tax return. The author assumes no liability for incorrect tax calculations.

## License

[MIT](LICENSE)

---

*Dieses Tool berechnet die Korrekturen f&uuml;r die deutsche Steuererkl&auml;rung bei Stillhaltergesch&auml;ften (verkaufte Optionen) &uuml;ber Interactive Brokers / CapTrader. Die Vereinnahmung der Stillhalterpr&auml;mie erfolgt nach deutschem Steuerrecht zum Zeitpunkt des Verkaufs, nicht bei Glattstellung -- daher weicht die IBKR-Abrechnung von der steuerlichen Behandlung ab.*
