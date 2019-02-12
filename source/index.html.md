---
title: FF Convos API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell


toc_footers:
  - <a href='https://github.com/miamiyankee13/ff-convos-api'>View FF Convos API on GitHub</a>
  - <a href='https://github.com/miamiyankee13/ff-convos-client'>View FF Convos Client on GitHub</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the FF Convos API! You can use our API to access FF Convos API endpoints, which can interact with our database of players & users, as well as the FF Convos client application.

# Registration
> To register with FF Convos, use this code:

```shell
curl -X POST
  https://ff-convos-api.herokuapp.com/api/users
  -H 'Content-Type: application/json'
  -d '{
	"username": "<your username>",
	"password": "<your password>",
	"firstName": "<your first name>",
	"lastName": "<your last name>"
}'
```

> The above command returns a 201 status code and a JSON object containing the user information like this:

```json
{
    "_id": "5c2a84e2ddca060017e13899",
    "username": "testuser",
    "firstName": "test",
    "lastName": "tester",
    "players": []
}
```
FF Convos uses JSON Web Token (JWT) to conrol access to the API. To obtain a token, you must have an account. To create an account, you can either register from the <a href='https://ff-convos-client.herokuapp.com'>client</a> or make a POST request to the `/api/users` endpoint.

### HTTP Request

`POST https://ff-convos-api.herokuapp.com/api/users`

### Required Fields
Field | Description
----- | -----------
username | desired username
password | desired password
firstName | your first name
lastName | your last name

# Authentication

> To obtain a token, use this code:

```shell
curl -X POST 
  https://ff-convos-api.herokuapp.com/api/auth/login
  -H 'Content-Type: application/json'
  -d '{
	"username": "<your username>",
	"password": "<your password>"
}'
```
> The above command returns a 200 status code and a JSON object containing a token like this:

```json
{
    "authToken": "<token>"
}
```

Once you have an account, you must either login from the <a href='https://ff-convos-client.herokuapp.com'>client</a> or make a POST request to the `/api/auth/login` endpoint in order to obtain a token.

### HTTP Request

`https://ff-convos-api.herokuapp.com/api/auth/login`

### Required Fields
Field | Description
----- | -----------
username | your username
password | your password

The returned token must be added in the header of any request to a protected endpoint via Bearer Authentication. The header should look as follows:

`Authorization: Bearer <token>`

<aside class="notice">
You must replace <code>&lt;token&gt;</code> with your token.
</aside>


# Players

## Get All Players

> To retrieve all players, use this code:

```shell
curl -X GET 
  https://ff-convos-api.herokuapp.com/api/players
```

> The above command returns a 200 status code and a JSON object containing an array of all existing players like this:

```json
{
  "players": [
    {
      "_id": "5c61ad37be03e80f47283c1c",
      "name": "Alvin Kamara",
      "position": "RB",
      "number": "41",
      "team": "Saints",
      "comments": []
    },
    {
      "_id": "5c5db356f29f0045fe63fde9",
      "name": "Odell Beckham Jr.",
      "position": "WR",
      "number": "13",
      "team": "Giants",
      "comments": []
    }
  ]
}
```

This endpoint retrieves all existing players from the database.

This endpoint is NOT protected, therefore does not require a token. 

### HTTP Request

`GET https://ff-convos-api.herokuapp.com/api/players`


## Get an Individual Player

> To retrieve an individual player, use this code:

```shell
curl -X GET
  https://ff-convos-api.herokuapp.com/api/players/<id>
```

> The above command returns a 200 status code and a JSON object containing specific player information like this:

```json
{
  "_id": "5c61ad37be03e80f47283c1c",
  "name": "Alvin Kamara",
  "position": "RB",
  "number": "41",
  "team": "Saints",
  "comments": []
}
```

This endpoint retrieves an individual player from the database.

This endpoint is NOT protected, therefore does not require a token. 


### HTTP Request

`GET https://ff-convos-api.herokuapp.com/api/players/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the player to retrieve

## Create a Player

> To create a player, use this code:

```shell
curl -X POST
  https://ff-convos-api.herokuapp.com/api/players
  -H 'Authorization: Bearer <token>'
  -H 'Content-Type: application/json'
  -d '{
	"name": "New Player",
	"position": "QB",
	"number": "7",
	"team": "Dolphins"
}'
```

> The above command returns a 201 status code and a JSON object containing the player information like this:

```json
{
    "_id": "5c2a3758d1490900174e9da8",
    "name": "New Player",
    "position": "QB",
    "number": "7",
    "team": "Dolphins",
    "comments": []
}
```

This endpoint creates a new player, adding it to the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`POST https://ff-convos-api.herokuapp.com/api/players`

### Required Fields
Field | Description
----- | -----------
name | player name
position | player position
number | player number
team | current team of player

## Edit a Player

> To edit a player, use this code:

```shell
curl -X PUT
  https://ff-convos-api.herokuapp.com/api/players/<id>
  -H 'Authorization: Bearer <token>'
  -H 'Content-Type: application/json'
  -d '{
	"_id": "5c2a3758d1490900174e9da8",
	"name": "Old Player",
	"position": "RB",
	"number": "21",
	"team": "Eagles"
}'
```

> The above command returns a 200 status code and a JSON object containing the updated data like this:

```json
{
    "_id": "5c2a3758d1490900174e9da8",
    "name": "Old Player",
    "position": "RB",
    "number": "21",
    "team": "Eagles",
    "comments": []
}
```

This endpoint edits an exisintg player in the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`PUT https://ff-convos-api.herokuapp.com/api/players/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the player to edit

### Required Fields
Field | Description
----- | -----------
_id | id of the player to edit

### Optional Fields
Field | Description
----- | -----------
name | player name
position | player position
number | player number
team | current team of player


## Delete a Player

> To delete a player, use this code:

```shell
curl -X DELETE
  https://ff-convos-api.herokuapp.com/api/players/<id>
  -H 'Authorization: Bearer <token>'
```

> The above command returns a 204 status code.

This endpoint deletes an existing player from the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`DELETE https://ff-convos-api.herokuapp.com/api/players/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the player to delete

## Add Comment to Player

> To add a comment to a player, use this code:

```shell
curl -X POST
  https://ff-convos-api.herokuapp.com/api/players/<id>
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
    "message": "Comment added to player"
}
```

This endpoint adds a comment to an existing player in the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`POST https://ff-convos-api.herokuapp.com/api/players/<id>`

### URL Parameters
Parameter | Description
--------- | -----------
id | id of the player you would like to add the comment to

### Required Fields
Field | Description
----- | -----------
comment | object containing "content" and "author"
- content | content of the comment
- author | author of the comment


## Remove Comment from Player

> To remove a comment from a player, use this code:

```shell
curl -X DELETE 
  https://ff-convos-api.herokuapp.com/api/players/<id>/<commentId>
  -H 'Authorization: Bearer <token>' 
```

> The above command returns a 200 status code and a JSON object containing a message like this:

```json
{
    "message": "Comment removed from player"
}
```

This endpoint removes an existing comment from an existing player in the database.

This endpoint IS protected, therefore requires a token.

### HTTP Request

`DELETE https://ff-convos-api.herokuapp.com/api/players/<id>/<commentId>`

### URL Parameters
Parameter | Description
--------- | -----------
id | id of the player to remove the comment from
commentId | id of the comment to remove

# Users

## Get All User Players

> To retrieve all user players, use this code:

```shell
curl -X GET
  https://ff-convos-api.herokuapp.com/api/users/players
  -H 'Authorization: Bearer <token>'
 
```

> The above command returns a 200 status code, as well as a JSON object containing an array of all existing user players and the user id like this:

```json
{
    "players": [
        {
            "_id": "5c5db356f29f0045fe63fde4",
            "name": "Drew Brees",
            "position": "QB",
            "number": "9",
            "team": "Saints",
            "comments": []
        },
        {
            "_id": "5c5db356f29f0045fe63fde5",
            "name": "Nick Chubb",
            "position": "RB",
            "number": "24",
            "team": "Browns",
            "comments": []
        }
    ],
    "_id": "5c5ef7c10eadf4488389a0b4"
}
```

This endpoint retrieves all existing user players.

This endpoint IS protected, therefore requires a token. 

### HTTP Request

`GET https://ff-convos-api.herokuapp.com/api/users/players`

## Get All User Players by Position

> To retrieve all user players by position, use this code:

```shell
curl -X GET
  https://ff-convos-api.herokuapp.com/api/users/players/<position>
  -H 'Authorization: Bearer <token>'
 
```

> The above command returns a 200 status code, as well as a JSON object containing an array of all existing user players by position and the user id like this:

```json
{
    "players": [
        {
            "_id": "5c5db356f29f0045fe63fde4",
            "name": "Drew Brees",
            "position": "QB",
            "number": "9",
            "team": "Saints",
            "comments": []
        },
        {
            "_id": "5c5db356f29f0045fe63fde7",
            "name": "Mitchell Trubisky",
            "position": "QB",
            "number": "10",
            "team": "Bears",
            "comments": []
        }
    ],
    "_id": "5c5ef7c10eadf4488389a0b4"
}
```

This endpoint retrieves all existing user players by position.

This endpoint IS protected, therefore requires a token. 

### HTTP Request

`GET https://ff-convos-api.herokuapp.com/api/users/players/<position>`

### URL Parameters
Parameter | Description
--------- | -----------
position | position to filter players by

## Add Player to User

> To add a player, use this code:

```shell
curl -X PUT 
  https://ff-convos-api.herokuapp.com/api/users/players/<id>
  -H 'Authorization: Bearer <token>'
```

> The above command returns a 200 status code and a JSON object containing a message like this:

```json
{
    "message": "Player added to user"
}
```

This endpoint adds an existing player to the user.

This endpoint IS protected, therefore requires a token. 

### HTTP Request

`PUT https://ff-convos-api.herokuapp.com/api/users/players/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the player to add

## Remove Player from User

> To remove a player, use this code:

```shell
curl -X DELETE 
  https://ff-convos-api.herokuapp.com/api/users/players/<id>
  -H 'Authorization: Bearer <token>'
```

> The above command returns a 200 status code and a JSON object containing a message like this:

```json
{
    "message": "Player removed from user"
}
```

This endpoint removes an existing player from the user.

This endpoint IS protected, therefore requires a token. 

### HTTP Request

`DELETE https://ff-convos-api.herokuapp.com/api/users/players/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | id of the player to remove