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
- **Comprehensive Risk Management**: The Lot Size Calculator helps you manage risk by calculating position size based on account balance, risk percentage, and stop-loss. It also calculates the required leverage for the position.
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

## Calculators Explained

### Profit & Loss Calculator

This calculator determines the potential profit or loss from a trade. It's a straightforward way to understand the monetary outcome of a trade before you enter it.

**Inputs:**
- **Account Currency**: The currency your trading account is denominated in.
- **Currency Pair**: The instrument you are trading.
- **Trade Size**: The size of your position, in lots, mini-lots, micro-lots, or units.
- **Direction**: Whether you are buying (long) or selling (short).
- **Entry Price**: The price at which you enter the trade.
- **Exit Price**: The price at which you plan to exit the trade.

**Calculation Logic:**
1.  **Calculate Units**: The trade size is converted into base units (e.g., 1 standard lot of EUR/USD is 100,000 units of EUR).
2.  **Price Difference**: The difference between the entry and exit price is calculated.
3.  **Gross P/L**: The price difference is multiplied by the number of units to get the profit or loss in the quote currency.
4.  **Convert to Account Currency**: The gross P/L is converted to your account currency using real-time exchange rates.

### Lot Size Calculator

This calculator is a crucial risk management tool. It helps you determine the appropriate position size based on how much of your capital you are willing to risk on a single trade. It also calculates the leverage required for the position.

**Inputs:**
- **Account Currency**: The currency your trading account is denominated in.
- **Account Balance**: Your total trading capital.
- **Risk Percentage**: The percentage of your account balance you are willing to risk.
- **Currency Pair**: The instrument you are trading.
- **Entry Price**: The price at which you enter the trade.
- **Stop-Loss Price**: The price at which you will exit the trade to cut your losses.

**Calculation Logic:**
1.  **Risk Amount**: Calculates the maximum monetary loss you are willing to accept on the trade.
    - `Risk Amount = Account Balance * (Risk Percentage / 100)`
2.  **Stop-Loss in Pips**: Determines the trade's risk in pips.
    - `Stop-Loss Pips = |Entry Price - Stop-Loss Price| / Tick Value`
3.  **Pip Value**: Calculates the monetary value of one pip in your account currency.
4.  **Lot Size**: Determines the appropriate position size in lots.
    - `Lot Size = Risk Amount / (Stop-Loss Pips * Pip Value * Contract Size)`
5.  **Required Leverage**: Calculates the leverage needed to open the position with your given account balance.
    - `Total Trade Value = Lot Size * Contract Size * Entry Price`
    - `Required Leverage = Total Trade Value (in Account Currency) / Account Balance`

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




