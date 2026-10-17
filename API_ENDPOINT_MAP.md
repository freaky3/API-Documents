# Onlinefact API Endpoint Map

Doel: centrale referentie om API-implementatie en Slate-documentatie synchroon te houden.

## Routering

- URL rewrite (`api_onlinefact/.htaccess`): `/{request_page}/{id?}` -> `index.php?request_page={...}&id={...}`
- Dispatcher (`api_onlinefact/index.php`): include `api2/{request_page}.php`

## Gedocumenteerde endpoint families (Slate)

- `products`
  - `GET /products/`
  - `GET /products/<ID>`
  - `POST /products/`
  - `PUT /products/<ID>`
  - `DELETE /products/<ID>`
- `categories`
  - `GET /categories/`
  - `GET /categories/<ID>`
  - `POST /categories/`
  - `PUT /categories/<ID>`
  - `DELETE /categories/<ID>`
- `brands`
  - `GET /brands/`
  - `GET /brands/<ID>`
  - `POST /brands/`
  - `PUT /brands/<ID>`
  - `DELETE /brands/<ID>`
- `customers`
  - `GET /customers/`
  - `GET /customers/<ID>`
  - `POST /customers/`
  - `PUT /customers/<ID>`
  - `DELETE /customers/<ID>`
- `vouchers`
  - `GET /vouchers/`
  - `GET /vouchers/<ID>`
  - `POST /vouchers/`
  - `PUT /vouchers/<ID>`
  - `DELETE /vouchers/<ID>`
- `documents`
  - `GET /documents/`
  - `GET /documents/<ID>`
  - `POST /documents/`
  - `PUT /documents/<ID>`
- `payments`
  - `GET /payments/`
  - `GET /payments/<ID>`
  - `POST /payments/`
  - `DELETE /payments/<ID>`
- `banktransactions`
  - `GET /banktransactions/`
  - `GET /banktransactions/<ID>`
- `reports`
  - `GET /reports/documents/`
  - `GET /reports/saletotals/`
  - `GET /reports/salesperproduct/`
  - `GET /reports/salesperrelation/`

## API-bestanden met extra/ondersteunende endpoints

Deze bestaan in code en moeten expliciet gedocumenteerd of bewust intern gehouden worden:

- `api2/info.php`
- `api2/payments.php` (`GET`, `POST`, `PUT`=not supported, `DELETE`)
- `api2/dopayment.php`
- `api2/dopayments/*.php`

## Verplichte sync-checklist bij elke API wijziging

1. Bepaal impacted endpoint(s) in `api2/**/*.php`.
2. Pas bijhorende sectie(s) aan in `source/includes/_*.md`.
3. Verifieer endpoint URL en HTTP-methodes exact tegen code.
4. Verifieer parameters, responsevelden en foutcases.
5. Controleer `source/index.html.md` includes voor nieuwe secties.

## Payments contract notes (canoniek)

Deze afspraken zijn expliciet vastgelegd en moeten consistent blijven tussen code en docs:

- `PUT /payments/<ID>` wordt niet gedocumenteerd als werkende endpoint (update-flow is niet ondersteund).
- `POST /payments/` gebruikt `document_id` als input; expliciete multi-link input via `document_ids` is verwijderd.
- Als `POST` geen `date` bevat, gebruikt de API automatisch de huidige datum (`Y-m-d`).
- Payment responses geven gekoppelde documenten terug onder `documents` (object), met document-id als key.
- Elke `documents` entry bevat minstens:
  - `document_id`
  - `document_type`
  - `document_number`
  - `customer_ref`
  - `customer_name`
  - `document_date`
- `GET /payments/` ondersteunt filtering/sortering via:
  - filters: `document_id`, `type`, `date`, `min_date`, `max_date`
  - sorting: `order_by` (`payment_id|date|type|amount`) en `order_dir` (`ASC|DESC`)
- Z-report koppeling (`zreport_id`) is verwijderd uit het payments endpoint contract.

## Bank transactions (read-only)

- Geen `/bankaccounts` endpoint.
- Alleen `GET /banktransactions/` en `GET /banktransactions/<ID>`. HTTP Basic met API key en secret. Resultaten zijn beperkt tot het account van de API key.
- `GET /banktransactions/` geeft een JSON-array van bankrekeningen. Elke rekening bevat `transactions[]`. Rekeningen zonder match blijven in de lijst met `transactions: []`.
- Velden rekening: `id`, `name` (naam uit de backoffice, `null` indien leeg), `iban`, `bic`, `currency`, `institution_id`, `active`, `balance`, `last_sync`, `connection_valid`, `total_transactions` (aantal matches over alle pagina's), `transactions`.
- Velden transactie: `id`, `date`, `amount`, `direction`, `counterparty_name`, `counterparty_iban`, `message`, `reference`, `open_amount`, `payments[]` (`payment_id`, `amount`, `date`, `documents[]` met `document_id`, `document_type`, `document_number`, `amount`).
- Filters gelden voor transacties: `min_date`/`max_date` (alias `from`/`to`), `direction` (`in`|`out`, alias `outgoing=1`), `onlyopen=1`, `bank_account_id` (alias `iban`, het numerieke id), `include_inactive=1`, `limit` (default 100, max 1000), `page`, `order_by` (`id`, `date`, `amount`), `order_dir`.
- Inactieve rekeningen (`active` = 0) worden weggelaten tenzij `include_inactive=1`.
- Er is een volgende pagina wanneer `page * limit` kleiner is dan de som van `total_transactions`.
- `GET /banktransactions/<ID>` geeft één rekeningobject met alleen die transactie.
- `open_amount` is unsigned: absolute waarde van `amount` min de absolute bedragen van de gekoppelde betalingen.
- `GET /documents/`: `document_type` 11 is een aankoopfactuur. Als zowel `min_date` als `max_date` gezet zijn, is het bereik maximaal 14 dagen.

## Release-gate voor API changes

Een API wijziging is niet klaar wanneer:
- docs ontbreken voor een nieuw endpoint,
- voorbeeldpayloads niet meer kloppen,
- URL/methode in docs niet overeenkomt met implementatie.
