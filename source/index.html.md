---
title: Loyalty Plus API Documentation

language_tabs:
  - Shell
  - Ruby
  - JSON

toc_footers:
  - <a href='#'>Loyalty Plus Help</a>

includes:
  - errors
  - artists

search: true
---

# Introduction

Welcome to the Loyalty Plus API! Please make sure you've subscribed to our API Changelog Mailing List to receive updates on new versions of our api. We document all our API changes in our [API Changelog](#api-changelog).

We have language bindings in Shell, Ruby, and Java! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# API Changelog

When we make backwards-incompatible changes to any of our APIs, we release new dated versions. To use a new version of the API, use the release date in the URL of request as indicated below.

`GET https://api.500friends.com/2016-07-01/api/ping`

### 2016-07-01

**Enrolling users with a locale**

>You can now enroll users with a locale by passing in the optional locale parameter.

```json
    {
        "locale": "en-us"
    }
```


List changes here

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>


# Enrollment

## Enroll a user into the program

### HTTP Request
`GET https://api.500friends.com/api/enroll.json?uuid=account_id&email=customer_email`

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.500friends.com/api/enroll.json?uuid=sETBsiU51WIQlyl&email=nobody%40example.com")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)

response = http.request(request)
puts response.read_body
```

```shell
curl
"https://api.500friends.com/api/enroll.json?uuid=account_id&email=customer_email"
```
> If the enrollment is successful, the above command returns JSON structured like this:

```json
{
   "success":true,
   "data":{
      "points":10
   }
}
```

> If there is an error,

```json
{
   "success":true,
   "data":{
      "points":10,
      "referral_failed":{
         "code":351,
         "message":"The code is invalid"
      }
   }
}
```

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
uuid	|×|	your Account ID
email	|*†|	email address of a customer to be enrolled
external_customer_id	|*|	id you use to uniquely identify a customer
name|	 |	customer's full name
first_name	 |	|customer's first name
last_name	| |	customer's last name
address_line_1|	 |	first line of customer's address
address_line_2	| |	second line of customer's address
city	 	 ||
state	 	| |
postal_code	| 	|
country	 ||	for recognition by the pulldown in the LoyaltyPlus Portal customer edit section, use the two-character ISO country code
home_phone	| |
work_phone	| |
mobile_phone|	|
birthdate	| |	for birthday-based promotions. Format: YYYY-MM-DD or MM-DD
home_store	| |	the customer's primary store
brand	 |	|the engagement brand -- for web sites with more than one brand area from which a member may enroll
channel	 ||	the engagement channel i.e. web vs. instore vs. mobile where the event occurred
sub_channel	 ||	 additional details about the channel, such as store ID, store location or website section
sub_channel_detail||	 	accepts text value to identify additional details about the sub_channel, such as Sales Rep or Employee ID Number
unsubscribed	| |	set to true to unsubscribe a customer; defaults to false. Note: unsubscribed customers will not receive any scheduled emails from your program; however, they will continue to receive transactional emails.
unsubscribed_sms|	| 	set to false to subscribe a customer to mobile communications via sms; defaults to true.
enrolled_at	| |	Date field to indicate enrollment date of the customer, if different from the current date.
expiration_date	 ||	date at which the enrollment points should expire. If this date is not specified, the points will expire according to the configured expiration. Note: expiration_date cannot be in the past; however it can be today's date.
password|	†|	the user's password (if storing authentication variables)
password_confirmation	| |	the user's password again (if storing authentication variables)
vendor|	†	|the vendor used for authentication, e.g., facebook or twitter
vendor_id	|†	|e.g., 23edv4434rwe
custom_attributes|	|	custom attributes for the customer, in JSON format.
referral_code	|	|the code for attributing a user's referral
referral_channel|	|	channel set by the client for the referral source

###Response Parameters

Parameter  | Description
---------  | -----------
points | number of points recorded for the event
referral_failed	 | details of an invalid referral code (enrollment will not fail if referral code is invalid)

sig	| 	|security signature
<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

# Topics

## Date Formats

Here is a list of valid date formats to be used as attributes in requests.

Format  | Example | Notes
---------  | ----------- | -----------
yyyy-mm-dd |    2011-07-21	|day boundaries are based on Pacific time zone
yyyy/mm/dd |	2011/07/21	|day boundaries are based on Pacific time zone
mm/dd/yyyy |	07/21/2011	|day boundaries are based on Pacific time zone
yyyy-mm-ddThh:mm:ss	|2013-06-19T13:00:00	|Pacific time zone


