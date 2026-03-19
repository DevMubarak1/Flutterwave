---
name: flutterwave-mobile-money
description: Run mobile money collections with operator-aware routing and delayed status handling.
---

# flutterwave-mobile-money

## Purpose

Run mobile money collections with operator-aware routing and delayed status handling.

## Implementation Pattern (TypeScript)

```ts
const FLW_V3_BASE_URL = process.env.FLW_V3_BASE_URL ?? "https://api.flutterwave.com/v3";
const FLW_V4_BASE_URL = process.env.FLW_V4_BASE_URL ?? "https://developersandbox-api.flutterwave.com";
const FLW_SECRET_KEY = process.env.FLW_SECRET_KEY;

type FlwResponse<T> = {
  status: "success" | "error";
  message: string;
  data: T;
};

class FlutterwaveApiError extends Error {
  constructor(
    message: string,
    public readonly statusCode: number,
    public readonly payload?: unknown,
  ) {
    super(message);
    this.name = "FlutterwaveApiError";
  }
}

function getBaseUrl(version: "v3" | "v4") {
  return version === "v3" ? FLW_V3_BASE_URL : FLW_V4_BASE_URL;
}

async function flwRequest<T>(
  version: "v3" | "v4",
  path: string,
  init: RequestInit = {},
): Promise<FlwResponse<T>> {
  if (!FLW_SECRET_KEY) {
    throw new Error("Missing FLW_SECRET_KEY");
  }

  const response = await fetch(`${getBaseUrl(version)}${path}`, {
    ...init,
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${FLW_SECRET_KEY}`,
      ...(init.headers ?? {}),
    },
  });

  const body = (await response.json()) as FlwResponse<T>;

  if (!response.ok || body.status === "error") {
    throw new FlutterwaveApiError(
      body.message || "Flutterwave API request failed",
      response.status,
      body,
    );
  }

  return body;
}
```

## Endpoint Operations and References

| Version | Method | Route / Operation | Use Case | Reference |
| --- | --- | --- | --- | --- |
| v3 | POST | /charges?type=mobile_money | Initiate mobile money charge | https://developer.flutterwave.com/docs/mobile-money |
| v3 | GET | /transactions/{id}/verify | Verify completion | https://developer.flutterwave.com/docs/getting-started |
| v4 | GET | /mobile-networks | Get supported operators | https://developer.flutterwave.com/reference/mobile_networks_get |
| v4 | POST | /charges | Create mobile money charge | https://developer.flutterwave.com/reference/charges_post |

## Country / Rail Matrix

| Market | Rail | Integration Notes |
| --- | --- | --- |
| KE | M-Pesa | Primary rail for many consumer use cases. |
| GH | MTN/Vodafone | Select operator per customer phone prefix. |
| UG/RW/TZ/ZM | Airtel/MTN variants | Handle delayed completion with webhook + poll. |

## Usage Example

```ts
export async function runMobileMoneyExample() {
  return flwRequest<unknown>("v4", "/charges", {
    method: "GET",
  });
}
```

## Reliability Checklist

- Re-verify successful payment state server-side before fulfillment.
- Store provider ids, internal ids, tx_ref, and event ids for reconciliation.
- Keep webhook consumers idempotent and return HTTP 200 quickly.
- Separate version-specific request/response types to avoid schema mixups.

## v4 Delta Note

v4 is resource-centric in the public reference. Keep route maps and payload types versioned in code so agent-generated integrations remain accurate.

