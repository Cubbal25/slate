---
title: Edgar API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Edgar API! You can use our API to access Edgar API endpoints, which can get information on various Edgar Files in our Database.

We have language bindings in Shell and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

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

Edgar uses API keys to allow access to the API. You can register a new Edgar API key at our [developer portal](http://example.com/developers).

Edgar expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: yourkey`

<aside class="notice">
You must replace <code>yourkey</code> with your personal API key.
</aside>


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

<aside class="notice">
Every Parameter has a Placement number, this is where it is placed in the URL after CMBS. You can only use them in ascending order.
</aside>


Parameter | Description and Placement
--------- | -----------
DEAL | Either the ID, CIK, or Name of a specific deal, 1.
FILE |  A specific File within the deal, 2.
FileType | 3.

JSON Attributes:

Attribute | Description 
--------- | -----------
Id | 1.
Date |  2.
Type | 3.
Extracted | 4.

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs/DEAL" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/DEAL',headers=ploads)
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

>A sample response.

```JSON
{
      "name": "BMARK 2018-B2"
}
```


### Options

Similar to the other CMBS above, there are several options within CMBS.

Parameter | Description and Placement
--------- | -----------
DEALNAME | Name of a specific deal, 1.
File |  2.
FileType | 3.

```shell
curl --location --request GET "https:edgar.halider.io/cmbs/DEALNAME" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/cmbs/DEALNAME',headers=ploads)
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

