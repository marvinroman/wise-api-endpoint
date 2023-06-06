# Wise API Endpoint

## Authentication

API Documentation - GET Method (action=fetchTasks)

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
