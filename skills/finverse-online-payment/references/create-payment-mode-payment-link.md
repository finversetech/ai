## Create Payment Mode Payment Link

**POST** `{baseURL}/payment_links`

Required fields:
- `mode` – `"PAYMENT"`
- `amount` (int) – minor units (e.g. 10000 = 100.00 HKD)
- `currency` (string) – 3-letter code (SGD, HKD, USD)
- `unique_reference_id` – globally unique identifier
- `sender` – `{ external_user_id, name, email? }` (external_user_id = your internal user ID)
- `payment_details` – `{ description, external_transaction_reference }` - `external_transaction_reference` can be up to 35 characters
- `link_customizations` – `{ ui_mode: "redirect", redirect_uri: "{callbackURL}" }`

In the checkout page, ask the user to input `amount`, `currency`, `name` and `email`. The rest should be generated before making the request.

Response includes `url` (redirect target) and `payment_link_id`.
