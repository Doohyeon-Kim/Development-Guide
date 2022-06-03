
# API Response Examples

## Server API Response 

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

### Error

```bash
{
  # If you want to clarify 'Status Code', or 'result of the response' you can add it.
  
  # "status": "ERROR",
  # "statusCode": 404,


  # "data": null  /* or optional error payload */
  
  "error": {
    "type": "dooadex-01", # /* Something a unique identifier for the error */
  # "code": 404 /* optional */
    "message": "The server can not find the requested resource."  # /* Message for Developer. */
    "detail": "Unregistered user." # /* Message for User. */
  },
}
```
<br>

<br>

## Recommendation

### SUCCESS

```bash
{
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

### Error

```bash
{
  "error": {
    "type": "dooadex-01", # /* Something a unique identifier for the error */
    "message": "The server can not find the requested resource."  # /* Message for Developer. */
    "detail": "Unregistered user." # /* Message for User. */
  },
}
```
