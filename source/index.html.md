---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---
# Organization API

> API Endpoint:

```
	https://api.huntr.co/org
```

The Huntr API for Organizations is organized around [REST](https://en.wikipedia.org/wiki/Representational_State_Transfer). You can use this API to access Huntr's Organizational API endpoints, which can get information on your members plus the jobs, employers, and events they manage through Huntr.

This API is for you if you are an organization using Huntr to follow the job search progress of your members (job seekers). This is the case if you have an [organizational account](https://huntr.co/advisors) with member boards managed by your organization's advisors.

This API will **NOT** work for you if multiple job seekers have **personally** shared their boards with you, in that case there is no concept of an organization, and you are using Huntr as an individual.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> Make sure to replace `ORG_ACCESS_TOKEN` with your API key.


Huntr uses Access Tokens to allow access to the organization API. You can register a new Huntr Access Token in your organization's admin dashboard.

Huntr expects for the Access Token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer <ORG_ACCESS_TOKEN>`

<aside class="notice">
You must replace <code>ORG_ACCESS_TOKEN</code> with your organization's Access Token.
</aside>

<aside class="warning">
Your Access Token carries many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.
</aside>

# Pagination

> A sample list query:

```shell
curl "https://api.huntr.co/org/members?limit=2"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "_id" : "5a70a7361d06bb000f0f1c43",
      "email" : "leod@purpleuniversity.edu",
      "givenName" : "Leo",
      "familyName" : "Davis",
      "createdAt" : "2018-01-30T17:11:18.399Z",
      "isActive": true
    },
    {
      "_id" : "5a70a7361d06bb000f0f1c44",
      "email" : "johnj@purpleuniversity.edu",
      "givenName" : "John",
      "familyName" : "Johnson",
      "createdAt" : "2018-01-28T17:11:18.399Z",
      "isActive": true
    }
  ],
  "next": "5a70a7361d06bb000f0f1c44"
}
```

> Retrieve the second page of results with this query:


```shell
curl "https://api.huntr.co/org/members?limit=2&next=5a70a7361d06bb000f0f1c44"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

Some API resources have support for bulk fetches via "list" API methods. For instance, you can list members, jobs, and job events. These list API methods share a common structure, taking these two parameters: `limit`, `next`.

Huntr uses cursor-based pagination via the `next` parameter which takes an existing object ID value. Every "list" endpoint result includes a `data` parameter, with the results from the call, along with a `next` parameter. The `next` parameter represents the id of the last object returned. Passing this parameter in subsequent call will return the next page of results. So, if you've made your first query and set 100 as a limit and want to retrieve the second page of results, you can repeat the same query, but with the `next` parameter set with the value returned by your first call. You can see an example set of queries on the right.

# Members

## Member Resource

> A member of your organization:

```json
{
  "_id" : "5a70a7361d06bb000f0f1c43",
  "email" : "leod@purpleuniversity.edu",
  "givenName" : "Leo",
  "familyName" : "Davis",
  "createdAt" : "2018-01-30T17:11:18.399Z",
  "isActive": true
}
```

A member is one of the job seekers using Huntr under your organization. If you are an educational institution, your job seeking students qualify as your members.

Field | Description
----- | -----------
_id | Member's Huntr id
email | Member's email
givenName | Member's first name
familyName | Member's last name
createdAt | Date the member signed up for Huntr
isActive | Defines if member has been deactivated by an organization admin or advisor. Deactivating a member prevents them from continuing activity on boards managed by your organization, and it is usually done for members who have found a job and stopped using Huntr.

## List Members

```shell
curl "https://api.huntr.co/org/members?active=true"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "_id" : "5a70a7361d06bb000f0f1c43",
      "email" : "leod@purpleuniversity.edu",
      "givenName" : "Leo",
      "familyName" : "Davis",
      "createdAt" : "2018-01-30T17:11:18.399Z",
      "isActive": true
    },
    {
      "_id" : "5a70a7361d06bb000f0f1c44",
      "email" : "johnj@purpleuniversity.edu",
      "givenName" : "John",
      "familyName" : "Johnson",
      "createdAt" : "2018-01-28T17:11:18.399Z",
      "isActive": true
    }
  ],
  "next": "5a70a7361d06bb000f0f1c44"
}

```

This endpoint retrieves all members for your organization.

### HTTP Request

`GET https://api.huntr.co/org/members`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
active | none | If set to `true`, the result will only include active members. If set to `false` the result will only include deactivated members.
limit | 100 | Defines the maximum number of members to return
next | none | If set, returns the next set of results after the object id provided

## Retrieve a Member

```shell
curl "https://api.huntr.co/org/members/5a70a7361d06bb000f0f1c43"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "_id" : "5a70a7361d06bb000f0f1c43",
  "email" : "leod@purpleuniversity.edu",
  "givenName" : "Leo",
  "familyName" : "Davis",
  "createdAt" : "2018-01-30T17:11:18.399Z",
  "isActive": true
}
```

This endpoint retrieves a specific member.

### HTTP Request

`GET https://api.huntr.co/org/members/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the member to retrieve

# Jobs

## Job Resource

> A job in one of your members' boards:

```json
{
    "_id" : "5b105edfb83359000f7b6353",
    "_member": "5a70a7361d06bb000f0f1c43",
    "title" : "Full Stack Engineer",
    "description": "If you want to experience what it's like working with a tight team of entrepreneurs, and you'd like to maximize the pace of personal development, Routific might be the place for you to jump-start your career...",
    "salary": "$95,000",
    "url" : "https://angel.co/routific/jobs/200057-full-stack-engineer",
    "deadline": "2018-05-25T20:45:25.933Z",
    "applicationDate": "2018-05-31T20:45:25.933Z",
    "firstInterviewDate": null,
    "secondInterviewDate": null,
    "offerDate": null,
    "_employer" : {
      "_id": "5af1cd55a2642af3c39b8e2f",
      "name": "Routific",
      "domain": "routific.com"
    },
    "_list" : {
      "name": "Applied"
    },
    "location" : {
      "lat" : "49.2827",
      "lng" : "-123.1207",
      "name" : "Vancouver",
      "address" : "Vancouver, BC, Canada"
    },
    "updatedAt" : "2018-05-31T20:45:25.933Z",
    "createdAt" : "2018-05-31T20:45:19.742Z",
}
```

A job is an instance of a job opportunity that one of your members has added to one of their boards managed by your organization. Each job card in the Huntr UI represent a different Job.

Field | Description
----- | -----------
_id | Job's id
_member | id for the member who owns the job
title | Job title
description | Job description
salary | Job salary
url | URL for the original job post (i.e: https://indeed.com/jobs/001)
deadline | Date of job's application deadline
applicationDate | Date of application deadline
firstInterviewDate | Date of first interview
secondInterviewDate | Date of second interview
offerDate | Date offer was received
_employer  | Object representing the employer
_list  | Object representing the list the job is in. A list is a stage in a specific Huntr board.
location | Object representing job's location
createdAt | Date the job was created by the member
updatedAt | Date the job entity was last updated

## List Jobs

```shell
curl "https://api.huntr.co/org/jobs"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "_id" : "5b105edfb83359000f7b6353",
      "_member": "5a70a7361d06bb000f0f1c43",
      "title" : "Full Stack Engineer",
      "description": "If you want to experience what it's like working with a tight team of entrepreneurs, and you'd like to maximize the pace of personal development, Routific might be the place for you to jump-start your career...",
      "salary": "$95,000",
      "url" : "https://angel.co/routific/jobs/200057-full-stack-engineer",
      "deadline": "2018-05-25T20:45:25.933Z",
      "applicationDate": "2018-05-31T20:45:25.933Z",
      "firstInterviewDate": null,
      "secondInterviewDate": null,
      "offerDate": null,
      "_employer" : {
        "_id": "5af1cd55a2642af3c39b8e2f",
        "name": "Routific",
        "domain": "routific.com"
      },
      "_list" : {
        "name": "Applied"
      },
      "location" : {
        "lat" : "49.2827",
        "lng" : "-123.1207",
        "name" : "Vancouver",
        "address" : "Vancouver, BC, Canada"
      },
      "updatedAt" : "2018-05-31T20:45:25.933Z",
      "createdAt" : "2018-05-31T20:45:19.742Z",
    },
    {
      "_id" : "5b105edfb83359000f7b6353",
      "_member": "5a70a7361d06bb000f0f1c44",
      "title" : "Product Manager",
      "description": "At Stripe, Product Managers help build innovative payments products for our customers. Product development is an extremely collaborative effort between engineering, design, analytics and customer-facing teams. In this role, youd be supporting the full stack engineering team that builds the...",
      "salary": "$105,000",
      "url" : "https://betalist.com/jobs/171556-product-manager-seattle-at-stripe",
      "deadline": "2018-05-23T20:45:25.933Z",
      "applicationDate": "2018-05-29T20:45:25.933Z",
      "firstInterviewDate": "2018-05-31T20:45:25.933Z",
      "secondInterviewDate": "2018-06-02T20:45:25.933Z",
      "offerDate": "2018-06-03T20:45:25.933Z",
      "_employer" : {
        "_id": "5af1cd55a2642af3d39d8e2f",
        "name": "Stripe",
        "domain": "stripe.com"
      },
      "_list" : {
        "name": "Offer"
      },
      "location" : {
        "lat" : "49.2827",
        "lng" : "-123.1207",
        "name" : "Seattle",
        "address" : "Seattle, WA, USA"
      },
      "updatedAt" : "2018-05-31T20:45:25.933Z",
      "createdAt" : "2018-05-21T20:45:19.742Z",
    }
  ],
  "next": "5b105edfb83359000f7b6353"
}
```

This endpoint lists all jobs added by members from your organization, jobs are sorted by the date they were created, starting from the most recently created.

### HTTP Request

`GET https://api.huntr.co/org/jobs?member_id=<MEMBER_ID>`

### Query Parameters

Parameter | Default | Required | Description
--------- | ------- | -------- | -----------
member_id | none | no | If set, will only return jobs belonging to member
created_after | none | no | If set, will only return jobs created after time sent
created_before | none | no | If set, will only return jobs created before time sent
limit | 100 | Defines the maximum number of jobs to return
next | none | If set, returns the next set of results after the object id provided

## Retrieve a Job

```shell
curl "https://api.huntr.co/org/jobs/5b105edfb83359000f7b6353"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "_id" : "5b105edfb83359000f7b6353",
  "_member": "5a70a7361d06bb000f0f1c44",
  "title" : "Product Manager",
  "description": "At Stripe, Product Managers help build innovative payments products for our customers. Product development is an extremely collaborative effort between engineering, design, analytics and customer-facing teams. In this role, youd be supporting the full stack engineering team that builds the...",
  "salary": "$105,000",
  "url" : "https://betalist.com/jobs/171556-product-manager-seattle-at-stripe",
  "deadline": "2018-05-23T20:45:25.933Z",
  "applicationDate": "2018-05-29T20:45:25.933Z",
  "firstInterviewDate": "2018-05-31T20:45:25.933Z",
  "secondInterviewDate": "2018-06-02T20:45:25.933Z",
  "offerDate": "2018-06-03T20:45:25.933Z",
  "_employer" : {
    "_id": "5af1cd55a2642af3d39d8e2f",
    "name": "Stripe",
    "domain": "stripe.com"
  },
  "_list" : {
    "name": "Offer"
  },
  "location" : {
    "lat" : "49.2827",
    "lng" : "-123.1207",
    "name" : "Seattle",
    "address" : "Seattle, WA, USA"
  },
  "updatedAt" : "2018-05-31T20:45:25.933Z",
  "createdAt" : "2018-05-21T20:45:19.742Z",
}
```

This endpoint retrieves a specific job.

### HTTP Request

`GET https://api.huntr.co/org/jobs/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the job to retrieve

# Job Events

## Job Events Resource

> A job event object:

```json
{
    "_id" : "5b105edfb83359000f7b6353",
    "_member": "5a70a7361d06bb000f0f1c43",
    "_job": "5b105edfb83359000f7b6353",
    "date": "2018-05-18T20:45:19.742Z",
    "eventType": "JOB_APPLICATION_DATE_SET",
    "createdAt": "2018-05-21T20:45:19.742Z"
}
```

A job event represents one of many events that can occur during the lifetime of a job, therefore a single job may have **multiple** events related to it:

Field | Description | Included in event types
----- | ----------- | -----------------------
_id | id for the event | All
_member | Member id of member who owns the job | All
_job |  Job id for which event applies to | All
eventType | Type of event | All
date | Date when the event actually happened (will happen), this date is usually set by the job seekers themselves | JOB_APPLICATION_DATE_SET, JOB_FIRST_INTERVIEW_DATE_SET, JOB_SECOND_INTERVIEW_DATE_SET, JOB_OFFER_DATE_SET
_fromList | List from which a job was moved in a job moved event | JOB_MOVED
_toList | List to which a job was moved in a job moved event | JOB_MOVED
createdAt | Date the event was logged. As an example, if a member logged an interview date today, but the actual interview date logged is Friday week, then `createdAt` is **today**, and `date` is **next friday**  | All

## Event Types

There are multiple event types which include:

Event Name | Description | Extra fields
---------- | ----------- | -----------
JOB_ADDED | A new job is added | none
JOB_MOVED | An existing job is moved | _fromList, _toList
JOB_APPLICATION_DATE_SET |  Application date is set for an existing job | date
JOB_FIRST_INTERVIEW_DATE_SET |  First interview date is set for an existing job | date
JOB_SECOND_INTERVIEW_DATE_SET |  Second interview date is set for an existing job | date
JOB_OFFER_DATE_SET |  Offer date is set for an existing job | date

## List Job Events

```shell
curl "https://api.huntr.co/org/job-events"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
        "eventType": "JOB_FIRST_INTERVIEW_DATE_SET",
        "_id" : "5b105edfb83359000f7b6353",
        "_member": "5a70a7361d06bb000f0f1c43",
        "_job": "5b105edfb83359000f7b6353",
        "date": "2018-05-18T20:45:19.742Z",
        "createdAt": "2018-06-01T20:45:19.742Z"
    },
    {
        "eventType": "JOB_APPLICATION_DATE_SET",
        "_id" : "5b105edfb83359000f7b6351",
        "_member": "5a70a7361d06bb000f0f1c43",
        "_job": "5b105edfb83359000f7b6353",
        "date": "2018-05-18T22:25:19.742Z",
        "createdAt": "2018-05-20T20:45:19.742Z"
    },
    {
        "eventType": "JOB_MOVED",
        "_id" : "5b105edfb83359000f7b6341",
        "_member": "5a70a7361d06bb000f0f1c43",
        "_job": "5b105edfb83359000f7b6353",
        "_fromList": {
          "name": "Wishlist"
        },
        "_toList": {
          "name": "Applied"
        },
        "createdAt": "2018-05-20T20:45:19.742Z"
    },
    {
        "eventType": "JOB_ADDED",
        "_id" : "5b105edfb83359000f7b6330",
        "_member": "5a70a7361d06bb000f0f1c43",
        "_job": "5b105edfb83359000f7b6353",
        "createdAt": "2018-05-19T20:45:19.742Z"
    }
  ],
  "next": "5b105edfb83359000f7b6330"
}
```

This endpoint retrieves all job events for your organization.

### HTTP Request

`GET https://api.huntr.co/org/job-events`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
member_id | none | If set, will only return job events belonging to member
job_id | none | If set, will only return job events belonging to job
event_type | none | If set, will only return job events of the specific type. See [event types](#event-types) section for possible event name values.
limit | 100 | Defines the maximum number of jobs to return
next | none | If set, returns the next set of results after the object id provided

# Employers

## Employer Resource

> An employer for which your members have added a job for:

```json
{
  "_id" : "5a70a7361d06bb000f0f1c43",
  "name" : "Routific",
  "domain" : "routific.com",
  "foundedYear": "2015",
  "description" : "Routific is a market-leading route optimization solution. Our software can help you plan routes in minutes.",
  "location" : "788 Beatty St. Level B1, Vancouver, BC, V6B2M1, Canada",
  "jobCount": 4,
  "applicationCount": 2,
  "interviewCount": 1,
  "offerCount": 1
}
```

An employer for which your members have added a job for.

Field | Description
----- | -----------
_id | Employer's id
name | Name of the employer
domain | Website for the employer
foundedYear | Year the employer was founded
description | Employer description
location | Full address for the employer
jobCount | Number of jobs added for this employer
applicationCount | Number of applications to this employer
interviewCount | Number of interviews from this employer
offerCount | Number of offers from this employer

## List Employers

```shell
curl "https://api.huntr.co/org/employers"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "_id" : "5a71a7361d04bb000f1f1c43",
      "name" : "Routific",
      "domain" : "routific.com",
      "foundedYear": "2015",
      "description" : "Routific is a market-leading route optimization solution. Our software can help you plan routes in minutes.",
      "location" : "788 Beatty St. Level B1, Vancouver, BC, V6B2M1, Canada",
      "jobCount": 4,
      "applicationCount": 2,
      "interviewCount": 1,
      "offerCount": 1
    },
    {
      "_id" : "5a74a7361d08bc000f1f1c49",
      "name" : "Hootsuite",
      "domain" : "hootsuite.com",
      "foundedYear": "2015",
      "description" : "Enhance your social media management with Hootsuite, the leading social media dashboard. Manage multiple networks and profiles and measure your ...",
      "location" : "5 E 8th Ave, Vancouver, BC V5T 1R6, Canada",
      "jobCount": 8,
      "applicationCount": 2,
      "interviewCount": 0,
      "offerCount": 0
    },
    ...
  ],
  "next": "5a74a7361d08bc000f1f1c50"
}

```

This endpoint retrieves all employers to which your members have added a job for.

### HTTP Request

`GET https://api.huntr.co/org/employers`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Defines the maximum number of employers to return
next | none | If set, returns the next set of results after the object id provided

## Retrieve an Employer

```shell
curl "https://api.huntr.co/org/employers/5a71a7361d04bb000f1f1c43"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "_id" : "5a71a7361d04bb000f1f1c43",
  "name" : "Routific",
  "domain" : "routific.com",
  "foundedYear": "2015",
  "description" : "Routific is a market-leading route optimization solution. Our software can help you plan routes in minutes.",
  "location" : "788 Beatty St. Level B1, Vancouver, BC, V6B2M1, Canada",
  "jobCount": 4,
  "applicationCount": 2,
  "interviewCount": 1,
  "offerCount": 1
}
```

This endpoint retrieves a specific employer.

### HTTP Request

`GET https://api.huntr.co/org/employers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the employer to retrieve

# Webhooks

## Receiving Webhooks

Huntr can send webhook events to notify your application any time a [job event](#job-events) happens within your organization.

You can register webhook URLs in your organization's admin dashboard, under the **Developers** tab that we will notify any time a job event happens in your organization. When the event occurs—a new job is added, an interview date set, etc.—Huntr creates a **[Job Event](#job-events)** object.

The Job Event object contains all the information about the event that just happened, including the type of event and the data associated with that event. Huntr will send the **Job Event** via an `HTTP POST` request, to any endpoint URLs that you have defined in your organization's Webhooks settings.

## Responding to a webhook

To acknowledge receipt of a webhook, your endpoint should return a `200` HTTP status code. All other response codes will indicate to Huntr that you did not receive the webhook.
