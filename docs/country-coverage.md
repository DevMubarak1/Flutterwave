# Country and Rail Coverage Guide

Flutterwave supports a broad set of countries and rails.
Coverage can change by product, merchant profile, and compliance status.

## Key Design Rules

- Build feature flags by country and payment rail.
- Validate currency/country combinations before charge initialization.
- Present only available options in checkout by geolocation and merchant config.
- Keep fallback rail logic (card -> bank transfer -> mobile money -> USSD).

## Mobile Money Focus

The mobile money flow is a key differentiator for this skill repository.
Typical high-demand rails include:

- Kenya: M-Pesa
- Ghana: MTN and Vodafone
- Uganda, Rwanda, Zambia, Tanzania: Airtel/MTN variants

## Reconciliation Rules

- Normalize amounts in minor units where your app needs strict integer math.
- Track local currency settlement separately from billing currency.
- Include rail, provider, and country in transaction metadata.

## Launch Checklist

- Confirm live-mode rail enablement per country.
- Confirm webhook destination reliability and retries.
- Validate refund and dispute process by country.
- Run pilot transactions in each target market before broad launch.
