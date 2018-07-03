# T-book Web Service API

Our web service API provides users with fast and reliable access to your T-book data. You can use the T-book Web Service API to retrieve and manage T-book content.

## Getting Started

This section describes how to access our Web Service API.

### Prerequisites

First, you need to generate your API credentials : 

* Login to your T-book client account
  - [Test platform](https://test.t-book.fr)
  - [Live platform](https://t-book.fr)
* Go to the API section. If you can't find this section, please [contact us](mailto:alban.miconnet@digitaluniversity.education), we will have to grant you access. 
* On the API page of your account, you will find your Client ID, and be able to generate your Client Secret. **BEWARE : you will only be able to see your Client Secret this one time!** Store precautiously your credentials, they will be needed in any request to the API. You will however be able to regenerate your Client Secret if needed.

> Note : Your Client Secret is confidential. Avoid storing it in plain text. 

### Endpoints

All paths in the documentation are relative to the base API url. 
* **Test mode** : https://test.t-book.fr/api
* **Live mode** : https://t-book.fr/api

## Authentication

The access to the API is private. Therefore, you need to provide a JSON Web Token (JWT) as a mean of authentication. Tokens can be obtained by using the token endpoint with your API credentials.

### Request ###

Replace your Client ID and Client Secret with your credentials
```
curl -X POST /token \
  -F grant_type= client_credential \
  -F client_id= Your_Client_Id \
  -F client_secret= Your_Client_Secret \
```
### Response ###

A successful token response may look like the following
```
{
  "access_token":"MTQ0NjJkZmQ5OTM2NDE1ZTZjNGZmZjI3",
  "token_type":"bearer",
  "expires_in":3600,
}
```
## Error codes

Our API uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, etc.). Codes in the 5xx range indicate an error with our servers (seldom).

```json
{
  "error": {
    "code": "code",
    "message": "message"
  }
}
```

| HTTP status | Code | Description |
| - | - | - |
| 200 | ok | Everything worked as expected. |
| 400 | bad_request |	The request was unacceptable, often due to missing a required parameter. |
| 401 | unauthorized | No valid API key provided. |
| 402 | request_failed | The parameters were valid but the request failed. |
| 403 | forbidden | The requested data is not accessible to you. |
| 404 | not_found | The requested resource doesn't exist. |
| 405 | method_not_allowed | The HTTP method used isn't supported for this endpoint. |
| 409 | conflict	| The request conflicts with another request (perhaps due to using the same idempotent key). |
| 429 | too_many_requests |	Too many requests hit the API too quickly. We recommend an exponential backoff of your requests. |
| 500, 502, 503, 504 | server_errors	| Something went wrong on our end. |

## Methods
[Client & Entities](#client--entities)
	[Get client](#get-client)
	[Create new entity](#create-new-entity)
	[Get all entities](#get-all-entities)
	[Get one entity](#get-one-entity)

## Client & Entities

All resources (student, courses etc.) are attached to an entity. The client can administrate its entities. The client can have one or several entities. 

### Get client ###
```
curl -X GET /getClient \
  -H 'authorization: Bearer your_access_token' \
```
**Response example**
```
{
  "name":"Client Name",
  "logo":"path/logo.jpg",
  "description":"Nam ut risus a velit lobortis egestas convallis at dolor",
  "city":"Paris",
  "address":"2 avenue Charles de Gaulle",
  "zip":"75008"
}
```

### Create new entity ###
Not yet available

### Get all entities ###
```
curl -X GET /getEntities \
  -H 'authorization: Bearer your_access_token' \
```
**Response example**
```
[
	{
		"entityID":"24",
		"name":"Entity Name",
		"description":"Nam ut risus a velit lobortis egestas convallis at dolor",
		"address":"2 avenue Charles de Gaulle",
		"city":"Paris",
		"zip":"75008",
		"language":"FR"
	},
	...
]
```

### Get one entity ###
```
curl -X GET /getEntity/:entityID \
  -H 'authorization: Bearer your_access_token' \
```
**Response example**
```
{
	"entityID":"24",
	"name":"Entity Name",
	"description":"Nam ut risus a velit lobortis egestas convallis at dolor",
	"address":"2 avenue Charles de Gaulle",
	"city":"Paris",
	"zip":"75008",
	"language":"FR"
}
```
## Learner

Learners are attached to one entity.

### Get all learners ###

To retrieve all the learners of a client
```
curl -X GET /getAllLearners \
  -H 'authorization: Bearer your_access_token' \
```
To retrieve all the learners of an entity
```
curl -X GET /getAllLearners/:entityID \
  -H 'authorization: Bearer your_access_token' \
```
**Response example**
```
[
	{
		"learnerID": "1",
		"login" : "login@email.fr",
		"entity": {
			"entityID":"24",
			"entityName":"Entity n°1"
		},
		"civility": "m",
		"lastname": "Doe",
		"firstname": "John",
		"code" : "23563",
		"jobTitle": "Communication officer",
		"seniority": "3",
		"level": "Master",
		"contactEmail": ["email@email.fr"],
		"phone": "+33600000000",
		"filter": ["Session 1", "May 2018"],
		"location": ["Paris"],
		"country": ["France"],
		"testUser":0
	},
	...
]
```
### Get one learner ###
```
curl -X GET /getOneLearner/:learnerID \
  -H 'authorization: Bearer your_access_token' \
```
**Response example**
```
{
	"learnerID": "1",
	"login" : "login@email.fr",
	"entity": {
		"entityID":"24",
		"entityName":"Entity n°1"
	},
	"civility": "m",
	"lastname": "Doe",
	"firstname": "John",
	"code" : "23563",
	"jobTitle": "Communication officer",
	"seniority": "3",
	"level": "Master",
	"contactEmail": ["email@email.fr"],
	"phone": "+33600000000",
	"filter": ["Session 1", "May 2018"],
	"location": ["Paris"],
	"country": ["France"],
	"testUser":0
}
```
### Insert a learner ###


