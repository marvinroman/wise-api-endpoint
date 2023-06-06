# Wise API Endpoint

## Token Authentication for NetSuite RESTlet

Token-based authentication is used to authenticate and authorize requests made to a NetSuite RESTlet. This authentication mechanism involves generating an access token and including it in the request headers to authenticate the API calls.

### Prerequisites

Before using token authentication, ensure you have the following:

- NetSuite account with RESTlet access enabled
- Valid API credentials (email, password, account ID)
- RESTlet script deployed in your NetSuite account

### Authentication Flow

The token authentication flow consists of the following steps:

1. Generate an Access Token: To obtain an access token, you need to call the NetSuite Token-Based Authentication API with your API credentials. This API request will return an access token, which is a long-lived token that you can use to authenticate subsequent requests.

2. Include the Access Token in Request Headers: After obtaining the access token, you should include it in the `Authorization` header of your RESTlet API requests. The header should be formatted as follows:

```
Authorization: Bearer <access_token>
```

Replace `<access_token>` with the actual access token value obtained in step 1.

### Example

Here's an example of how to authenticate using token authentication for a NetSuite RESTlet:

1. Generate an Access Token:

Make a `POST` request to the NetSuite Token-Based Authentication API endpoint:

```
POST /services/rest/auth/oauth2/v1/token
```

The request body should include the following parameters:

- `grant_type`: Set this to `password`.
- `clientId`: Your NetSuite integration's client ID.
- `clientSecret`: Your NetSuite integration's client secret.
- `username`: Your NetSuite account username (email).
- `password`: Your NetSuite account password.
- `scope`: Set this to `restlets`.

The response will include an access token that you'll use in subsequent API requests.

2. Include the Access Token in Request Headers:

For each RESTlet API request you make, include the access token in the `Authorization` header:

```
GET /services/rest/record/v1/customer/1234
Authorization: Bearer <access_token>
```

Replace `<access_token>` with the actual access token value obtained in step 1.

### Error Handling

If the access token expires or becomes invalid, you'll receive an HTTP `401 Unauthorized` response. In such cases, repeat the authentication flow to obtain a new access token and update your requests with the new token.

### Security Considerations

- Keep your API credentials (client ID, client secret, account credentials) secure and avoid exposing them in client-side applications.
- Protect the access token by using secure channels (HTTPS) and not including it in publicly accessible code or repositories.
- Consider using token expiration and refresh mechanisms to ensure the security of your authentication flow.

### Response Codes

The following response codes may be encountered during the authentication process:

- 200 OK: The authentication request was successful, and an access token was obtained.
- 401 Unauthorized: The access token is invalid or has expired. Repeat the authentication flow to obtain a new token.
- 400 Bad Request: The authentication request was malformed or contained invalid parameters.
- 500 Internal Server Error: An error occurred on the server while processing the authentication request.


## GET Method (action=fetchTasks)

Endpoint:
```
GET /tasks?action=fetchTasks
```

Description:
This API endpoint is used to retrieve task information. By making a GET request to this endpoint with the specified action parameter, you will receive a JSON response containing details about the tasks.

Parameters:
- `action` (required, string): The action parameter must be set to "fetchTasks".

Response:
The response to the API request will be a JSON object with the following structure:

```json
{
    "shipdate": "YYYY-MM-DD",
    "entity": {
        "id": 1234,
        "name": "Local food distributor"
    },
    "address": {
        "addr1": "1234 Main St",
        "addr2": "",
        "city": "Main City",
        "state": "VA",
        "zip": "12345"
    },
    "contact": {
        "phone": "123-123-1234",
        "email": "user@company.com",
        "name": "Jane Lane"
    },
    "start": "2023-02-04T08:00:00.000Z",
    "end": "2023-02-04T10:00:00.000Z",
    "weight": 1100,
    "transaction": {
        "id": 123789,
        "display": "SO12345"
    },
    "memo": "Ship Date: 02/04/2023 8:00 am",
    "delivery_message": "Please use side door.",
    "inventory": [
        {
            "tracking": "123B-984U-E573",
            "storage_type": "refrigerated",
            "weight": 500
        },
        {
            "tracking": "123B-984U-E574",
            "storage_type": "dry",
            "weight": 600
        }
    ]
}
```

Properties:
- `shipdate` (string): The date of shipment in the format "YYYY-MM-DD".
- `entity` (object): Information about the entity associated with the task.
  - `id` (number): The ID of the entity.
  - `name` (string): The name of the entity.
- `address` (object): The address associated with the task.
  - `addr1` (string): Address line 1.
  - `addr2` (string): Address line 2.
  - `city` (string): The city.
  - `state` (string): The state.
  - `zip` (string): The ZIP code.
- `contact` (object): Contact information related to the task.
  - `phone` (string): The phone number.
  - `email` (string): The email address.
  - `name` (string): The name of the contact.
- `start` (string): The start time of the task in UTC format.
- `end` (string): The end time of the task in UTC format.
- `weight` (number): The weight associated with the task in lbs.
- `transaction` (object): Information about the transaction related to the task.
  - `id` (number): The ID of the transaction.
  - `display` (string): A display string for the transaction.
- `memo` (string): A memo associated with the task.
- `delivery_message` (string): A delivery message for the task.
- `inventory` (array): An array of inventory items associated with the task.
  - `tracking` (string): The tracking number of the inventory item.
  - `storage_type` (string): The type of storage for the inventory item.
  - `weight` (number): The weight of the inventory item in lbs.

Note: Replace "YYYY-MM-DD" and other placeholder values with the actual data in the response.

Example Request:
```
GET /tasks?action=fetchTasks
```

Example Response:
```json
{
    "shipdate": "2023-06-01",
    "entity": {
        "id": 1234,
        "name": "Local food distributor"
    },
    "address": {
        "addr1": "1234 Main St",
        "addr2": "",
        "city": "Main City",
        "state": "VA",
        "zip": "12345"
    },
    "contact": {
        "phone": "123-123-1234",
        "email": "user@company.com",
        "name": "Jane Lane"
    },
    "start": "2023-02-04T08:00:00.000Z",
    "end": "2023-02-04T10:00:00.000Z",
    "weight": 1100,
    "transaction": {
        "id": 123789,
        "display": "SO12345"
    },
    "memo": "Ship Date: 02/04/2023 8:00 am",
    "delivery_message": "Please use side door.",
    "inventory": [
        {
            "tracking": "123B-984U-E573",
            "storage_type": "refrigerated",
            "weight": 500
        },
        {
            "tracking": "123B-984U-E574",
            "storage_type": "dry",
            "weight": 600
        }
    ]
}
```

Response Status Codes:
- 200 OK: The request was successful, and the task information is included in the response.
- 400 Bad Request: The request is missing the required parameters or contains invalid data.
- 500 Internal Server Error: An error occurred on the server while processing the request.
