---
title: Edgar API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript
  - r
  - julia
  

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

We have language bindings in Shell, Python, R, Julia, and Javascript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

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

```javascript
fetch('url', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("url", add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "url", ["x-api-key" => "yourkey"])
```

> Make sure to replace `yourkey` with your API key.

<aside class = "warning">
The R examples use simple raw JSON as output, so they will not look the same as the other examples. R is also not able to use certain calls that are not JSON compatible.
    
The Julia examples actually output a bunch of server status information, then the response in text. Fully compatible, but will look very different. <code> pkg </code> is also a mode that is activated with "]" for installing packages, rather then typing it in.
</aside>        

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

>A basic example in Shell (Make sure you are on the Shell tab to see it):

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

For authorization errors, Edgar returns the HTTP <code>401 Unauthorized</code> status code. See OAuth 2.0 authorization protocol.

Access token-related issues often cause authorization errors.

Ensure that the access token is valid and present and not expired.

# ABS

### Start

An asset-backed security (ABS) is a security whose income payments and hence value are derived from and collateralized (or "backed") by a specified pool of underlying assets.

The pool of assets is typically a group of small and illiquid assets which are unable to be sold individually. Pooling the assets into financial instruments allows them to be sold to general investors, a process called securitization, and allows the risk of investing in the underlying assets to be diversified because each security will represent a fraction of the total value of the diverse pool of underlying assets. The pools of underlying assets can include common payments from credit cards, auto loans, and mortgage loans, to esoteric cash flows from aircraft leases, royalty payments, or movie revenues.

This endpoint returns the asset classes available from data filed in Edgar SEC.gov as part of the Reg AB Schedule AL asset-level disclosures. More information here: https://www.sec.gov/oit/announcement/regabii-asset-level-requirements-compliance.html

```shell
curl --location --request GET "https://edgar.halider.io/abs" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/abs', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/abs",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/abs", ["x-api-key" => "yourkey"])
```


This returns a list of options, choose one and put it into the URL to proceed. 

>The list of options, Null isn't a real option.

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

```javascript
fetch('https://edgar.halider.io/abs/cmbs', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/abs/cmbs",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/abs/cmbs", ["x-api-key" => "yourkey"])
```


This returns the document with all of the CMBS on it.

> An Example.

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
Id | Edgar form submission identifier.
Date | Edgar CIK code. The Central Index Key (CIK) is used on the SEC's computer systems to identify corporations and individual people who have filed disclosure with the SEC.
assetType | CMBS.
Extracted | One or more names for the CMBS deal.

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs/dealId" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/dealId',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/abs/cmbs/dealId', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/abs/cmbs/dealId",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/abs/cmbs/dealId", ["x-api-key" => "yourkey"])
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
curl --location --request GET "https:edgar.halider.io/abs/cmbs/dealId/file" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/dealId/file',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/abs/cmbs/dealId/file', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/abs/cmbs/dealId/file",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/abs/cmbs/dealId/file", ["x-api-key" => "yourkey"])
```

JSON Attributes:

Attribute | Description 
--------- | -----------
assetId | A
dealId | Y 
dealCik | L
dealNames | M
assetType | A
data | O
assetTypeNumber | Used to Indentify the source of the Asset number.  For all securitizations this should be "Prospectus Loan ID"
assetNumber | The identification number(s) assigned to each asset in the annex of the prospectus supplement.  For a partial defeasance where the loan is bifurcated, the Prospectus Loan ID for the original/non-defeased loan is appended with an "A", and the new/defeased loan is appended with a "B".  If there is no Prospectus Loan ID assigned, for example in a single asset deal, the default should be 1. In Schedule AL, this field is named Asset Number.
reportingPeriodBeginningDate | Reporting period begin date for the first reporting cycle is equal to Closing Date, otherwise it should be populated as the prior month Determination Date plus 1 day.  SS reporting period should follow same as MS.
reportingPeriodEndDate | Reporting period end date should be the determination date.  SS reporting period should follow same as MS.
originatorName | Name of entity ultimately responsible for the representations and warranties of the loan contributed. For Schedule AL reporting, this entity shall be the originator.
originationDate | Date the loan was originated as presented in the Annex.
originalLoanAmount | The amount of the loan at origination.  For split loans/notes, this amount is the Original Note Amount for the split loan/note piece.
originalTermLoanNumber | The number of months from the loan origination date until the Maturity Date of the loan.
maturityDate | Date final scheduled payment is due per the loan documents. Not the same as anticipated repayment date related to hyper-amortization loans.  If the loan has been defeased and the loan agreement provided for, or the servicer has consented to, prepayment prior to maturity in connection with a defeasance, this represents the date the Trust can expect full repayment.  The borrower may have the right to pre-pay the defeased loan prior to the final scheduled payment date in accordance with the loan documents.
originalInterestRatePercentage | The rate at which the note earned interest, as of the origination date.
interestRateSecuritizationPercentage | The annual gross rate used to calculate interest for the loan at the closing date of the transaction.
interestAccrualMethodCode | Code indicating the 'number of days' convention used to calculate interest. See Interest Accrual Method Legend.
originalInterestRateTypeCode | Code indicating the type of interest payable by a borrower on the securitized portion of a loan.  See Interest Rate Type Legend.
originalInterestOnlyTermNumber | Number of months the loan is interest only, calculated from the origination date to the end of the IO term.
firstLoanPaymentDueDate | Date on which the borrower must pay the first full interest and/or principal payment due on the mortgage in accordance with the loan documents.
underwritingIndicator |  Y or N field, to be provided by issuer, defined as whether the loan or asset met the criteria for the first level of solicitation, credit-granting, or underwriting criteria used to originate the pool asset.
lienPositionSecuritizationCode | A lien is a claim placed on property to make sure debt is repaid.  Lien Position at Contribution is the position as of the closing date of the transaction.  The lien position determines the repayment of debt upon asset resolution.  First lien paid first, second lien paid second, etc.  See Lien Position at Contribution Legend.
loanStructureCode | Code indicating type of loan structure including the seniority of participated mortgage loan components. Code relates to loan within the securitization.   See Loan Structure Legend.
paymentTypeCode | Code indicating the type or method of payment for a loan.  See Payment Type Legend.
periodicPrincipalAndInterestPaymentSecuritizationAmount | The total amount of principal and interest due on the loan in effect as of the closing date of the transaction.  Amount should equal the sum of the Scheduled Principal Amount (L24) and Scheduled Interest Amount (L23) as of the initial determination date.
scheduledPrincipalBalanceSecuritizationAmount | The scheduled principal balance of the mortgage loan at the closing date for the transaction, as disclosed in the final prospectus.  For split loans/notes, this amount is the scheduled beginning balance for the split loan/note piece at the closing date for the transaction.
paymentFrequencyCode | Code representing the frequency mortgage loan payments are required to be made. See Payment Frequency Legend.
NumberPropertiesSecuritization | The number of  properties which serve as mortgage collateral as of the closing date of the transaction.
NumberProperties | The current number of properties which serve as mortgage collateral for the loan.  This number should not include defeasance collateral, therefore if a loan is fully defeased, field should be populated with zero.
interestOnlyIndicator | Flag indicating if, at contribution, this is a loan for which scheduled interest only is payable, whether for a temporary basis or until the full loan balance is due.  For field A28, populate True of False.
balloonIndicator | Indicator = Y if the loan documents require a lump-sum payment of principal at maturity.  If not required = N.  If data not yet available = empty.  Field A29 should populate either True or False.
prepaymentLockOutEndDate | The effective date after which the lender allows prepayment of a loan.
property | 
propertyName | The name of the property which serves as mortgage collateral.  If the property has been defeased, populate with "Defeased".  For loan level reporting, if multiple properties, print "Various".  For substituted properties, populate with the new property name.
propertyAddress | The address of the property which serves as mortgage collateral.  If the property has been defeased, then leave field empty.  For loan level reporting, if multiple properties, then print "Various". For substituted properties, populate with the new property information.
propertyCity | The city name where the property or properties which serve as mortgage collateral are located.  If the property has been defeased, then leave field empty.  For loan level reporting, if multiple properties have the same city then print the city, otherwise print "Various".  If missing information, print "Incomplete". For substituted properties, populate with the new property information.
propertyState | The 2 character abbreviated code representing the state in which the property or properties which serve as mortgage collateral are located.  If the property has been defeased, then leave field empty.  For loan level reporting, if multiple properties have the same state then print the state's 2 character abbreviated code, otherwise print "XX" for various.  If missing information, print "ZZ".   For substituted properties, populate with the new property state code.
propertyZip | The zip (or postal) code for the property or properties which serve as mortgage collateral.  If the property has been defeased, then leave field empty.  For loan level reporting, if multiple properties have the same zip code then print the zip code, otherwise print "Various". If missing information, print "Incomplete".  For substituted properties, populate with the new property zip code.
propertyCounty | The county in which the property or properties which serve as mortgage collateral are located.  If the property has been defeased, then leave field empty.  For loan level reporting, if multiple properties have the same county then print the county, otherwise print "Various".  If missing information, print "Incomplete".  For substituted properties, populate with the new property information.
propertyTypeCode | Code assigned to a property from the Property Type Legend based on how the property is used.  If the property has been defeased, populated with "SE".  For loan level reporting, if multiple property types, print "XX".  If missing information, print "ZZ".  For Schedule AL purposes code "SF" should be reported as "MF".  For substituted properties, populate with the new property type.
netRentableSquareFeetNumber | The current net rentable square feet area of a property as of the determination date.  This field should be utilized for Office, Retail, Industrial, Warehouse, and Mixed Use properties.  If there are multiple properties, and all the same Property Type, sum the values.  If not all the same Property Type or if any are missing, then leave field empty.
netRentableSquareFeetSecuritizationNumber | The net rentable square feet area of a property as determined at the time the property is contributed to the mortgage pool as collateral.  This field should be utilized for Office, Retail, Industrial, Warehouse, and Mixed Use properties. For multiple properties, if all the same Property Type, sum the values. If missing any, leave empty.
yearBuiltNumber | The year the property was built.  For multiple properties, if all the same print the year, else leave empty.
yearLastRenovated | Year that last major renovation/new construction was completed on the property.
valuationSecuritizationAmount | The valuation amount of the property as of the Valuation Date at Contribution.  For the Loan Setup File, if multiple properties, sum the values.  If missing any, leave empty.
valuationSourceSecuritizationCode | Code used to identify the source of property valuation at securitization (as reported in Value Amount at Contribution S67, P49).  See Most Recent Valuation Source Legend.  If multiple properties and all the same then print the type.  If missing any, then leave empty. 
valuationSecuritizationDate | The date the Valuation Amount at Contribution was determined.  For the Loan Setup File, if multiple properties and missing any or not the same date, leave empty.
physicalOccupancySecuritizationPercentage | The percentage of rentable space occupied by tenants as of the closing date of the transaction. Should be derived from a rent roll or other document indicating occupancy. If multiple properties, populate with the weighted average based on square feet or units.  If missing any, leave empty at the loan level.
mostRecentPhysicalOccupancyPercentage | The most recent available percentage of rentable space occupied. Should be derived from a rent roll or other document indicating occupancy consistent with most recent documentation. If property is vacant, input zero. If multiple properties, populate with the weighted average based on square feet or units.  If missing any, leave empty at the loan level.
propertyStatusCode | Code showing status of property.  See Property Status Legend.
defeasanceOptionStartDate | Date when defeasance option becomes available.  If mortgagor opts to repay principal amount, mortgagee may elect to have mortgagor replace loan cash flow by purchasing securities which are equivalent to the scheduled cash flow of the loan.  This serves as a disincentive to early prepayment of the loan by the mortgagor.  Defeasance is allowed only if in accordance with the loan documents.
DefeasedStatusCode | A code indicating if a loan has or is able to be defeased.  See Defeasance Status Legend.  When a loan becomes “Full Defeasance”, at a minimum populate Property Status (P18) with 3, populate Property Type (P13) with SE, populate Property Name with "Defeased", and preceding year, second preceding year and most recent operating performance related data fields, lease and tenant related data fields and property condition related data fields should be left empty.  For Schedule AL map code P to code IP.
largestTenant | At a property level the name of the tenant that leases the largest square feet of the property based on the most recent annual lease rollover review.  If tenant is not occupying the space but is still paying rent, the servicer may print "Dark" after tenant name.  If tenant has sub-leased space, may print "Sub-leased/name" after tenant name.  For Office, Retail, Industrial, Other or Mixed Use property types as applicable.
squareFeetLargestTenantNumber | Total square feet leased by the largest tenant in field P37.  Based on the most recent annual lease roll over review.
leaseExpirationLargestTenantDate | Lease term expiration.  Companion field for P37 & P38.
secondLargestTenant | At a property level the name of the tenant that leases the second largest square feet of the property based on the most recent annual lease rollover review.  If tenant is not occupying the space but is still paying rent, the servicer may print "Dark" after tenant name.  If tenant has sub-leased space, may print "Sub-leased/name" after tenant name.  For Office, Retail, Industrial, Other or Mixed Use property types as applicable.
squareFeetSecondLargestTenantNumber | Total square feet leased by the 2nd largest tenant in P39.  Based on the most recent annual lease roll over review.
leaseExpirationSecondLargestTenantDate | Lease term expiration.  Companion field for P39 & P40.
thirdLargestTenant | At a property level the name of the tenant that leases the third largest square feet of the property based on the most recent annual lease rollover review.  If tenant is not occupying the space but is still paying rent, the servicer may print "Dark" after tenant name.  If tenant has sub-leased space, may print "Sub-leased/name" after tenant name.  For Office, Retail, Industrial, Other or Mixed Use property types as applicable.
squareFeetThirdLargestTenantNumber | Total square feet leased by the 3rd largest tenant in P41.  Based on the most recent annual lease roll over review.
leaseExpirationThirdLargestTenantDate | Lease term expiration.  Companion field for P41 & P42.
financialsSecuritizationDate | The date of the underwritten operating statements for the property. If available, use most recent ending financial date provided, else should equal transaction closing date. If multiple properties and all the same, print the date.  If missing any, leave empty.
mostRecentFinancialsStartDate | The first day of the period for the most recent, hard copy operating statement (e.g. year to date or trailing 12 months) after the preceding fiscal year end statement.  (Note - the beginning and end date of the operating statement from the borrower used to annualize should be reported.)  If multiple properties and all  the same start and end date, print start date.  If missing any, leave empty.
mostRecentFinancialsEndDate | The last day of the period for the most recent, hard copy operating statement (e.g. year to date or trailing 12 months) after the preceding fiscal year end statement. (Note - the beginning and end date of the operating statement from the borrower used to annualize should be reported.) If multiple properties and all the same start and end date, print the end date.  If missing any, leave empty.
revenueSecuritizationAmount | The sum of all income produced by the property or properties securing a loan, based on the final prospectus or as provided by depositor at closing of transaction.  It is often derived by calculating the maximum rental income achievable at market rates, net of adjustments that reflects vacancies, credit loss and other such deductions. The Effective Gross income also includes other income, such as parking and laundry fee, typically calculated based on historical collection adjusted to reflect actual occupancy or use.  If missing data or if all received/consolidated, use the DSCR Indicator Legend rule.
mostRecentRevenueAmount | Total revenues for the most recent operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end statement.   If multiple properties exist and the related data is comparable (same financial indicators and same financial start and end dates), total the revenue of the underlying properties.  If multiple properties exist and comparable data is not available for all properties or if received/consolidated, populate using the DSCR Indicator Legend rule.
operatingExpensesSecuritizationAmount | The sum of all expenses incurred in performing normal business operation for the property or properties securing a loan, based on the final prospectus or as provided by the issuer or depositor at closing date of the transaction. Such expenses typically include employee salaries, utilities, maintenance and repairs, marketing, insurance and real estate taxes, but exclude capital expenditures, tenant improvements, and leasing commissions. It may also include expenses for the previous 12 months as well as adjustments to reflect inflation, occupancy changes or other major changes. If missing date or if all received/consolidated, use DSCR Indicator Legend Rule.
operatingExpensesAmount | Total operating expenses for the most recent operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end statement. Included are real estate taxes, insurance, management fees, utilities and repairs and maintenance. Excluded are capital expenditures, tenant improvements, and leasing commissions.  If multiple properties exist and the related data is comparable (same financial indicators and same financial start and end dates), total the operating expenses of the underlying properties.  If multiple properties exist and comparable data is not available for all properties or if received/consolidated, populate using the DSCR Indicator Legend rule.
netOperatingIncomeSecuritizationAmount | Net Operating Income (NOI) is the total underwritten revenues less total underwritten operating expenses prior to application of mortgage payments and capital items for all properties per the final prospectus or as provided by the issuer or depositor at the closing date of the transaction.  If multiple properties, sum the values.  If missing data or if all received/consolidated, use the DSCR Indicator Legend rule.
mostRecentNetOperatingIncomeAmount | Total revenues less total operating expenses before capital items and debt service per the most recent operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end  statement.  If multiple properties exist and the related data is comparable (same financial indicators and same financial start and end dates), total the NOI of the underlying properties.  If multiple properties exist and comparable data is not available for all properties or if received/consolidated, populate using the DSCR Indicator Legend rule.
netCashFlowFlowSecuritizationAmount | Net Cash Flow (NCF) is Effective Gross Income (EGI) less Total Operating Expenses (TOE) and Capital Expenditures, and prior to the application of Debt Service payments, per the final prospectus or as provided by the issuer or depositor as of the closing date of the transaction.   If missing data or if all received/consolidated, populate using DSCR Indicator Legend.
mostRecentNetCashFlowAmount | Total revenues less total operating expenses and capital items but before debt service per the most recent operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end  statement.  If multiple properties exist and the related data is comparable (same financial indicators and same financial start and end dates), total the NCF of the underlying properties.  If multiple properties exist and comparable data is not available for all properties or if received/consolidated, populate using the DSCR Indicator Legend rule.
netOperatingIncomeNetCashFlowSecuritizationCode | <b>Guess</b> Code indicating the method used to calculate net operating income or net cash flow at contribution.  See NOI/NCF Indicator Legend rule.  If multiple properties are all the same, print the value. If missing any or the values are not the same, leave empty.
netOperatingIncomeNetCashFlowCode | Code indicating the method used to calculate net operating income or net cash flow.  See NOI/NCF Indicator Legend rule.  If multiple properties and all the same, print the value. If missing any or the values are not the same, leave empty.
mostRecentDebtServiceAmount | Total scheduled or actual payments that cover the same number of months as the most recent financial operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end statement.  Payments include scheduled or actual principal and or interest as required by the loan documents. Calculate using the current allocated percentage (P20) to get the allocated amount for each property.  If multiple properties covering the same period (same financial statement as of start and end dates), sum the value.  If missing any or all received/consolidated then populate using the DSCR Indicator Legend rule.
debtServiceCoverageNetOperatingIncomeSecuritizationPercentage | A ratio of underwritten net operating income (NOI) to debt service as shown in the final prospectus or as provided by the issuer or depositor at the closing date of the transaction.  If multiple properties, populate using the DSCR Indicator Legend rule.
mostRecentDebtServiceCoverageNetOperatingIncomePercentage | A ratio of net operating income (NOI) to debt service for the most recent operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end statement. If multiple properties exist and the related data is comparable (same financial indicators and same financial start and end dates), calculate the DSCR of the underlying properties.  If multiple properties exist and comparable data is not available for all properties or if received/consolidated, populate using the DSCR Indicator Legend rule.
debtServiceCoverageNetCashFlowSecuritizationPercentage | The Debt Service Coverage Ratio (DSCR) is calculated by dividing the Net Cash Flow (NCF) by the required Debt Service payments.  A DSCR of 1.0x implies that the property generates just enough cash flow to service the debt.  A higher DSCR means the property is generating more cash than needed to cover the debt service payments and therefore represents less risk of payment default.  The higher the DSCR, the more Term Risk is mitigated.  If multiple properties, populate using DSCR Indicator Legend.
mostRecentDebtServiceCoverageNetCashFlowpercentage | A ratio of net cash flow (NCF) to debt service for the most recent financial operating statement reported by the servicer (e.g. year to date, year to date annualized, or trailing 12 months, but all normalized) after the preceding fiscal year end statement.  If multiple properties exist and the related data is comparable (same financial indicators and same financial start and end dates), calculate the DSCR of the underlying properties.  If multiple properties exist and comparable data is not available for all properties or if received/consolidated, populate using the DSCR Indicator Legend rule.
debtServiceCoverageSecuritizationCode | Code used to explain how DSCR was calculated when there are multiple properties.  Specific codes apply.  See DSCR Indicator Legend.
mostRecentDebtServiceCoverageCode | Code describing how DSCR is calculated for the most recent financial operating statement, as reported by the servicer, after the preceding fiscal year end statement.  See DSCR Indicator Legend.
reportPeriodBeginningScheduleLoanBalanceAmount | The scheduled or stated principal balance for a loan (defined in the servicing agreement) as of the beginning of the reporting period, which is usually the preceding determination date. This balance should be equal to the Current Ending Scheduled Balance in the previous reporting period.  For full and partial defeasances, the balance should reflect the appropriate allocation of the balance of the non-defeased and defeased loans based on the provisions of the loan documents.
totalScheduledPrincipalInterestDueAmount | The total amount of principal and interest due on the loan in the month corresponding to the current distribution date and should equal the sum of fields L23 and L24.
reportPeriodInterestRatePercentage | Annualized gross rate used to calculate the current period Scheduled Interest Amount. For split loans/notes, this is the gross rate used to calculate the Scheduled Interest Amount for the split loan/note included in the related trust.
servicerTrusteeFeeRatePercentage | Sum of annual fee rates payable to the servicer(s)) and trustee (should not include any fees represented in fields L13 through L17 of the Loan Periodic Update File or fields S47 through S51 of the Loan Setup File in order to avoid double counting). 
scheduledInterestAmount | The amount of gross interest scheduled to be paid to the trust for the current distribution period based on the trust's beginning scheduled principal balance and a full month's interest accrual amount.  This amount may not be the same as the amount of gross interest scheduled to be paid by the borrower for the related payment date.  If loan has been deemed non-recoverable, then populate with zero.
reportPeriodEndActualBalanceAmount | Outstanding actual balance of the loan as of the determination date. This figure represents the legal remaining outstanding principal balance related to the borrower’s mortgage note.  For partial defeasances, the balance should reflect the appropriate allocation of the balance prior to the defeasance between the non-defeased and defeased  loans based on the provisions of the loan documents.
reportPeriodEndScheduledLoanBalanceAmount | The scheduled or stated principal balance for a loan (defined in the servicing agreement) as of the end of the reporting period, which is usually the current determination date.  This balance is usually determined by considering scheduled and unscheduled principal payments received during the collection period relating to the Distribution Date. A realized loss will also have an impact on this balance during the period it is reported.  For split note/loans, this should include the balance in the related trust.  For full and partial defeasances, the balance should reflect the appropriate allocation of the balance prior to the defeasance between the non-defeased and defeased loans based on the provisions of the loan documents.
paidThroughDate | Date the loan's scheduled principal and interest is paid through as of the determination date. One frequency less than the due date for the loan's next scheduled payment.  For split loans/notes, this is the date the scheduled principal and interest for the split loan/note piece has been paid through.
servicingAdvanceMethodCode | Indicate the code that describes the manner in which principal and/or interest are advanced by the servicer.  See Servicer Advance Methodology Legend.  Amounts are assumed Net, otherwise use 99.
primaryServicerName | The entity responsible for collection of the mortgage payments and accounting for the securitization as well as for remitting all collections and reporting all data to the Trustee/Certificate Administrator so that it can be forwarded to the certificateholders.  This entity also protects the interests of CMBS certificateholders by actively administering the mortgage loans and collateral that are the security for the bondholders’ investment. See Master Servicer Legend. For Schedule AL, the master servicer is used to populate the Primary Servicer field.



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
curl --location --request GET "https:edgar.halider.io/abs/autoLoan" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/autoLoan',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/abs/autoLoan', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/abs/autoLoan",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/abs/autoLoan", ["x-api-key" => "yourkey"])
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
curl --location --request GET "https:edgar.halider.io/abs/autoLease" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/autoLease',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/abs/autoLease', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/abs/autoLease",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/abs/autoLease", ["x-api-key" => "yourkey"])
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

```javascript
fetch('https://edgar.halider.io/cmbs', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/cmbs",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/cmbs", ["x-api-key" => "yourkey"])
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

```javascript
fetch('https://edgar.halider.io/cmbs/dealName', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/cmbs/dealName",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/cmbs/dealName", ["x-api-key" => "yourkey"])
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

```javascript
fetch('https://edgar.halider.io/filing/filingDate/filingNumber', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/filing/filingDate/filingNumber",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/filing/filingDate/filingNumber", ["x-api-key" => "yourkey"])
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

Unlike ABS and CMBS, Filing starts and <b>requires two parameters</b> to use it. It also has a third parameter that can be used optionally.

Parameter | Description 
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
print(r.text)
```

```javascript
fetch('https://edgar.halider.io/filing/filingDate/filingNumber/documentType', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.txt())
  .then(data => console.log(data));
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/filing/filingDate/filingNumber/documentType", ["x-api-key" => "yourkey"])
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



# Corporate

### Start

The start of corporate is ticker, which gives a list of companies stock tickers and their ciks.

```shell
curl --location --request GET "https:edgar.halider.io/ticker" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/ticker',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/ticker', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/ticker",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/ticker", ["x-api-key" => "yourkey"])
```

>Example Response.

```JSON

  {
    "ticker": "a",
    "cik": 1090872
  }
```

Parameter | Description 
--------- | -----------
ticker | 1.
filing |  2.

JSON Attributes:

Attributes | Description 
--------- | -----------
ticker | 1.
cik |  2.


### Ticker 

Select a specific company by their stock ticker.


```shell
curl --location --request GET "https:edgar.halider.io/ticker/stockTicker" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/ticker/stockTicker',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/ticker/stockTicker', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/ticker/stockTicker",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/ticker/stockTicker", ["x-api-key" => "yourkey"])
```

> Example Response


```JSON
{
      "dateFiled": "2020-08-05",
      "type": "4",
      "filing": "0001127602-20-022974"
}
```


JSON Attributes:

Attributes | Description 
--------- | -----------
dateFiled | 3.
type | 4.
filing | 5.
companyNames | 6.
filingCounts | 7.
3 | 8.
4 | 9.
5 | 10.
FWP | 11.
DEFA14A | 12.
SC TO-I | 13.
S-3ASR | 14.
424B3 | 15.
11-K | 16.
CORRESP | 17.
SC TO-I/A | 18.
4/A | 19.
POS AM | 20.
8-K | 21.
DFAN14A | 22.
10-Q | 23.
10-Q/A | 24.
SC 13G/A | 25.
DEFR14A | 26.
8-K/A | 27.
S-8 POS | 28.
S-8 | 29.
SD | 30.
424B5 | 31.
SC 13G | 32.
ARS | 33.
NO ACT | 34.
3/A | 35.
PRE 14A | 36.
CT ORDER | 37.
10-K | 38.
DEF 14A | 39.
SC TO-C | 40.
SC 13D | 41.
UPLOAD | 42.


### Filing  

Select a specific filing from the previously specified company. 


```shell
curl --location --request GET "https:edgar.halider.io/ticker/stockTicker/filing" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/ticker/stockTicker/filing',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/ticker/stockTicker/filing', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```r
>library(httr)
>library(jsonlite)
>res=GET("https://edgar.halider.io/ticker/stockTicker/filing",add_headers('x-api-key':'yourkey'))
>rawToChar(res$content)
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/ticker/stockTicker/filing", ["x-api-key" => "yourkey"])
```

>Example Response

```JSON
{
  "filingDate": "20200805",
  "filingNumber": "0001127602-20-022974",
  "filingFile": "0001127602-20-022974.nc",
  "documents": [
    {
      "documentType": "4",
      "documentName": "form4.xml",
      "documentDescription": "PRIMARY DOCUMENT",
      "documentText": "\n<XML>\n<?xml version=\"1.0\"?>\n<ownershipDocument>\n\n    <schemaVersion>X0306</schemaVersion>\n\n    <documentType>4</documentType>\n\n    <periodOfReport>2020-08-03</periodOfReport>\n\n.......
     etc.
    }
  ]
}
```


JSON Attributes:

Attributes | Description 
--------- | -----------
filingDate | 43.
filingNumber | 44.
filingFile | 45.
documents | 46.
documentType | 47.
documentName | 48.
documentDescription | 49.
documentText | 50.


### File  

Select a specific file from the previously specified filing. 

<aside class = warning>
 This is another option that isn't compatible with JSON.
</aside>

```shell
curl --location --request GET "https:edgar.halider.io/ticker/stockTicker/filing/file" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/ticker/stockTicker/filing/file',headers=ploads)
```

```javascript
fetch('https://edgar.halider.io/ticker/stockTicker/filing/file', {
    headers: { 
      'X-Api-Key':'yourkey'}
})
  .then(response => response.json())
  .then(data => console.log(data));
```

```julia
pkg add JSON
pkg add HTTP
using JSON, HTTP
r = HTTP.request("GET", "https://edgar.halider.io/ticker/stockTicker/filing/file", ["x-api-key" => "yourkey"])
```

>Example XML Response

```XML
<XML>
<?xml version="1.0"?>
<ownershipDocument>

    <schemaVersion>X0306</schemaVersion>

    <documentType>4</documentType>

    <periodOfReport>2020-08-03</periodOfReport>

    <issuer>
        <issuerCik>0001090872</issuerCik>
        <issuerName>AGILENT TECHNOLOGIES, INC.</issuerName>
        <issuerTradingSymbol>A</issuerTradingSymbol>
    </issuer>
```
> etc.

# Sample Code

This is an example of plugging in variables into the URL to get further into what you specifically want. It is in Python so you will have to switch to that tab to see it.

```python
import json
import requests

Docs = input("Document Type? ")
Tick = input("Ticker? ")

Url = 'https://edgar.halider.io/ticker/' + (Tick)

ploads = {'X-Api-Key':'wtSxYJuwWUaDg6PbBy8JB9GRENTDW5yi255SNfmw'}
r =requests.get(Url,headers=ploads)
json_object = json.loads(r.text)
print(json.dumps(json_object, indent=2))


Url2 = 'https://edgar.halider.io/ticker/' + (Tick) + '/0001144370-03-000002/' + (Docs)

ploads = {'X-Api-Key':'wtSxYJuwWUaDg6PbBy8JB9GRENTDW5yi255SNfmw'}
r =requests.get(Url2,headers=ploads)
print(r.text)
```


> Which Outputs all of these:


```JSON
    {
      "dateFiled": "2013-06-03",
      "type": "S-8",
      "filing": "0001090872-13-000014"
    },
``` 


> And the beginning of an HTML document specificed by the second URL


```HTML
<HTML>
<HEAD>
<TITLE>
</TITLE>
</HEAD>
<BODY>
SEC Form 4
<TABLE width=1272 border=1>
<TR>
<TD width=254 valign=top>
    <div align=center><font size=4 face=Tahoma><b>FORM 4</b></font></div>
    <br><div><font size=1 face=Tahoma><b>[ ] Check this box if no longer<br>subject to Section 16.  Form 4 or Form<br>5 obligations may continue.<br><i>See</i> Instruction 1(b).</b></font></div>
<br><font size=1 face=Tahoma>(Print or Type Responses)</font>
</TD>
```


> etc.


