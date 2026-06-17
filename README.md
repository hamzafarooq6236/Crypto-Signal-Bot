# Crypto Workflow 

An n8n workflow that scans Binance markets, filters liquid USDT pairs, calculates RSI on 15-minute candles, and sends Telegram alerts when a symbol meets the alert condition.

## What It Does

1. Manually starts the workflow.
2. Fetches 24-hour ticker data from Binance.
3. Filters for USDT trading pairs with quote volume above $50M.
4. Loops through each candidate symbol.
5. Requests 15-minute candlestick data.
6. Calculates RSI(14) and the latest 15-minute price change.
7. Sends a Telegram message when RSI is below 50.

## Workflow Logic

- **Data source:** Binance public API
- **Filter rule:** `USDT` pairs only, excluding leveraged tokens that contain `UP` or `DOWN`
- **Volume threshold:** `50,000,000` quote volume
- **Signal condition:** RSI(14) less than `50`
- **Notification channel:** Telegram

## Nodes

- **When clicking `Execute workflow`** - manual trigger
- **HTTP Request** - fetches Binance 24hr ticker data
- **filter usdt pairs** - keeps only high-volume USDT pairs
- **Loop Over Items** - processes symbols one by one
- **HTTP Request1** - fetches 15m klines for each symbol
- **Code in JavaScript1** - packages candles for RSI calculation
- **Code in JavaScript** - calculates RSI and 15m change
- **If** - checks whether RSI is below 50
- **Send a text message** - sends the Telegram alert

## Requirements

- An active [n8n](https://n8n.io/) instance
- Binance API access to public market endpoints
- Telegram bot credentials configured in n8n
- A valid Telegram chat ID

## Setup

1. Import the workflow JSON into n8n.
2. Set your Telegram credentials in the Telegram node.
3. Replace `YOUR_TELEGRAM_CHAT_ID` with your chat ID.
4. Confirm the Binance endpoints are reachable from your n8n environment.
5. Run the workflow manually to test the alert path.

## Customization

You can adjust these values in the code nodes:

- `MIN_VOLUME` - raise or lower the liquidity threshold
- RSI period - change the `RSI(14)` calculation
- Alert threshold - change the `If` node condition from `50` to your preferred value
- Telegram message text - edit the message template in the Telegram node

## Example Alert

```text
📊 Crypto Signal Alert

Symbol: BTCUSDT
RSI (14): 42.18
15m Change: -1.24 %
```

## Notes

- The workflow currently uses a manual trigger, so it runs only when started in n8n.
- The Telegram node still needs a real chat ID before it can send messages.
- The workflow is configured with `alwaysOutputData` on the RSI and klines steps to keep the loop moving even when some items do not produce output.

## License

Add your preferred license here.
