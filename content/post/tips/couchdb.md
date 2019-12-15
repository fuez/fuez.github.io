---
title: couchdb
date: 2017-07-06T12:05:25+08:00
lastmod: 2017-11-13T15:42:49+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

### Tips
## Another REST example to get data, and make a list from result:
`curl -L -H "Content-Type:application/json" -H "Accept:application/json" -X POST http://ja03:5984/rong/_find -d @t.json | awk -F ':' '{ print $3}' | cut -d '"' -f 2`

## REST query on couchdb
  ```
  curl -L -H "Content-Type:application/json" -H "Accept:application/json"  -X POST http://192.168.20.143:5984/rong/_find -d @s.json
  ```
  s.json is documented as:

  ```json
  {
    "selector": {
      "data": {
        "docType": "order",
        "operations": {
          "$elemMatch": {
            "time": {
              "$gt": "2017-05-27T08:47:00Z"
            }
          }
        }
      }
    },

    "fields": [
      "data.id"
    ],
    "limit": 10000,
    "skip": 0
  }
  ```

## Complex mongo query on couchdb, including *$and*:
  ```
  {
  "submissionTime": {
          "$and": [
            {
              "$gt": "2017-05-25T07:19:19Z"
            },
            {
              "$lt": "2017-05-25T08:19:19Z"
            }
          ]
        }
  }
  or:

  {
    "$and": [
      {
        "submissionTime": {
          "$gt": "2017-05-25T07:19:19Z"
        }
      },
      {
        "submissionTime": {
          "$lt": "2017-05-25T08:19:19Z"
        }
      }
    ]
  }
  ```

## How to add mongo index?
From [admin portal](http://host:5894/_utils/), Choose Databases, some db, *Design Documents*, *plus button*, *mongo indexes*,
and then add an index like the following:
```
{
  "index": {
    "fields": [
      "data.docType"
    ]
  },
  "type": "json"
}
```

## How to add multiple fields index by curL and input user/password?
```

curl -H 'Content-Type: application/json' \
            -X POST http://admin:password@ja03:5984/rong/_index  \
            -d '{ "index": { "fields": [ "data.orderFormId","data.contractId", "data.docType"] },"type": "json"}'

curl -H 'Content-Type: application/json' \
            -X POST http://admin:password@ja03:5984/rong/_index  \
            -d '{ "index": { "fields": [ "data.orderFormId","data.docType"] },"type": "json"}'

curl -H 'Content-Type: application/json' \
            -X POST http://admin:password@ja03:5984/rong/_index  \
            -d '{ "index": { "fields": [ "data.orderFormId","data.contractId", "data.status"] },"type": "json"}'

curl -H 'Content-Type: application/json' \
            -X POST http://admin:password@ja03:5984/rong/_index  \
            -d '{ "index": { "fields": [ "data.deliveryRecordFormId","data.contractId", "data.status"] },"type": "json"}'

curl -H 'Content-Type: application/json' \
            -X POST http://admin:password@ja03:5984/rong/_index  \
            -d '{ "index": { "fields": [ "data.docType","data.buyerId"] },"type": "json"}'

curl -H 'Content-Type: application/json' \
            -X POST http://admin:password@ja03:5984/rong/_index  \
            -d '{ "index": { "fields": [ "data.docType","data.sellerId"] },"type": "json"}'

```
