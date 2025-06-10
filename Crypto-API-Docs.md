## Simplified Crypto API Documentation

### Version

1.0.0

### Description

A basic API for managing cryptocurrency data and transactions.

### Server

* **URL:** [https://api.yourcryptoapp.com/v1](https://api.yourcryptoapp.com/v1)
* **Description:** Production server for the Crypto API

---

## Endpoints

### GET /coins

**Summary:** Get a list of cryptocurrencies
**Description:** Retrieve a comprehensive list of available cryptocurrencies with their current price and market capitalization.

**Responses:**

* **200 OK**

  * Returns a list of cryptocurrencies.
  * **Content-Type:** application/json
  * **Schema:** Array of `Coin` objects
  * **Example:**

    ```json
    [
      {
        "id": "bitcoin",
        "name": "Bitcoin",
        "symbol": "btc",
        "current_price_usd": 60000.00,
        "market_cap_usd": 1200000000000
      },
      {
        "id": "ethereum",
        "name": "Ethereum",
        "symbol": "eth",
        "current_price_usd": 3000.00,
        "market_cap_usd": 360000000000
      }
    ]
    ```
* **500 Internal Server Error**

  * **Example:**

    ```json
    {
      "code": 500,
      "error": "Internal Server Error",
      "message": "An unexpected error occurred on the server"
    }
    ```

---

### GET /coins/{coinId}

**Summary:** Get details of a single cryptocurrency
**Description:** Retrieve detailed information for a specific cryptocurrency by its ID.

**Path Parameters:**

* `coinId` (string, required): The ID of the cryptocurrency to retrieve

**Responses:**

* **200 OK**

  * **Schema:** `Coin`
  * **Example:**

    ```json
    {
      "id": "bitcoin",
      "name": "Bitcoin",
      "symbol": "btc",
      "current_price_usd": 60000.00,
      "market_cap_usd": 1200000000000
    }
    ```
* **404 Not Found**

  * **Example:**

    ```json
    {
      "code": 404,
      "error": "Not Found",
      "message": "Coin with ID 'nonexistentcoin' not found"
    }
    ```
* **500 Internal Server Error**

  * **Example:**

    ```json
    {
      "code": 500,
      "error": "Internal Server Error",
      "message": "An unexpected error occurred on the server"
    }
    ```

---

### GET /coin/simpleprice

**Summary:** Get current price(s) of coin(s) against a currency
**Description:** Retrieve the current price for one or multiple cryptocurrencies against a specified currency (currently only USD).

**Query Parameters:**

* `vs_currency` (string, required): The currency to compare against (only `usd` supported)
* `id` (string, optional): Comma-separated coin IDs (e.g., bitcoin,ethereum)

**Responses:**

* **200 OK**

  * **Schema:** Object mapping coin IDs to price objects
  * **Examples:**

    ```json
    {
      "bitcoin": { "usd": 60000.00 },
      "ethereum": { "usd": 3000.00 }
    }
    ```
* **400 Bad Request**

  * **Example:**

    ```json
    {
      "code": 400,
      "error": "Bad Request",
      "message": "Required query parameter 'vs_currency' is missing or invalid"
    }
    ```
* **404 Not Found**

  * **Example:**

    ```json
    {
      "code": 404,
      "error": "Not Found",
      "message": "One or more coins specified were not found"
    }
    ```
* **500 Internal Server Error**

  * **Example:**

    ```json
    {
      "code": 500,
      "error": "Internal Server Error",
      "message": "An unexpected error occurred on the server"
    }
    ```

---

### POST /transactions

**Summary:** Create a new transaction
**Description:** Record a new cryptocurrency transaction, such as a buy, sell, or transfer. Requires API key authentication.

**Security:** Requires `ApiKeyAuth` via `X-API-KEY` header

**Request Body:**

* **Schema:** `Transaction`
* **Example:**

  ```json
  {
    "transactionId": "trx_12345",
    "coinId": "bitcoin",
    "type": "buy",
    "amount": 0.05,
    "price_usd": 60000.00,
    "timestamp": 1678886400
  }
  ```

**Responses:**

* **201 Created**

  * **Example:**

    ```json
    {
      "message": "Transaction created successfully",
      "transaction": {
        "transactionId": "trx_12345",
        "coinId": "bitcoin",
        "type": "buy",
        "amount": 0.05,
        "price_usd": 60000.00,
        "timestamp": 1678886400
      }
    }
    ```
* **400 Bad Request**

  * **Example:**

    ```json
    {
      "code": 400,
      "error": "Bad Request",
      "message": "Invalid request body. Missing required fields or incorrect data types"
    }
    ```
* **500 Internal Server Error**

  * **Example:**

    ```json
    {
      "code": 500,
      "error": "Internal Server Error",
      "message": "An unexpected error occurred on the server"
    }
    ```

---

### GET /transactions/{transactionId}

**Summary:** Get details of a transaction
**Description:** Retrieve the details of a specific transaction by its ID.

**Path Parameters:**

* `transactionId` (string, required): The ID of the transaction to retrieve

**Responses:**

* **200 OK**

  * **Schema:** `Transaction`
  * **Example:**

    ```json
    {
      "transactionId": "trx_12345",
      "coinId": "bitcoin",
      "type": "buy",
      "amount": 0.05,
      "price_usd": 60000.00,
      "timestamp": 1678886400
    }
    ```
* **404 Not Found**

  * **Example:**

    ```json
    {
      "code": 404,
      "error": "Not Found",
      "message": "Transaction with ID 'nonexistenttrx' not found"
    }
    ```
* **500 Internal Server Error**

  * **Example:**

    ```json
    {
      "code": 500,
      "error": "Internal Server Error",
      "message": "An unexpected error occurred on the server"
    }
    ```

---

## Components

### Schema: Coin

Represents a cryptocurrency with essential metadata and market data.

| Field               | Type           | Description                              | Example       |
| ------------------- | -------------- | ---------------------------------------- | ------------- |
| id                  | string         | Unique identifier for the cryptocurrency | "bitcoin"     |
| name                | string         | Full name of the cryptocurrency          | "Bitcoin"     |
| symbol              | string         | Symbol of the cryptocurrency             | "btc"         |
| current\_price\_usd | number (float) | Current price in USD                     | 60000.00      |
| market\_cap\_usd    | number (float) | Market capitalization in USD             | 1200000000000 |

**Required:** id, name, symbol, current\_price\_usd

---

### Schema: Transaction

Defines the structure of a cryptocurrency transaction.

| Field         | Type            | Description                                     | Example      |
| ------------- | --------------- | ----------------------------------------------- | ------------ |
| transactionId | string          | Unique identifier for the transaction           | "trx\_12345" |
| coinId        | string          | ID of the cryptocurrency involved               | "bitcoin"    |
| type          | string (enum)   | Type of transaction (`buy`, `sell`, `transfer`) | "buy"        |
| amount        | number (float)  | Amount of cryptocurrency involved               | 0.05         |
| price\_usd    | number (float)  | Price in USD at the time of the transaction     | 60000.00     |
| timestamp     | integer (int64) | Unix timestamp of the transaction               | 1678886400   |

**Required:** transactionId, coinId, type, amount, price\_usd, timestamp

---

### Schema: Error

Standard error response structure.

| Field   | Type    | Description            | Example             |
| ------- | ------- | ---------------------- | ------------------- |
| code    | integer | HTTP status code       | 400                 |
| error   | string  | General error category | "Bad Request"       |
| message | string  | Detailed error message | "Invalid ID format" |

**Required:** code, error, message

---

### Security Scheme: ApiKeyAuth

* **Type:** apiKey
* **In:** header
* **Name:** X-API-KEY

To authenticate, provide your API key in the header:

```
X-API-KEY: your_api_key_here
```
