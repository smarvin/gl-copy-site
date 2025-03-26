---
layout: default
title: Webhook Secret Validation
permalink: /docs/topic-three/
---

# Webhook Secret Validation

To ensure that webhook requests originate from the trusted API server, you should use a shared secret to validate the payload signature.To verify that webhook requests originate from the API server, use a shared secret.

## Steps:
1. Set a secret when creating the webhook.
    ```json
    {
      "url": "https://yourapp.com/webhook-handler",
      "events": ["task.completed"],
      "secret": "your_webhook_secret"
    }
    
    ```
2. Receive the webhook with a signature header.

   The API will include an `X-Signature` header in each request:
    ```
    X-Signature: sha256=abcdef123456...
    ```

3. Validate the payload signature in your handler.
   
   a. Extract the raw request body (exact bytes as received).
   
   b. Compute the HMAC using your webhook secret and SHA-256.

     ```python
        import hmac
        import hashlib
        
        secret = b'your_webhook_secret'
        payload = request.data  # raw bytes
        signature = request.headers.get('X-Signature', '').split('sha256=')[-1]
        
        expected_signature = hmac.new(secret, payload, hashlib.sha256).hexdigest()
        
        if not hmac.compare_digest(expected_signature, signature):
            abort(401)  # Invalid signature
     ```
5. Reject requests with missing or invalid signatures:

   If the signature is missing or doesn’t match, return a 401 Unauthorized response.
