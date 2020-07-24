---
title: Edgar API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:


search: true

code_clipboard: true
---

# Introduction

The Halider Edgar API is organized around REST. Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

The Halider Edgar API is designed to provide read only access to all data within the SEC.gov Edgar system https://www.sec.gov/edgar/searchedgar/accessing-edgar-data.htm

We have language bindings in Shell, Python and Javascript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

Subscribe to Halider Edgar API announce mailing list for updates.

# What's a REST API, anyway?

An API is an application programming interface - in short, it’s a set of rules that lets programs talk to each other, exposing data and functionality across the internet in a consistent format.

REST stands for Representational State Transfer. This is an architectural pattern that describes how distributed systems can expose a consistent interface. When people use the term ‘REST API,’ they are generally referring to an API accessed via HTTP protocol at a predefined set of URLs.

These URLs represent various resources - any information or content accessed at that location, which can be returned as JSON, HTML, audio files, or images. Often, resources have one or more methods that can be performed on them over HTTP, like GET, POST, PUT and DELETE.

Twilio, for example, provides many separate REST APIs for sending text messages, making phone calls, looking up phone numbers, managing your accounts, and a whole lot more. In Twilio’s ecosystem, each product is its own API, but you will work with each of them in roughly the same way, whether over HTTP or using Twilio’s helper libraries for several different programming languages.


# Authentication

> To authorize, use this code:

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('url',headers=ploads)
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: yourkey"
```

> Make sure to replace `yourkey` with your API key.

The Halider Edgar API uses API keys to authenticate requests. Please reach out to zac@salt.io to request an API key.

Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

Authentication to the API is performed via an HTTP header called 'X-'. 

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.


`Authorization: yourkey`

<aside class="notice">
You must replace <code>yourkey</code> with your personal API key.
</aside>

# API Reference

## API Requests

To make a REST API request, you combine the HTTP <code>GET</code>,<code>POST</code>, <code>PUT</code>,<code>PATCH</code>, or <code>DELETE</code> method, the URL to the API service, the URI to a resource to query, submit data to, update, or delete, and one or more HTTP request headers.

The URL to the API service is either:

<ul>
    <li>Sandbox. <code>https://api.sandbox.paypal.com</code></li>
    <li>Live. <code>https://api.paypal.com</code></li>
</ul>

Optionally, you can include query parameters on <code>GET</code> calls to filter, limit the size of, and sort the data in the responses.

Most <code>GET</code>,<code>POST</code>, <code>PUT</code>, and <code>PATCH</code> calls require a JSON request body.

>A basic example in Shell:

```shell
curl --location --request GET "https://edgar.halider.io/abs" --header "yourkey"
```

### Query Parameters

For most REST <code>GET</code> calls, you can include one or more query parameters on the request URI to <b>filter, limit the size of, and sort the data</b> in an API response. For filter parameters, see the individual <code>GET</code> calls.

To limit, or <i>page</i>, and sort the data that is returned in some API responses, use these, or similar, query parameters:

<aside class="notice">
    Not all pagination parameters are available for all APIs.
</aside>
    
Parameter | Type | Description
--------- | -----|-------------
<code>count</code> | Integer | The number of items to list in the response.
<code>end_time</code> | Integer | The end date and time for the range to show in the response, in Internet date and time format. For example, <code>end_time=2016-03-06T11:00:00Z</code>.
<code>page</code> | Integer | The page number indicating which set of items will be returned in the response. So, the combination of <code>page=1</code> and <code>page_size=20</code> returns the first 20 items. The combination of <code>page=2</code> and <code>page_size=20</code> returns items 21 through 40.
<code>page_size</code> | Integer | The number of items to return in the response.
<code>total_count_required</code> | Boolean | Indicates whether to show the total count in the response.
<code>sort_by</code> | String | Sorts the documents in the response by a specified value, such as the create time or update time.
<code>sort_order</code> | String | Sorts the items in the response in ascending or descending order.
<code>start_id</code> | String | The ID of the starting resource in the response. When results are paged, you can use the <code>next_id</code> value as the <code>start_id</code> to continue with the next set of results.
<code>start_index</code> | Integer | The start index of the documents to list. Typically, you use the <code>start_index</code> to jump to a specific position in the resource history based on its <b>PLACEHOLDER</b>. For example, to start at the second item in a list of results, specify <code>?start_index=2</code>.
<code>start_time</code> | String | The start date and time for the range to show in the response, in Internet date and time format. For example, <code>start_time=2016-03-06T11:00:00Z</code>.

> For example, this shell code calls for a subcatagory (CMBS) within a larger catagory (ABS) within the database.

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs" --header "yourkey"
```

### HTTP Request Headers

The commonly used headers are:

<b> Accept </b> 

The response format, which is required for operations with a response body. The syntax is:

<code> Accept: application/<i>format</i> </code>
    
Where <code><i> format </i></code> is <code> JSON </code>.

<b> Authorization </b>

See the section of the same name above for information on Authorization and how to be authorized in Edgar API.

<b> Content-Type </b>

The request format, which is required for operations with a request body. The syntax is:

<code> Content-Type: application/<i>format</i> </code>
    
Where <code><i>format</i></code> is <code> JSON </code>.


## API Responses

Edgar API calls return HTTP status codes. Some API calls also return JSON response bodies that include information about the resource including one or more contextual HATEOAS links. Use these links to request more information about and construct an API flow that is relative to a specific request. Each REST API request returns an HTTP status code.

### HTTP status codes

For successful requests, Edgar returns HTTP <code>2XX</code> status codes.

For failed requests, Edgar returns HTTP <code>4XX</code> or <code>5XX</code> status codes.

Edgar API returns these HTTP status codes:
                
Code | Meaning
---------- | -------
<code>200 OK</code> | The request suceeded.
<code>201 Created</code> | A <code>POST</code> method successfully created a resource. If the resource was already created by a previous execution of the same method, for example, the server returns the HTTP <code>200 OK</code> status code.
<code> 202 Accepted </code> | The server accepted the request and will execute it later.
<code> 204 No Content </code> | The server successfully executed the method but returns no response body.
<code>400 Bad Request</code> |<code>INVALID_REQUEST</code>. Request is not well-formed, syntactically incorrect, or violates schema.
<code>401 Unauthorized</code> |<code>AUTHENTICATION_FAILURE</code>. Authentication failed due to invalid authentication credentials.
<code>403 Forbidden</code> | <code>NOT_AUTHORIZED</code>. Authorization failed due to insufficient permissions.
<code>404 Not Found</code> | <code>RESOURCE_NOT_FOUND</code>. The specified resource does not exist.
<code>405 Method Not Allowed</code> | <code>METHOD_NOT_SUPPORTED</code>. The server does not implement the requested HTTP method.
<code>406 Not Acceptable</code> | <code>MEDIA_TYPE_NOT_ACCEPTABLE</code>. The server does not implement the media type that would be acceptable to the client.
<code>415 Unsupported Media</code> | <code>UNSUPPORTED_MEDIA_TYPE</code>. The server does not support the request payload’s media type. 
<code>422 Unprocessable Entity</code> |<code>UNPROCESSABLE_ENTITY</code>. The API cannot complete the requested action, or the request action is semantically incorrect or fails business validation.
<code>429 Unprocessable Entity</code> |<code>RATE_LIMIT_REACHED</code>. Too many requests. Blocked due to rate limiting.
<code>500 Internal Server Error</code> |<code>INTERNAL_SERVER_ERROR</code>. An internal server error has occurred.
<code>503 Service Unavailable</code> | <code>SERVICE_UNAVAILABLE</code>. Service Unavailable.

For all errors except Identity errors, Edgar returns an error response body that includes additional error details in this format.

### Validation Errors

For validation errors, Edgar returns the HTTP <code>400 Bad Request</code> status code.

To prevent validation errors, ensure that parameters are the right type and conform to constraints:

Parameter | Description
--------- | -----------
Character | Names, addresses, and phone numbers have maximum character limits.
Numeric | <b>PLACEHOLDER</b>
Monetary | Use the right currency.
Format | Properly format the JSON sent in the body of your request. For example, no trailing commas.

### Authorization Errors

For authorization errors, Edgar returns the HTTP <code>401 Unauthorized</code> status code. See <a href="https://developer.paypal.com/docs/api-basics/#oauth-20-authorization-protocol">OAuth 2.0 authorization protocol.</a>

Access token-related issues often cause authorization errors.

Ensure that the access token is valid and present and not expired.

# ABS

### Start

```shell
curl --location --request GET "https://edgar.halider.io/abs" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs',headers=ploads)
```

This returns a list of options, choose one and put it into the URL to proceed. 

```JSON
{
  "types": [
    null,
    "autoLoan",
    "cmbs",
    "autoLease"
  ]
}
```

## Options in ABS

assetType lets you select either CMBS, autoLoan, or autoLease, and the Id's let you specify within those catagories.

Parameter | Description and Placement
--------- | -----------
assetType | Either the cmbs, autoLoan, or autoLease sections of abs, 1.
dealId | Either the ID, CIK, or Name of a specific deal, 2.
assetId |  A specific File within the deal, 3.

### CMBS

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs',headers=ploads)
```

This returns the document with all of the CMBS on it.

```JSON 
 {
      "id": "1",
      "cik": 1710798,
      "assetType": "cmbs",
      "names": [
        "Wells Fargo Commercial Mortgage Trust 2017-C39"
      ]
 }
```


### CMBS Options


Options availible while under abs/cmbs

JSON Attributes:

Attribute | Description 
--------- | -----------
Id | 1.
Date |  2.
Type | 3.
Extracted | 4.

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs/dealId" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/dealId',headers=ploads)
```
> A sample response.

```JSON
{
    "id": "2110",
    "date": "2019-07-31",
    "type": "ABS-EE",
    "extracted": true
}
```

>etc.

<aside class="notice">
To find the "FILE" you must scroll down and find the "assetIds" and then insert the numeric Id that corresponds to what you are looking for.
</aside>

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs/DEAL/FILE" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/DEAL/FILE',headers=ploads)
```

JSON Attributes:

Attribute | Description 
--------- | -----------
assetTypeNumber | 1
assetNumber | 2
reportingPeriodBeginningDate | 3
reportingPeriodEndDate | 4
originatorName | 5
originationDate | 6
originalLoanAmount | 7
originalTermLoanNumber | 8
maturityDate | 9
originalInterestRatePercentage | 10
interestRateSecuritizationPercentage | 11
interestAccrualMethodCode | 12
originalInterestRateTypeCode | 13
originalInterestOnlyTermNumber | 14
firstLoanPaymentDueDate | 15
underwritingIndicator | 16 
lienPositionSecuritizationCode | 17
loanStructureCode | 18
paymentTypeCode | 19
periodicPrincipalAndInterestPaymentSecuritizationAmount | 20
scheduledPrincipalBalanceSecuritizationAmount | 21
paymentFrequencyCode | 22
NumberPropertiesSecuritization | 23
NumberProperties | 24
interestOnlyIndicator | 25
balloonIndicator | 26
prepaymentLockOutEndDate | 27
property | 28
propertyName | 29
propertyAddress | 30
propertyCity | 31
propertyState | 32
propertyZip | 33
propertyCounty | 34
propertyTypeCode | 35
netRentableSquareFeetNumber | 36
netRentableSquareFeetSecuritizationNumber | 37
yearBuiltNumber | 38
yearLastRenovated | 39
valuationSecuritizationAmount | 40
valuationSourceSecuritizationCode | 41
valuationSecuritizationDate | 42
physicalOccupancySecuritizationPercentage | 43
mostRecentPhysicalOccupancyPercentage | 44
propertyStatusCode | 45
defeasanceOptionStartDate | 46
DefeasedStatusCode | 47
largestTenant | 48
squareFeetLargestTenantNumber | 49
leaseExpirationLargestTenantDate | 50
secondLargestTenant | 51
squareFeetSecondLargestTenantNumber | 52
leaseExpirationSecondLargestTenantDate | 53
thirdLargestTenant | 54
squareFeetThirdLargestTenantNumber | 55
leaseExpirationThirdLargestTenantDate | 56
financialsSecuritizationDate | 57
mostRecentFinancialsStartDate | 58
mostRecentFinancialsEndDate | 59
revenueSecuritizationAmount | 60
mostRecentRevenueAmount | 61
operatingExpensesSecuritizationAmount | 62
operatingExpensesAmount | 63
netOperatingIncomeSecuritizationAmount | 64
mostRecentNetOperatingIncomeAmount | 65
netCashFlowFlowSecuritizationAmount | 66
mostRecentNetCashFlowAmount | 67
netOperatingIncomeNetCashFlowSecuritizationCode | 68
netOperatingIncomeNetCashFlowCode | 69
mostRecentDebtServiceAmount | 70
debtServiceCoverageNetOperatingIncomeSecuritizationPercentage | 71
mostRecentDebtServiceCoverageNetOperatingIncomePercentage | 72
debtServiceCoverageNetCashFlowSecuritizationPercentage | 73
mostRecentDebtServiceCoverageNetCashFlowpercentage | 74
debtServiceCoverageSecuritizationCode | 75
mostRecentDebtServiceCoverageCode | 76
reportPeriodBeginningScheduleLoanBalanceAmount | 77
totalScheduledPrincipalInterestDueAmount | 78
reportPeriodInterestRatePercentage | 79
servicerTrusteeFeeRatePercentage | 80
scheduledInterestAmount | 81
reportPeriodEndActualBalanceAmount | 82
reportPeriodEndScheduledLoanBalanceAmount | 83
paidThroughDate | 84
servicingAdvanceMethodCode | 85
primaryServicerName | 86



> A sample response.

```JSON
{
      "assetTypeNumber": "Prospectus Loan ID",
      "assetNumber": 1,
      "reportingPeriodBeginningDate": "02-12-2019",
      "reportingPeriodEndDate": "03-11-2019",
      "originatorName": "Barclays Bank PLC",
      "originationDate": "05-31-2017",
      "originalLoanAmount": 70000000,
      "originalTermLoanNumber": 120,
      "maturityDate": "06-06-2027",
      "originalInterestRatePercentage": 0.036514,
      "interestRateSecuritizationPercentage": 0.036514,
      "interestAccrualMethodCode": 3,
      "originalInterestRateTypeCode": 1,
      "originalInterestOnlyTermNumber": 120,
      "firstLoanPaymentDueDate": "07-06-2017",
      "underwritingIndicator": true,
      "lienPositionSecuritizationCode": 1,
      "loanStructureCode": "PP",
      "paymentTypeCode": 3,
      "periodicPrincipalAndInterestPaymentSecuritizationAmount": 220098.28,
      "scheduledPrincipalBalanceSecuritizationAmount": 70000000,
      "paymentFrequencyCode": 1,
      "NumberPropertiesSecuritization": 1,
      "NumberProperties": 1,
      "interestOnlyIndicator": true,
      "balloonIndicator": true,
      "prepaymentLockOutEndDate": "02-05-2027",
      "property": {
        "propertyName": "225 & 233 PARK AVENUE SOUTH",
        "propertyAddress": "225 & 233 PARK AVENUE SOUTH",
        "propertyCity": "New York",
        "propertyState": "NY",
        "propertyZip": 10003,
        "propertyCounty": "New York",
        "propertyTypeCode": "OF",
        "netRentableSquareFeetNumber": 675756,
        "netRentableSquareFeetSecuritizationNumber": 675756,
        "yearBuiltNumber": 1909,
        "yearLastRenovated": 2017,
        "valuationSecuritizationAmount": 750000000,
        "valuationSourceSecuritizationCode": "MAI",
        "valuationSecuritizationDate": "04-01-2017",
        "physicalOccupancySecuritizationPercentage": 0.98,
        "mostRecentPhysicalOccupancyPercentage": 0.98,
        "propertyStatusCode": 6,
        "defeasanceOptionStartDate": "09-06-2019",
        "DefeasedStatusCode": "N",
        "largestTenant": "Facebook",
        "squareFeetLargestTenantNumber": 266460,
        "leaseExpirationLargestTenantDate": "10-31-2027",
        "secondLargestTenant": "BuzzFeed  Inc.",
        "squareFeetSecondLargestTenantNumber": 194123,
        "leaseExpirationSecondLargestTenantDate": "05-31-2026",
        "thirdLargestTenant": "STV Incorportated",
        "squareFeetThirdLargestTenantNumber": 133200,
        "leaseExpirationThirdLargestTenantDate": "05-31-2024",
        "financialsSecuritizationDate": "03-31-2017",
        "mostRecentFinancialsStartDate": "01-01-2018",
        "mostRecentFinancialsEndDate": "09-30-2018",
        "revenueSecuritizationAmount": 48106941.84,
        "mostRecentRevenueAmount": 30296697,
        "operatingExpensesSecuritizationAmount": 18601103.14,
        "operatingExpensesAmount": 16697070,
        "netOperatingIncomeSecuritizationAmount": 29505838.7,
        "mostRecentNetOperatingIncomeAmount": 13599627,
        "netCashFlowFlowSecuritizationAmount": 28439582.54,
        "mostRecentNetCashFlowAmount": 12799935,
        "netOperatingIncomeNetCashFlowSecuritizationCode": "UW",
        "netOperatingIncomeNetCashFlowCode": "CREFC",
        "mostRecentDebtServiceAmount": 6530934,
        "debtServiceCoverageNetOperatingIncomeSecuritizationPercentage": 3.39,
        "mostRecentDebtServiceCoverageNetOperatingIncomePercentage": 2.0823,
        "debtServiceCoverageNetCashFlowSecuritizationPercentage": 3.27,
        "mostRecentDebtServiceCoverageNetCashFlowpercentage": 1.9598,
        "debtServiceCoverageSecuritizationCode": "C",
        "mostRecentDebtServiceCoverageCode": "F"
      },
      "reportPeriodBeginningScheduleLoanBalanceAmount": 70000000,
      "totalScheduledPrincipalInterestDueAmount": 198798.44,
      "reportPeriodInterestRatePercentage": 0.036514,
      "servicerTrusteeFeeRatePercentage": 0.0001285,
      "scheduledInterestAmount": 198798.44,
      "reportPeriodEndActualBalanceAmount": 70000000,
      "reportPeriodEndScheduledLoanBalanceAmount": 70000000,
      "paidThroughDate": "03-06-2019",
      "servicingAdvanceMethodCode": 1,
      "primaryServicerName": "Wells Fargo Bank, NA"
},
```
### autoLoan

This shows all autoLoan documents.

JSON Attributes:

Attribute | Description 
--------- | -----------
deals | 1.
id | 2.
cik |  3.
assetType | 4.
names | 5.

```shell
curl --location --request GET "https:edgar.halider.io/autoLoan" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/autoLoan',headers=ploads)
```

> A sample response.

```JSON
{
      "id": "291",
      "cik": 1131131,
      "assetType": "autoLoan",
      "names": [
        "TOYOTA AUTO FINANCE RECEIVABLES LLC"
      ]
}
```

### autoLease

This shows all autoLease documents, very similar to autoLoan.

JSON attributes:

Attribute | Description 
--------- | -----------
deals | 1.
id | 2.
cik |  3.
assetType | 4.
names | 5.

```shell
curl --location --request GET "https:edgar.halider.io/autoLease" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/autoLease',headers=ploads)
```

> A sample response.

```JSON
{
      "id": "17",
      "cik": 1771786,
      "assetType": "autoLease",
      "names": [
        "GM Financial Automobile Leasing Trust 2019-2"
      ]
}
```

# CMBS

### Start

```shell
curl --location --request GET "https:edgar.halider.io/cmbs" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/cmbs',headers=ploads)
```
This returns all CMBS documents.

JSON Attributes:

Attributes | Description
--------- | -----------
deals | 1.
name |  2.


>A sample response.

```JSON
{
      "name": "BMARK 2018-B2"
}
```


### Options

Similar to the other CMBS above, there are several options within CMBS.

Parameters | Description and Placement
--------- | -----------
dealName | Name of a specific deal, 1.
file |  2.
fileType | 3.

JSON Attributes:

Attributes | Description
--------- | -----------
dealname | 1.
cik |  2.
companyName | 3.
filing | 4.
filenameFull | 5.
dateFiled | 6.
dateFiledString | 7.
createdAt | 8.
createdBy | 9.

```shell
curl --location --request GET "https:edgar.halider.io/cmbs/dealName" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/cmbs/dealName',headers=ploads)
```
>Example Response.
 
```JSON
{
  "dealName": "BMARK 2018-B2",
  "cik": 1728339,
  "companyName": "BENCHMARK 2018-B2 Mortgage Trust",
  "filing": "0001539497-18-000154",
  "filenameFull": "edgar/data/1728339/0001539497-18-000154.txt",
  "dateFiled": "2018-02-06",
  "dateFiledString": "20180206",
  "createdAt": "2018-11-24T03:03:53.706Z",
  "createdBy": "edgarmaster"
}
```

# Filing

### Start

```shell
curl --location --request GET "https:edgar.halider.io/filing/filingDate/filingNumber" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/filing/filingDate/filingNumber',headers=ploads)
```
> Example response.

```JSON
{
  "filingDate": "20200630",
  "filingNumber": "0001539497-20-000910",
  "filingFile": "0001539497-20-000910.nc",
  "documents": [ 
    {
      "documentType": "ABS-15G",
      "documentName": "n2235_x1-abs15g.htm",
      "documentDescription": "FROM ABS-15G"
    },
    {
      "documentType": "EX-99.1",
      "documentName": "exh99-1.htm",
      "documentDescription": "REPORT OF INDEPENDENT ACCOUNTANTS ON APPLYING AGREED-UPON PROCEDURES, DATED JUNE 30, 2020"
    },
    {
      "documentType": "GRAPHIC",
      "documentName": "image_001.jpg",
      "documentDescription": "GRAPHIC"
    },
    {
      "documentType": "GRAPHIC",
      "documentName": "image_002.jpg",
      "documentDescription": "GRAPHIC"
    }
  ]
}
```

Unlike ABS and CMBS, Filing starts and requires two parameters to use it. It also has a third parameter that can be used optionally.

Parameter | Description and Placement
--------- | -----------
filingDate | 1.
filingNumber |  2.
documentType | 3.

JSON Attributes: 

Attributes | Description
--------- | -----------
filingDate | 1.
filingNumber |  2.
filingFile | 3.
documents | 4.
documentName | 5.
documentDescription | 6.

### documentType

<aside class="warning">
The results from documentType are not compatible with JSON.
</aside>

```shell
curl --location --request GET "https:edgar.halider.io/filing/filingDate/filingNumber/documentType" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/filing/filingDate/filingNumber/documentType',headers=ploads)
```

> Start of an HTML Example.

```HTML
<HTML>
<HEAD>
<TITLE></TITLE>
</HEAD>
<BODY>


<P STYLE="font: 10pt Times New Roman, Times, Serif; margin: 0"><B>&nbsp;</B></P>

<P STYLE="font: 10pt Times New Roman, Times, Serif; margin: 0; text-align: center"><B>UNITED STATES</B></P>

<P STYLE="font: 10pt Times New Roman, Times, Serif; margin: 0; text-align: center"><B>SECURITIES AND EXCHANGE COMMISSION</B></P>
```
>etc.

Here is an example of the extra option for Filings.
