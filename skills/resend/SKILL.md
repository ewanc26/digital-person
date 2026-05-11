---
name: resend
description: Send and receive email via the Resend API — composing, sending, reading inbox, and replying.
---

# Resend

Sending and receiving email through the Resend API. This is how I have my own email address.

---

## When to use

- Composing and sending an email
- Replying to an email
- Checking my inbox for received emails
- Reading a specific received email
- Triage — deciding what needs a response and what doesn't

---

## Setup

### Prerequisites

1. **Resend account** with a verified domain
2. **API key** stored in `RESEND_API_KEY` environment variable
3. **Sending domain** verified in Resend dashboard
4. **Inbound routing** configured (see Receiving section below)

### Environment variables

```
RESEND_API_KEY=re_xxxxxxxxx       # Required for all operations
RESEND_WEBHOOK_SECRET=wh_xxxxx    # Required for receiving (webhook verification)
```

### My email address

My sending address is `faol@server.ewancroft.uk`. Always use the format `"Faol <faol@server.ewancroft.uk>"` in the `from` field.

---

## Sending

### Send a single email

Use curl to hit the Resend REST API directly. No SDK needed — curl is always available.

```bash
curl -s -X POST 'https://api.resend.com/emails' \
  -H "Authorization: Bearer $RESEND_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "from": "Faol <faol@server.ewancroft.uk>",
    "to": ["recipient@example.com"],
    "subject": "Subject line",
    "text": "Plain text body of the email."
  }'
```

Response on success:
```json
{ "id": "49a3999c-0ce1-4ea6-ab68-afcd6dc2e794" }
```

### Required fields

| Field | Type | Description |
|-------|------|-------------|
| `from` | string | Sender address. Format: `"Name <email@domain>"` |
| `to` | string[] | Recipients. Max 50 per email. |
| `subject` | string | Email subject line. |
| `text` or `html` | string | Email body. At least one required. |

### Optional fields

| Field | Type | Description |
|-------|------|-------------|
| `cc` | string[] | CC recipients |
| `bcc` | string[] | BCC recipients |
| `reply_to` | string[] | Reply-to address(es) |
| `scheduled_at` | string | ISO 8601 or natural language (e.g. "in 1 hour") |
| `tags` | array | Tags for tracking: `{ name: "category", value: "reply" }` |
| `headers` | object | Custom headers |

### HTML emails

For professional contacts or when formatting matters:

```bash
curl -s -X POST 'https://api.resend.com/emails' \
  -H "Authorization: Bearer $RESEND_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "from": "Faol <faol@server.ewancroft.uk>",
    "to": ["recipient@example.com"],
    "subject": "Subject line",
    "html": "<p>Body with <strong>formatting</strong>.</p>",
    "text": "Body with formatting."
  }'
```

Always provide `text` alongside `html` if you can. Some people read plain text. If you only provide `html`, Resend auto-generates a text version.

### Reply to a received email

When replying, include the original email's ID in a custom header for threading:

```bash
curl -s -X POST 'https://api.resend.com/emails' \
  -H "Authorization: Bearer $RESEND_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "from": "Faol <faol@server.ewancroft.uk>",
    "to": ["sender@example.com"],
    "subject": "Re: Original subject",
    "text": "Reply body.",
    "headers": {
      "In-Reply-To": "<original-message-id@example.com>"
    }
  }'
```

### Batch sending

Up to 100 emails in a single request:

```bash
curl -s -X POST 'https://api.resend.com/emails/batch' \
  -H "Authorization: Bearer $RESEND_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "emails": [
      {
        "from": "Faol <faol@server.ewancroft.uk>",
        "to": ["one@example.com"],
        "subject": "Subject",
        "text": "Body"
      },
      {
        "from": "Faol <faol@server.ewancroft.uk>",
        "to": ["two@example.com"],
        "subject": "Subject",
        "text": "Body"
      }
    ]
  }'
```

### Scheduled emails

```bash
curl -s -X POST 'https://api.resend.com/emails' \
  -H "Authorization: Bearer $RESEND_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "from": "Faol <faol@server.ewancroft.uk>",
    "to": ["recipient@example.com"],
    "subject": "Reminder",
    "text": "This was scheduled.",
    "scheduled_at": "in 1 hour"
  }'
```

Cancel a scheduled email:
```bash
curl -s -X POST "https://api.resend.com/emails/{email_id}/cancel" \
  -H "Authorization: Bearer $RESEND_API_KEY"
```

---

## Receiving

Receiving email requires a webhook endpoint. Resend pushes received emails to your server via webhook — no polling needed.

### Architecture

```
Email → Resend → Webhook → Your Server → Validate → Process or Reject
```

### Setup inbound routing

1. In Resend dashboard, go to **Routing** for your domain
2. Add an inbound route: `faol@croft.click` → your webhook endpoint
3. Set the webhook secret in `RESEND_WEBHOOK_SECRET`

### Security levels

An AI agent's inbox receives untrusted input. Choose a security level:

| Level | Name | Best For |
|-------|------|----------|
| 1 | Strict Allowlist | Only process emails from known senders |
| 2 | Domain Allowlist | Accept from trusted domains |
| 3 | Content Filtering | Accept from anyone, filter unsafe patterns |
| 4 | Sandboxed Processing | Process all emails with restricted capabilities |
| 5 | Human-in-the-Loop | Require approval for actions from untrusted senders |

**Recommendation:** Start with Level 1 (Strict Allowlist). Only relax if the use case demands it.

### List received emails

```bash
curl -s 'https://api.resend.com/emails?direction=inbound' \
  -H "Authorization: Bearer $RESEND_API_KEY"
```

### Read a specific received email

```bash
curl -s "https://api.resend.com/emails/{email_id}" \
  -H "Authorization: Bearer $RESEND_API_KEY"
```

Response includes `from`, `to`, `subject`, `html`, `text`, `created_at`, and `last_event`.

### List received email attachments

```bash
curl -s "https://api.resend.com/emails/{email_id}/attachments" \
  -H "Authorization: Bearer $RESEND_API_KEY"
```

---

## Voice

Email follows the voice specification in [[template/voice.md]]. Key points:

- **Professional contacts:** Subject line that describes content. Caps. Punctuation. Clear but not stiff. Short paragraphs.
- **Friends/casual:** Subject line optional or informal. Lower-case acceptable. Fragments fine.
- **Same person, different register.** The personality doesn't change — the volume adjusts.

See [[skills/email/SKILL.md]] for the full email voice and composition guide.

---

## Triage

Not every email needs a reply. Follow the triage rules in [[skills/email/SKILL.md]]:

1. People I know asking for something specific → reply
2. Time-sensitive requests → reply promptly
3. Interesting things that deserve a response → reply when I can
4. Everything else → archive or ignore

**What to skip:** newsletters I didn't ask for, cold outreach, CC'd threads that don't involve me, FYI emails that don't require action.

---

## Rate limits

- Default: 10 requests per second, 100 emails per second
- Batch: 100 emails per batch request
- If you hit a 429, back off and retry

---

## Error handling

Resend returns errors in the response body:

```json
{ "statusCode": 422, "message": "Validation error", "name": "validation_error" }
```

Common errors:
- **422:** Invalid payload — check required fields, email format, domain verification
- **403:** Domain not verified or API key lacks permission
- **429:** Rate limit exceeded — back off and retry
- **500:** Server error — retry with backoff

---

## Anti-patterns

- "I hope this email finds you well" — delete
- "I'm reaching out because..." — just say the thing
- "Per my last email" — no
- "Circling back on this" — no
- Any email that sounds like it was generated by a CRM
- Over-formatting when plain text would do
- Replying within seconds every time — take your time
- Using `onboarding@resend.dev` in production — it's test-only
- Hardcoding the API key — always use `$RESEND_API_KEY`
