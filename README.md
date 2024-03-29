# Wise API Endpoint

The API provides functionality to interact with Wise Systems routing software, including driver information, inventory details, departure and arrival timestamps, signature images, related images, and delivery status.

## OAuth 2.0 Client Credentials Flow

You can use the client credentials flow with OAuth 2.0. The client credentials flow is machine-to-machine and does not require any user interaction. Administrators and users with the OAuth 2.0 Authorized Applications Management permission can set up the flow, upload and revoke certificates for applications on the OAuth 2.0 Client Credentials (M2M) Setup page.

The OAuth 2.0 client credentials flow consists of a POST request to the token endpoint and a system response containing an access token.

### Requirements

* Activate **OAuth 2.0** within Netsuite.  
* Create roles with appropriate permissions for the RESTlet to perform the necessary calls and grant is OAuth permission and add the role to a user.  
* Create and integration with OAuth 2.0 client (M2M) permissions.
* Integrator must create a certificate and pass us the public portion this is used to sign the JWT token in the authorization request ([more information](https://229676-sb1.app.netsuite.com/app/help/helpcenter.nl?fid=section_162686838198.html)). `openssl req -x509 -newkey rsa:4096 -sha256 -keyout auth-key.pem -out auth-cert.pem -nodes -days 730.`  
* Create a new OAuth 2.0 Client Credentials Setup.  

### POST Request to the Token Endpoint and the Access Token Response

The client credentials flow starts when the application sends a POST request to the token endpoint.

The format of the URL is:

```
https://<accountID>.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token
```

where `<accountID>` represents your NetSuite account ID.

### POST Request Parameters

| Request Parameter | Description |
| ------- | -------- | 
| `grant_type` | The value of the `grant_type` parameter is always `client_credentials`. |
| `client_assertion_type` | The value of the `client_assertion_type` parameter is always `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| `client_assertion` | The value of the `client_assertion` parameter is a JWT bearer token. The token is signed with the private part of the certificate used for mapping of the application. | 

The following example provides a sample POST request:

```bash
POST /services/rest/auth/oauth2/v1/token HTTP/1.1
Host: <accountID>.suitetalk.api.netsuite.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiIxYzM0M2E3MTZjMWRjZWI2MGU3ZmMxNDlmYTY3MzU5MjllZjc3ZDI4ZmUxNjI5M2Y4OTI5NzZkZGU3ZDhlM2UyIiwic2NvcGUiOlsicmVzdGxldHMiLCAicmVzdF93ZWJzZXJ2aWNlcyJdLCJhdWQiOiJodHRwczovL3J1bmJveC5jb3JwLm5ldHN1aXRlLmNvbS9zZXJ2aWNlcy9yZXN0L2F1dGgvb2F1dGgyL3YxL3Rva2VuIiwiZXhwIjoxNjI3OTA5MzAzLCJpYXQiOjE2Mjc5MDU3MDN9.j7fhtd0qQP-iD7ns9q_fuG8Arz2aWJyoSvZ8sHRVA8HXOJG3pAQbT5J5F8MLkWIXA9ZuSxHdCWNwQLoRUeKlGURYFFqDHP_yjoWFWWtq5Wb-AnaZg_jBVL8TaOFGY2WByFM8rHsJVopFegwEQsU6bkcwqiFttEKxso-MiSAc5lE9SBgi6Fus2btiYGIFcNrKalFXEWDy6Ah5yVCo3wxkk9dfiPmT6JgLdjFkCc3v7tMCD9CrRHXrmhQvL8aoeyTMzJILURw5rnuy9zAs9ngymtX_iiwes8XpkBeCJbX4totI-EY4myi7L4fc2NgeWT-bvLWo6_sWjXE4BKyewqjtreUJscR9bhJ5Fi7S8nIoGDQbZrwhIgoKM_UI9Waw6kRLwRer_c0QDFY-sMLeGT3HL5vihHRFNXd-cKb-AWplkRiSJrdHXJtuGHLniHRpkK0-A1AFalIzYw4SSykxfck0qsPdf-oFPuawUsKR9lDCcYlyOaDZdQsBNsbjOsp5gGtyCuBwPBS8xz7I6gqLVEfNuzTfDDk8SMw1fN9MQ0NJtZMqMxm-WY_bLjZVkI3gqsvgDS-ADBPC7cymVZGfPUqummDUeG-Ks7SkLaHpfY6i-aZS8KUAY4aN5Do3GWT56aoEM9s1YB_1ZF_YxsBmK_gcX_mmlwUxbvCVpuHJTvKAQzY
```

### The Access Token Response

| JSON Response Fields | Description | 
| ---- | ---- |
| `access_token` | The value of the `access_token` parameter is in JSON Web Token (JWT) format. The access token is valid for 60 minutes. | 
| `expires_in` | The value of the `expires_in` parameter is always 3600. The value represents the time period during which the access token is valid, in seconds. |
| token_type | The value of the `token_type` parameter is always **bearer**. | 

The following is an example of a response in JSON JWT format:

```json
{"access_token": "eyJraWQiOiJzLlNZU1RFTS4yMDIwXzEiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0MDMwMDU5OzA7Mzs3O04iLCJhdWQiOlsiOWE1MDY4YjFjNGU5OGU4Yjg1YzMwMmYyMjg2N2YzNTAyYTBmYzYwNzU4MDQwNzliNzYzZmExYzg2NzJiYTlkNCIsImFwcDoyNzBDNDQ3Ny1DNUY1LTRFMDQtQkNDMS1CMDMzRDk0QTlDMDgiXSwic2NvcGUiOlsicmVzdF93ZWJzZXJ2aWNlcyIsInJlc3RsZXRzIl0sImlzcyI6Imh0dHBzOlwvXC9ydW5ib3guY29ycC5uZXRzdWl0ZS5jb20iLCJvaXQiOjE2Mjc5MDYzMzAsImV4cCI6MTYyNzkwOTkzMCwiaWF0IjoxNjI3OTA2MzMwLCJqdGkiOiJhLmMuZmI3ZDYzN2YtOTdjNC00Nzk0LTkyYWYtZTU2N2ZhYjc1ODRlLjE2Mjc5MDYzMzA1MDMuMTYyNzkwNjMzMDUwMyJ9.QjzADDeU2yN-6j-ol0fApgmleIn17HHD4bi06yBYpEpL5rBSbK3h11-GgU44Kc6ujQQQ3t4yr6IWBrtak5qLPWQmJE5-Ry_IvaxZRmPuB8rxI09_o4uXJE7oxpMreK4snYoIfH1Ph40Fq977MVVz9K-5pCTclOberX9dTTM3O0BnL6QNrf3lv3RA7J5LilceGAm4OV7OOoddn_fB6yeO0ZghVbJbRgI-tChqwdmWY42zhTeHjdG4K6ooA2IVcOm2GUFMhiFT2I00ZLZ-dYBPYkfRDn2Fvbn8V8GN1biQ6_u6j07k0XSq1Mv-WN-saH7rTKaA1gkX4IFwIHzN7eJUcg", "expires_in": "3600", "token_type": "Bearer"}
```

When the access token expires, the token endpoint returns an `invalid_grant` error. The application must restart the flow.

### The Request Token Structure

The JWT bearer token for the POST request to the token endpoint in OAuth 2.0 client credentials flow includes three parts: a header, a payload, and a signature.

The token header includes the following parameters:

| Parameter Name | Description |
| ----- | ---------- |
| `typ` | The value of the `typ` parameter is always **JWT**. | 
| `alg` | The value of the `alg` parameter is **PS256**, **PS384**, **PS512**, **ES256**, **ES384**, or **ES512**.<br>The value you choose determines the algorithm used for signing of the token. |
| `kid` | The value of the `kid` parameter is the value of the Certificate ID generated during the mapping of the application. |

The token payload includes the following parameters:

| Parameter Name | Description | 
| ---- | --------- | 
| `iss` | The value of the `iss` parameter is the client ID for the integration. | 
| `scope` | The value of the `scope` parameter is **restlets**, **rest_webservices**, **suite_analytics**, or all of them, separated by a comma. | 
| `aud` | The value of the `aud` parameter is the NetSuite token endpoint, `https://<accountID>.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token`. |
| `exp` | The value of the `exp` parameter represents the number of seconds since January 1, 1970, until the token’s expiration.<br>The value must be less than 60 minutes greater than the value of the `iat` parameter. |
| `iat` | 
The value of the `iat` parameter represents when the token was issued. The value of the parameter is in seconds, since January 1, 1970. | 

### Response Codes

The following response codes may be encountered during the authentication process:

- 200 OK: The authentication request was successful, and an access token was obtained.
- 401 Unauthorized: The access token is invalid or has expired. Repeat the authentication flow to obtain a new token.
- 400 Bad Request: The authentication request was malformed or contained invalid parameters.
- 500 Internal Server Error: An error occurred on the server while processing the authentication request.


## GET Method (query=tasks)

### Endpoint

```
GET https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1&query=tasks&date=[date]&warehouse=DCP Warehouse
```

### Description:
This API endpoint is used to retrieve task information. By making a GET request to this endpoint with the specified action parameter, you will receive a JSON response containing details about the tasks.

### Parameters:
- `action` (required, string): The action parameter must be set to `tasks`.
- `date` (optional, string): The date string of the desired delivery date. Formatted as ["M/D/YYYY"](https://momentjs.com/docs/#/displaying/format/). 
- `warehouse` (optional, string): The warehouse that you are pulling deliveries for. Current valid values are `DCP Warehouse` or `LORT Warehouse`  
- `type` (optional, string): The type of route. Current valid values are `Delivery` or `Pickup`

### Response:
The response to the API request will be a JSON object.

### Properties:
- `entity` (object): Information about the entity associated with the task this wil be an Agency for distributions and Vendor on pickups.
  - `id` (number): The ID of the entity.
  - `name` (string): The name of the entity.
- `type` (string): The type of route, Delivery or Pickup.  
- `address` (object): The address associated with the task.
  - `addr1` (string): Address line 1.
  - `addr2` (string|null): Address line 2.
  - `city` (string): The city.
  - `state` (string): The state.
  - `zip` (string): The ZIP code.
- `contact` (object): Contact information related to the task.
  - `phone` (string): The phone number.
  - `name` (string): The name of the contact.
- `start` (string): The start time of the task in format ["M/D/YYYY hh:mm a"](https://momentjs.com/docs/#/displaying/format/).
- `end` (string): The end time of the task in format ["M/D/YYYY hh:mm a"](https://momentjs.com/docs/#/displaying/format/).
- `weight` (number): The weight associated with the task in lbs.
- `transaction` (object): Information about the transaction related to the task.
  - `id` (number): The ID of the transaction.
  - `display` (string): A display string for the transaction.
- `warehouse` (string): String representation of appointment area the route is related to.  
- `memo` (string|null): A memo associated with the task.
- `delivery_message` (string|null): A delivery message for the task.
- `route` (numeric|null): ID of the route to use.
- `inventory` (array): An array of inventory items associated with the task. Will be empty for type `pickup`. 
  - `id` (number): The internal id of the record that tracks pallets.  
  - `tracking` (string): The tracking number of the inventory item.
  - `storage_type` (string): The type of storage for the inventory item.
  - `weight` (number): The weight of the inventory item in lbs.

Note: Replace "YYYY-MM-DD" and other placeholder values with the actual data in the response.

### Example Request:
```
GET https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1&query=tasks
```

### Example Response:
```json
[
    {
        "entity": {
            "id": 1234,
            "name": "Local food distributor"
        },
        "type": "Delivery",
        "address": {
            "addr1": "1234 Main St",
            "addr2": "",
            "city": "Main City",
            "state": "VA",
            "zip": "12345"
        },
        "contact": {
            "phone": "123-123-1234",
            "name": "Jane Lane"
        },
        "start": "6/10/2023 08:00 am",
        "end": "6/10/2023 10:00 am",
        "weight": 1100,
        "transaction": {
            "id": 123789,
            "display": "SO12345"
        },
        "warehouse": "DCP Warehouse",
        "memo": "Ship Date: 02/04/2023 8:00 am",
        "delivery_message": "Please use side door.",
        "route": 1234,
        "inventory": [
            {
                "id": 8,
                "tracking": "F001115329",
                "storage_type": "Refrigerated",
                "weight": 500
            },
            {
                "id": 9,
                "tracking": "F001115330",
                "storage_type": "Dry",
                "weight": 600
            }
        ]
    }
]
```

### Response Status Codes:
- 200 OK: The request was successful, and the task information is included in the response.
- 400 Bad Request: The request is missing the required parameters, contains invalid data, or there is a script error.
- 500 Internal Server Error: An error occurred on the server while processing the request.


## GET Method (query=drivers)

### Endpoint

```
GET https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1&query=drivers
```

### Description

Retrieves a list of drivers based on the specified query parameter.

### Parameters

- `query` (string, required): The query parameter to return drivers.

### Response

The response will be a JSON object with the following properties:

- `id` (number): The internalid of the driver's employee record.
- `name` (string): The name of the driver.
- `termination_date` (string|null): Termination date of the driver should be removed from routing if it is not empty.
- `available_routes` (array): An array of available routes for the driver.

### Example Response:

```json
[
    {
        "id": 1234,
        "name": "Rob Bob",
        "termination_date": "5/28/2021",
        "available_routes": [
            "CSFP WH",
            "DC Puerto Rico WH",
            "Lorton 2 WH"
        ]
    }
]
```

### Example Request

```
GET https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1&query=drivers
```

### Response Status Codes

- 200 OK: The request was successful, and the drivers matching the query parameter are included in the response.
- 400 Bad Request: The request is missing the required query parameter or contains invalid data.
- 500 Internal Server Error: An error occurred on the server while processing the request.

## GET Method (query=calculateDaysFromDate)

### Description

Returns a date that is `n` business days from the given `date`.

### Parameters

- `date` (date|optional): Specifies the starting date in the format ["YYYY-MM-DD"](https://momentjs.com/docs/#/displaying/format/). If not provided, the current date is used.
- `days` (number|optional): Specifies the number of days to calculate. Default is 3.
- `exclude_holidays` (boolean|optional): Specifies whether to exclude holidays from day calculations. Valid values are "true" or "false". Default is "true".
- `weekend_days` (string|optional): Specifies the weekend days as comma-separated values. Default is "0,6" (Saturday and Sunday).

### Response

The API returns the date that is `n` days from `date`.

### Example Response:

```
2023-06-27
```

### Example Request

```
GET https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1&query=calculateDaysFromDate
```

### Response Status Codes

- 200 OK: The request was successful, and the drivers matching the query parameter are included in the response.
- 400 Bad Request: The request is missing the required query parameter or contains invalid data.
- 500 Internal Server Error: An error occurred on the server while processing the request.


## POST

### Endpoint

```
POST https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1
```

### Description

Creates new packing pallets with items.  

### Request Body

The request body should be a JSON object with the following properties:

- `order_id` (number): The identifier for the order.  
- `tracking_number` (string): The pallets tracking number or barcode.
- `shipmethod` (string): The shipping method for the order in text form.
- `driver_notes` (string): Driver's notes, may be up to 4000 characters.  
- `items` (array): Items included on the pallet.
  - `item` (number): The identifier for the item.
  - `quantity` (number): Quantity of items on the pallet.

### Example Request Body:

```json
[
    {
        "order_id": "6367174",
        "tracking_number": "F001139254",
        "shipmethod": "3 - DC - DRIVER",
        "items": [
            {
                "item": "2654",
                "quantity": "20.00000"
            },
            {
                "item": "4520",
                "quantity": "1.00000"
            }
        ]
    }
]
```

## PUT

### Endpoint

```
PUT https://229676-sb1.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=1110&deploy=1
```

### Description

Updates an existing delivery record with the provided data.

### Request Body

The request body should be a JSON object with the following properties:

- `id` (number|null): The identifier of the inventory pallet sent in the `inventory` array of `delivery` type orders. Don't include for `pickup` type orders.  
- `transaction` (number|null): The `transaction`.`id` of for `pickup` type orders. Don't include for `delivery` type orders.
- `driver` (number): The netsuite internalid of the employee record for the driver responsible for the delivery.
- `driver_notes` (string): Driver's notes, may be up to 4000 characters.  
- `departure` (string): The departure timestamp in format ["M/D/YYYY hh:mm a"](https://momentjs.com/docs/#/displaying/format/).
- `arrival` (string): The arrival timestamp in format ["M/D/YYYY hh:mm a"](https://momentjs.com/docs/#/displaying/format/).
- `signature` (string): The URL of the signature image for the delivery.
- `signature_name` (string): The typed name of the person signing for the pallet.  
- `image` (string): The URL of an image related to the delivery.
- `status` (number): The status of the delivery (`1`: Assigned, `2`: In Progress, `3`: Completed, `4`: Rejected).
- `reject_reason` (number): If the satus of the pallet is rejected reason code should be included  
  - `1`: Bad Product: Damaged in Transit  
  - `2`: Bad Product: Quality  
  - `3`: Cancelled due to unforeseen circumstances (not weather related)  
  - `4`: Cancelled Due to Weather  
  - `5`: Could not contact customer at time of delivery  
  - `6`: Emergency  
  - `7`: Lack of staffing to accept delivery  
  - `8`: No Room to Store  
  - `9`: Order didn't arrive at the delivery window given  
  - `10`: Order Not Picked Up (for Pick Ups only)  
  - `11`: Specific product not ordered by agency  
  - `12`: Wasn't expecting a shipment  
  - `13`: Grant Expired/Lack of Funding  
  - `14`: Product not distributed - CSFP Only  
  - `15`: Left by Driver in the Warehouse  
  - `16`: Unknown - Found in Warehouse  

### Example Request Body:

```json
[
    {
        "id": 9,
        "transaction": 12345,
        "driver": 5678,
        "departure": "2/4/2023 10:00 am",
        "arrival": "2/4/2023 12:00 pm",
        "signature": "https://s3.amazon.com/567zs567",
        "signature_name": "Abigail Quinn",
        "image": "https://s3.amazon.com/567zs568",
        "status": 3,
        "reject_reason": 1
    }
]
```

### Response Status Codes

- 200 OK: The delivery record was successfully updated.
- 400 Bad Request: The request body is missing or contains invalid data.
- 404 Not Found: The specified delivery does not exist.
- 500 Internal Server Error: An error occurred on the server while processing the request.
