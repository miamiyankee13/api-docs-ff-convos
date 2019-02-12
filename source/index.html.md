---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell


toc_footers:
  - <a href='https://github.com/miamiyankee13/medicine-cabinet'>View Medicine Cabinet on GitHub</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Medicine Cabinet API! You can use our API to access Medicine Cabinet API endpoints, which can interact with our database of medicinal cannabis strains, as well as manage your digital medicine cabinet.

# Registration
> To register with Medicine Cabinet, use this code:

```shell
curl -X POST
  https://medicine-cabinet.herokuapp.com/users
  -H 'Content-Type: application/json'
  -d '{
	"userName": "<your username>",
	"password": "<your password>",
	"firstName": "<your first name>",
	"lastName": "<your last name>"
}'
```

> The above command returns a 201 status code and a JSON object containing the user information like this:

```json
{
    "_id": "5c2a84e2ddca060017e13899",
    "firstName": "test",
    "lastName": "tester",
    "userName": "testuser",
    "strains": []
}
```
Medicine Cabinet uses JSON Web Token (JWT) to conrol access to the API. To obtain a token, you must have an account. To create an account, you can either register from the <a href='https://medicine-cabinet.herokuapp.com/'>client</a> or make a POST request to the `users` endpoint.

### HTTP Request

`POST https://medicine-cabinet.herokuapp.com/users`

### Required Fields
Field | Description
----- | -----------
userName | desired username
password | desired password
firstName | your first name
lastName | your last name

# Authentication

> To obtain a token, use this code:

```shell
curl -X POST 
  https://medicine-cabinet.herokuapp.com/auth/login
  -H 'Content-Type: application/json'
  -d '{
	"userName": "<your username>",
	"password": "<your password>"
}'
```
> The above command returns a 200 status code and a JSON object containing a token like this:

```json
{
    "authToken": "<token>"
}
```

Once you have an account, you must either login from the <a href='https://medicine-cabinet.herokuapp.com/'>client</a> or make a POST request to the `auth/login` endpoint in order to obtain a token.

### HTTP Request

`https://medicine-cabinet.herokuapp.com/auth/login`

### Required Fields
Field | Description
----- | -----------
userName | your username
password | your password

The returned token must be added in the header of any request to a protected endpoint via Bearer Authentication. The header should look as follows:

`Authorization: Bearer <token>`

<aside class="notice">
You must replace <code>&lt;token&gt;</code> with your token.
</aside>


# Strains

## Get All Strains

> To retrieve all strains, use this code:

```shell
curl -X GET 
  https://medicine-cabinet.herokuapp.com/strains
```

> The above command returns a 200 status code and a JSON object containing an array of all existing strains like this:

```json
{
  "strains": [
    {
      "_id": "5c1151437a2d9625e08a7c8c",
      "name": "A-Dub",
      "type": "Hybrid",
      "description": "A cross between Sour Double & Alien Dawg",
      "flavor": "Earthy",
      "comments": []
    },
    {
      ...next strain
    }
  ]
}
```

This endpoint retrieves all existing strains from the database.

This endpoint is NOT protected, therefore does not require a token. 

### HTTP Request

`GET https://medicine-cabinet.herokuapp.com/strains`


## Get a Specific Strain

> To retrieve a specific strain, use this code:

```shell
curl -X GET
  https://medicine-cabinet.herokuapp.com/strains/<id>
```

> The above command returns a 200 status code and a JSON object containing specific strain information like this:

```json
{
  "_id": "5c1151437a2d9625e08a7c8c",
  "name": "A-Dub",
  "type": "Hybrid",
  "description": "A cross between Sour Double & Alien Dawg",
  "flavor": "Earthy",
  "comments": []
}
```

This endpoint retrieves a specific strain from the database.

This endpoint is NOT protected, therefore does not require a token. 


### HTTP Request

`GET https://medicine-cabinet.herokuapp.com/strains/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the strain to retrieve

## Create a Strain

> To create a strain, use this code:

```shell
curl -X POST
  https://medicine-cabinet.herokuapp.com/strains/
  -H 'Authorization: Bearer <token>'
  -H 'Content-Type: application/json'
  -d '{
	"name": "New Strain",
	"type": "Hybrid",
	"description": "A very new strain",
	"flavor": "Sweet"
}'
```

> The above command returns a 201 status code and a JSON object containing the strain information like this:

```json
{
    "_id": "5c2a3758d1490900174e9da8",
    "name": "New Strain",
    "type": "Hybrid",
    "description": "A very new strain",
    "flavor": "Sweet",
    "comments": []
}
```

This endpoint creates a new strain, adding it to the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`POST https://medicine-cabinet.herokuapp.com/strains/`

### Required Fields
Field | Description
----- | -----------
name | name of strain
type | type of strain
description | description of strain
flavor | flavor of strain

## Update a Specific Strain

> To update a strain, use this code:

```shell
curl -X PUT
  https://medicine-cabinet.herokuapp.com/strains/<id>
  -H 'Authorization: Bearer <token>'
  -H 'Content-Type: application/json'
  -d '{
	"_id": "5c2a3758d1490900174e9da8",
	"name": "Old Strain",
	"type": "Indica",
	"description": "This strain is somewhat older now",
	"flavor": "Sweet"
}'
```

> The above command returns a 200 status code and a JSON object containing the updated data like this:

```json
{
    "_id": "5c2a3758d1490900174e9da8",
    "name": "Old Strain",
    "type": "Indica",
    "description": "This strain is somewhat older now",
    "flavor": "Sweet",
    "comments": []
}
```

This endpoint updates an exisintg strain in the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`PUT https://medicine-cabinet.herokuapp.com/strains/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the strain to update

### Required Fields
Field | Description
----- | -----------
_id | id of strain to update

### Optional Fields
Field | Description
----- | -----------
name | updated name of strain
type | updated type of strain
description | updated description of strain
flavor | updated flavor of strain


## Delete a Specific Strain

> To delete a strain, use this code:

```shell
curl -X DELETE
  https://medicine-cabinet.herokuapp.com/strains/<id>
  -H 'Authorization: Bearer <token>'
```

> The above command returns a 204 status code.

This endpoint deletes an existing strain from the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`DELETE https://medicine-cabinet.herokuapp.com/strains/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the strain to delete

## Add Strain Comment 

> To add a comment to a strain, use this code:

```shell
curl -X POST
  https://medicine-cabinet.herokuapp.com/strains/<id>
  -H 'Authorization: Bearer <token>'
  -H 'Content-Type: application/json'
  -d '{
	"comment": {
		"content": "This is a test comment",
		"author": "testuser"
	}
}'
```

> The above command returns a 201 status code and a JSON object containing a message like this:

```json
{
    "message": "Comment added to strain"
}
```

This endpoint adds a comment to an existing strain in the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`POST https://medicine-cabinet.herokuapp.com/strains/<id>`

### URL Parameters
Parameter | Description
--------- | -----------
id | id of the strain you would like to add the comment to

### Required Fields
Field | Description
----- | -----------
comment | object containing "content" and "author"
- content | content of the comment
- author | author of the comment


## Delete Strain Comment

> To remove a comment from a strain, use this code:

```shell
curl -X DELETE 
  https://medicine-cabinet.herokuapp.com/strains/<id>/<commentid>
  -H 'Authorization: Bearer <token>' 
```

> The above command returns a 204 status code.

This endpoint removes an existing comment from an existing strain in the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`DELETE https://medicine-cabinet.herokuapp.com/strains/<id>/<commentid>`

### URL Parameters
Parameter | Description
--------- | -----------
id | id of the strain to remove the comment from
commentid | id of the comment to remove

# Users

## Get All Cabinet Strains

> To retrieve all strains from your cabinet, use this code:

```shell
curl -X GET
  https://medicine-cabinet.herokuapp.com/users/strains/
  -H 'Authorization: Bearer <token>'
 
```

> The above command returns a 200 status code, as well as a JSON object containing an array of all existing cabinet strains and the user id like this:

```json
{
    "strains": [
        {
            "_id": "5c1151437a2d9625e08a7c92",
            "name": "Blueberry",
            "type": "Indica",
            "description": "2000 High Times Cannabis Cup Winner for Best Indica",
            "flavor": "Blueberry",
            "comments": [
                {
                    "_id": "5c1fd48fd251bc001769435f",
                    "content": "This strain is perfect for anyone who is trying to relax and wind down after a long day. Tastes great too!",
                    "author": "miamiyankee13"
                }
            ]
        },
        {
          ...next strain
        }
    ],
    "_id": "5c297938214f280017250552"
}
```

This endpoint retrieves all existing strains from your cabinet.

This endpoint IS protected, therefore does not require a token. 

### HTTP Request

`GET https://medicine-cabinet.herokuapp.com/users/strains/`

## Add a Cabinet Strain

> To add a strain to your cabinet, use this code:

```shell
curl -X PUT 
  https://medicine-cabinet.herokuapp.com/users/strains/<id>
  -H 'Authorization: Bearer <token>'
```

> The above command returns a 200 status code and a JSON object containing a message like this:

```json
{
    "message": "Strain added to user"
}
```

This endpoint adds an existing strain to your cabinet.

This endpoint IS protected, therefore does not require a token. 

### HTTP Request

`PUT https://medicine-cabinet.herokuapp.com/users/strains/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the strain to add

## Delete a Cabinet Strain

> To remove a strain from your cabinet, use this code:

```shell
curl -X DELETE 
  https://medicine-cabinet.herokuapp.com/users/strains/<id>
  -H 'Authorization: Bearer <token>'
```

> The above command returns a 204 status code.

This endpoint removes an existing strain from your cabinet.

This endpoint IS protected, therefore does not require a token. 

### HTTP Request

`DELETE https://medicine-cabinet.herokuapp.com/users/strains/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the strain to remove