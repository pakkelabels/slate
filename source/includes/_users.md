# Users

## Login

```php
<?php
$label = new Pakkelabels('api_user', 'api_key'); 
?>
```

```shell
curl --data "api_user=your_api_user&api_key=your_api_key" https://app.pakkelabels.dk/api/public/v2/users/login
```

> The above command returns JSON structured like this:

```json
{
	"token":"fr79337HtCkRBGp3OysgWvqtJrX62cpqsdkX2H8m",
	"expires_at":"2015-10-14T10:02:03.255+02:00"
}
```

Login and retch a token. The returned token is required for all subsequent calls to the API.
<aside class="notice">
The PHP library automatically logs in when the class is initiated, and it automatically adds the token to all request. 
</aside>

### HTTP Request

`POST https://app.pakkelabels.dk/api/public/v2/users/login`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
api_user| string | true |Your API user
api_key| string | true |Your API key

## Balance
```php
<?php
echo $label->balance();
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v1/users/balance?token=fr79337HtCkRBGp3OysgWvqtJrX62cpqsdkX2H8m
```

> The above command returns JSON structured like this:

```json
{
	"balance":777.15
}
```

Fetch the current balance

### HTTP Request

`GET https://app.pakkelabels.dk/api/public/v2/users/balance`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |The token obtained when loggin in

## Payment Requests
```php
<?php
print_r($label->payment_requests());
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v1/users/payment_requests?token=fr79337HtCkRBGp3OysgWvqtJrX62cpqsdkX2H8m
```

> The above command returns JSON structured like this:

```json
[
	{
	    "id":10,
	    "reference": "Efterkrav for pakkelabel 29000",
	    "amount": 42.0
	},
	{
	    "id":11,
	    "reference":"Efterkrav for pakkelabel 29001",
	    "amount":10.0
}
]
```

Fetch any open payment requests

### HTTP Request

`GET https://app.pakkelabels.dk/api/public/v2/users/payment_requests`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |The token obtained when loggin in

