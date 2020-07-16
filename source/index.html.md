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

# Getting A Document


```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('url',headers=ploads)
```

```shell
curl --location --request GET "https://edgar.halider.io/filing/20191230/0001193125-19-326069/S-8?="
  -H "Authorization: yourkey"
```

> The above command returns JSON structured like this:

```json
Placeholder
```

### HTTP Request

`GET http://example.com/api/edgar`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
a | false | If set to true, the result will also include cats.
a | true | If set to false, the result will include kittens that have already been adopted.
a | true | Something

<aside class="success">
Remember — a happy document is an authenticated document!
</aside>


### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Date_Filed| The date the document was filed.
Filing_Number| The identification of the specified document.
Document Type| The classification of the document.

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

> This returns a list of options 

>{"types":[null,"autoLoan","cmbs","autoLease"]}

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

>{"deals":[{"id":"1","cik":1710798,"assetType":"cmbs","names":["Wells Fargo Commercial Mortgage Trust 2017-C39"]},>>>>>{"id":"2","cik":1716602,"assetType":"cmbs","names":["CSAIL 2017-CX9 Commercial Mortgage Trust"]},{"id":"4","cik":1773339,"assetType":"cmbs","names":>["Morgan Stanley Capital I Trust 2019-H6"]},{"id":"14","cik":1547361,"assetType":"cmbs","names":["Morgan Stanley Capital I Inc."]},

>etc.

### CMBS Options

<aside class="notice">
Every Parameter has a Placement number, this is where it is placed in the URL after CMBS. You can only use them in ascending order.
</aside>


Parameter | Description and Placement
--------- | -----------
DEAL | Either the ID, CIK, or Name of a specific deal, 1.
FILE |  A specific File within the deal, 2.
FileType | 3.

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs/DEAL" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/DEAL',headers=ploads)
```
> A sample response.
>{"deal":{"id":"1","cik":1710798,"assetType":"cmbs","names":["Wells Fargo Commercial Mortgage Trust 2017-C39"],"filings":{"id":"2110","date":"2019-07-31","type":"ABS-EE","extracted":true},{"id":"2224","date":"2019-03-28","type":"ABS-EE","extracted":true},{"id":"3489","date":"2019-04-30","type":"ABS-EE","extracted":true},

>etc.

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs/DEAL/FILE" --header "yourkey"
```

```python
import requests
ploads = {'X-Api-Key':'yourkey'}
r =requests.get('https://edgar.halider.io/abs/cmbs/DEAL/FILE',headers=ploads)
```
> A sample response.
> Placeholder

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

>"deals":{"name":"BMARK 2018-B2"},{"name":"MSC 2018-H3"},{"name":"CD 2018-CD7"},{"name":"UBSCM 2017-C2"},{"name":"CSAIL 2018-CX12"},{"name":"GSMS 2018-GS10"},{"name":"MSBAM 2016-C32"},{"name":"JPMCC 2016-JP4"},{"name":"JPMDB 2017-C5"},{"name":"CSMC 2016-NXSR"},{"name":"GSMS 2017-GS7"},{"name":"CGCMT 2017-C4"},{"name":"BANK 2018-BNK15"},

>etc.

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
 
>{"dealName":"BMARK 2018-B2","cik":1728339,"companyName":"BENCHMARK 2018-B2 Mortgage Trust","filing":"0001539497-18-000154","filenameFull":"edgar/data/1728339/0001539497-18-000154.txt","dateFiled":"2018-02-06",

>etc.

