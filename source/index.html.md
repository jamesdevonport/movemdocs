---
title: Movem Partner API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://movem.co.uk/partner-api'>Get your API Key</a>
  - <a href='https://help.movem.co.uk'>Movem Support</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Movem Partner API! You can use our API to create new tenant references, get the status of a reference, update a reference or check your credit balance.

We have language bindings in Shell, Ruby, Python, and JavaScript. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

To play around with a few examples, we recommend a REST client called Postman. Simply tap the button below to import a pre-made collection of examples.

<div class="wrap">
<div class="postman-run-button content"
data-postman-action="collection/import"
data-postman-var-1="a85d7a184d876c0c496c"></div>
<script type="text/javascript">
  (function (p,o,s,t,m,a,n) {
    !p[s] && (p[s] = function () { (p[t] || (p[t] = [])).push(arguments); });
    !o.getElementById(s+t) && o.getElementsByTagName("head")[0].appendChild((
      (n = o.createElement("script")),
      (n.id = s+t), (n.async = 1), (n.src = m), n
    ));
  }(window, document, "_pm", "PostmanRunObject", "https://run.pstmn.io/button.js"));
</script>
</div>

# Authentication

> To authorize, include your API key in all the headers of all requests you make.

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
curl "https://api.movem.co.uk/application"
  -H "x-api-key: demo"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `demo` with your API key.

Movem uses API keys to allow access to the API. You can register a new Movem API key at our [developer portal](http://developers.movem.co.uk).

Movem expects for the API key to be included in the header of all API requests.

`Authorization: demo`

<aside class="notice">
You must replace <code>demo</code> with your personal API key.
</aside>

# Application

To begin a reference you must first create a new application.  Each tenant should have their own application, which has a unique `id`.  Once the tenant clicks on the application `link` and starts the reference they are automatically given a `reference ID`.

## CREATE

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
CREATE "https://api.invite.movem.co.uk/application/"
  -B email: "john.appleseed@tenant.com",
  -B name: "John Appleseed",
  -B property: "124 Test Street"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json

{
    "id": "51356a30-ac6b-11e8-8c10-aa3fd76e3a0c",
    "reference_id": null,
    "status": "REQUESTED",
    "name": "John Appleseed",
    "property": "123 Infinite Loop",
    "email": "john.appleseed@tenant.com",
    "token": "3ad9791c6101b39ac76bb3083ed9191ef931bf1e3ae1ae58e5e92524a0d73a92",
    "expired_at": "2018-09-29",
    "created_at": "2018-08-30 16:42:35",
    "link": "https://reference.invite.movem.co.uk/3ad9791c6101b39ac76bb3083ed9191ef931bf1e3ae1ae58e5e92524a0d73a92"
}

```

This creates a unique URL which you can direct the tenant to in order to start their reference.  You will be deducted one credit from your account balance when you create a new application.

### HTTP Request

`GET https://api.movem.co.uk/application`

### Body Parameters

Parameter | Description
--------- | -----------
name  | The name of the tenant who you want to complete the reference.
email | The email address of the tenant who you want to complete the application.
property  | The name of the property linked to this tenant.

<aside class="notice">
We don't use the email addresses you send us to contact the tenant directly.
</aside>

### Response

Parameter | Description
--------- | -----------
id  | The unique Application ID.
reference_id | The unique ID for the reference.
status  | The current status of the application.
name  | The name of the tenant who you want to complete the reference.
property  | The name of the property linked to this tenant.
email | The email address of the tenant who you want to complete the application.
token | The unique token for the current application used in the invite URL.
expired_at | The date and time the application was created.
created_at | The date and time the application was created.
link | The unique URL for the tenant to complete their application and create a reference.

<aside class="success">
Applications can have the staus of <code>REQUESTED</code>, <code>IN PROGRESS</code>, <code>COMPLETED</code> and <code>REVOKED</code></aside>

## GET

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
GET "https://api.invite.movem.co.uk/application/[APPLICATION ID]"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
    "id": "57829366-a489-11e8-ac55-aa3fd76e3a2c",
    "reference_id": "379530",
    "status": "IN_PROGRESS",
    "name": "John Appleseed",
    "property": "123 Infinite Loop",
    "email": "john.appleseed@tenant.com",
    "token": "93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaec03407edc",
    "expired_at": "2018-09-19",
    "created_at": "2018-08-20 15:57:22",
    "link": "https://reference.invite.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaed13a07edc"
}
```

This endpoint retrieves details about a specific application.

### HTTP Request

`GET https://api.invite.movem.co.uk/application/<APPLICATION ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Application ID | The unique ID of the reference provided to you. This should be passed directly in the URL.

### Response

Parameter | Description
--------- | -----------
id  | The unique ID for the application.
reference_id | The unique ID for the reference.
status  | The current status of the application.
name  | The name of the tenant who you want to complete the reference.
property  | The name of the property linked to this tenant.
email | The email address of the tenant who you want to complete the application.
token | The unique token for the current application used in the invite URL.
expired_at | The date and time the application was created.
created_at | The date and time the application was created.
link | The unique URL for the tenant to complete their application and create a reference.

## UPDATE

This endpoint updares a specific application.  You are able to update the `name` and `property` fields only.  If you wish to update the `email` field you should `DELETE` the application and `CREATE` a replacement.


```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
`PUT https://api.invite.movem.co.uk/application/<APPLICATION ID>`

```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

### HTTP Request

`PUT https://api.invite.movem.co.uk/application/<APPLICATION ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Application ID | The unique ID of the reference provided to you. This should be passed directly in the URL.



## DELETE

This endpoint retrieves delete a specific application.  Applications can only be delted if their status is not yet `IN PROGRESS`.  You will automatically refunded one credit to your balance when the application is deleted.


```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
`DELETE https://api.invite.movem.co.uk/application/<APPLICATION ID>`

```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

### HTTP Request

`DELETE https://api.invite.movem.co.uk/application/<APPLICATION ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Application ID | The unique ID of the reference provided to you. This should be passed directly in the URL.


## LIST

This endpoint retrieves every `application` in your account, along with all their metadata.

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
GET "https://api.invite.movem.co.uk/application/"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
[
{
    "id": "57829366-a489-11e8-ac55-aa3fd76e3d2c",
    "reference_id": "379530",
    "status": "IN_PROGRESS",
    "name": "John Appleseed",
    "property": "123 Infinite Loop",
    "email": "john.appleseed@tenant.com",
    "token": "93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaec03407edc",
    "expired_at": "2018-09-19",
    "created_at": "2018-08-20 15:57:22",
    "link": "https://reference.invite.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaed13a07edc"
}
{
    "id": "57829366-a489-11e8-ac55-as3fd76e3a2c",
    "reference_id": "379531",
    "status": "COMPLETED",
    "name": "Susan Kare",
    "property": "123 Infinite Loop",
    "email": "susan.kare@icloud.com",
    "token": "93bba95f8c6d6d3d60679bda646a448ce56f01e27e348546fdbbdaec03407edc",
    "expired_at": "2018-09-19",
    "created_at": "2018-08-20 15:57:22",
    "link": "https://reference.invite.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbaaed13a07edc"
}
{
    "id": "57829366-a489-11e8-ac55-aa3fd76e3a2c",
    "reference_id": "379532",
    "status": "REVOKED",
    "name": "Andy Herzfeld",
    "property": "5 Cupertino Drive",
    "email": "andy.herzfeld@gmail.com",
    "token": "93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdagc03407edc",
    "expired_at": "2018-09-19",
    "created_at": "2018-08-20 15:57:12",
    "link": "https://reference.invite.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbgaed13a07edc"
}
]
```


### HTTP Request

`GET https://api.invite.movem.co.uk/application/`



### Response

An array of all your applications with the following data included.

Parameter | Description
--------- | -----------
id  | The unique ID for the application.
reference_id | The unique ID for the reference.
status  | The current status of the application.
name  | The name of the tenant who you want to complete the reference.
property  | The name of the property linked to this tenant.
email | The email address of the tenant who you want to complete the application.
token | The unique token for the current application used in the invite URL.
expired_at | The date and time the application was created.
created_at | The date and time the application was created.
link | The unique URL for the tenant to complete their application and create a reference.

# Credits

## GET

This endpoint retrieves your current credit balance.  You use one credit every time a new `application` is created.


```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
`GET https://api.movem.co.uk/credits`

```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
    "credits": 100
}
```

### HTTP Request

`DELETE https://api.invite.movem.co.uk/application/<APPLICATION ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Application ID | The unique ID of the reference provided to you. This should be passed directly in the URL.
