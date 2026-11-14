# Setup

## 1. Google Sheet
- Tab `Orders`, row 1: `order_id, token, placed_at, placed_hour_ist, customer, items, total, status, ready_at`
- Tab `Menu`: import `data/menu.csv`
- Note the Sheet ID (between `/d/` and `/edit` in the URL).

## 2. Telegram
- @BotFather: `/newbot`, save the token. Keep it only in n8n credentials.
- Create a **private staff group** and add the bot (make it admin so it can see messages).
- Create a **public channel** for the ready board and add the bot as an admin with Post Messages on.
- Get chat IDs: send a message, then open `https://api.telegram.org/bot<TOKEN>/getUpdates`. Groups are negative numbers, channels start with `-100`. Use the newest ID if a group is upgraded to a supergroup.

## 3. n8n
1. Import `workflow/bluedove_workflow.json`.
2. Attach Google Sheets credentials to Log Order, Mark Ready In Sheet and Read Orders. Select your document and the `Orders` sheet.
3. Attach the Telegram credential to the three Telegram nodes.
4. Replace placeholders: `YOUR_GOOGLE_SHEET_ID`, `YOUR_STAFF_CHAT_ID` (Notify Kitchen and Send Summary), `YOUR_READY_BOARD_CHANNEL_ID` (Post To Ready Board), and `YOUR-N8N-URL` in the **Build Order** code node.
5. On **Log Order** set Mapping Column Mode to Map Automatically. On **Mark Ready In Sheet** keep only `order_id`, `status` and `ready_at`.
6. Publish.

## 4. Test
```
curl -X POST https://YOUR-N8N-URL/webhook/bluedove-order \
  -H "Content-Type: application/json" \
  -d '{"customer":"Test","items":[{"id":"B01","name":"Veg Burger","qty":1,"price":89}]}'
```
Expect a token in the response, a new row in `Orders`, and a message in the staff chat. Tap the ready link: status becomes `ready`, `ready_at` fills in, and the channel receives the token.

## 5. Order page
- In `web/index.html` set `ORDER_URL` to your production webhook, and `TELEGRAM_URL` to the channel invite link.
- Host on any static host (Netlify Drop, GitHub Pages) and generate a QR code from the link.

## 6. Troubleshooting
Open **Executions** in n8n, click the failed run and read the error on the red node.
