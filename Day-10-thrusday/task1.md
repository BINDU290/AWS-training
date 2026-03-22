# AWS Lambda — Serverless Tasks

## Task 1 : Create a Serverless API with Lambda

### Objective

> Develop a Lambda function
> Trigger it using API Gateway
> Validate all API requests via CloudWatch Logs
> Test API requests using Postman
> Watch all logs in cloudwatch

### Ceate the Lamda Funcrion.py

```bash

import json
from datetime import datetime

def lambda_handler(event, context):

    # LOG every incoming request to CloudWatch
    print("===== INCOMING REQUEST =====")
    print("Timestamp     :", datetime.utcnow().isoformat())
    print("HTTP Method   :", event.get("httpMethod"))
    print("Path          :", event.get("path"))
    print("Query Params  :", json.dumps(event.get("queryStringParameters")))
    print("Headers       :", json.dumps(event.get("headers")))
    print("Body          :", event.get("body"))
    print("============================")

    # Parse body safely
    request_body = {}
    if event.get("body"):
        try:
            request_body = json.loads(event["body"])
        except Exception as e:
            print("Body parse error:", str(e))

    # Route based on HTTP method
    http_method = event.get("httpMethod", "")
    status_code  = 200

    if http_method == "GET":
        response_body = {
            "message"   : "GET request successful!",
            "query"     : event.get("queryStringParameters") or {},
            "timestamp" : datetime.utcnow().isoformat()
        }

    elif http_method == "POST":
        response_body = {
            "message"      : "POST request successful!",
            "receivedData" : request_body,
            "timestamp"    : datetime.utcnow().isoformat()
        }

    elif http_method == "PUT":
        response_body = {
            "message"     : "PUT request successful!",
            "updatedData" : request_body,
            "timestamp"   : datetime.utcnow().isoformat()
        }

    elif http_method == "DELETE":
        response_body = {
            "message"   : "DELETE request successful!",
            "timestamp" : datetime.utcnow().isoformat()
        }

    else:
        status_code   = 405
        response_body = { "error": "Method Not Allowed" }

    # Log the outgoing response
    print("===== OUTGOING RESPONSE =====")
    print("Status Code:", status_code)
    print("Response   :", json.dumps(response_body))
    print("=============================")

    return {
        "statusCode" : status_code,
        "headers"    : {
            "Content-Type"                : "application/json",
            "Access-Control-Allow-Origin" : "*"    # Enable CORS
        },
        "body" : json.dumps(response_body)
    }
    
    ```


![alt text](<Screenshot (176).png>)

![alt text](<Screenshot (177).png>)

Watch cloudwatch through logs

![alt text](<Screenshot (178).png>)

