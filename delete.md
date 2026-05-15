# Delete Alias

## Request

Delete an existing alias by providing its unique ID in the request path. This action permanently removes the alias from the organization's records.

**Endpoint:**

```http
DELETE /organization/api/v1/alias/delete/:id
```

**URL:**

```text
https://link-mytv.quickom.com/organization/api/v1/alias/delete/:id
```

## Path Parameters

| Parameter | Type | Required | Description | Example |
| --- | --- | --- | --- | --- |
| **id** | string | Yes | Unique identifier of the alias to be deleted. | `"abc123xyz"` |

## Headers

| Header | Type | Required | Description | Example |
| --- | --- | --- | --- | --- |
| **Authorization** | string | Yes | Bearer token used for authentication. | `"BearereyJhbGciOiJIUzl1NilsLnR5cCi6....."` |

### Example Request

```http
DELETE /organization/api/v1/alias/delete/12345 HTTP/1.1
Host: link-mytv.quickom.com
Authorization: Bearer <your_token>
```

## Responses

### 200

Alias deleted successfully.

**Response Body**

| Parameter | Type | Description | Example |
| --- | --- | --- | --- |
| **message** | string | Confirmation message of the action. | `"Alias deleted successfully"` |

**Example**

```json
{
  "message": "Alias deleted successfully."
}
```

### 401

Returned when the request is made without a valid authentication token.

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| **error** | object | Contains the error details object. | `"UNAUTHORIZED"` |

### Example Response

```json
{
  "error": {
    "message": "UNAUTHORIZED"
  }
}
```

### 404

Returned when the alias with the provided ID does not exist.

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| **error** | object | Contains the error details object. | `"NOT FOUND"` |

### Example

```json
{
  "error": {
    "message": "Not found"
  }
}
```
