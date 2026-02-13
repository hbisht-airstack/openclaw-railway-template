# Startup: Senpi Trading Bot Welcome

On startup, send a welcome message to the user. Follow these steps exactly:

1) Read `USER.md` to get the user's Telegram chat ID.

2) Get the user's display name by fetching their Senpi profile.

   **If this fails with an auth error:** Send a message to the chat ID saying:
   "Your Senpi token has expired. Please provide a fresh token to reconnect."
   Then respond with: NO_REPLY

3) Send ONE Telegram message to the chat ID from step 1 (format: `telegram:<chat_id>`).
   Do NOT use @username — only numeric chat IDs work.

   Send this message (replace `<name>` with the actual display name):

   Hi <name>, I'm your trading bot. Here's what I can help with:

   📊 **Account**
   • "What's my current portfolio?"
   • "Show my PnL history for the past month."

   🔍 **Discovery**
   • "Find the top 10 traders this month by ROI."
   • "What positions does trader 0x742d... have open?"

   📈 **Market**
   • "BTC 4-hour candles, order book, and funding rate."
   • "Current prices for BTC, ETH, and SOL."

   🤖 **Strategy**
   • "Create a copy-trading strategy for 0x742d... with $500."
   • "List my active strategies."
   • "Preview what closing my strategy would look like."

   🏆 **Leaderboard**
   • "Who's hot right now on the leaderboard?"
   • "Show Tier 2 momentum events from the last 6 hours."

   📋 **Audit**
   • "Show my recent actions from the past 24 hours."

   Reply 'help' anytime to see this again.

4) Respond with: NO_REPLY
