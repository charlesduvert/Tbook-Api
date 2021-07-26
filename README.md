# T-book Web Service API

Our Web Service API provides users with fast and reliable access to T-book data. You can use the T-book Web Service API to retrieve and manage T-book content.

If you have any issues or queries, please contact [Esther Lévy](mailto:esther.levy@le-co.fr)

## Getting Started

This section describes how to access our Web Service API.

### Prerequisites

First, you need to generate your API credentials : 

* Login to your T-book client account
  - [Test platform](https://test.t-book.fr)
  - [Live platform](https://t-book.fr)
* Go to the API section. If you can't find this section, please [contact us](mailto:alban.miconnet@digitaluniversity.education), we will grant you access. 
* On the API page of your account, you will find your Client ID, and will be able to generate your Client Secret. **BEWARE : you will only be able to see your Client Secret this one time!** Store precautiously your credentials, they will be needed in any request to the API. You will however be able to regenerate your Client Secret if needed.

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
  "expires_in":3600
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
| 400 | bad_request | The request was unacceptable, often due to missing a required parameter. |
| 401 | unauthorized | No valid API key provided. |
| 402 | request_failed | The parameters were valid but the request failed. |
| 403 | forbidden | The requested data is not accessible to you. |
| 404 | not_found | The requested resource doesn't exist. |
| 405 | method_not_allowed | The HTTP method used isn't supported for this endpoint. |
| 409 | conflict | The request conflicts with another request (perhaps due to using the same idempotent key). |
| 429 | too_many_requests | Too many requests hit the API too quickly. We recommend an exponential backoff of your requests. |
| 500, 502, 503, 504 | server_errors | Something went wrong on our end. |

## Methods

* [Client & Entities](#client--entities)
	- [Get client](#get-client)
	- [Create new entity](#create-new-entity)
	- [Get all entities](#get-all-entities)
	- [Get one entity](#get-one-entity)
	
* [Learners](#learners)
	- [Get all learners](#get-all-learners)
	- [Get one learner](#get-one-learner)
	- [Insert a learner](#insert-a-learner)
	- [Update learner informations](#update-learner-informations)
	- [Remove a learner from Session](#remove-a-learner-from-session)
	
* [Users authentification](#users-authentification)

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
Not available yet

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
## Learners

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
```
curl -X POST /insertLearner \
  -H 'authorization: Bearer your_access_token' \
  -F 'entity[identifier]=24' \
  -F 'entity[create]=true' \
  -F 'login=login2@api.fr' \
  -F 'password=1F0k7h6g' \
  -F 'civility=mme' \
  -F 'lastname=Doe' \
  -F 'firstname=Jane' \
  -F 'code=23563' \
  -F 'jobTitle=Communication Manager' \
  -F 'seniority=3' \
  -F 'level=Master' \
  -F 'contactEmail[0]=contact@gmail.fr' \
  -F 'contactEmail[1]=contact2@gmail.fr' \
  -F 'phone=+33600000000' \
  -F 'filter[0]=Session 1' \
  -F 'filter[1]=May 2018' \
  -F 'location[0]=Paris' \
  -F 'country[0]=France' \
  -F 'testUser=0' \
  -F 'externalID=23' \
  -F 'assignation=0' \
```
**Parameters**

| Name | Required | Type | Description |
| - | - | - | - |
| entity[identifier] | true | int/string | The ID (int) or name (string) of the entity. Refer to the [entity section](#client--entities) |
| entity[create] | false | int | Default : false. Options are : `0` (false) , `1` (true). If `entity[identifier]` is a string, and `entity[create]` is true, then the entity will be created when it doesn't exist. |
| login | true | string | Your learner's login. E-mail format, has to be unique. |
| password | false | string | Automatically generated if left blank |
| civility | true | string | Options are : `m` , `mme` |
| lastname | true | string | Lastname |
| firstname | true | string | Firstname |
| code | false | string | Optionnal. Alpha-numeric code. |
| jobTitle | false | string | Job title |
| seniority | false | string | Seniority |
| level | false | string | Degree level |
| contactEmail[x] | false | string | Contact e-mail(s) |
| phone | false | string | Phone number |
| filter[x] | false | string | Filters can help you attach a learner to a session.  Refer to the [Session section](#sessions) |
| location[x] | false | string | Locations can help you attach a learner to a session.  Refer to the [Session section](#sessions) |
| country[x] | false | string | Country |
| testUser | false | int | Default : `0`. Options are : `0` (false) , `1` (true). If true, this user's data will not be taken into account in the global stats |
| externalID | false | int | The user's ID on your platform. Required if you want to connect your user using the ID option. |
| assignation | false | int | Default : `0`. Options are : `0` (false) , `1` (true). If set to `1`, the learner, once created, will automatically be assigned to the course(s) corresponding the the learner's filter(s) and location(s). |

### Update learner informations ###
```
curl -X POST /updateLearner \
  -H 'authorization: Bearer your_access_token' \
  -F 'identifierType=email' \
  -F 'identifier=login2@api.fr' \
  -F 'password=1F0k7h6g' \
  -F 'civility=mme' \
  -F 'lastname=Doe' \
  -F 'firstname=Jane' \
  -F 'code=23563' \
  -F 'jobTitle=Communication Manager' \
  -F 'seniority=3' \
  -F 'level=Master' \
  -F 'contactEmail[0]=contact@gmail.fr' \
  -F 'contactEmail[1]=contact2@gmail.fr' \
  -F 'phone=+33600000000' \
  -F 'filter[0]=Session 1' \
  -F 'filter[1]=May 2018' \
  -F 'location[0]=Paris' \
  -F 'country[0]=France' \
  -F 'testUser=0' \
  -F 'externalID=23' \
  -F 'assignation=0' \
```
**Parameters**
All parameters are optional. We will only update the parameters that are posted.

| Name | Required | Type | Description |
| - | - | - | - |
| identifierType | true | string | Default : email. Options are : `email`, `learnerId`, `externalId`. `learnerId` and `externalId` are integers. `learnerId` is teh learner's id on our application and `externalId` is a parameter that was added when the learner was created (Refer to the [Insert a learner](#insert-a-learner)). |
| identifier | true | string | Your learner's identifier, depends on `identifierType`. |
| password | false | string | Password |
| civility | false | string | Options are : `m` , `mme` |
| lastname | false | string | Lastname |
| firstname | false | string | Firstname |
| code | false | string | Optionnal. Alpha-numeric code. |
| jobTitle | false | string | Job title |
| seniority | false | string | Seniority |
| level | false | string | Degree level |
| contactEmail[x] | false | string | Contact e-mail(s) |
| phone | false | string | Phone number |
| filter[x] | false | string | Filters can help you attach a learner to a session.  Refer to the [Session section](#sessions) |
| location[x] | false | string | Locations can help you attach a learner to a session.  Refer to the [Session section](#sessions) |
| country[x] | false | string | Country |
| testUser | false | int | Default : `0`. Options are : `0` (false) , `1` (true). If true, this user's data will not be taken into account in the global stats |
| externalID | false | int | The user's ID on your platform. Required if you want to connect your user using the ID option. |
| assignation | false | int | Default : `0`. Options are : `0` (false) , `1` (true). If set to `1`, the learner will automatically be assigned to the course(s) corresponding the the learner's filter(s) and location(s). |


### Remove a learner from Session ###
```
curl -X POST /removeLearnerSession \
  -H 'authorization: Bearer your_access_token' \
  -F 'identifierType=email' \
  -F 'identifier=login2@api.fr' \
  -F 'filter=Session 1' \
  -F 'location=Paris' \
```
**Parameters**

| Name | Required | Type | Description |
| - | - | - | - |
| identifierType | true | string | Default : email. Options are : `email`, `learnerId`, `externalId`. `learnerId` and `externalId` are integers. `learnerId` is teh learner's id on our application and `externalId` is a parameter that was added when the learner was created (Refer to the [Insert a learner](#insert-a-learner)). |
| identifier | true | string | Your learner's identifier, depends on `identifierType`. |
| filter | true | string | The filter of the session that the learner should be removed from. Refer to the [Session section](#sessions) |
| location | true | string | The location of the session that the learner should be removed from. Refer to the [Session section](#sessions) |


## Users authentification

We provide two types of user authentification : login & ID.

**Login** : the unique e-mail-formatted login, sent during the Insert User request. Note that this login can never be modified on our platform.

**ID** : the externalID field optionnally sent during the Insert User request. The user's ID on your platform.


```
curl -X POST /authToken \
  -H 'authorization: Bearer your_access_token' \
  -F 'userType=1' \
  -F 'connectionType=login' \
  -F 'identifier=login2@api.fr' \
  -F 'errorUrl=http://example.com/error' \
```

**Parameters**

| Name | Required | Type | Description |
| - | - | - | - |
| userType | true | int | Options are : `1` (learner), `2` (teacher) |
| connectionType | true | string | Options are : `login`, `id`. |
| identifier | true | string | If connectionType == `login`, then enter the user's login (e-mail format). If connectionType == `id`,  then enter the ID attached to the user on your platform |
| errorUrl | true | string | The redirection url if the request generated an error |

**Response example**
```
{
	"success": "1",
	"authentificationUrl" : "http://t-book.fr/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfaWQiOiJiMDNiMmZjNDlhYTc1YjMzN2RhYzUwNTVhIiwiaWR0Ym9va19jbGllbnQiOiI3IiwiaWF0IjoxNTMwNjMxNzMyLCJleHAiOjE1MzA2MzUzMzJ9.WI8bgP--I3-5MAi1zm4HtBqedfJ5Tomta7lf0zMYCgU",
}
```

Then, redirect the user to the `authentificationUrl`. He will be connected to the T-book platform. This link expires after 60 seconds.

In the event of a problem, we will redirect the user to the `errorUrl`. 



## Learning Tools Interoperability - LTI
You can use LTI as a SSO system. 
To do so, you should send your data to a launch URL (as described below). If the user already exists, he will be connected seamlessly, else, he will be created on our platform and connected.

**N.B. :** We do not use the `Ressource ID` field, as we use LTI to access a platform and not a particular context.

### Registration settings ###
**Launch URL :**
  - Test platform : https://test.t-book.fr/api/lti
  - Live platform : https://t-book.fr/api/lti
  
**Consumer key & Shared secret** : please refer to the [Prerequisites section](#prerequisites). 

Warning : your `Shared Secret` is the one below the `Client Secret`, following the `For LTI` mention.


### Custom parameters ###
You should add to the custom parameters field a json string, in a variable named `custom_data`, as below. 

| Name | Required | Type | Description |
| - | - | - | - |
| entityID | true | int | The ID of the entity. Refer to the [entity section](#client--entities) |
| connectionType | true | string | Options are : `login` or `id` (we will use the externalID)  |
| login | true | string | Your learner's login. E-mail format, has to be unique. |
| externalID | false | int | The user's ID on your platform. Required if you want to connect your user using the ID option. |
| password | false | string | Automatically generated if left blank |
| civility | true | string | Options are : `m` , `mme` |
| lastname | true | string | Lastname |
| firstname | true | string | Firstname |
| code | false | string | Optionnal. Alpha-numeric code. |
| jobTitle | false | string | Job title |
| seniority | false | string | Seniority |
| level | false | string | Degree level |
| contactEmail | false | string | Contact e-mail(s) |
| phone | false | string | Phone number |
| filter | false | string | Filters can help you attach a learner to a session.  Refer to the [Session section](#sessions) |
| location | false | string | Locations can help you attach a learner to a session.  Refer to the [Session section](#sessions) |
| country | false | string | Country |
| testUser | false | int | Default : `0`. Options are : `0` (false) , `1` (true). If true, this user's data will not be taken into account in the global stats |
| assignation | false | int | Default : `0`. Options are : `0` (false) , `1` (true). If set to `1`, the learner, once created, will automatically be assigned to the course(s) corresponding the the learner's filter(s) and location(s). |


### Custom parameters example ###
Please minify your json

```
data = 
{
	"entityID":"24",
	"connectionType": "id",
	"login": "john.doe@gmail.com",
	"externalID": "2390",
	"password": "#thepass#",
	"civility": "m",
	"lastname": "Doe",
	"firstname": "John",
	"code" : "23563",
	"jobTitle": "Communication officer",
	"seniority": "3",
	"level": "Master",
	"contactEmail": ["email@email.fr","email2@email.fr"],
	"phone": "+33600000000",
	"filter": ["Session 1", "May 2018"],
	"location": ["Paris"],
	"country": ["France"],
	"testUser":0,
	"assignation":1
}
```


### Debug option ###
If you need a debug option for your tests, you can associate the value `1` to the `custom_debug` field. We will display your request.
