# Shopnix — Cloudflare Pages + D1

Shopnix includes the storefront, categories, product search, cart/quantity controls, unique-browser visitor counter, password-protected admin panel, product add/remove and pricing, and WhatsApp/Telegram order handoff. No delivery-address form or delivery-management system is included.

## Cloudflare setup
1. Create a D1 database in Cloudflare called `shopnix-db`.
2. Run the complete `schema.sql` in the D1 Console.
3. Upload this folder's contents to the root of a GitHub repository.
4. In Cloudflare Workers & Pages, create a Pages project connected to that repository. Framework: **None**; build command blank; output directory `public`.
5. In Pages → Settings → Functions → D1 database bindings, add binding `DB` pointing to your database.
6. In Pages → Settings → Variables and Secrets, add production secrets:
   - `ADMIN_PASSWORD`: a strong admin password
   - `ADMIN_SESSION_SECRET`: a random secret at least 32 characters long
7. Redeploy after adding bindings/secrets. Admin URL: `/admin.html`.
8. In the admin panel, set your WhatsApp number with country code, digits only (e.g. `919876543210`) and Telegram username (without @ is accepted). Add products and prices there.

## Order behaviour
Customer taps Order Now from the cart and sees direct WhatsApp and Telegram contact options. WhatsApp opens a prefilled message with the selected products. Telegram opens the configured account; the customer can paste the order message using the copy button. The site does not collect delivery details or run a delivery workflow.

## Important scope note
The admin login is protected by server-side secrets and an HttpOnly signed session cookie. Customer email/password registration and login are included. Passwords are stored as PBKDF2 hashes. This starter does not include Google login. Product and visitor data are persistent only after D1 is correctly bound and schema is applied.
