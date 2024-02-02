
# API Response Examples

## Server API Response 

<br>

## My Rcommendation

##### I referenced RFC 7807, Google API design guide, Apple, Meta, Twitter, and AWS APIs.

<br>

### SUCCESS

```bash
{
  "statusCode": 200,
  "data": {
    "userId": "FfPS9TQdoKpuIvJaUhSMk8yQVrhPHdluQYqMHoteCj1zmgAAAYEoHfB-",
    "userName": "Doohyeon"
  },
  "messageId" : "dfgh56dfg-qwe90scv-2nmk34"
}
```

<br>

### ERROR

```bash
{
  "status_code" : 400 # /* The HTTP Response code
  "error": {
    "type": "doohyeon-01", # /* A URI identifier that categorizes the error or Something a unique identifier for the error */
    "message": "The server can not find the requested resource."  # /* Message for Developer. */
    "title": "Unregistered user." # /* A brief, human-readable message about the error */
    "detail": "The ID doesn't exist, Please sign-up to use this service." # /* A human-readable explanation of the error or Message for User. */
  },
  "messageId" : "dfgh56dfg-qwe90scv-2nmk34"
}
```

<br><br>

## RFC 7807
#### link: https://datatracker.ietf.org/doc/rfc7807/
### ERROR
```bash
   {
    "type": "https://example.com/probs/out-of-credit",
    "title": "You do not have enough credit.",
    "detail": "Your current balance is 30, but that costs 50.",
    "instance": "/account/12345/msgs/abc",
    "balance": 30,
    "accounts": ["/account/12345",
                 "/account/67890"]
   }
```

<br><br>

## JSON API
#### link: https://jsonapi.org/
```bash
{
  "links": {
    "self": "http://example.com/articles",
    "next": "http://example.com/articles?page[offset]=2",
    "last": "http://example.com/articles?page[offset]=10"
  },
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON:API paints my bikeshed!"
    },
    "relationships": {
      "author": {
        "links": {
          "self": "http://example.com/articles/1/relationships/author",
          "related": "http://example.com/articles/1/author"
        },
        "data": { "type": "people", "id": "9" }
      },
      "comments": {
        "links": {
          "self": "http://example.com/articles/1/relationships/comments",
          "related": "http://example.com/articles/1/comments"
        },
        "data": [
          { "type": "comments", "id": "5" },
          { "type": "comments", "id": "12" }
        ]
      }
    },
    "links": {
      "self": "http://example.com/articles/1"
    }
  }],
  ...
}
```

<br><br>

## Google
#### link: https://cloud.google.com/apis/design/errors
### ERROR
``` bash
{
  "error": {
    "code": 400,
    "message": "API key not valid. Please pass a valid API key.",
    "status": "INVALID_ARGUMENT",
    "details": [
      {
        "@type": "type.googleapis.com/google.rpc.ErrorInfo",
        "reason": "API_KEY_INVALID",
        "domain": "googleapis.com",
        "metadata": {
          "service": "translate.googleapis.com"
        }
      }
    ]
  }
}
```

<br><br>

## Apple

### SUCCESS
```bash
{
"type": "...",
"...": "...:, */ data /*
}
```

### ERROR
```bash
{
"serviceErrors": [{
    "code": "-20101", 
    "message": "Your Apple ID or password was incorrect.", 
    "suppressDismissal": false
  }]
}
```

<br><br>

## Meta (Facebool & Instagram)
### SUCCESS

```bash
{
    "data": {
         ...
    },
    "pagination": {
         "next_url": "...",
         "next_max_id": "13872296"
    }
}
```

### ERROR
```bash
{
    "error": {
        "message": "Missing redirect_uri parameter.",
        "type": "OAuthException",
        "code": 191,
        "fbtrace_id": "AWswcVwbcqfgrSgjG80MtqJ"
    }
}
```

<br><br>


## ETC

### SUCCESS

```bash
{
  # If you want to clarify 'Status Code', or 'result of the response' you can add it.
  
  # "status": "SUCCESS",
  # "statusCode": 200,


  "data": {
    "user_id": "FfPS9TQdoKpuIvJaUhSMk8yQVrhPHdluQYqMHoteCj1zmgAAAYEoHfB-",
    "user_name": "Doohyeon"
  },
  
  
  # Something what you need
  
  # "pagenation": {
  #  "page_size": 10,
  #  "page_number": 1
  # }
  
  # "description": "This is the descriptoon"
  
}
```

<br>

### ERROR

```bash
{
  # If you want to clarify 'Status Code', or 'result of the response' you can add it.
  
  # "status": "ERROR",
  # "statusCode": 404,


  # "data": null  /* or optional error payload */
  
  "error": {
    "type": "error-01", # /* Something a unique identifier for the error */
  # "code": 404 /* optional */
    "message": "The server can not find the requested resource."  # /* Message for Developer. */
    "detail": "Unregistered user." # /* Message for User. */
  },
}
```
