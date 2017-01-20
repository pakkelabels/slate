# Shipments

## Create shipment
```php
<?php
$data = array(
 'shipping_agent' => 'pdk',
 'weight' => '1000',
 'receiver_name' => 'John Doe',
 'receiver_address1' => 'Some Street 42',
 'receiver_zipcode' => '5230',
 'receiver_city' => 'Odense M',
 'receiver_country' => 'DK',
 'receiver_email' => 'test@test.dk',
 'receiver_mobile' => '12345678',
 'sender_name' => 'John Wayne',
 'sender_address1' => 'The Batcave 1',
 'sender_zipcode' => '5000',
 'sender_city' => 'Odense C',
 'sender_country' => 'DK',
 'shipping_product_id' => '517',
 'services' => '191,192',
 'test' => 'true'
);
$shipment = $label->create_shipment($data);
print_r($shipment);
?>
```

```shell
curl --data "token=BpLqF4fQtp4NLwp10dI-YvdF5LGIBkFE3GYuhq4M&shipping_agent=pdk\
&weight=1000&receiver_name=John Doe&receiver_address1=Some Street 42\
&receiver_zipcode=5230&receiver_city=Odense M&receiver_country=DK\
&receiver_email=test@test.dk&receiver_mobile=12345678\
&sender_name=John Wayne&sender_address1=The Batcave 1&sender_zipcode=5000\
&sender_city=Odense C&sender_country=DK&shipping_product_id=517\
&services=191,192&test=true" https://app.pakkelabels.dk/api/public/v2/shipments/shipment
```

> The above command returns JSON structured like this:

```json
{
	"shipment_id": 42,
	"order_id": 101,
	"pkg_no": "00370720682192068269",
	"base64": "..."
}
```

> For some shipping agents, there can be an additional element called additional_data, which includes hipping agent specific data like below:

```json
{
    "shipment_id": 42,
    "order_id": 101,
    "pkg_no": "JD014600003459067113",
    "base64": "...",
    "additiondal_data": {
        "airway_bill_number": "4458467871"
    }
}
```

Create a shipment. The price of the shipment is automatically substracted from your balance. If the balance of the user is not enough, an error will be raised.
Once the shipment has been created, the base64 encoding of the label is returned in **base64**.

<aside class="notice">
When testing, set the paramter test to true. You can then create labels even if your balance is zero.
</aside>

### HTTP Request

`POST https://app.pakkelabels.dk/api/public/v2/shipments/shipment`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
receiver_name|string|true|Receiver name
receiver_attention|string|false|Receiver attention
receiver_address1|string|true|Receiver address 1. For GLS shipments to "PakkeShop", this should contain the address of the PakkeShop and **not** the address of the receiver.
receiver_address2|string|false|Receiver address 2. For GLS shipments to "PakkeShop", the format should be: Pakkeshop: xxxxx You can get the correct address 2 from the API call to gls_droppoints
receiver_zipcode|string|true|Receiver zipcode
receiver_city|string|true|Receiver city
receiver_country|string|true|Receiver country (e.g. DK)
receiver_email|string|false|Receiver email
receiver_mobile|string|false|Receiver mobile
auto_select_droppoint|string|false|Specify if you want the nearest droppoint (currently only supports GLS "PakkeShop") to be seleced automatically based on the receivers address. If this is true, receiver_address2 should not be filled, and receiver_address1 should contain the address of the receiver.
sender_name|string|true|Sender name
sender_address1|string|true|Sender address 1
sender_address2|string|false|Sender address 2
sender_zipcode|string|true|Sender zipcode
sender_city|string|true|Sender city
sender_country|string|true|Sender country
sender_email|string|false|Sender email
custom_delivery|boolean|false|Specify if you want delivery to a custom droppoint. This only applies for Post Danmark and DAO shipments without delivery. If this is set to true, all fields starting with delivery_ must be set, as well as service_point_id. To find valid droppoints, use the API call **pdk_droppoints** or **dao_droppoints**. Defaults to **false**
delivery_name|string|false|The name of the delivery point. Note that this is **NOT** the name of the person which will pick of the parcel. Use the field **company_name** from the API call **pdk_droppoints** if using Post Danmark / Post Nord or use **dao_droppoints** if using DAO
delivery_address1|string|false|The address of the delivery point.
delivery_zipcode|string|false|The zipcode of the delivery point.
delivery_city|string|false|The city of the delivery point.
delivery_country|string|false|country of the delivery point.
service_point_id|integer|false|The ID of the service point. Use the field **number** from the API call **pdk_droppoints** if Post Danmark / PostNord is chosen or **dao_droppoints** if DAO is chosen. Note, this should only be used for Post Danmark or DAO shipments.
shipping_agent|string|true|Shipping agent. Currently supporting pdk, gls and dao.
shipping_product_id|integer|true|The ID of shipping product to use. You can see which products are available to your user from the API call **freight_rates**
services|string|false|A comma separated list of services to add to the product. You can see which services are available to your user from the API call **freight_rates**
weight|integer|true|Specify the weight of the shipment. You can get a list of supported weights from the API call **freight_rates**.
order_id|string|false|Allows you to specify a custom order id to a shipment. This id has no functional impact, but is only for your own convenience
receipt|string|false|Specifies if you want a receipt send to your mail. Defaults to false
label_format|string|false|Specify the format of the label (a5, 10x19 or zpl). Defaults to **a5**
reference|string|false|Specify a custom text to be printed on the label. This can be used to display the order number or other useful information
add_to_print_queue|string|false|If set to true, the newly created shipment is added to the printqueue
delivery_instruction|string|false|Specify the delivery instruction that is transffered to shipping agent and visible label. Currently only available for PostNord / Post Danmark. Service ID 33 - Flexdelivery also needs to be used.
test|boolean|false|If this is true, a test label will be generated. No money will be charges from your account. Defaults to **false**

## Create shipment own customer number
```php
<?php
$data = array(
 'shipping_agent' => 'pdk',
 'weight' => '1000',
 'receiver_name' => 'John Doe',
 'receiver_address1' => 'Some Street 42',
 'receiver_zipcode' => '5230',
 'receiver_city' => 'Odense M',
 'receiver_country' => 'DK',
 'receiver_email' => 'test@test.dk',
 'receiver_mobile' => '12345678',
 'sender_name' => 'John Wayne',
 'sender_address1' => 'The Batcave 1',
 'sender_zipcode' => '5000',
 'sender_city' => 'Odense C',
 'sender_country' => 'DK',
 'shipping_product_id' => '517',
 'services' => '191,192',
 'test' => 'true'
);
$shipment = $label->create_shipment_own_customer_number($data);
print_r($shipment);
?>
```

```shell
curl --data "token=BpLqF4fQtp4NLwp10dI-YvdF5LGIBkFE3GYuhq4M&shipping_agent=pdk\
&weight=1000&receiver_name=John Doe&receiver_address1=Some Street 42\
&receiver_zipcode=5230&receiver_city=Odense M&receiver_country=DK\
&receiver_email=test@test.dk&receiver_mobile=12345678\
&sender_name=John Wayne&sender_address1=The Batcave 1&sender_zipcode=5000\
&sender_city=Odense C&sender_country=DK&shipping_product_id=517\
&services=191,192&test=true" https://app.pakkelabels.dk/api/public/v2/shipments/shipment_own_customer_number
```

> The above command returns JSON structured like this:

```json
{
	"shipment_id": 42,
	"order_id": 101,
	"pkg_no": "00370720682192068269",
	"base64": "..."
}
```

> For some shipping agents, there can be an additional element called additional_data, which includes hipping agent specific data like below:

```json
{
    "shipment_id": 42,
    "order_id": 101,
    "pkg_no": "JD014600003459067113",
    "base64": "...",
    "additiondal_data": {
        "airway_bill_number": "4458467871"
    }
}
```

> Example for the **extra** element

```json
{
    "extra": {
        "dfm":{  
            "delivery_instruction":"The delivery instruction",
            "dot_type":"0",
            "insurance_amount":"10000",
            "insurance_type":"ZFA",
            "pallets1":"0",
            "pallets2":"0",
            "pallets4":"1",
            "pickup_date":"2017-01-13", 
            "pickup_instruction":"The pickup instruction",
            "goods":[
                {  
                    "amount":"1",
                    "content":"Test Content",
                    "length":"30",
                    "width":"20", 
                    "height":"10",
                    "weight":"10",
                    "volume":"0.006",
                    "running_meter":"",
                    "packing":"PL4",
                }
            ]
        }
    }

}
```

```code
"delivery_instruction": Optional
"dot_type": DOT type for Danske Fragtmænd, send 0 as default. Other values D01, D02, D03 or D04
"insurance_amount": Insurance Amount in DKK, optional
"insurance_type": Insurance Type, optional
"pallets1": Pallet Exchange, quantity of whole pallet
"pallets2": Pallet Exchange, quantity of half pallet
"pallets4": Pallet Exchange, quantity of quart pallet
"pickup_date": Mandatory
"pickup_instruction": "The pickup instruction",
"goods": Can contain more lines, not all fields in goods line are mandatory.
"amount": Quantity of collis with same dimensions described in this line
"content": Description of contents
"length": In CM
"width": In CM
"height": In CM
"weight": In KG
"volume": In cubic meter 
"running_meter": Running Meter
"packing": Packing type
```

Create a shipment using your own customer number/contract from the shipping agent. In this way, you will pay directly to the corresponding shipping agent. The customer number will automatically be fetched from your account.
Once the shipment has been created, the base64 encoding of the label is returned in base64.

<aside class="notice">
You need to contact us in order to set up the contract number
</aside>

### HTTP Request

`POST https://app.pakkelabels.dk/api/public/v2/shipments/shipment_own_customer_number`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
receiver_name|string|true|Receiver name
receiver_attention|string|false|Receiver attention
receiver_address1|string|true|Receiver address 1. For GLS shipments to "PakkeShop", this should contain the address of the PakkeShop and **not** the address of the receiver.
receiver_address2|string|false|Receiver address 2. For GLS shipments to "PakkeShop", the format should be: Pakkeshop: xxxxx You can get the correct address 2 from the API call to gls_droppoints
receiver_zipcode|string|true|Receiver zipcode
receiver_city|string|true|Receiver city
receiver_country|string|true|Receiver country (e.g. DK)
receiver_email|string|false|Receiver email
receiver_mobile|string|false|Receiver mobile
auto_select_droppoint|string|false|Specify if you want the nearest droppoint (currently only supports GLS "PakkeShop") to be seleced automatically based on the receivers address. If this is true, receiver_address2 should not be filled, and receiver_address1 should contain the address of the receiver.
sender_name|string|true|Sender name
sender_address1|string|true|Sender address 1
sender_address2|string|false|Sender address 2
sender_zipcode|string|true|Sender zipcode
sender_city|string|true|Sender city
sender_country|string|true|Sender country
sender_email|string|false|Sender email
custom_delivery|boolean|false|Specify if you want delivery to a custom droppoint. This only applies for Post Danmark and DAO shipments without delivery. If this is set to true, all fields starting with delivery_ must be set, as well as service_point_id. To find valid droppoints, use the API call **pdk_droppoints** or **dao_droppoints**. Defaults to **false**
delivery_name|string|false|The name of the delivery point. Note that this is **NOT** the name of the person which will pick of the parcel. Use the field **company_name** from the API call **pdk_droppoints** if using Post Danmark / Post Nord or use **dao_droppoints** if using DAO
delivery_address1|string|false|The address of the delivery point.
delivery_zipcode|string|false|The zipcode of the delivery point.
delivery_city|string|false|The city of the delivery point.
delivery_country|string|false|country of the delivery point.
service_point_id|integer|false|The ID of the service point. Use the field **number** from the API call **pdk_droppoints** if Post Danmark / PostNord is chosen or **dao_droppoints** if DAO is chosen. Note, this should only be used for Post Danmark or DAO shipments.
shipping_agent|string|true|Shipping agent. Currently supporting pdk, gls, dao, bring, dhl_express and dfm.
shipping_product_id|integer|true|The ID of shipping product to use. You can see which products are available to your user from the API call **freight_rates**
services|string|false|A comma separated list of services to add to the product. You can see which services are available to your user from the API call **freight_rates**
weight|integer|true|Specify the weight of the shipment. You can get a list of supported weights from the API call **freight_rates**.
order_id|string|false|Allows you to specify a custom order id to a shipment. This id has no functional impact, but is only for your own convenience
receipt|string|false|Specifies if you want a receipt send to your mail. Defaults to false
label_format|string|false|Specify the format of the label (a5, 10x19 or zpl). Defaults to **a5**
reference|string|false|Specify a custom text to be printed on the label. This can be used to display the order number or other useful information
add_to_print_queue|string|false|If set to true, the newly created shipment is added to the printqueue
number_of_collis|string|false|Specifies the number of collis for this shipment. Defaults to 1
delivery_instruction|string|false|Specify the delivery instruction that is transffered to shipping agent and visible label. Currently only available for PostNord / Post Danmark. Service ID 33 - Flexdelivery also needs to be used.
test|boolean|false|If this is true, a test label will be generated. No money will be charges from your account. Defaults to **false**
extra|object|false|This parameter can be used in order to send additional data


## Create imported shipment
```php
<?php
$data = array(
 'shipping_agent' => 'pdk',
 'receiver_name' => 'John Doe',
 'receiver_address1' => 'Some Street 42',
 'receiver_zipcode' => '5230',
 'receiver_city' => 'Odense M',
 'receiver_country' => 'DK',
 'receiver_email' => 'test@test.dk',
 'receiver_mobile' => '12345678',
 'sender_name' => 'John Wayne',
 'sender_address1' => 'The Batcave 1',
 'sender_zipcode' => '5000',
 'sender_city' => 'Odense C',
 'sender_country' => 'DK',
 'shipping_agent' => 'pdk',
 'order_id' => '42'
);
$shipment = $label->create_imported_shipment($data);
print_r($shipment);
?>
```

```shell
curl --data "token=BpLqF4fQtp4NLwp10dI-YvdF5LGIBkFE3GYuhq4M&shipping_agent=pdk\
&weight=1000&receiver_name=John Doe&receiver_address1=Some Street 42\
&receiver_zipcode=5230&receiver_city=Odense M&receiver_country=DK\
&receiver_email=test@test.dk&receiver_mobile=12345678\
&sender_name=John Wayne&sender_address1=The Batcave 1&sender_zipcode=5000\
&sender_city=Odense C&sender_country=DK&shipping_agent=pdk&order_id=42" https://app.pakkelabels.dk/api/public/v2/shipments/imported_shipment
```

> The above command returns JSON structured like this:

```json
{
    "id": 42
}
```

Create an imported shipment. Imported shipments is completely free to create. From the webinterface, they can be used as a template to buy a label.
Below are listed some scenarios where imported shipments are useful:

- You (or your customer) wants to buy the labels through our webinterface, but uses a webplatform we don’t have an official integration for.
- You have special requirements which means that our officlal integrations aren’t feasible.

<aside class="notice">
Our official webshop integrations create imported shipments as well.
</aside>

### HTTP Request

`POST https://app.pakkelabels.dk/api/public/v2/shipments/imported_shipment`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
receiver_name|string|true|Receiver name
receiver_attention|string|false|Receiver attention
receiver_address1|string|true|Receiver address 1
receiver_address2|string|false|Receiver address 2.
receiver_zipcode|string|true|Receiver zipcode
receiver_city|string|true|Receiver city
receiver_country|string|true|Receiver country (e.g. DK)
receiver_email|string|false|Receiver email
receiver_mobile|string|false|Receiver mobile
sender_name|string|true|Sender name
sender_address1|string|true|Sender address 1
sender_address2|string|false|Sender address 2
sender_zipcode|string|true|Sender zipcode
sender_city|string|true|Sender city
sender_country|string|true|Sender country
sender_email|string|false|Sender email
shipping_agent|string|false|Shipping agent
order_id|string|false|Allows you to specify a custom order id to a shipment. This id has no functional impact, but is only for your own convenience

## Add to print queue
```php
<?php
$label->add_to_print_queue([42, 1337]);
?>
```


```shell
curl --data "token=McYR8DzKS1OfGd1q1avjCi0vv3kZL5f0itgF_QGW&ids=42,1337" https://app.pakkelabels.dk/api/public/v2/shipments/add_to_print_queue
```

> The above command returns JSON structured like this:

```json
{
    "message": "2 labels added to the printqueue"
}
```

Add the labels to the print queue, such that they will be printed by the print client.
<aside class="notice">
The print client program has to be installed for this to work
</aside>

`POST https://app.pakkelabels.dk/api/public/v2/shipments/add_to_print_queue`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
ids|string|true|Comma separated list of shipments IDs

## List shipments
```php
<?php
$labels = $label->shipments(array('shipping_agent' => 'pdk', 'receiver_country' => 'DK'));
print_r($labels);
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/shipments?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&shipping_agent=pdk&receiver_country=DK
```

> The above command returns JSON structured like below.
> For some shipping agents, there can be an additional element called additional_data, which includes hipping agent specific data.

```json
{
    "shipment_count": 25,
    "total_pages": 2,
    "shipments": [
        {
            "id": 42,
            "receiver_name": "John Doe",
            "receiver_attention": "",
            "receiver_address1": "Test Street 1",
            "receiver_address2": "",
            "receiver_country": "DK",
            "receiver_zipcode": "5000",
            "receiver_email": "test@test.dk",
            "receiver_telephone": null,
            "sender_name": "Sender Doe",
            "sender_attention": "",
            "sender_address1": "Sender Street 1",
            "sender_address2": "",
            "sender_country": "DK",
            "sender_zipcode": "5230",
            "sender_email": null,
            "sms_notification": null,
            "email_notification": null,
            "delivery": null,
            "display_name": "1kg til 5kg",
            "shipping_agent": "pdk",
            "created_at": "2014-11-13T13:37:42.195+01:00",
            "pkg_no": "00370720682192051391",
            "order_id": "100"
        },
        {
            "id": 43,
            "receiver_name": "John Doe",
            "receiver_attention": "",
            "receiver_address1": "Test Street 2",
            "receiver_address2": "",
            "receiver_country": "DK",
            "receiver_zipcode": "5000",
            "receiver_email": "test@test.dk",
            "receiver_telephone": null,
            "sender_name": "Sender Doe",
            "sender_attention": "",
            "sender_address1": "Sender Street 2",
            "sender_address2": "",
            "sender_country": "DK",
            "sender_zipcode": "5230",
            "sender_email": null,
            "sms_notification": null,
            "email_notification": null,
            "delivery": null,
            "display_name": "0kg til 1kg",
            "shipping_agent": "pdk",
            "created_at": "2014-11-13T13:37:42.195+01:00",
            "pkg_no": "00370720682192051392",
            "order_id": "100",
            "additiondal_data": {
                "airway_bill_number": "4458467871"
            }
        },
        {...}
    ]
}
```


This method supports pagination. 20 shipments are returned on each page.
The total number of shipments which matches the conditions is returned in **shipment_count**, and the total number of pages in **total_pages**.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/shipments`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
page|integer|false|The page to return. Defaults to 1 if not set
shipping_agent|string|false|Filter by shipping agent
receiver_country|string|false|Filter by receiver country
pkg_no|string|false|Filter by package/tracking number
order_id|string|false|ilter by any custom order id set
created_at_min|string|false|Filter by creation time
created_at_max|string|false|Filter by creation time

## List imported shipments
```php
<?php
$labels = $label->imported_shipments(array('shipping_agent' => 'pdk', 'receiver_country' => 'DK'));
print_r($labels);
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/imported_shipments?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&shipping_agent=pdk&receiver_country=DK
```

> The above command returns JSON structured like this:

```json
{
    "shipment_count": 25,
    "total_pages": 2,
    "shipments": [
        {
            "id": 42,
            "receiver_name": "John Doe",
            "receiver_attention": "",
            "receiver_address1": "Test Street 1",
            "receiver_address2": "",
            "receiver_country": "DK",
            "receiver_zipcode": "5000",
            "receiver_email": "test@test.dk",
            "receiver_telephone": null,
            "sender_name": "Sender Doe",
            "sender_attention": "",
            "sender_address1": "Sender Street 1",
            "sender_address2": "",
            "sender_country": "DK",
            "sender_zipcode": "5230",
            "sender_email": null,
            "order_id": "100"
        },
        {
            "id": 43,
            "receiver_name": "John Doe",
            "receiver_attention": "",
            "receiver_address1": "Test Street 2",
            "receiver_address2": "",
            "receiver_country": "DK",
            "receiver_zipcode": "5000",
            "receiver_email": "test@test.dk",
            "receiver_telephone": null,
            "sender_name": "Sender Doe",
            "sender_attention": "",
            "sender_address1": "Sender Street 2",
            "sender_address2": "",
            "sender_country": "DK",
            "sender_zipcode": "5230",
            "sender_email": null,
            "order_id": "100"
        },
        {...}
    ]
}
```

Note that once a imported shipment has been used to create a shipment, it is moved from **imported_shipments** to **shipments**.
This method supports pagination. 20 shipments are returned on each page.
The total number of shipments which matches the conditions is returned in **shipment_count**, and the total number of pages in **total_pages**.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/imported_shipments`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
page|integer|false|The page to return. Defaults to 1 if not set
shipping_agent|string|false|Filter by shipping agent
receiver_country|string|false|Filter by receiver country
created_at_min|string|false|Filter by creation time
created_at_max|string|false|Filter by creation time

## PDF
```php
<?php
$base64 = $label->pdf(42);
$pdf = base64_decode($base64);
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/pdf?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&id=42
```

> The above command returns JSON structured like this:

```json
{
    "base64": "..."
}
```

Returns the base64 encoding of a label created earlier in PDF format

`GET https://app.pakkelabels.dk/api/public/v2/shipments/pdf`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
id|integer|true|Shipment ID
label_format|string|false|Label format (a5 or 10x19)

## PDF Multiple
```php
<?php
$base64 = $label->pdf_multiple([42, 1337]);
$pdf = base64_decode($base64);
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/pdf_multiple?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&ids=42,1337
```

> The above command returns JSON structured like this:

```json
{
    "base64": "..."
}
```

Returns the base64 encoding of the labels specified in PDF format. The labels will be concatenated to a single PDF.
This can be used for bulk printing.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/pdf_multiple`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
ids|string|true|Comma separated list of shipments IDs
label_format|string|false|Label format (a5 or 10x19)

## ZPL
```php
<?php
$base64 = $label->zpl(42);
$zpl = base64_decode($base64);
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/zpl?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&id=42
```

> The above command returns JSON structured like this:

```json
{
    "base64": "..."
}
```

Returns the base64 encoding of a label created earlier in ZPL format

`GET https://app.pakkelabels.dk/api/public/v2/shipments/zpl`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
id|integer|true|Shipment ID

## Freigt Rates
```php
<?php
print_r($label->freight_rates());
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/freight_rates?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9
```

> The above command returns JSON structured like this:

```json
{
   "DK":{
      "pdk":{
         "name":"Post Danmark",
         "code":"pdk",
         "rates":[
            {
               "from_weight":0,
               "to_weight":1000,
               "display_name":"0kg til 1kg",
               "price":39.0,
               "price_includes_vat":true
            },
            {
               "from_weight":1000,
               "to_weight":5000,
               "display_name":"1kg til 5kg",
               "price":49.0,
               "price_includes_vat":true
            }
         ],
         "products":[
            {
               "id":517,
               "name":"Privatpakke u. omdeling",
               "price":0.0,
               "delivery":false,
               "services":[
                  {
                     "id":191,
                     "name":"Email advisering",
                     "price":0.0
                  },
                  {
                     "id":192,
                     "name":"SMS advisering",
                     "price":0.0
                  }
               ]
            },
            {
               "id":516,
               "name":"Privatpakke m. omdeling",
               "price":19.0,
               "delivery":true,
               "services":[
                  {
                     "id":191,
                     "name":"Email advisering",
                     "price":0.0
                  },
                  {
                     "id":192,
                     "name":"SMS advisering",
                     "price":0.0
                  }
               ]
            }
         ]
      },
      "gls":{
         "name":"GLS",
         "code":"gls",
         "rates":[
         	{}
         ],
         "products":[
            {}
         ]
      }
   },
  "SE": {
  }
}
```

Returns the countries, shipping agents, products and services which are available for your user. The list also includes the price for the different products and services.
The IDs of the products should be used in **shipping_product_id**, and the IDs of the services in **services**, when creating a shipment.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/freight_rates`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
country|string|false|Filter by country

## Post Denmark Drop points
```php
<?php
print_r($label->pdk_droppoints(array('zipcode' => '5240', 'street' => 'Strandvejen')));
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/pdk_droppoints?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&zipcode=5240&street=Strandvejen
```

> The above command returns JSON structured like this:

```json
[
    {
        "number": "3870",
        "company_name": "Pakkeboks 3870",
        "address": "Skibhusvej 53",
        "zipcode": "5000",
        "city": "ODENSE C"
    },
    {
        "number": "5003",
        "company_name": "POSTHUS",
        "address": "Dannebrogsgade 2",
        "zipcode": "5000",
        "city": "ODENSE C"
    },
    {
        "number": "655",
        "company_name": "Pakkeboks 655",
        "address": "Dannebrogsgade 2",
        "zipcode": "5000",
        "city": "ODENSE C"
    }
]
```

Find the nearest droppoint (“Service Point”) based on zipcode and address.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/pdk_droppoints`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
zipcode|string|true|Zipcode
street|string|false|Street
number|string|false|The number of droppoints to return (defaults to 3)
country|string|false|Country (defaults to DK)

## GLS Drop points
```php
<?php
print_r($label->gls_droppoints(array('zipcode' => '5240', 'street' => 'Strandvejen')));
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/gls_droppoints?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&zipcode=5240&street=Strandvejen
```

> The above command returns JSON structured like this:

```json
[
    {
        "number": "97229",
        "company_name": "OK Plus Havnegade",
        "address": "Havnegade 1",
        "address2": "Pakkeshop: 97229",
        "zipcode": "5000",
        "city": "Odense C"
    },
    {
        "number": "95210",
        "company_name": "Statoil",
        "address": "Kochsgade 94",
        "address2": "Pakkeshop: 95210",
        "zipcode": "5000",
        "city": "Odense C"
    },
    {
        "number": "97871",
        "company_name": "Shell 7-Eleven Næsbyvej",
        "address": "Næsbyvej 51",
        "address2": "Pakkeshop: 97871",
        "zipcode": "5000",
        "city": "Odense C"
    }
]
```

Find the nearest droppoint (“Service Point”) based on zipcode and address.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/gls_droppoints`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
zipcode|string|true|Zipcode
street|string|false|Street
number|string|false|The number of droppoints to return (defaults to 3)

## DAO Drop points
```php
<?php
print_r($label->dao_droppoints(array('zipcode' => '5240', 'street' => 'Strandvejen')));
?>
```

```shell
curl https://app.pakkelabels.dk/api/public/v2/shipments/dao_droppoints?token=8oH1hMoITVHdcPYiKAkgagVNEJ_UWFknVtfcTWB9&zipcode=5240&street=Strandvejen
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
    }
]
```

Find the nearest droppoint (“Service Point”) based on zipcode and address.

`GET https://app.pakkelabels.dk/api/public/v2/shipments/dao_droppoints`

### Query Parameters

Parameter | Type        | Required | Description
--------- | ----------- | ----------- | -----------
token| string | true |Authentication token
zipcode|string|true|Zipcode
street|string|false|Street
number|string|false|The number of droppoints to return (defaults to 3)
country|string|false|Country (defaults to DK)