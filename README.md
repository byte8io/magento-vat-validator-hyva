# Byte8_VatValidatorHyva — Hyvä companion for Byte8_VatValidator

Adds a live, debounced VAT-validation indicator to Hyvä-themed registration
and address forms. Calls the standard
`Byte8_VatValidator` REST endpoint and shows a green / red badge directly
under the VAT input as the customer types.

## Requirements

- `byte8/module-vat-validator` ≥ 0.1.0
- A Hyvä-based theme (`hyva-themes/magento2-default-theme`)
- No additional JS dependencies — Alpine + Tailwind only, both ship with
  Hyvä

## Core module

This is a storefront companion — it does nothing on its own. Install the
core validator first: **[byte8/module-vat-validator](https://github.com/byte8io/magento-vat-validator)**.
That module provides the EU VIES / UK HMRC / Swiss UID validation, REST
endpoints, customer-group mapping, audit log, and CLI. This module just
wires a live indicator into Hyvä forms.

```bash
composer require byte8/module-vat-validator
```

## Install

```bash
composer require byte8/module-vat-validator-hyva
bin/magento module:enable Byte8_VatValidatorHyva
bin/magento setup:upgrade
bin/magento cache:flush
```

## What it does

| Page | Behaviour |
|---|---|
| `customer/account/create` | Watches the `taxvat` field; shows live indicator |
| `customer/address/form` | Watches the `vat_id` + `country_id` fields; shows live indicator |

Behaviour:

- 600 ms debounce — won't spam the REST endpoint while the user types
- Recognises both 2-letter ISO prefixes (`DE…`, `GB…`) and the Swiss
  3-letter `CHE…` prefix
- Falls back to the country dropdown's value if no prefix is typed
- Hides itself silently on `unavailable` / `skipped` outcomes — never
  shows a misleading red badge when VIES is just slow

## Customising

Override
`Byte8_VatValidatorHyva::vat-indicator.phtml` in your theme. The whole
component is a single self-contained Alpine `x-data` block — drop in your
own copy / icons / Tailwind classes.

## Hyvä Checkout (Magewire)

Not yet shipped. The standard Hyvä Checkout uses Magewire components
which need a server-side wire-class. Open an issue if you need it and
we'll prioritise.

## License

MIT
