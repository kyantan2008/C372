# Feature: Prepare payment details

## 1. User goal
As a pet owner, I want to review the selected pet service and its amount before payment.

## 2. Scope and service catalogue
The endpoint returns the selected service details using the server-side catalogue. The client must not supply the payment amount.

| ID | Service name | Price |
|---:|---|---:|
| 1 | Vaccination | 35 |
| 2 | Pet Grooming | 50 |
| 3 | Boarding | 80 |

Prices and response amounts are numbers. Service names must match the catalogue, including capitalization.

## 3. Request
`GET /payments/prepare/:serviceId`

No login is required for this classroom mock.

## 4. Successful response
For a service ID in the catalogue, return HTTP 200 with a JSON object containing exactly these five fields and no additional fields:

| Field | Type | Value |
|---|---|---|
| `serviceId` | number | The selected catalogue ID |
| `serviceName` | string | The selected catalogue service name |
| `amount` | number | The selected service's server-side catalogue price |
| `currency` | string | `"sgd"` |
| `status` | string | `"READY_FOR_PAYMENT"` |

## 5. Unsuccessful response
For a service ID that is not in the catalogue, return HTTP 404 with exactly this JSON object:

```json
{"error":"Service not found"}
```

## 6. Acceptance criteria
- **AC1:** `GET /payments/prepare/1` returns HTTP 200 with `serviceId` 1, `serviceName` `"Vaccination"`, `amount` 35, `currency` `"sgd"`, and `status` `"READY_FOR_PAYMENT"`.
- **AC2:** `GET /payments/prepare/2` returns HTTP 200 with `serviceId` 2, `serviceName` `"Pet Grooming"`, `amount` 50, `currency` `"sgd"`, and `status` `"READY_FOR_PAYMENT"`.
- **AC3:** `GET /payments/prepare/3` returns HTTP 200 with `serviceId` 3, `serviceName` `"Boarding"`, `amount` 80, `currency` `"sgd"`, and `status` `"READY_FOR_PAYMENT"`.
- **AC4:** `GET /payments/prepare/999` returns HTTP 404 with exactly `{"error":"Service not found"}`.
- **AC5:** Every successful response contains exactly the five fields in Section 4, with the specified types and values. The amount comes from the server-side catalogue, not from client input.

## 7. Out of scope
Charging money, payment gateway integration, transactions, new database tables, EJS pages, and changes to existing features.

## 8. Open question
- What response should the endpoint return when `serviceId` is missing or malformed (for example, non-numeric, zero, or negative)?
