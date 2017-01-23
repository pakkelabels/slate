# Service Points

## Pickup Points
```php
<?php
print_r($label->pickup_points(array('agent' => 'dao', 'zipcode' => '5240', 'address' => 'Strandvejen 6')));
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/pickup_points?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&agent=dao&zipcode=5240&address=Strandvejen%206
```

> The above command returns JSON structured like this:

```json
[
    {
        "number": "58318",
        "company_name": "Center Kiosken",
        "address": "Vollsmose Alle 10",
        "address2": "",
        "zipcode": "5240",
        "city": "Odense Nø",
        "country": "DK",
        "distance": 1915,
        "longitude": 10.4413,
        "latitude": 55.4068,
        "agent": "dao"
    },
    {
        "number": "59033",
        "company_name": "Påskeløkkens Købmand",
        "address": "Paaskeløkkevej 11",
        "address2": "",
        "zipcode": "5000",
        "city": "Odense C",
        "country": "DK",
        "distance": 2385,
        "longitude": 10.4159,
        "latitude": 55.4086,
        "agent": "dao"
    },
    {
        "number": "58093",
        "company_name": "Ok Plus Sandhusvej",
        "address": "Sandhusvej 19",
        "address2": "",
        "zipcode": "5000",
        "city": "Odense C",
        "country": "DK",
        "distance": 2466,
        "longitude": 10.4047,
        "latitude": 55.4177,
        "agent": "dao"
    },
    ...
]
```

Find the nearest pickup points ("Service Point" or "Drop Point") based on shipping agent / carrier, country, zipcode and address.

`GET https://app.pakkelabels.dk/api/public/v2/pickup_points`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true | Authentication token
agent| string | true | Shipping Agent / Carrier code. Available values: bring, dao, gls and pdk
country | string | true | Country code. Available values: DK, NO, SE, FI, NL, DE, BE, LU. Default: DK
zipcode | string | true | Zipcode
address | string | false | Street address (contains street name and house number)
number| integer | false | The number of droppoints to return (defaults to 10)