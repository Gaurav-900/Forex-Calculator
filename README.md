# Forex Trading Calculators

This project contains two essential forex trading calculators in a single, responsive web application:
1.  **Profit & Loss Calculator**: Estimate the P/L for a trade in your account currency.
2.  **Lot Size Calculator**: Determine the appropriate position size based on your risk tolerance.

Built with semantic HTML5, modern CSS, and vanilla JavaScript, this single-file application is easy to use and modify. It features a clean, navy and gold themed UI that is both desktop and mobile-friendly.

### Key Features
- **Dual Calculators**: Profit & Loss and Lot Size calculators in one interface.
- **Live FX Conversion**: Uses the ExchangeRate-API v6 to provide real-time exchange rates for accurate calculations.
- **Broad Instrument Support**: Works with major forex pairs and commodities like Gold (XAU) and Silver (XAG).
- **Flexible Unit Sizing**: Supports standard, mini, and micro lots, as well as custom unit sizes.
- **Risk Management**: The Lot Size Calculator helps you manage risk by calculating position size based on account balance, risk percentage, and stop-loss.
- **Responsive Design**: A clean, modern interface that adapts to any screen size.
- **Single-File Application**: All HTML, CSS, and JavaScript are contained in a single `index.html` file for simplicity.

## Try
- This Website is Deployed using Netlify
- **Checkout**: https://gs-forex-calculator.netlify.app/

## Tech Stack
- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Fonts**: Google Fonts `Inter`
- **API**: ExchangeRate-API v6 (open/latest endpoint)

## Project Structure
```
Forex calculatoe/
  ├─ index.html      # Single-file application
  └─ README.md       # This file
```

## Usage
1. Choose your `Account Currency`.
2. Select the `Currency Pair` base and quote (includes `XAU` and `XAG`).
3. Set `Trade Size` and units (lots/mini/micro/units). Labels auto-adjust for metals.
4. Select direction (`Buy` or `Sell`).
5. Enter `Entry Price` and `Exit Price`.
6. Click `Calculate`.

The app computes raw P/L in the pair’s quote currency and converts it to your account currency using live rates.

## How It Works

### Profit & Loss Calculator

This calculator determines the profit or loss from a trade based on the entry and exit prices, trade size, and direction.
Let:
- `base` = base instrument (e.g., EUR or XAU)
- `quote` = quote currency (e.g., USD)
- `units` = position size in base units
- `entry` = entry price (quote per base)
- `exit` = exit price (quote per base)
- `direction` ∈ {buy, sell}

Steps:
- Units from size and type:
  - FX: lots×100000, mini×10000, micro×1000, units=units
  - XAU: lots×100, mini×10, micro×1
  - XAG: lots×5000, mini×500, micro×50
- Price difference in quote currency:
  - If `buy`: `diff = exit - entry`
  - If `sell`: `diff = entry - exit`
- Raw P/L in quote currency: `pnlQuote = diff × units`
- If `accountCurrency === quote`: `pnlAccount = pnlQuote`
- Else convert with live rate `quote -> account`: `pnlAccount = pnlQuote × rate(quote→account)`

Notes:
- This calculator uses price difference × units instead of pip abstractions to naturally support both FX and metals.
- Precision is handled by standard JavaScript number arithmetic and formatted to two decimals for display.

### Lot Size Calculator

This calculator helps you determine the appropriate position size (in lots) based on your desired risk parameters.

Let:
- `accountBalance` = Your total account equity
- `riskPercentage` = The percentage of your account you are willing to risk (e.g., 1%)
- `entryPrice` = The price at which you enter the trade
- `stopLossPrice` = The price at which you will exit the trade if it moves against you
- `accountCurrency` = The currency of your trading account
- `baseCcy` = The base currency of the pair being traded (e.g., EUR)
- `quoteCcy` = The quote currency of the pair being traded (e.g., USD)

Steps:
1.  **Calculate Risk Amount**: The total amount you are willing to risk in your account currency.
    - `riskAmount = accountBalance * (riskPercentage / 100)`
2.  **Determine Contract Size**: The number of units in one standard lot for the traded instrument.
    - Forex: 100,000 units
    - Gold (XAU): 100 troy ounces
    - Silver (XAG): 5,000 troy ounces
3.  **Calculate Stop-Loss in Pips**: The distance between your entry and stop-loss price.
    - `stopLossPips = |entryPrice - stopLossPrice| / tickValue`
4.  **Calculate Pip Value**: The value of one pip in your account currency.
    - `pipValue = tickValue * conversionRate`
5.  **Calculate Lot Size**: The final position size in lots.
    - `lotSize = riskAmount / (stopLossPips * pipValue * contractSize)`

## Exchange Rates API
- Default endpoint used by the app (no API key required):
  - `https://open.er-api.com/v6/latest/{BASE}`
  - The app requests with `{BASE} = quote currency` to obtain the conversion rate to the selected account currency.

Example request used when the quote is USD:
```bash
GET https://open.er-api.com/v6/latest/USD
```

Response fields used:
- `result` should be `"success"`
- `rates[ACCOUNT_CCY]` provides `1 QUOTE = rate ACCOUNT_CCY`

### Optional: Using your personal v6 API key
If you prefer to use a key-based v6 endpoint, replace the fetch URL in `index.html` accordingly:
```javascript
// Current (open endpoint)
const url = `https://open.er-api.com/v6/latest/${encodeURIComponent(quote)}`;

// Optional (key-based)
// const API_KEY = 'YOUR_API_KEY_HERE';
// const url = `https://v6.exchangerate-api.com/v6/${API_KEY}/latest/${encodeURIComponent(quote)}`;
```

Please observe the provider’s usage limits and terms.

## Accessibility & UX
- Visible focus styles for inputs/selects.
- Loader with `aria-busy` and live text.
- Result coloring: profit (green), loss (red), neutral (default).

## Known Limitations / Future Work
- Add more pairs and commodities by default.
- Persist last-used settings in `localStorage`.
- Better handling of instruments quoted with fewer decimals.

## Contributing
Issues and pull requests are welcome. For larger changes, open an issue to discuss your proposal first.

## License (MIT)
Copyright (c) 2025 Gaurav Sharma

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


