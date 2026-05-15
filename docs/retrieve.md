# Retrieve Alias

## Request

Retrieve detailed information for a specific alias by providing its unique ID. This endpoint is typically used by clients or front-end applications to fetch alias configuration before joining a session or stream.

**Endpoint:**

```http
GET /api/v1/alias/:id
```

**URL:**

```text
https://link-mytv.quickom.com/api/v1/alias/:id
```

## Path Parameters

| Parameter | Type   | Required | Description                      | Example   |
| --------- | ------ | -------- | -------------------------------- | --------- |
| **id**    | string | Yes      | The ID of the alias to retrieve. | `"bf6oi"` |

## Headers

| Header            | Type   | Required | Description                      | Example     |
| ----------------- | ------ | -------- | -------------------------------- | ----------- |
| **Authorization** | string | No       | API key used for authentication. | `<api_key>` |

### Example Request

```http
GET /api/v1/alias/bf6oi HTTP/1.1
Host: link-mytv.quickom.com
Authorization: <api_key>
```

## Responses

### 200

The request was successful, and the alias information was returned in the response body.

| Field           | Type    | Description                                   | Example        |
| --------------- | ------- | --------------------------------------------- | -------------- |
| **id**          | string  | Alias ID.                                     | `"12345"`      |
| **type**        | string  | Alias type.                                   | `"conference"` |
| **name**        | string  | Alias name.                                   | `"Alias Name"` |
| **jwt_secret**  | string  | Alias JWT secret.                             | `null`         |
| **expired_at**  | integer | Expiry time as a UNIX timestamp in seconds.   | `1737356308`   |
| **inserted_at** | integer | Inserted time as a UNIX timestamp in seconds. | `1737456308`   |

**Example Response**

```json
{
  "data": {
    "id": "12345",
    "type": "conference",
    "name": "Alias Name",
    "jwt_secret": null,
    "expired_at": 1737356308,
    "inserted_at": 1737456308
  }
}
```

### 400

This error is returned when the provided alias ID is invalid, does not exist, or when the request is malformed.

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
