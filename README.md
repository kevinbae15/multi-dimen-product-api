# multi-dimen-product
> Simple api that works with multidimensional product variants


## Setup

### Startup DB

> Note this is for Mac. `brew install postgres` if not already installed and start the service: `brew services start postgresql
`

```
psql postgres
CREATE DATABASE mabledb;
CREATE USER mableadmin WITH PASSWORD 'mableadmin';
GRANT ALL PRIVILEGES ON DATABASE "mabledb" to mableadmin;
\q
```

### Setup virtual environment

```
python3 -m venv dev
. dev/bin/activate
pip3 install -r requirements.txt
```

### Setup migrations

```
python3 manage.py flush
python3 manage.py makemigrations api
python3 manage.py migrate
```

### Start

```
python3 manage.py runserver <port_number>
```

## Endpoints
- GET  `/api/products`
- POST `/api/products`

### GET `/api/products`

Request Payload: N/A

Success Response Example: 

```
{
	"status": "success",
	"data": [
		{
			"name": "Example Chips",
			"attributes": [
				{
					"name": Flavor",
					"options": [
						{
							"name": "Spicy",
						},
						{
							"name": "Salty"
						}
					],
				},
				{
					"name": "Size",
					"options": [
						{
							"name": "Small"
						},
						{
							"name": "Medium"
						},
						{
							"name": "Large"
						}
					]
				}
			]
		},
		{
			"name": "One Variant Snack",
			"attributes": []
		}
	]
}
```

### POST `/api/products`

> Note, duplicate products are allowed, BUT duplicate options for a single attribute or duplicate attributes for a single product are not allowed. Also, if an attribute is provided, a non-empty list of options for the attribute must be provided as well.

Request Payload:

```
{
	"name": "Example Chips",
	"attributes": [
		{
			"name": "Flavor",
			"options": [
				{
					"name": "Spicy"
				},
				{
					"name": "Salty"
				}
			]
		},
		{
			"name": "Size",
			"options": [
				{
					"name": "Small"
				},
				{
					"name": "Medium"
				},
				{
					"name": "Large"
				}
			]
		}
	]
}
```

Success Response Example:

```
{
	"status": "success",
	"data": []
}
```

Fail Response Example:

```
{
	"status": "error",
	"errorMessage": "Name of product cannot be empty"
}
```
