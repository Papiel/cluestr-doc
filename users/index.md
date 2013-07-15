---
title: Users API
layout: doc
---

## Accessing all users

Lists all the users in the company.

`GET /users`

### Input

- `is_admin` _optional_ **boolean** Display users matching the staff status

Format:

	$ curl -u "username:password" -i https://api.papiel.fr/users
	HTTP/1.1 200 OK
	X-Poll-Interval: 60

	[
		{
			"type": "User",
			"id": "dqsvj667bkqvljdfv23678",
			"email": foo@bar.fr,
			"name": "Foo Bar"
			"is_admin": true;
			"user_url": "https://api.papiel.fr/users/dqsvj667bkqvljdfv23678"
		}
		...
	]

- `is_admin` is a boolean flag indicating whether the user can update other users or not.

## Accessing single user

There are 2 ways of accessing a single user. First, with the user id, or accessing the current authenticated user with a specific endpoint.

### Get a single user

`GET /users/:user`

	$ curl -u "username:password" -i https://api.papiel.fr/users/dqsvj667bkqvljdfv23678
	HTTP/1.1 200 OK
	X-Poll-Interval: 60

	{
		"type": "User",
		"uuid": "dqsvj667bkqvljdfv23678",
		"self_url": "/users/1/"
		"email": foo@bar.fr,
		"name": "Foo Bar"
		"is_admin": true;
	}


### Get the authenticated user

`GET /user`

	$ curl -u "username:password" -i https://api.papiel.fr/user
	HTTP/1.1 302 FOUND
	X-Poll-Interval: 60
	Location: https://api.papiel.fr/users/dqsvj667bkqvljdfv23678

## Manage users

### Create an user

`POST /users`

> Note:  Authenticated user must be part of the staff to be able to create new users.

#### Input

- `name` **string**
- `email` **string**
- `is_admin` _optional_ **boolean**, set to `false` by default

	{
		"name": "Foo Bar",
		"email": "foo.bar@baz.com"
	}

#### Response

You'll be redirected to the new user url.

	HTTP/1.1 302 FOUND
	X-Poll-Interval: 60
	Location: https://api.papiel.fr/users/dqsvj667bkqvljdfv23678

### Update an user

`PATCH /users/:uuid`

> Note:  Authenticated user must be part of the staff to be able to update other users.

> Note:  Authenticated user can't change `is_admin` for himself.

#### Input

- `name` _optional_ **string**
- `email` _optional_ **string**
- `is_admin` _optional_ **boolean**

Format:

	{
		"name": "Foo Baz",
		"email": "foo.baz@bar.com"
	}

#### Response

You'll be redirected to the updated user.

	HTTP/1.1 302 FOUND
	X-Poll-Interval: 60
	Location: https://api.papiel.fr/users/dqsvj667bkqvljdfv23678