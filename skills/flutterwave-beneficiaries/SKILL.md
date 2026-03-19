---
name: flutterwave-beneficiaries
description: Maintain reusable payout recipients and senders for clean transfer orchestration.
---

# flutterwave-beneficiaries

## Purpose

Maintain reusable payout recipients and senders for clean transfer orchestration.

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
| v3 | POST | /beneficiaries | Create beneficiary | https://developer.flutterwave.com/docs/general-transfer-flow |
| v4 | GET | /transfers/recipients | List recipients | https://developer.flutterwave.com/reference/transfers_recipients_list |
| v4 | POST | /transfers/recipients | Create recipient | https://developer.flutterwave.com/reference/transfers_recipients_create |
| v4 | DELETE | /transfers/recipients/{id} | Delete recipient | https://developer.flutterwave.com/reference/transfers_recipients_delete |
| v4 | POST | /transfers/senders | Create sender | https://developer.flutterwave.com/reference/transfers_senders_create |

## Country / Rail Matrix

| Market | Rail | Integration Notes |
| --- | --- | --- |
| NG | Bank beneficiaries | Validate account ownership first. |
| KE/GH | MoMo beneficiaries | Store operator + phone metadata. |
| All | Sender/recipient separation | Required for compliance audits. |

## Usage Example

```ts
export async function runBeneficiariesExample() {
  return flwRequest<unknown>("v4", "/transfers/recipients", {
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

