VAddy Web API Scan Document
======================

Document Version 2.0.2

This specification defines "start scan", "cancel scan" and "get scan results".


## Start Scan
Scan start request from client(browser, Jenkins, etc) to VAddy WebAPI server.


### Scan Start Request
https://api.vaddy.net/v2/scan  
Method : POST  

    action=start
    user=vaddyuser
    auth_key=123456
    project_id=6eb1f9fcbdb6a5a
    crawl_id=30
    scan_type="SQLI,XSS,RFI,..."

`crawl_id` is optional. If you don't specify it, VAddy uses the latest crawl data for scan. You can see the crawl ID number in the Proxy Crawling page of console page.

`scan_type` is optional. [Scan type list document](https://github.com/vaddy/WebAPI-document/blob/master/VAddy-WebApi-ScanType.md)

The `auth_key` is WebAPI authentication key. You can create this key on VAddy management page(https://console.vaddy.net/user/webapi).  
The `auth_key` can also be specified by `X-API-KEY` in http header.

Set Your UserID(LoginID) of VAddy management page on `user` parameter.  

The `project_id` is grouping ID for scan target servers.  It shows on the server menu of VAddy console screen.  


### Scan Start Response
#### Success
Status Code : 200  
content-type  : application/json  
Contents:

    {"scan_id": "xxxxxxxxxxxx"}

Scan_id is created by VAddy scan server. It is required to get vaddy scan result data.



#### Authorization error
Status Code : 401  Unauthorized  

#### Fail
Status Code : 400  
content-type  : application/json  
Contents:

    {"error_message":"xxxxxx"}

Examples of error message.  

- This project_id does not exist.
- can not start scan because it has not finished previous scan.
- no crawling data.

----------------------------------------------------------------------------------

## Cancel Scan
Scan cancel request from client(browser, Jenkins, etc) to VAddy WebAPI server.

### Scan Cancel Request
https://api.vaddy.net/v2/scan  
Method : POST  

    action=cancel
    user=vaddyuser
    auth_key=123456
    scan_id=xxxxxxxxxxxx

scan_id is issued by starting the scan.



### Scan Cancel Response
#### Success
Status Code : 200  
content-type  : application/json  
Contents:

    {"scan_id": "xxxxxxxxxxxx"}


#### Authorization error
Status Code : 401  Unauthorized  

#### Fail
Status Code : 400  
content-type  : application/json  
Contents:

    {"error_message":"xxxxxx"}


- This project_id does not exist.
- This scan_id does not exist.
- can not cancel because the scan process has already been finished.


----------------------------------------------------------------------------------


## Get Scan Results

Scan results request from client(browser, Jenkins, etc) to VAddy WebAPI server.


### Scan Result Request
https://api.vaddy.net/v2/scan/result  
Method : GET  

    user=vaddyuser
    auth_key=123456
    scan_id=xxxxxxxxxxxx

Set scan_id which is included in the response of scan start.


### Scan Result Response
#### Still Scanning
Status Code : 200  
content-type  : application/json  
Contents:

    {"status":"scanning"}

#### canceled
Status Code : 200  
content-type  : application/json  
Contents:

    {"status":"canceled"}


#### Finished
Status Code : 200  
content-type  : application/json  
Contents:

    { "status":"finish",
      "project_id" : "6eb1f9fcbdb6a5a",
      "scan_id" : "1-837b5f9f-e088-4af5-9491-67f7ce8035a4",
      "scan_count" : 22,
      "alert_count" : 1,
      "timezone" : "UTC",
      "start_time" :  "2014-06-12T11:11:11+0000",
      "end_time" :  "2014-06-12T11:11:11+0000",
      "scan_result_url" : "https://console.vaddy.net/scan_status/1/1-837b5f9f-e088-4af5-9491-67f7ce8035a4",
      "complete" : 100,
      "crawl_id" : 30,
      "crawl_label" : "user edit scenario label",
      "scan_list" : ["XSS","SQL Injection"]
    }

Timestamp is UTC and ISO 8601 timestamp format.



#### Authorization error
Status Code : 401  Unauthorized  

#### Fail
Status Code : 400  
content-type  : application/json  
Contents:

    {"error_message":"xxxxxx"}

- This project_id does not exist.
- This scan_id does not exist.


----------------------------------------------------------------------------------


## check running scan process

VAddy doesn't allow to scan same ProjectId simultaneously.  
This API provide to check running scan process.  


### Request of checking scan process
https://api.vaddy.net/v2/scan/runcheck  
Method : GET  

    user=vaddyuser
    auth_key=123456
    project_id=6eb1f9fcbdb6a5a


### Response

#### No scan process running

Status Code : 200  
content-type  : application/json  
Contents:

    {"running_process": 0}

running_process 0 means no scan process running.  
You can start scan.  


#### scan process running
Status Code : 200  
content-type  : application/json  
Contents:

    {"running_process": 1}

A scan process is running, so you can't start scan now and need to wait.  
Call this API until getting `running_process : 0` at regular intervals.
