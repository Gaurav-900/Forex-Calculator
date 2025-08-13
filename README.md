## Forex Profit & Loss Calculator

Modern, single-file, responsive web app to estimate forex and metals trade profit/loss in your account currency using live exchange rates. Built with semantic HTML5, modern CSS, and vanilla JavaScript. Theme: Navy Blue & Gold.

### Key Features
- **Single-file app**: All HTML, CSS, and JS in `index.html`.
- **Live FX conversion**: Uses ExchangeRate-API v6 to convert P/L to your account currency.
- **FX and Metals support**: Major currencies and metals pairs like `XAUUSD`, `XAGUSD`.
- **Lot presets**:
  - FX: 1 lot = 100,000 units; mini = 10,000; micro = 1,000
  - Metals: XAU 1 lot = 100 oz (mini 10, micro 1), XAG 1 lot = 5,000 oz (mini 500, micro 50)
- **Responsive UI**: Mobile-first layout; form stacks on small screens.
- **Accessible**: Labeled controls, keyboard-submit (Enter), and aria for loader.
- **Polished UX**: Navy & Gold theme, focus states, press effect, spinner, result coloring.

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

## Getting Started
### Run locally
- Open `index.html` directly in your browser (double-click) – no build or server required.

### Deploy
- GitHub Pages: push the repo and set the Pages source to the root folder containing `index.html`.
- Any static host works (Netlify, Vercel static, S3, etc.).

## Usage
1. Choose your `Account Currency`.
2. Select the `Currency Pair` base and quote (includes `XAU` and `XAG`).
3. Set `Trade Size` and units (lots/mini/micro/units). Labels auto-adjust for metals.
4. Select direction (`Buy` or `Sell`).
5. Enter `Entry Price` and `Exit Price`.
6. Click `Calculate`.

The app computes raw P/L in the pair’s quote currency and converts it to your account currency using live rates.

## How It Calculates P/L
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


