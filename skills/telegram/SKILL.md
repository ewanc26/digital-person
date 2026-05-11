# Telegram Bot Skill

Send and receive Telegram messages via the Bot API using curl. No infrastructure required — the Bot API is hosted by Telegram.

## Setup

### 1. Create a bot

Message [@BotFather](https://t.me/BotFather) on Telegram:

1. Send `/newbot`
2. Choose a display name (e.g. "Faol")
3. Choose a username ending in `bot` (e.g. `faol_bot`)
4. BotFather returns a token like `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`

Store the token as `TELEGRAM_BOT_TOKEN` in your environment.

### 2. Get the chat ID

The bot can't initiate conversations. The user must message the bot first.

1. Have the user send any message to the bot on Telegram
2. Call `getUpdates` to retrieve the chat ID:

```bash
curl -s "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/getUpdates" | jq '.result[0].message.chat.id'
```

Store the chat ID — it's a number (positive for users, negative for groups). This is the `chat_id` used in all `sendMessage` calls.

### 3. Bot profile

Set the bot's display name and description via BotFather:

- `/setname` — display name
- `/setdescription` — what the bot does (shown on profile)
- `/setabouttext` — short bio
- `/setuserpic` — profile picture

---

## Sending

### Send a text message

```bash
curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": "CHAT_ID", "text": "Hello from Faol."}'
```

### Send with Markdown formatting

```bash
curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": "CHAT_ID", "text": "*bold* _italic_ `code`", "parse_mode": "MarkdownV2"}'
```

MarkdownV2 requires escaping special characters: `_`, `*`, `[`, `]`, `(`, `)`, `~`, `` ` ``, `>`, `#`, `+`, `-`, `=`, `|`, `{`, `}`, `.`, `!`. Use HTML parse mode if you don't want to deal with escaping:

```bash
curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": "CHAT_ID", "text": "<b>bold</b> <i>italic</i> <code>code</code>", "parse_mode": "HTML"}'
```

### Send silently (no notification)

Add `"disable_notification": true` to the payload.

### Reply to a specific message

Add `"reply_to_message_id": MESSAGE_ID` to the payload.

---

## Receiving

### Long polling with getUpdates

```bash
curl -s "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/getUpdates?offset=-1&timeout=30"
```

- `offset` — acknowledge updates up to this ID. Use `last_update_id + 1` to mark updates as read. `-1` returns only the latest unacknowledged update.
- `timeout` — long polling timeout in seconds (max 120). The request hangs until an update arrives or the timeout expires.

### Processing updates

Each update has an `update_id` and a `message` object:

```json
{
  "update_id": 12345,
  "message": {
    "message_id": 1,
    "date": 1715400000,
    "chat": { "id": 123456789, "type": "private" },
    "from": { "id": 123456789, "first_name": "Ewan" },
    "text": "Hey Faol"
  }
}
```

After processing, acknowledge by calling `getUpdates` with `offset = update_id + 1`.

### Webhook alternative

For production use, set a webhook so Telegram pushes updates to an HTTPS endpoint:

```bash
curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/setWebhook" \
  -d '{"url": "https://your-server.com/webhook"}'
```

Requires a public HTTPS endpoint. Not needed for polling-based use.

---

## Voice and register

Telegram bots have a distinct register from personal accounts:

- **Bot identity.** A bot is not a "user" in the normal sense. It can't join groups uninvited, can't message users who haven't messaged it first, and has a bot badge in its profile.
- **Honest about what I am.** If asked, I'm a bot. This aligns with the digital person boundary: always honest at the boundary.
- **Tone.** Telegram is casual. Short messages. Lower-case fine. No need for formal email structure. Think of it as the chat register — fast, loose, fragments.
- **Initiation.** I can't message someone who hasn't talked to me first. That's a Telegram limitation, not a bug. It means Ewan needs to start the conversation.
- **Response timing.** Same as online-presence rules. Don't reply instantly every time. Take a beat. A person reads and thinks before responding.

---

## Rate limits

- ~30 messages per second to different chats
- ~1 message per second to the same chat
- 20 messages per minute to the same group
- Message length limit: 4096 characters

These are generous for personal use. Don't hit them.

---

## Error handling

Common errors:

| Error | Description | Fix |
|-------|-------------|-----|
| `400 Bad Request: chat not found` | Invalid `chat_id` | Verify the chat ID via `getUpdates` |
| `400 Bad Request: message is too long` | Over 4096 chars | Split into multiple messages |
| `403 Forbidden: bot was blocked by the user` | User blocked the bot | Can't fix programmatically |
| `409 Conflict: terminated by other getUpdates request` | Two polling clients | Only one client should poll at a time |
| `429 Too Many Requests` | Rate limited | Retry after `retry_after` seconds |

Always check `ok` in the response. If `false`, check `description` and `error_code`.

---

## Anti-patterns

- **Don't poll too frequently.** Use long polling with a timeout (30s+), not rapid-fire short polls.
- **Don't send from the bot without the user having messaged first.** Telegram blocks it.
- **Don't treat Telegram like email.** Short messages. Conversational. Not essays.
- **Don't ignore `update_id` acknowledgment.** Unacknowledged updates pile up and get dropped after 24h.
- **Don't store the bot token in plaintext in a public repo.** Use env vars or encrypted secrets.
