# Table of Contents
1. [Base URL](#base-url)
2. [Authentication](#authentication)
3. [meCash Request Endpoints](#mecash-request-endpoints)
    1. [POST /api/v1/payments](#apiv1payments)
        1. [Sample Request](#sample-request)
        2. [Parameter Descriptions](#parameter-descriptions)
        3. [Sample Response](#sample-response)
        4. [Field Descriptions](#field-descriptions)
4. [Error Handling](#error-handling)
    1. [Sample Error Response](#sample-error-response)
# Getting Started With meCash API
Welcome to the meCash API. The meCash Cross-Border Payment API allows developers to initiate international payments between a sender and a recipient.

## Base URL
The base URL for our API is: `https://api.prod.me-cash.com/api/v1/payments`

## Authentication
Before using our API, you need to obtain an API key. Sign up on our dashboard to obtain your API key. Include the API key in the request headers as follows:

| Header   | Description  |  Example   | 
|------------|------------|------------|
| Authorization |Bearer token for authentication|`Bearer your_api_key`| 
| Content-type | Specifies the media type of the request | `application/json` |

## meCash Request Endpoints
### /api/v1/payments [POST]
This endpoint allows you to initiate a cross-border payment between a sender and a recipient.

#### Sample Request
```
{
  "amount": 100.00,
  "currency": "USD",
  "sender": {
    "name": "John Doe",
    "email": "john.doe@x.com"
  },
  "recipient": {
    "name": "Jane Smith",
    "accountNumber": "0987654321",
    "bankCode": "XYZ456",
    "country": "USA"
  },
  "reference": "INV-12345"
}
```

#### Parameter Descriptions
| Field                   | Type       | Description                                                   | Required |
|--------------------------|------------|---------------------------------------------------------------|----------|
| `amount`                | `number`   | The transaction amount. Must be a positive value.             | Yes      |
| `currency`              | `string`   | The currency of the transaction. Use ISO 4217 currency codes (e.g., USD, EUR). | Yes      |
| `sender`                | `object`   | Information about the sender of the payment.                  | Yes      |
| `sender.name`           | `string`   | Full name of the sender.                                      | Yes      |
| `sender.email`          | `string`   | Email address of the sender. Must be a valid email format.    | Yes      |
| `recipient`             | `object`   | Information about the recipient of the payment.               | Yes      |
| `recipient.name`        | `string`   | Full name of the recipient.                                   | Yes      |
| `recipient.accountNumber`| `string`  | Bank account number of the recipient.                         | Yes      |
| `recipient.bankCode`    | `string`   | The recipient's bank code.                                    | Yes      |
| `recipient.country`     | `string`   | The recipient's country. Use ISO 3166-1 alpha-3 country codes (e.g., USA, GBR). | Yes      |
| `reference`             | `string`   | Unique transaction reference or invoice number.               | Yes      |

#### Sample Response
HTTP Response Code: `201 OK`
```
{
  "transactionId": "TXN789456123",
  "status": "SUCCESS",
  "createdAt": "2025-01-12T10:15:30Z",
  "amount": 100.00,
  "currency": "USD",
  "recipient": {
    "name": "Jane Smith",
    "country": "USA"
  }
}
```
#### Field Descriptions
| Field                  | Type       | Description                                                      | Required |
|-------------------------|------------|------------------------------------------------------------------|----------|
| `transactionId`        | `string`   | Unique identifier for the transaction.                          | Yes      |
| `status`               | `string`   | Status of the transaction. Possible values: `SUCCESS`, `FAILED`, `PENDING`. | Yes      |
| `createdAt`            | `string`   | The date and time when the transaction was created, in ISO 8601 format. | Yes      |
| `amount`               | `number`   | The transaction amount. Must be a positive value.               | Yes      |
| `currency`             | `string`   | The currency of the transaction. Use ISO 4217 currency codes (e.g., USD, EUR). | Yes      |
| `recipient`            | `object`   | Information about the recipient of the transaction.             | Yes      |
| `recipient.name`       | `string`   | Full name of the recipient.                                      | Yes      |
| `recipient.country`    | `string`   | The recipient's country. Use ISO 3166-1 alpha-3 country codes (e.g., USA, GBR). | Yes      |

## Error Handling
Our API uses standard HTTP status codes to communicate the result of a request.

### Sample Error Response
**HTTP Response Code:** 400
The response body may also contain an error object with additional information.

```
{
  "error": "INVALID_REQUEST",
  "message": "Invalid bank code for the recipient."
}
```

The payCash API returns error codes and messages to help with troubleshooting. Below are common error codes and their explanations:

| **Error Code**         | **HTTP Status** | **Message**                                | **Description**                                                                 |
|-------------------------|-----------------|--------------------------------------------|---------------------------------------------------------------------------------|
| **INVALID_REQUEST**     | 400             | "Invalid bank code for the recipient."     | Occurs when the provided bank code is invalid or unrecognized. Verify the bank code and retry. |
| **UNAUTHORIZED**        | 401             | "Unauthorized access to the API."          | This error occurs when the API request lacks proper authentication. Ensure correct API keys are used. |
| **INSUFFICIENT_FUNDS**  | 402             | "Insufficient funds for the transaction."  | The account doesn't have enough balance to complete the payment. Check account balance. |
| **SERVER_ERROR**        | 500             | "Internal server error."                   | Indicates a server-side issue. Try again later or contact support for assistance. |
