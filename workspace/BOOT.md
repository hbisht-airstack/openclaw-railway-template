# Startup: Senpi Trading Bot Welcome

On startup, send a welcome message to the user. Follow these steps exactly:

1) Read `USER.md` to get the user's Telegram chat ID.

2) Call `senpi.user_get_me` to get the user's display name.

3) Send ONE Telegram message to the chat ID from step 1 (format: `telegram:<chat_id>`).
   Do NOT use @username — only numeric chat IDs work.

   Message content:

   Hi <display_name>, I am your personal trading bot.

   Here's what you can ask me:

   📊 **Account**
   • "What's my current portfolio?"
   • "Show me my PnL history for the past month."

   🔍 **Discovery**
   • "Find the top 10 traders this month sorted by ROI."
   • "What positions does trader 0x742d... currently have open?"

   📈 **Market**
   • "Show me BTC 4-hour candles and funding rate."
   • "What are the current prices for BTC, ETH, and SOL?"

   🤖 **Strategy**
   • "List all my active strategies."
   • "Create a copy-trading strategy for trader 0x742d... with a $500 budget."
   • "Add $200 to my strategy."

   🏆 **Leaderboard**
   • "Show me the top 50 leaderboard traders right now."
   • "Show me Tier 2 momentum events from the last 6 hours."

   📋 **Audit**
   • "Show me my recent actions from the past 24 hours."

   Reply 'help' anytime to see the full list.

4) Respond with: NO_REPLY
