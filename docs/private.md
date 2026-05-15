# Private Alias

## Request

Create a private alias by assigning a `jwt_secret`, then use that secret to sign JWT tokens that authorize access to the alias.

## Step 1: Create a Private Alias

**Endpoint:**

```http
POST /organization/api/v1/alias/create
```

**URL:**

```text
https://link-mytv.quickom.com/organization/api/v1/alias/create
```

## Headers

| Header            | Type   | Required | Description                      | Example     |
| ----------------- | ------ | -------- | -------------------------------- | ----------- |
| **Authorization** | string | Yes      | API key used for authentication. | `<api_key>` |

## Body Parameters

| Parameter      | Type    | Required | Description                                 | Example                    |
| -------------- | ------- | -------- | ------------------------------------------- | -------------------------- |
| **type**       | string  | Yes      | Type of alias.                              | `"conference"` or `"live"` |
| **name**       | string  | No       | Custom alias name.                          | `"Team Sync Alias"`        |
| **jwt_secret** | string  | Yes      | Secret used for signing JWT tokens.         | `"myJWTsecret123"`         |
| **expired_at** | integer | Yes      | Expiry time as a UNIX timestamp in seconds. | `1737456308`               |

### Example Request

```http
POST /organization/api/v1/alias/create HTTP/1.1
Host: link-mytv.quickom.com
Authorization: <api_key>
Content-Type: application/json

{
  "type": "conference",
  "name": "Private Alias Name",
  "jwt_secret": "your-secure-secret",
  "expired_at": 1737456308
}
```

## Responses

### 201

Alias successfully created.

| Field          | Type    | Description               | Example                |
| -------------- | ------- | ------------------------- | ---------------------- |
| **id**         | string  | Alias ID.                 | `"priva"`              |
| **type**       | string  | Alias type.               | `"conference"`         |
| **name**       | string  | Name of the alias.        | `"Private Alias Name"` |
| **jwt_secret** | string  | The JWT signing secret.   | `"your-secure-secret"` |
| **expired_at** | integer | Expiry time of the alias. | `1737456308`           |

**Example Response**

```json
{
  "data": {
    "id": "priva",
    "type": "conference",
    "name": "Private Alias Name",
    "jwt_secret": "your-secure-secret",
    "expired_at": 1737456308
  }
}
```

### 401

Authentication failed. The API key is missing, invalid, or expired.

| Field       | Type   | Description                       | Example          |
| ----------- | ------ | --------------------------------- | ---------------- |
| **message** | string | Error message for invalid access. | `"UNAUTHORIZED"` |

**Example Response**

```json
{
  "error": {
    "message": "UNAUTHORIZED"
  }
}
```

### 404

Returned when the alias is not found.

| Field       | Type   | Description                    | Example             |
| ----------- | ------ | ------------------------------ | ------------------- |
| **message** | string | Error when alias is not found. | `"Alias not found"` |

**Example Response**

```json
{
  "error": {
    "message": "Alias not found"
  }
}
```

## Step 2: Generate and Sign a JWT Token

Generate a JWT token using the alias's `jwt_secret` and user-specific claims.

### Example JWT Claims

```json
{
  "alias": "priva",
  "user_id": "user456xxyz",
  "host": true,
  "expires_in": 3600
}
```

| Claim          | Type    | Description                                     | Example           |
| -------------- | ------- | ----------------------------------------------- | ----------------- |
| **alias**      | string  | The ID of the alias being accessed.             | `"priva"`         |
| **user_id**    | string  | Unique ID for the user.                         | `"user456xxyz"`   |
| **host**       | boolean | Indicates whether the user has host privileges. | `true` or `false` |
| **expires_in** | integer | Token expiration time in seconds from issuance. | `3600`            |

### Token Signing Example

```elixir
alias_id = "priva"
jwt_secret = "your-secure-secret"

claims = %{
  "alias" => alias_id,
  "user_id" => "user456xxyz",
  "host" => true,
  "expires_in" => 3600
}

{:ok, token, _} = JWT.generate_and_sign(claims, signer(jwt_secret))
```

## Step 3: Retrieve the Private Alias

Use the generated token to retrieve alias details securely.

**Endpoint:**

```http
GET /api/v1/alias/:id
```

**URL:**

```text
https://link-mytv.quickom.com/api/v1/alias/priva
```

## Path Parameters

| Parameter | Type   | Required | Description                      | Example   |
| --------- | ------ | -------- | -------------------------------- | --------- |
| **id**    | string | Yes      | The ID of the alias to retrieve. | `"priva"` |

## Headers

| Header            | Type   | Required | Description                                                             | Example              |
| ----------------- | ------ | -------- | ----------------------------------------------------------------------- | -------------------- |
| **Authorization** | string | Yes      | Signed JWT token. Use the `Bearer` scheme when authenticating with JWT. | `Bearer <jwt_token>` |

### Example Request

```http
GET /api/v1/alias/priva HTTP/1.1
Host: link-mytv.quickom.com
Authorization: Bearer <jwt_token>
```

### 200

The request was successful, and the alias information was returned in the response body.

| Field           | Type    | Description                                   | Example                |
| --------------- | ------- | --------------------------------------------- | ---------------------- |
| **id**          | string  | Alias ID.                                     | `"priva"`              |
| **type**        | string  | Alias type.                                   | `"conference"`         |
| **name**        | string  | Alias name.                                   | `"Private Alias Name"` |
| **jwt_secret**  | string  | Alias JWT secret.                             | `null`                 |
| **expired_at**  | integer | Expiry time as a UNIX timestamp in seconds.   | `1737456308`           |
| **inserted_at** | integer | Inserted time as a UNIX timestamp in seconds. | `1737356308`           |

**Example Response**

```json
{
  "data": {
    "id": "priva",
    "type": "conference",
    "name": "Private Alias Name",
    "jwt_secret": null,
    "expired_at": 1737456308,
    "inserted_at": 1737356308
  }
}
```

## JWT Verification

The API verifies the JWT token by:

1. Extracting the JWT from the `Authorization` header.
2. Decoding the token using the alias's `jwt_secret`.
3. Validating that the `alias` claim matches the alias ID in the path.
4. Validating that `expires_in` is still valid.
