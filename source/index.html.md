---
title: Movem Partner API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://app.movem.co.uk/dashboard/settings/api'>Get your API Key</a>
  - <a href='https://help.movem.co.uk'>Movem Support</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Movem Partner API! 

You can use our API to create new tenant references, get the status of a reference, update a reference, get reference data or check your credit balance.

<aside class="notice">
If you are testing the API in our sandbox environment ensure you use the <code>https://api.qa1.movem.co.uk</code> endpoint.
</aside>

To play around with a few examples, we recommend a REST client called Postman. Simply tap the button below to import a pre-made collection of examples.

Remember to include the API version number you require in the URL.  The current version is `v1`.

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



```shell
curl "https://api.movem.co.uk/v1/application"
  -H "x-api-key: demo"
```

> Make sure to replace `demo` with your API key.

Movem uses API keys to allow access to the API. You can get your Movem API key at the [API Settings Page](https://app.movem.co.uk/dashboard/settings/api).

Movem expects for the API key to be included in the header of all API requests.

`Authorization: demo`

<aside class="notice">
You must replace <code>demo</code> with your personal API key.
</aside>

# Application

To begin a `reference` you must first create a new `application`.  

Each tenant should have their own `application`, which has a unique `id`.  

Once the tenant clicks on the application `link` and starts the `reference` they are automatically given a `reference ID`.


## CREATE

```shell
POST "https://api.movem.co.uk/v1/application/"
  -B email: "john.appleseed@tenant.com",
  -B name: "John Appleseed",
  -B property: "124 Test Street"
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
    "link": "https://reference.movem.co.uk/3ad9791c6101b39ac76bb3083ed9191ef931bf1e3ae1ae58e5e92524a0d73a92"
}

```

This creates a unique URL which you can direct the tenant to in order to start their reference.  You will be deducted one credit from your account balance when you create a new application.

### HTTP Request

`POST https://api.movem.co.uk/v1/application`

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


```shell
GET "https://api.movem.co.uk/v1/application/<APPLICATION ID>"
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
    "link": "https://reference.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaed13a07edc"
}
```

This endpoint retrieves details about a specific application.

### HTTP Request

`GET https://api.movem.co.uk/v1/application/<APPLICATION ID>`

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

This endpoint updates a specific application.  You are able to update the `name` and `property` fields only.  If you wish to update the `email` field you should `DELETE` the application and `CREATE` a replacement.



```shell
PUT "https://api.movem.co.uk/v1/application/<APPLICATION ID>"

```

> The above command returns JSON structured like this:

```json
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
    "link": "https://reference.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaed13a07edc"
}
```

### HTTP Request

`PUT https://api.movem.co.uk/v1/application/<APPLICATION ID>`

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


## DELETE

This endpoint deletes a specific application.  Applications can only be deleted if their status is not yet `IN PROGRESS`.  You will automatically refunded one credit.




```shell
DELETE "https://api.movem.co.uk/v1/application/<APPLICATION ID>"

```



### HTTP Request

`DELETE https://api.movem.co.uk/v1/application/<APPLICATION ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Application ID | The unique ID of the reference provided to you. This should be passed directly in the URL.


## LIST

This endpoint retrieves every `application` in your account, along with all their metadata.


```shell
GET "https://api.movem.co.uk/v1/application/"
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
    "link": "https://reference.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbdaed13a07edc"
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
    "link": "https://reference.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbaaed13a07edc"
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
    "link": "https://reference.movem.co.uk/93bba95f8c6d693d60679bda646a448ce56f01e27e348546fdbbgaed13a07edc"
}
]
```


### HTTP Request

`GET https://api.movem.co.uk/v1/application/`



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
expired_at | The date and time the application expired.
created_at | The date and time the application was created.
link | The unique URL for the tenant to complete their application and create a reference.


# Status

When the tenant completes an `application`, Movem generates a `reference`.

Movem will notify you when the `reference` object has been generated using our Update Webhook.  

This will push status updates to your configured endpoint.   You can figure where Movem sends updates on your [API Settings Page](https://app.movem.co.uk/dashboard/settings/api).

##Parameters

The Update Webhook will return the following parameters whenever the status of one of your references changes.

```shell
```

> Movem will send updates to your configured endpoint in this format:

```json
 {
        "application_id": "7937028e-f25a-11e8-a507-080027721fc3",
        "reference_id": "805974",
        "status": {
            "application": "COMPLETED",
            "reference": "REVIEW_NEEDED"
        },
        "timestamp": "2018-11-27 15:43:01"
    }
```

Parameter | Description | Field
--------- | ----------- | -----------
application_id  | The unique ID for the application.
reference_id | The unique ID for the reference.
status  | application  | The current status of the application.  See below.
  | reference | The current status of the reference.  See below.
timestamp  | Timestamp for when this activity occured.

## Application Statuses

List of possible results which could be returned for the application status field.

Status | Description
--------- | -----------
HOLD  | The application is on hold and has not been created.  This is usually when your account doesnt have a sufficient credit balance.
REQUESTED | The application has been generated.
IN_PROGRESS  | The tenant has clicked on the `application_link` and begun to complete the reference.
COMPLETED  | The tenant has completed the reference,
REVOKED  | The application was deleted.
EXPIRED | The application has expired.
ARCHIVED | The application has been archived.


## Reference Statuses

List of possible results which could be returned for the application status field.

Status | Description
--------- | -----------
IN_PROGRESS  | The tenant has clicked on the `application_link` and begun to complete the reference.
REVIEW_NEEDED | The tenant has completed the reference and has not automatically passed according to your decision scorecard rules and must be manually checked.  If you are using our Hybrid product a Movem Operator will review the reference manually.
PASSED  | The tenant has been passed automatically according to your Decision Scorecard rules.
FAILED  | The tenant has been failed automatically according to your Decision Scorecard rules.
MANUAL_PASSED  | The tenant has been passed manually.  If you are using our Hybrid product a Movem Operator will review the reference manually.
MANUAL_FAILED | The tenant has been failed manually.  If you are using our Hybrid product a Movem Operator will review the reference manually.

#Reference

When the user has finished the `application` process of running through the form we the then generate a `reference`.  The `reference` is created by combining all of the data collected in the form along with additional checks from third parties.  

When the `reference` is generated a `decision` will be created.  Once this `decision` is a `pass` or `fail` the PDF version of the reference will be available to pull through the Reference PDF endpoint described below.



##GET

You can then use the information returned by the `Status API` to retrieve details of a specific `reference`.


`GET https://api.movem.co.uk/v1/reference/<Reference ID>`

<aside class="notice">
The refernce data is returned as a single large block containing the sections listed below.</aside>

```shell
GET "https://api.movem.co.uk/v1/reference/<Reference ID>"

```

> The above command returns the reference json directly.

## Response

```shell
    "id": "650947",
    "application_id": "7ecb2dbe-1a60-11e9-8bfc-0a580a3000ec",
    "property": "123 Property Street",
    "full_name": "John Appleseed",
    "first_name": "John",
    "middle_name": null,
    "last_name": "Appleseed",
    "dob": "1990-01-01",
    "email": "john.appleseed@tenant.com",
```

This is the primary reference data block which is returned to you by calling the `reference` endpoint using the `reference_id`.



###Basic Data

This is the basic data about the tenant provided at the top of the reference block.

Parameter | Description
--------- | -----------
id  | The unique id for the reference.
application_id | The associated `application_id` for the reference.
full_name  | The tenant's full name. 
middle_name | The tenant's middle name. 
last_name | The tenant's last name.
dob | The tenant's date of birth.
email | The email address of the tenant who completed the application.

###Address

This address is retrieved from a sanitised database of valid and current Royal Mail PAF addresses.

```shell
"address": [
        {
            "sub_building": "111",
            "building_name": "York House",
            "building_number": null,
            "street": "Idlethorp Way",
            "district": null,
            "city": "Bradford",
            "postcode": "BD10 9ET",
            "province_name": "West Yorkshire",
            "from_date": "07/2018",
            "to_date": null,
            "full_address": "111 York House, Idlethorp Way, Bradford, BD10 9ET"
        }
```

Parameter | Description
--------- | -----------
sub_building  | Provided for some addresses to give extra detail.
building_name| Provided where available.
building_number | The building number of the tenant's address.
street  | Street name.
district  | Provided where available.
city  | City or town name.
postcode | Valid UK postcode.
province_name |  Provided where available.
from_date | The month & year the user declared as their move-in date for the property.
to_date | Date tenant declared they moved out of the property.  Only provided for second and previous addresses.
full_address  | The full address as a single string.



##Decision
```shell
 "decision": "PASS",
    "decision_text": "Passed",
    "decision_user": "Movem Operator",
    "decision_reason": "Verified income manually by contacting employer.",
```

This block returns the overall decision for the tenant.  To see the possible responses please refer to the [Reference Statuses](#reference-statuses) section.  This section is only returned when the reference is completed and a final decision has been made, either by our automatic scoring system or a Movem Operator, depending on your account type.

Parameter | Description
--------- | -----------
decision  | This is the decision result for the tenant.  A full list of possible comes can be viewed in the [Reference Statuses](#reference-statuses) section.
decision_text | This is a human readable version of the decision of the decision you can use in your application.  This will only ever be `Passed` or `Failed`
decision_user | This is only provided when a manual decision was required for the tenant.  If you made a manual decision internally the user name of the user who made the decision is provided.  If you used the Movem Hybrid manual check service `Movem Operator` will be returned.
decision_reason | This is the reason provided by the user who made the manual decision to pass or fail the tenant.

##Summary
This section contains an overall summary for the outcome of the decision on the tenant.  The result that can be `Pass`, `Caution` or `Fail`. Which on our PDF translates to a green, orange or red icon.

###Max Address History
```shell
        "max_address_search_exceeded": false,
```
Parameter | Description
--------- | -----------
max_address_search_exceeded  | This result is used as a flag to indicate whether we are displaying all of the address history available, or whether there is potentially more history and it exceeded our maximum of 5 addresses. This is not necessarily a negative, it just means the applicant has credit records at several addresses, but any negative records would still have been found.

###Identity Summary
```shell
"summary": {
            "identity_summary": {
                "message": "Report based on an address match",
                "result": "Fail"
            },
            
            
            
            
           
            },
            
           
       

```
This is the primary flag for a successful credit history lookup. A `caution` should be investigated manually.

Result | Message
--------- | -----------
Pass  | Confirmed
Caution  |  Report based on a surname match
Fail    |   Report based on a address match
Fail    |   No match was found for this individual or address.

###Aliases
```shell
"aliases": {
                "message": "No aliases were found for this individual",
                "result": "Pass"
            },
```
Name aliases are listed here. On our PDF, when no aliases are found, we simply don’t display this result, we only display this on a negative. But we always output the result on the JSON.

Result | Message
--------- | -----------
Pass  | No aliases were found for this individual
Caution    |   Aliases were found for this individual


###Identity Result
```shell
        "identity_result": {
            "id": "3",
            "message": "We found record of this tenant at the supplied address and background information has been returned."
        },
```
Confirms whether a full match was found for the individual, or if only a surname or address match was found.

Result | Message
--------- | -----------
0  | We were unable to locate this individual at this address in a credit referencing agency database.
1    |   We were unable to locate information about this individual in a credit referencing agency database based on name. The report below is based only on the address provided. Some credit history could be missing.
2   |   We were able to locate information about this individual in a credit referencing agency database based on surname, but not on full name. Some credit history could be missing.
3   |   We found record of this tenant at the supplied address and background information has been returned.




###Address
```shell
"address": {
                "message": "Not found at address",
                "result": "Caution"
            },
```
This is another primary flag for a successful credit history lookup. This is driven by the same result as the `identity_summary`. But is reduced to a binary result.

Result | Message
--------- | -----------
Pass  | Found at address
Caution    |   Not found at address


###Income
```shell
"incomes": {
                "message": "Verified by detected income",
                "result": "Pass"
            },
```
This section returns the outcome of the income part of our form. The user can choose to connect a real bank account or upload documents such as bank statements or payslips. They can also declare they have no income. When a real bank is connected and we detected regular income payments and group them, we’ll display these groups for the user to select from. When this grouping is not correct, the user can manually select from their incoming payments.

Result | Message
--------- | -----------
Pass  | Verified by detected income
Caution    |   Income selected from verified transactions
Caution |   Bank statements uploaded to show income
Fail    |   Applicant declares they earn no income


###Expenses
```shell
"expenses": {
                "message": "Verified by detected outgoing payments",
                "result": "Pass"
            },
```
This explains the outcome of the income part of our form. As above, we may have real bank transaction groups to display to the user for selection, or they can upload documents. And finally simply declare that they don’t have rent payments to show. There is not the option to manually select rent payments.

Result | Message
--------- | -----------
Pass  | Verified by detected outgoing payments
Caution    |   Bank statements uploaded to show rent
Fail    |   Applicant declares they currently pay no rent



###Employer
```shell
 "employer": {
                "message": "Employer contact information provided",
                "result": "Pass"
                },
```
If enabled, the user may provide employer contact information.

Result | Message
--------- | -----------
Pass  | Employer contact information provided
Caution    |   Employer contact information not provided


###Landlord
```shell
"landlord": {
                "message": "Landlord contact information provided",
                "result": "Pass"
            },
```
If enabled, the user may provide landlord contact information.

Result | Message
--------- | -----------
Pass  | Landlord contact information provided
Caution    |   Landlord contact information not provided





###Judgments
```shell
 "judgments": {
                "message": "No judgments found",
                "result": "Pass"
            },
```
If there are any CCJ records a Fail is returned.

Result | Message
--------- | -----------
Pass  | No judgments found
Fail    |   Judgments were found


###Bankruptcies
```shell
     "bankruptcies": {
                "message": "No Bankruptcies and insolvencies were found",
                "result": "Pass"
            },
```
If there are any Bankruptcy records a Fail is returned.

Result | Message
--------- | -----------
Pass  | No bankruptcies found
Fail    |   Bankruptcies were found

###Disputes & Corrections
```shell
            "disputeCorrection": {
                "message": "No Notices of Dispute or Correction were found",
                "result": "Pass"
            }
```
If there are any Notices of Dispute or Correction results, regardless of status, a `Caution` is returned. The requires needs manual review, and is not necessarily adverse credit.

Result | Message
--------- | -----------
Pass  | No notices or corrections found
Fail    |   Notices or corrections were found


##Address
This block confirms the address searched, and also linked addresses found.

###Searched Address
```shell
"searched_address": {
            "message": "Individual has been previously known on the Electoral Roll at their current address.",
            "declared": {
                "address": "606 ALLEY CAT LANE, TEST TOWN X9 9AA",
                "description": "Individual confirmed using a previous Electoral Roll."
            },
```
This section displays results based on the user-selected address that was used for the search. This is either the current or previous address declared on the form, depending on the move-in date.

Below is a list of the possible messages returned with this section.

Result | Message
--------- | -----------
1  | Individual confirmed on the Electoral Roll at the current address.
2  | Individual has been previously known on the Electoral Roll at their current address
3   |   The individual is not known on the Electoral Roll at the current address. However the surname has been confirmed on the Electoral Roll at the current address.
4   |   Surname confirmed as previously being on the Electoral Roll at the current address. The individual is not confirmed on the Electoral Roll at the current address.
5   |   The individual is not known on the Electoral Roll at the current address. However individual has been confirmed on the Electoral Roll at one of their searched addresses.
6   |   The individual is not known on the Electoral Roll at the current address. However individual previously known on the Electoral Roll at one of their searched addresses.
7   |   The individual is not known on the Electoral Roll at any of the entered addresses. However the surname has been confirmed on the Electoral Roll at one of their searched addresses.
8   |   The individual is not known on the Electoral Roll at any of the entered addresses. However the surname has been previously known on the Electoral Roll at one of their searched addresses.
9   |   Individual not known on the Electoral Roll at any of the entered addresses. However the individual has been matched at the current address to selected databases.
10  |   Individual is not known on the Electoral Roll at any of the entered addresses. However the individual has been matched by other selected databases.
11  |   Individual not known on the Electoral Roll or other selected databases at any of the entered addresses. However other individuals have been confirmed on the Electoral Roll at the current address.
12  |   Individual not known in the Electoral Roll or other selected databases at any of the entered addresses. However other individuals have been confirmed on the Electoral Roll at one of their searched addresses.
13  |   Individual is not known on the Electoral Roll or on other selected databases at any of the entered addresses.
14  |   Individual not known on the Electoral Roll at any of the entered addresses. However the individual has been matched at the current address to selected databases. Please note that the information used to confirm this address is less than 12 months old.
15  |   Individual not known on the Electoral Roll or other selected databases at any of the entered addresses. However other individuals have previously been known on the Electoral Roll at the current address.
16  |   Individual not known in the Electoral Roll or other selected databases at any of the entered addresses. However other individuals have been previously known on the Electoral Roll at one of their searched addresses.


###Address Links
```shell
      "links": [
                {
                    "address": "15, HIGH STREET, BRIGHTON BN1 1AA",
                    "type": "Current address",
                    "sources": "Bank (30\/09\/2010 to 09\/10\/2010)"
                }
                ]

```
This section lists related addresses found on this individual’s credit record.

Parameter | Description
--------- | -----------
Address  | Address provided as a string
Type    |   Type of address link
Sources |   The source of the address link data.
Dates   |   A from and to date is provided. It is possible only one date is returned.

##Aliases
```shell
        "aliases": [
            {
                "alias_found": "MR JON GRAPESEED",
                "first_recorded": "01\/01\/2004",
                "last_updated": "01\/01\/2004",
                "submitted_by": "Credit Card"
            }
        ],
```
If name aliases are found for this individual, they are detailed here, including the source.

Parameter | Description
--------- | -----------
alias_found  | The alternative name found for the individual
first_recorded    |   Date this alias was first recorded
last_updated    | Date the alias was most recently recorded
submitted_by    |   The source for this data.

##Judgements
```shell
        "judgments": [
            {
                "name": "MR FRED MANX",
                "address": "606 ALLEY CAT LANE X9 9AA",
                "current_address": "Yes",
                "court_name": "TESTTOWN",
                "court_type": "County Court Judgment",
                "case_number": "TEST 7825221",
                "status": "Satisfied",
                "amount": "250",
                "date": "18/07/2015",
                "notice": []
            }
        ],
```
The raw data of each judgement is returned in this block, if data is available.  Multiple judgements can be returned, and judgements can contain notices of dispute or correction.

Parameter | Description
--------- | -----------
name  | Name associated with the judgement.
address   |   Address associated with the judgement.
current_address    | Whether the address is the tenant's current address.
court_name    |   The county court which issued the judgement.
court_type  | The type of court which issued the judgement.
case_number | The judgement case number.
status  |   The current status of the judgement.  If the debt is still outstanding this will be `active`, if the amount is settled this will be `satisfied`.  If the judgement has been cancelled this will be `cancelled`.
amount  |   The amount in GBP the judgement is usued for.
date    |   Date the judgement was issued
notice  |   If there is a notice of correction it will be supplied here.


##Bankruptices & Insolvencies

```shell
           "bankruptcies_insolvencies": [
            {
                "court_name": "VOLUNTARY LIQUIDATION CASES",
                "status": "Active",
                "order_type": "Individual Voluntary Arrangement Made",
                "date": "14\/09\/2015",
                "case_year": "N\/A",
                "case_reference": "N\/A",
                "name": "MR JOHN DOE",
                "address": "5 HIGH STREET, BRIGHTON, BN1 1AA",
                "current_address": "No"
            },
            {
                "court_name": "BRIGHTON",
                "status": "Active",
                "order_type": "Bankruptcy Order",
                "date": "11\/09\/2009",
                "case_year": "2005",
                "case_reference": "410032",
                "name": "MR JOHN DOE",
                "address": "15 HIGH STREET, BRIGHTON, BN1 1AA",
                "current_address": "Yes"
            }
        ],

```

Details of known bankruptcies appear in this block. Bankruptcies can contain notices of dispute or correction.

Parameter | Description
--------- | -----------
court_name  | Name of the court which issued the notice.
status  |   The current status of the notice.  If the debt is still outstanding this will be `active`, if the amount is settled this will be `discharged` or `completed`.  If the judgement has been cancelled this will be `revoked`
case_year    |   Year the case was started
case_reference  |   Case reference number
name    |   Name of individual linked with the notice
address | Address linked with the notice
current_address |   A flag to confirm whether the address matches the searched address.

##Notices of Dispute & Correction
```shell
 "dispute_correction": [
            {
                "type": "Dispute",
                "reference_number": "123456",
                "date": "11\/06\/2010",
                "name": "MR JOHN DOE",
                "address": "15, HIGH STREET, BRIGHTON BN1 1AA",
                "current_address": "No",
                "address_type": "Miscellaneous Address Link",
                "description": "Example text. A general notice on the entire credit history record."
            }
        ],
```

A credit file may have a general notice of dispute or correction, containing an explanation.  

This block is optional and may rarely occur, it can occur on it’s own or it can exist as a block inside of other entities:

1. Address links
2. Judgements
3. Bankruptcies & insolvencies
4. Confirmed addresses
5. Electoral roll history

Parameter | Description
--------- | -----------
type    |   This can be a `dispute` or a `correction`.
reference_number  |   Case reference number
date    |   Date the issue was raised.
name    |   Name of individual linked with the notice
address | Address linked with the notice
current_address |   A flag to confirm whether the address matches the searched address.
address_type    |   Type of address and how the link was made.
description |   The body of the description of the correction.


##Income
```shell
 "incomes": [
            {
                "status": "INCOME_SELECTED",
                "company_number": "9837987398",
                "company_title": "Tesco PLC",
                "job_title": "Store Assistant",
                "employer_name": "Tesco PLC",
                "employer_phone": "01273 777888",
                "employer_email": "storemanager@tesco.com",
                "salary": "262687.01",
                "monthly_takehome": "12500",
                "affordability": "8756.23",
                "has_pension": 0,
                "has_student_loan": 0,
                "still_in_education": null,
                "loan_year": null,
                "country_of_loan": null,
```


This section provides data on the tenant's income.  The tenant can choose to connect a real bank account or upload documents such as bank statements or payslips. They can also declare they have no income. 

Parameter | Description
--------- | -----------
status    |   If the user had their income automatically detected this will be `INCOME_SELECTED`. If the user manually selected their transactions this will be `INCOME_MANUAL_SELECTED`.  If the tenant uploaded statements manually after failing to connect a bank account this will be `INCOME_STATEMENT_UPLOADED`.  If the tenant did not try to connect a bank account then manually uploaded statements this will be `INCOME_STATEMENT_UPLOADED_NO_BANK`.  If the tenant decalre they have no income this will be `NO_INCOME`.
company_number*  | The registered company number at companies house
company_title*   | The name of the company as provided by companies house
job_title   | The tenant's job title
employer_name   | The name of the tenant's employer 
employer_phone  | The phone number of the tenant's line manager (if available)
employer_email |  The email address of the tenant's line manager (if available)
salary  | The salary as calculated using their verified income data.
monthly_takehome | The tenant's monthly takehome pay as calculated by Movem.
affordability   | The maximum amount the tenat can afford each month on rent.  This varies depending on your settings.
has_pension | The tenant has a pension plan.
has_student_loan    | The tenant has a student loan.
still_in_education  | The tenant is still in education.
loan_year   | The year the tenant started their student loan repayments.
country_of_loan | The country which the tenant's student loan is from.

_*Note: company_number and company_title will be deprecated in April 2019._


### Income Transaction Set
```shell
"transactions": [
                        {
                            "date": "2019-01-01 00:00:00",
                            "description": "TESCO PLC WAGE XXXXXXXXXXXXXX6001",
                            "type": "C",
                            "category": "INCOME",
                            "amount": 1200,
                            "currency": "GBP"
                        },
                        {
                            "date": "2018-12-01 00:00:00",
                            "description": "TESCO PLC WAGE  XXXXXXXXXXXXXX6001",
                            "type": "C",
                            "category": "INCOME",
                            "amount": 1200,
                            "currency": "GBP"
                        },
                        {
                            "date": "2018-11-01 00:00:00",
                            "description": "MOVE M MOVEM WAGE XXXXXXXXXXXXXX6001",
                            "type": "C",
                            "category": "TESCO PLC WAGE ",
                            "amount": 1200,
                            "currency": "GBP"
                        },
]
```
When the income is verified by selection of real bank transactions. A transactions object will exist inside the incomes section. 

This object holds information about the bank account, and then an array of transactions.

Parameter | Description
--------- | -----------
date    | Date of the transaction as provided by the bank.
description | The payment reference of the transaction linked to the transaction in the tenant's bank account.
type    |   The type for the transaction.  `R` means the transaction was manually selected.  `C` represents when a credit when an income group was automatically selected.  `D` represents a debit when an income group was automatically selected.
category    | The name of the group of the transactions, based on the payment reference number.
amount  | A sumarised amount of the transaction.
currency    | The currency of the transactions.

###Account
```shell
                    "account": {
                        "bank_name": "Monzo",
                        "holder_name": "John Appleseed",
                        "name": "CURRENT ACCOUNT",
                        "type": "CHECKING",
                        "number": "xxxx1234",
                        "sortcode": null
                    }
```
At the bottom of the transaction set we will return information about the bank account which was connected to provide the data.

Parameter | Description
--------- | -----------
bank_name    |   Name of the bank which was connected.  
holder_name |   Name of the account holder as provided by the bank.
name    | Name of the account.
type    | The type of bank account, usually `CHECKING`.
number  |   Masked account number.
sortcode    |   Masked sort code.

##Rent 
```shell
       "rents": [
            {
                "status": "EXPENSE_SELECTED",
                "share_of_rent": "1200",
                "landlord_type": "Landlord",
                "landlord_name": "Mr. J Homeowner",
                "landlord_phone": "01237 454678",
                "landlord_email": "j.house@homeowner.com",
```
When rental payments are verified from bank transactions we provide information about the landlord, the bank account and an array of individual transactions.

Parameter | Description
--------- | -----------
status    |   `EXPENSE_SELECTED` is returned if the transactions were automatically detected by Movem.
share_of_rent   |  This is the amount of rent the tenant pays. If this value is the same as the amount in the transaction set summary, then the tenant hasn’t made an amendment. Otherwise, the user may have entered a lower number, to explain that they pay a full sum to the landlord on behalf of housemates. 
landlord_type   | This will return `agent` or `landlord`. _Optional_
landlord_name   |   The name of the landlord/agent as provided by the tenant. _Optional_
landlord_phone  |   This will return the phone number of the landlord/agent as provided by the tenant. _Optional_
landlord_email  |   This will retuen the email address of the landlord/agent as provided by the tenant. _Optional_

###Rent Transaction Set
```shell
                    "transactions": [
                        {
                            "date": "2019-01-05 00:00:00",
                            "description": "Standing Order JOHN APPLESEED 1 INFINTE LOOP",
                            "type": "D",
                            "category": "EXPENSE",
                            "amount": 1050,
                            "currency": "GBP"
                        },
                        {
                            "date": "2018-12-05 00:00:00",
                            "description": "Standing Order JOHN APPLESEED 1 INFINTE LOOP",
                            "type": "D",
                            "category": "EXPENSE",
                            "amount": 1050,
                            "currency": "GBP"
                        },
                        {
                            "date": "2018-11-05 00:00:00",
                            "description": "Standing Order JOHN APPLESEED 1 INFINTE LOOP",
                            "type": "D",
                            "category": "EXPENSE",
                            "amount": 1050,
                            "currency": "GBP"
                        },
```
When rent is verified by selection of real bank transactions. A transactions object will exist inside the rent section. 

This object holds information about the bank account, and an array of transactions.

Parameter | Description
--------- | -----------
date    | Date of the transaction as provided by the bank.
description | The payment reference of the transaction linked to the transaction in the tenant's bank account.
type    |   The type for the transaction.  `R` means the transaction was manually selected.  `C` represents when a credit when an income group was automatically selected.  `D` represents a debit when an income group was automatically selected.
category    | The name of the group of the transactions, based on the payment reference number.
amount  | A sumarised amount of the transaction.
currency    | The currency of the transactions.

###Account
```shell
                    "account": {
                        "bank_name": "Monzo",
                        "holder_name": "John Appleseed",
                        "name": "CURRENT ACCOUNT",
                        "type": "CHECKING",
                        "number": "xxxx1234",
                        "sortcode": null
                    }
```
At the bottom of the transaction set we will return information about the bank account which was connected to provide the data.  This can be a different account than the one used to verify income.

Parameter | Description
--------- | -----------
bank_name    |   Name of the bank which was connected.  
holder_name |   Name of the account holder as provided by the bank.
name    | Name of the account.
type    | The type of bank account, usually `CHECKING`.
number  |   Masked account number.
sortcode    |   Masked sort code.

##Requirements  
```shell
    "requirements": {
        "pets": {
            "key": "pets",
            "label": "Do you have pets?",
            "value": "Yes"
        },
        "smoke": {
            "key": "smoke",
            "label": "Do you smoke at home?",
            "value": "Yes"
        },
        "move_in": {
            "key": "move_in",
            "label": "When are you looking to move in?",
            "value": "In the next few weeks"
        },
        "benefits": {
            "key": "benefits",
            "label": "Do you receive housing benefits?",
            "value": "Yes"
        },
        "nationality": {
            "key": "nationality",
            "label": "Nationality:",
            "value": "United Kingdom"
        },
```
You are able to configure a number of custom questions which are asked to each tenant during the referencing process.  These are returned in the `requirements` block.  The data which is returned here will vary depending in your configuration.

#Reference PDF

##GET

You are able to request a copy of the Reference PDF for a given tenant using the API.  [You can download an example reference PDF file here.](https://cdn.movem.co.uk/downloads/JohnAppleseed.pdf)

`GET https://api.movem.co.uk/v1/reference/<Reference ID>/report`


```shell
GET "https://api.movem.co.uk/v1/reference/<Reference ID>/report"

```

> The above command returns the file directly.


#Statements

##GET

You are able to request a copy of the Statements uploaded by the tenant for manual review using the API.

`GET https://api.movem.co.uk/v1/reference/<Reference ID>/statements`


```shell
GET "https://api.movem.co.uk/v1/reference/<Reference ID>/statements"

```

> The above command returns the file directly.

#Reference Meta

##GET
```shell
GET "https://api.movem.co.uk/v1/reference/<Reference ID>/meta"

> The above command returns the meta data directly.

```
A selection of other information relating to the reference object is also available.  These include which step the tenant is currently on in the process, and all the notes linked to the reference.

### Notes
> The response below covers the notes for the reference.

```shell
    "notes": [
        {
            "user_id": 5355,
            "user_name": "Movem Operator",
            "note": "Verified income manually by contact employer",
            "created_at": "2019-01-17 14:10:51"
        }
    ],
```
Any notes connected with your reference will be returned here.  These can either be notes you have added yourself or which have been added by a Movem Operator when manually verifing the tenant's information.

Parameter | Description
--------- | -----------
user_id | The unique user ID of the user who made the note.
user_name | The name of the user who made the note.  If it is a member of the Movem team this will show as `Movem Operator`.
note  | The main body of the note.
created_at  | The time and date the note was created.



### Progress
> The response below covers the progress for the reference.

```shell
    "started_at": "2019-01-17 14:03:02",
    "finished_at": "2019-01-17 14:03:02",
    "progress": {
        "identity": "2019-01-17 14:05:05",
        "identity-verify": "2019-01-17 14:05:09",
        "verify-method": "2019-01-17 14:07:28",
        "bank_account_linked": "2019-01-17 14:07:43",
        "verify": "2019-01-17 14:07:53",
        "income": "2019-01-17 14:08:08",
        "rent": "2019-01-17 14:08:19",
        "identity-processing": "2019-01-17 14:08:22",
        "requirements": "2019-01-17 14:08:39",
        "complete": "2019-01-17 14:08:42"
    },
```
We store every action the user takes throught the referencing process.  You can use this data to work out where they are in the flow.  This can be useful if you wish to determine whether the user has connected their bank account for example.  

These fields are only provided back if they have a value.  For example, if the user has not connected their bank account we will not return the `bank_account_linked` value.

Parameter | Description
--------- | -----------
started_at  | The time the tenant began the referencing process.
finished_at | The time the tenant completed the referencing process.

Parameter | Description
--------- | -----------
identity  | The tenant has started the identity verification process.
identity-verify | The tenant has completed the identity verification process.
verify-method  | This is an internal flag for whether the user manually uploaded documents.
bank_account_linked  | The tenant connected a bank account.
verify  | The tenant completed the verification step.
income | The tenant has completed the income step.
rent | The tenant completed the rent step.
identity-processing | Identity processing with our data partners was completed.
requirements | The tenant completed the requirements step with your custom questions.
complete | The tenant completed the user journey of the referencing process.


# Credits

## GET

This endpoint retrieves your current credit balance.  You use one credit every time a new `application` is created.



```shell
GET "https://api.movem.co.uk/v1/credits"

```



> The above command returns JSON structured like this:

```json
{
    "credits": 100
}
```

### HTTP Request

`GET https://api.movem.co.uk/v1/credits`

### URL Parameters

Parameter | Description
--------- | -----------
Application ID | The unique IDs of the reference provided to you. This should be passed directly in the URL.


<script>
  window.intercomSettings = {
    app_id: "cb3oyq1f"
  };
</script>
<script>(function(){var w=window;var ic=w.Intercom;if(typeof ic==="function"){ic('reattach_activator');ic('update',w.intercomSettings);}else{var d=document;var i=function(){i.c(arguments);};i.q=[];i.c=function(args){i.q.push(args);};w.Intercom=i;var l=function(){var s=d.createElement('script');s.type='text/javascript';s.async=true;s.src='https://widget.intercom.io/widget/cb3oyq1f';var x=d.getElementsByTagName('script')[0];x.parentNode.insertBefore(s,x);};if(w.attachEvent){w.attachEvent('onload',l);}else{w.addEventListener('load',l,false);}}})();</script>