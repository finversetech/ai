---
name: finverse-online-payment
description: Implements a Finverse online payment flow with create (Payment link), redirect, callback, and status polling. Use when integrating Finverse payments, building checkout pages with Finverse API, or when the user mentions Finverse payment links or checkout sessions.
metadata:
  author: finverse
  version: "1.0.0"
---

# Finverse Online Payment Implementation

## Quick Start
1. **Get Client Id and Client Secret** from your [Finverse dashboard](https://dashboard.finverse.com)
2. Set **Redirect URI** in API settings to your callback URL (e.g. `https://yoursite.com/callback`).
3. **Set the Environment variables**

## Flow Overview

1. **Create** – Create payment link via Finverse API, get `url` and `payment_link_id`. Save `payment_link_id` and `unique_reference_id`.
2. **Redirect** – Redirect user to the payment link URL.
3. **Callback** – Finverse redirects back with `payment_link_id` and `unique_reference_id` query params. Verify the values matches what was stored earlier.
4. **Poll** – Poll `GET /payment_links/{paymentLinkId}` for up to 30 seconds, waiting for `session_status` is  `"COMPLETE"`.
5. **Result** – If `COMPLETE` → success page; else → error page.

## Sequence Diagrams

| Flow | Reference |
|------|-----------|
| **Online Payment** (user-initiated payment) | [references/online-payment-flow.wsd](references/online-payment-flow.wsd) |
| **Payment Method Setup** (store/edit payment method without payment) | [references/payment-method-setup-flow.wsd](references/payment-method-setup-flow.wsd) |
   
## Tech Stack
- If the user specifies a tech stack (Go, Node.js, Python, React, etc.), implement using that stack.
- Otherwise, infer from the project (e.g. go.mod → Go, package.json → Node, requirements.txt → Python).
- The flow, API calls, and checklist are the same; only the implementation language/framework changes.

## API Authentication

Use OAuth2 client credentials. POST to `{baseURL}/auth/customer/token`:

```json
{
  "client_id": "...",
  "client_secret": "...",
  "grant_type": "client_credentials"
}
```

Cache the `access_token`; use `Authorization: Bearer {token}` on all API requests. Refresh before expiry (use `expires_in` minus 60s buffer).

## Create Payment Link

If the user specified a payment link type or mode, use the following table for reference. If nothing is specified then assume `Payment`.

| Mode | Reference |
|----------|----------|
| **Payment** |[references/create-payment-mode-payment-link.md](references/create-payment-mode-payment-link.md)|
| **Setup** |[references/create-setup-mode-payment-link.md](references/create-setup-mode-payment-link.md)|

## Redirect to Finverse URL

After creating the payment link, redirect the user to `link.url`. **The redirect must be followed with GET**, not POST. Finverse's payment page expects a GET request.

- **Next.js**: `NextResponse.redirect()` defaults to 307, which preserves the request method (POST stays POST). Use **303 See Other** explicitly: `NextResponse.redirect(link.url, 303)` so the browser follows with GET.
- **Other frameworks**: Use HTTP 303 for "redirect after POST" so the redirect target is fetched with GET.

## Callback Handler

1. Read `payment_link_id` from query params (required)
2. Optionally validate `unique_reference_id` against your records
3. Poll `GET /payment_links/{paymentLinkId}` immediately, then every 2 seconds
4. Stop when: `session_status` is `COMPLETE` (success), or timeout (30s → error). Log the paymentId and paymentLinkId.

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `FINVERSE_CLIENT_ID` | Yes | 26-char client ID |
| `FINVERSE_CLIENT_SECRET` | Yes | 64–80 char secret |
| `CALLBACK_URL` | Yes | Callback URL for redirect_uri (e.g. `https://yoursite.com/callback`) |
| `FINVERSE_BASE_URL` | No | Default `https://api.prod.finverse.net` |

> **Security:** `FINVERSE_CLIENT_ID` and `FINVERSE_CLIENT_SECRET` must **never** be pushed to git or passed to the frontend. Keep them server-side only (e.g. environment variables, secrets manager).

## Implementation Checklist

- [ ] Finverse client with token caching and `CreatePaymentLink` / `GetPaymentLink`
- [ ] Checkout form → POST handler → create link → redirect back to `url` (GET method)
- [ ] Callback handler: extract `payment_link_id`, poll for session_status = `COMPLETE`, redirect to success/error
- [ ] Success and error pages
- [ ] Never expose client_id/client_secret to frontend


## API Reference

**Use [Finverse Swagger](https://api.prod.finverse.net/swagger.json) for API request and response schemas.** If any information in this skill conflicts with the Swagger spec, **the Swagger spec is the source of truth**—follow it.

For full documentation for payment API, see: [Finverse Payment API docs](https://docs.finverse.com/#4c635345-e445-4d7b-bd4a-92506e98ada3)

## SDK Reference
- [Go SDK](https://github.com/finversetech/sdk-go)
- [Typescript SDK](https://github.com/finversetech/sdk-typescript)