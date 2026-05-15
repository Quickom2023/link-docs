# Create Alias

## Request

The Create Alias endpoint allows you to generate a new alias. This alias can be configured with specific parameters such as expiration time and authentication type (Basic or JWT), enabling secure and time-bound access control.

**Endpoint:**

```http
POST /organization/api/v1/alias/create
```

**URL:**

```text
https://link-mytv.quickom.com/organization/api/v1/alias/create
```

## Body Parameters

| Parameter      | Type    | Required | Description                                            | Example                 |
| -------------- | ------- | -------- | ------------------------------------------------------ | ----------------------- |
| **type**       | string  | Yes      | The type of alias. Valid values: `"conference"`.       | `conference`            |
| **name**       | string  | No       | Custom name for the alias.                             | `WeeklySyncMeeting`     |
| **passcode**   | string  | No       | Optional passcode for basic authentication.            | `secure123`             |  |
| **jwt_secret** | string  | No       | Optional JWT secret for advanced authentication.       | `eyJhbGciOi...` or null |
| **expired_at** | integer | Yes      | Expiry time for the alias in UNIX timestamp (seconds). | `30`                    |

### Example Request

```http
POST /organization/api/v1/alias/create HTTP/1.1
Host: link-mytv.quickom.com
Authorization: Bearer <your_token>
Content-Type: application/json

{
  "type": "conference",
  "name": "New Alias Name",
  "passcode": "pass",
  "jwt_secret": null,
  "expired_at": 1737456308
}
```

## Responses

### 201

Alias successfully created.

**Response Body Parameters**

| Parameter      | Type    | Description                                                          | Example            |
| -------------- | ------- | -------------------------------------------------------------------- | ------------------ |
| **id**         | string  | Unique identifier for the created alias.                             | `xxyzx`            |
| **type**       | string  | Type of alias (`"conference"`).                                      | `"conference"`     |
| **name**       | string  | Name of the alias.                                                   | `DemoSessionAlias` |
| **passcode**   | string  | Passcode for basic authentication.                                   | `demo1234`         |
| **jwt_secret** | string  | JWT secret for advanced authentication. May be null if not provided. | `eyJhbGciOi...`    |
| **expired_at** | integer | Expiration time of the alias in UNIX timestamp format (seconds).     | `1737456308`       |

**Example**

```json
{
  "data": {
    "id": "xxyzx",
    "type": "conference",
    "name": "New Alias Name",
    "passcode": "pass",
    "jwt_secret": null,
    "expired_at": 1737456308
  }
}
```

### 400

| Field       | Type    | Description                            | Example       |
| ----------- | ------- | -------------------------------------- | ------------- |
| **message** | string  | Describes the type of error.           | `"NOT_FOUND"` |
| **status**  | integer | HTTP status code indicating the error. | `400`         |

**Example Response**

```json
{
  "error": {
    "message": "NOT_FOUND",
    "status": 400
  }
}
```

### 401

Authentication failed. The token is missing, invalid, or expired.

| Field     | Type   | Description                        | Example          |
| --------- | ------ | ---------------------------------- | ---------------- |
| **error** | object | Contains the error details object. | `"UNAUTHORIZED"` |

**Example Response**

```json
{
  "error": {
    "message": "UNAUTHORIZED"
  }
}
```
