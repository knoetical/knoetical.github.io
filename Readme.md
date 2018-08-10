Overview
=========

Introduction
------------

Welcome to Knoetical's API documentation page! Here you'll find the complete and updated informatoin about our API endpoints. Our <a href="https://www.knoetical.com/#/download" target="_blank">integrated clients</a> use this same API under the hood, if you are using one of them then this document will help you understand the inner functioning of Knoetical. If you are planning to integrate our API into your own production code then please take a look at our <a href="https://www.knoetical.com/#/tos" target="_blank">Terms of services</a> and <a href="https://www.knoetical.com/#/privacypolicy" target="_blank">Privacy policy</a>. Knoetical are working on providing official client libraries for our APIs, we will update this page and our <a href="https://github.com/knoetical" target="_blank">GitHub</a> account when they are released. For any query please drop a mail to [info@knoetical.com](mailto:info@knoetical.com)

Getting an account
------------------
Knoetical is in private beta and the sigups are limited. Please drop your mail in our <a href="https://www.knoetical.com/#/landing" href="_blank">HomePage</a> or send us a request [info@knoetical.com](mailto:info@knoetical.com)
Once you have an activation code you can head to our <a href="https://www.knoetical.com/account/register" target="_blank">Registraion</a> page and sign up.

Buying Credits
--------------
All new accounts get enough credits to get a trial of the Knoetical's service. These trial credits are subjected to rate limiting and lower priority. To buy more credits user has to login to our web application and make the purchase. Not all API calls are subjected to credit consumption. User can test a lot of API calls without ever needing credits. Almost all of the API calls are subjected to Oauth2 authorization except zero rated api calls. Documentation is anotated with such information

Authorization
-------------
Knoetical API uses Oauth2 authentication for authorization.<br />
Authentication end point is:
> https://www.knoetical.com/oauth/token

Knoetical's Oauth2 authentcation server supports following grant types:
> password
> authorization_code
> refresh_token
> implicit

Our web client users authorization_code grant type for communicating with our servers

Profile
=======
profile end point is there for editing and fetching user's own information.

UserInfo
--------

**GET /api/v1/profile/userinfo**

`requires Oauth2 authentication`

**returns:** user information in JSON format

**success HTTP code:** 200

``` json
{
    "username": "Email provided at the time of signup",
    "givenname": "Full name",
    "billingaddr": "Billing address",
    "subscribed": "boolean value representing subscription status",
    "emailverified": "Whether email has been verified",
    "creditbalance": "available credit" 
}
```

**POST /api/v1/profile/userinfo**

`requires Oauth2 authentication`

**body:** JSON containing user information

**success HTTP code:** 201

Credit Balance
--------------

**GET /api/v1/profile/creditbalance**

`requires Oauth2 authentication`

**success HTTP code:** 200

**returns:** credit balance in JSON format

``` json
{
    "creditbalance": "available credit" 
}
```

Payment History
---------------

**GET /api/v1/profile/paymenthistory**

`requires Oauth2 authentication`

**success HTTP code:** 200

**body:** JSON containing List of payment history

``` json
[
    {
        "date": "date of payment",
        "expiry": "date time when credits purchased are due to expire",
        "amount": "amount of credits purchased",
        "balance": "amount of credits remaining out of this purchase"
    },
    {
        ...
    }
]
```

Task
====

Task handling related APIs

creating task
------------
**POST /api/v1/task**

`requires Oauth2 authentication`

**success HTTP code:** 201

**body:**
``` json
{
    "taskname": "name of the task : default [Untitled]",
    "taskJsonType": "type of task : HNCOMMENT_OPINIONANALYSIS|CONTRACT_SUMMARY|JDRESUME_COMPARISON",
    "inputDataBatch": "input data, format depends on taskJsonType"
}
```

**returns:**
``` json
{
    "taskId": "internal unique ID assigned to this task",
    "taskname": "name of the task : default [Untitled]",
    "created": "time created",
    "taskJsonType": "type of task : HNCOMMENT_OPINIONANALYSIS|CONTRACT_SUMMARY|JDRESUME_COMPARISON",
    "inputDataBatch": "input data, format depends on taskJsonType"    
}
```

delete task
-----------

**DELETE /api/v1/tasl/{taskid}**

`requires Oauth2 authentication`

**success HTTP code:** 200

**PathParams:**
Param name | param value
--- | ---
taskid | valid task Id

retrieve task
-------------

**GET /api/v1/task/{taskid}**

`requires Oauth2 authentication`

**success HTTP code:** 200

**PathParams:**
Param name | param value
--- | ---
taskid | valid task Id

**returns:**
``` json
{
    "taskId": "internal unique ID assigned to this task",
    "taskname": "name of the task : default [Untitled]",
    "created": "time created",
    "taskJsonType": "type of task : HNCOMMENT_OPINIONANALYSIS|CONTRACT_SUMMARY|JDRESUME_COMPARISON",
    "inputDataBatch": "input data, format depends on taskJsonType",
    "resultBatch": "result data, format depends on taskJsonType"
}
```

execute task
------------

**GET /api/v1/task/{taskid}/execute**

`requires Oauth2 authentication`

**success HTTP code:** 201

**PathParams:**
Param name | param value
--- | ---
taskid | valid task Id

Input task data
---------------
**POST /api/v1/task/input**

`requires Oauth2 authentication`

**success HTTP code:** 201

**body:**
``` json
{
    "taskId": "internal unique ID assigned to this task",
    "taskname": "name of the task : default [Untitled]",
    "taskJsonType": "type of task : HNCOMMENT_OPINIONANALYSIS|CONTRACT_SUMMARY|JDRESUME_COMPARISON",
    "inputDataBatch": "input data, format depends on taskJsonType"
}
```

List tasks
----------

**GET /api/v1/task**

`requires Oauth2 authentication`

**success HTTP code:** 200

**returns:**
``` json
[
    {
        "taskId": "internal unique ID assigned to this task",
        "taskname": "name of the task : default [Untitled]",
        "taskJsonType": "type of task : HNCOMMENT_OPINIONANALYSIS|CONTRACT_SUMMARY|JDRESUME_COMPARISON",
        "inputDataBatch": "input data, format depends on taskJsonType"
    },
    {
        ...
    }
]
```

Dispatch Task Report
-------------------
**GET /api/v1/task/{taskid}/dispatch**

`requires Oauth2 authentication`

**success HTTP code:** 201

**PathParams:**
Param name | param value
--- | ---
taskid | valid task Id


Fetch snipped by Id
-------------------
**GET /api/v1/task/snippet/{snippetId}**

`requires Oauth2 authentication`

**success HTTP code:** 200

**PathParams:**
Param name | param value
--- | ---
snippetId | valid snippet Id

**returns:**
``` json
{
    "id": "internal ID assigned to this short lived snippt",
    "content": "text of snippet"
}
```

File Upload
===========

Single File upload
------------------
**POST /api/v1/upload**

`requires Oauth2 authentication`

**success HTTP code:** 201

**RequestParams:**
Param name | param value
--- | ---
file | multipartfile


**returns:** Unique file name
``` json
{
    "filename": "unique file name"
}
```

Zero
====

API that doesn't require authentication

Message
-------

**POST /zero/v1/message**

**success HTTP code:** 201

**body:** JSON containing user message

``` json
{
    "firstname": "First name",
    "lastname": "Last name",
    "email": "Email address",
    "subject": "message subject",
    "message": "message content"
}
```

Snippet
-------
**POST /zero/v1/snnipet**

**success HTTP code:** 200

**body:** JSON

``` json
{
    "content": "text of snippet"
}
```

**returns:** Snippet ID
``` json
{
    "id": "internal ID assigned to this short lived snippt",
    "content": "text of snippet"
}
```