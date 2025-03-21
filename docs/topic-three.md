---
layout: default
title: Webhook Secret Validation
permalink: /docs/topic-three/
---

To verify that webhook requests originate from the API server, use a shared secret.

### Steps:
1. Set a secret when creating the webhook:
```json
{
  "url": "https://yourapp.com/webhook-handler",
  "events": ["task.completed"],
  "secret": "your_webhook_secret"
}
```

2. Every webhook payload includes an `X-Signature` header:
```
X-Signature: sha256=abcdef123456...
```

3. Compute the HMAC-SHA256 of the request body with your secret and compare it to the signature.
