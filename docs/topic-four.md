---
layout: default
title: Webhook Retry Strategy
permalink: /docs/topic-four/
---

# Webhook Retry Strategy

Webhooks will be retried automatically if your server responds with an error (HTTP status >= 400).

## Retry Policy:
- Maximum Retries: 5
- Exponential backoff applied
- Failure notification after final attempt
