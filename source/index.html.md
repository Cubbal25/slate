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
import edgar

api = edgar.authorize('yourkey')
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

## Get All Documents

```python
import edgar

api = edgar.authorize('yourkey')
api.document.get()
```

```shell
curl --location --request GET "https://edgar.halider.io/filing/20191230/0001193125-19-326069/S-8?="
  -H "Authorization: yourkey"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all documents.

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

## Get a Specific Document

```python
import edgar

api = edgar.authorize('yourkey')
api.documents.get(2)
```

```shell
curl "http://example.com/api/edgar/2"
  -H "Authorization: yourkey"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific document.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Date_Filed| The date the document was filed.
Filing_Number| The identification of the specified document.
Document Type| The classification of the document.

# ABS

```shell
curl --location --request GET "https://edgar.halider.io/abs" --header "yourkey"
```
> This returns a list of options 

>{"types":[null,"autoLoan","cmbs","autoLease"]}

```shell
curl --location --request GET "https:edgar.halider.io/abs/cmbs" --header "yourkey"
```
> This returns the document with all of the CMBS on it.

>{"deals":[{"id":"1","cik":1710798,"assetType":"cmbs","names":["Wells Fargo Commercial Mortgage Trust 2017-C39"]},>>>>>{"id":"2","cik":1716602,"assetType":"cmbs","names":["CSAIL 2017-CX9 Commercial Mortgage Trust"]},{"id":"4","cik":1773339,"assetType":"cmbs","names":>["Morgan Stanley Capital I Trust 2019-H6"]},{"id":"14","cik":1547361,"assetType":"cmbs","names":["Morgan Stanley Capital I Inc."]},

>etc.

## Delete a Specific Document.

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

