VAddy Web API Crawl Document
======================

Document Version 2.0.1

This specification defines to operate crawling information.


## Get crawling records
The request of fetching crawl information from client(browser, Jenkins, etc) to VAddy WebAPI server.  
Results contains only records of Recorded status.
Cancel or Crawling status are ignored.


### Request
https://api.vaddy.net/v2/crawl  
Method : GET  

    user=vaddyuser
    auth_key=123456
    project_id=6eb1f9fcbdb6a5a
    search_label=all-crawl (optional)
    page=1 (optional)
    sort=desc (optional)

Parameters of page, sort, search_label are optional. Each request can fetch at most 30 records.  

- page parameter: page number. Default is 1.
- sort parameter: asc or desc. Default is desc.
- search_label parameter: Partial-word search. Default is null(search all records).   


The `auth_key` is WebAPI authentication key. You can create this key on VAddy management page(https://console.vaddy.net/user/webapi).  
The `auth_key` can also be specified by `X-API-KEY` in http header.

Set Your UserID(LoginID) of VAddy management page on `user` parameter.  

#### Request example

    https://api.vaddy.net/v2/crawl?user=vaddyuser&auth_key=123456&project_id=6eb1f9fcbdb6a5a

#### Request example with search word

    https://api.vaddy.net/v2/crawl?user=vaddyuser&auth_key=123456&project_id=6eb1f9fcbdb6a5a&search_label=all-data


### Reqponse
#### Success
Status Code : 200  
content-type  : application/json  
Contents:

    {"total":3,
     "page":1,
     "limit":30,
     "items":[
        {"id":2, "label": "all-data", "start":"2016-03-12T11:11:11+0000", "end":"2016-03-12T12:00:00+0000"},
        {"id":1, "label": null, "start":"2016-03-12T11:11:11+0000", "end":"2016-03-12T12:00:00+0000"}
     ]
    }


- total: Number of hit records
- page:  Current page number
- limit: Number of maximum fetching record
- items: Array of records.

Timestamp is UTC and ISO 8601 timestamp format.


#### Success (No hit)
Status Code : 200  
content-type  : application/json  
Contents:

    {"total":0,
     "page":1,
     "limit":30,
     "items":[]
    }


#### Authorization error
Status Code : 401  Unauthorized  


#### Fail
Status Code : 400  
content-type  : application/json  
Contents:

    {"error_message":"xxxxxx"}

- This project_id does not exist.
