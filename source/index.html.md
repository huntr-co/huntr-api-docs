---
title: Huntr for Organizations - API Reference

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
            "id": "5b0379b5712bb674fd0d561e",
            "givenName": "Graciela",
            "familyName": "Varela",
            "email": "gracev@huntr.co",
            "createdAt": 1526954421,
            "isActive": true
        },
        {
            "id": "5b037898b7f93473edc1c43a",
            "givenName": "Nicole",
            "familyName": "Haylock",
            "email": "nicoleh@huntr.co",
            "createdAt": 1526954136,
            "isActive": false
        }
    ],
    "next": "5b037898b7f93473edc1c43a"
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
    "id": "5b0379b5712bb674fd0d561e",
    "givenName": "Graciela",
    "familyName": "Varela",
    "email": "gracev@huntr.co",
    "createdAt": 1526954421,
    "isActive": true
}
```

A member is one of the job seekers using Huntr under your organization. If you are an educational institution, your job seeking students qualify as your members.

Field | Type | Description
----- | ---- | -----------
id | String | Member's Huntr id
givenName | String | Member's first name
familyName | String | Member's last name
email | String | Member's email
createdAt | Unix timestamp | Date the member signed up for Huntr
isActive | Boolean | Defines if member has been deactivated by an organization admin or advisor. Deactivating a member prevents them from continuing activity on boards managed by your organization, and it is usually done for members who have found a job and stopped using Huntr.

## List Members

```shell
curl "https://api.huntr.co/org/members?active=true&limit=3"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "5b0377c7b7f93473edc1c433",
            "givenName": "Roberto",
            "familyName": "Moscoso",
            "email": "robertom@huntr.co",
            "createdAt": 1526953927,
            "isActive": true
        },
        {
            "id": "5a9f699edd2f935a4e5b263a",
            "givenName": "Rennie",
            "familyName": "Haylock",
            "email": "rennie@huntr.co",
            "createdAt": 1520396702,
            "isActive": true
        },
        {
            "id": "5a9b3d392bd2e22e37dee4f0",
            "givenName": "John",
            "familyName": "Smith",
            "email": "johns@huntr.co",
            "createdAt": 1520123193,
            "isActive": true
        }
    ],
    "next": "5a9b3d392bd2e22e37dee4f0"
}
```

This endpoint retrieves all members for your organization.

### HTTP Request

`GET https://api.huntr.co/org/members`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`active` | Boolean | none | If set to `true`, the result will only include active members. If set to `false` the result will only include deactivated members.
`limit` | Integer | 100 | Defines the maximum number of members to return
`next` | String | none | If set, returns the next set of results after the object id provided

## Retrieve a Member

```shell
curl "https://api.huntr.co/org/members/5a9b3d392bd2e22e37dee4f0"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "id": "5a9b3d392bd2e22e37dee4f0",
    "givenName": "John",
    "familyName": "Smith",
    "email": "johns@huntr.co",
    "createdAt": 1520123193,
    "isActive": true
}
```

This endpoint retrieves a specific member.

### HTTP Request

`GET https://api.huntr.co/org/members/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
ID | String | The ID of the member to retrieve

# Jobs

## Job Resource

> A job in one of your members' boards:

```json
{
  "id": "5b088a7b12728af4ec0be9d6",
  "title": "Senior Engineer",
  "description": "A job description",
  "salary": "$90,000",
  "url": "https://linkedin.com/jobs/asdf234adf",
  "location": {
      "lng": "13.384453399999984",
      "lat": "52.53073269999999",
      "url": "https://maps.google.com/?cid=13193077415763177194",
      "id": "1c56fadc63fb73a87bfae16a1096735b2817793b",
      "placeId": "ChIJbTkS9uxRqEcR6u7KfAI6F7c",
      "name": "ResearchGate",
      "address": "Invalidenstraße 115, 10115 Berlin, Germany"
  },
  "deadline": 1527404400,
  "applicationDate": 1527490800,
  "firstInterviewDate": 1527750000,
  "secondInterviewDate": 1527836400,
  "offerDate": 1528095600,
  "createdAt": 1527286395,
  "member": {
      "id": "5a6bf0758f18344d6f76d118",
      "givenName": "Gracielita",
      "familyName": "Varelita",
      "email": "rennie+grace@huntr.co",
      "createdAt": 1517023349
  },
  "employer": {
      "id": "5b088a7bb9defaba88d36f1e",
      "name": "ResearchGate",
      "domain": "researchgate.net",
      "foundedYear": 2008,
      "description": "Find 100+ million publication pages, 15+ million researchers, and 700k+ projects. ResearchGate is where you discover scientific knowledge and share your work.",
      "location": "10115 Berlin, Germany",
      "jobCount": 0,
      "applicationCount": 0,
      "interviewCount": 0,
      "offerCount": 0
  },
  "list": {
      "name": "wishlist"
  }
}
```

A job is an instance of a job opportunity that one of your members has added to one of their boards managed by your organization. Each job card in the Huntr UI represent a different Job.

Field | Type | Description
----- | ---- | -----------
`id` | String | Job's id
`title` | String | Job title  
`description` | String | Job description
`salary` | String | Job salary  
`url` | String | URL for the original job post
`location` | Object | Object representing job's location
`deadline` | Unix timestamp | Date of job's application deadline
`applicationDate` | Unix timestamp | Date of application deadline
`firstInterviewDate` | Unix timestamp | Date of first interview
`secondInterviewDate` | Unix timestamp | Date of second interview
`offerDate` | Unix timestamp | Date offer was received
`createdAt` | Unix timestamp | Date the job was created by the member
`member` | Object | Object representing the member who added the job
`employer`  | Object | Object representing the employer
`list`  | Object | Object representing the list the job is in. A list is a stage in a specific Huntr board.

## List Jobs

```shell
curl "https://api.huntr.co/org/jobs?limit=2&member_id=589189b5448fe5e45f01256b"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "5b00d6695cadd481fb0536a2",
            "title": "Developer",
            "offerDate": 1526781545,
            "createdAt": 1526781545,
            "member": {
                "id": "589189b5448fe5e45f01256b",
                "givenName": "Samir",
                "familyName": "Marin",
                "email": "rennie+dev2@huntr.co",
                "createdAt": 1485932981
            },
            "employer": {
                "id": "5b00d669a2567c2730e038b8",
                "name": "StubHub",
                "domain": "stubhub.com",
                "foundedYear": 2000,
                "description": "Your ticket out   www.stubhub.com   Twitter / Instagram: @StubHub   Snapchat: stubhub  https://www.snapchat.com/add/stubhub",
                "location": "San Francisco, CA, USA",
                "jobCount": 0,
                "applicationCount": 0,
                "interviewCount": 0,
                "offerCount": 0
            },
            "list": {
                "name": "offer"
            }
        },
        {
            "id": "5b00b6745e6a971db86c43de",
            "title": "Developer",
            "firstInterviewDate": 1526773364,
            "createdAt": 1526773364,
            "member": {
                "id": "589189b5448fe5e45f01256b",
                "givenName": "Samir",
                "familyName": "Marin",
                "email": "rennie+dev2@huntr.co",
                "createdAt": 1485932981
            },
            "employer": {
                "id": "58a21e5cc9903d839de2f879",
                "name": "Reddit",
                "domain": "reddit.com",
                "foundedYear": 2005,
                "description": "reddit is a source for what's new and popular online. vote on links that you like or dislike and help decide what's popular, or submit your own!",
                "location": "548 Market St, San Francisco, CA 94104, USA",
                "jobCount": 0,
                "applicationCount": 0,
                "interviewCount": 0,
                "offerCount": 0
            },
            "list": {
                "name": "interview"
            }
        }
    ],
    "next": "5b00b6745e6a971db86c43de"
}
```

This endpoint lists all jobs added by members from your organization, jobs are sorted by the date they were created, starting from the most recently created.

### HTTP Request

`GET https://api.huntr.co/org/jobs`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`member_id` | String | none | If set, will only return jobs belonging to member
`created_after` | Unix timestamp | none | If set, will only return jobs created after `created_after`
`created_before` | Unix timestamp | none | If set, will only return jobs created before `created_before`
`limit` | Integer | 100 | Defines the maximum number of jobs to return
`next` | String | none | If set, returns the next set of results after the object id provided

## Retrieve a Job

```shell
curl "https://api.huntr.co/org/jobs/5b088a7b12728af4ec0be9d6"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "id": "5b088a7b12728af4ec0be9d6",
  "title": "Senior Engineer",
  "description": "A job description",
  "salary": "$90,000",
  "url": "https://linkedin.com/jobs/asdf234adf",
  "location": {
      "lng": "13.384453399999984",
      "lat": "52.53073269999999",
      "url": "https://maps.google.com/?cid=13193077415763177194",
      "id": "1c56fadc63fb73a87bfae16a1096735b2817793b",
      "placeId": "ChIJbTkS9uxRqEcR6u7KfAI6F7c",
      "name": "ResearchGate",
      "address": "Invalidenstraße 115, 10115 Berlin, Germany"
  },
  "deadline": 1527404400,
  "applicationDate": 1527490800,
  "firstInterviewDate": 1527750000,
  "secondInterviewDate": 1527836400,
  "offerDate": 1528095600,
  "createdAt": 1527286395,
  "member": {
      "id": "5a6bf0758f18344d6f76d118",
      "givenName": "Gracielita",
      "familyName": "Varelita",
      "email": "rennie+grace@huntr.co",
      "createdAt": 1517023349
  },
  "employer": {
      "id": "5b088a7bb9defaba88d36f1e",
      "name": "ResearchGate",
      "domain": "researchgate.net",
      "foundedYear": 2008,
      "description": "Find 100+ million publication pages, 15+ million researchers, and 700k+ projects. ResearchGate is where you discover scientific knowledge and share your work.",
      "location": "10115 Berlin, Germany",
      "jobCount": 0,
      "applicationCount": 0,
      "interviewCount": 0,
      "offerCount": 0
  },
  "list": {
      "name": "wishlist"
  }
}
```

This endpoint retrieves a specific job.

### HTTP Request

`GET https://api.huntr.co/org/jobs/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the job to retrieve

# Events

## Events Resource

> An event object:

```json
{
    "id": "5b05ca43f193b66c692c29a0",
    "eventType": "JOB_MOVED",
    "createdAt": 1527106115,
    "member": {
        "id": "5a5a88fae10d8c2cea07f2f7",
        "givenName": "Gwen",
        "familyName": "Johnson",
        "email": "gwenj@huntr.co",
        "createdAt": 1515882746
    },
    "job": {
        "id": "5a8a255bb4271026f0c0f749",
        "title": "Engineer",
        "applicationDate": 1527106115,
        "createdAt": 1519002971
    },
    "toList": {
        "name": "applied"
    },
    "fromList": {
        "name": "wishlist"
    }
}
```

An event represents one of many events that can occur during the lifetime of a job, therefore a single job may have **multiple** events related to it:

Field | Type | In all events | Description
----- | ---- | ------------- | -----------
`id` | String | true | id for the event
`eventType` | String | true | Type of event
`date` | Unix timestamp | false | Date when the event actually happened (will happen), this date is usually set by the job seekers themselves
`createdAt` | Unix timestamp | true | Date the event was logged. As an example, if a member logged an interview date today, but the actual interview date logged is next Friday, then `createdAt` is **today**, and `date` is **next friday**
`member` | Object | true | Member who owns the job
`job` | Object | true |  Job for which event applies to
`fromList` | Object | false | List from which job was moved in a job moved event
`toList` | Object | false | List to which a job was moved or added to

## Event Types

There are multiple event types which include:

Event Name | Description | Extra fields
---------- | ----------- | -----------
`JOB_ADDED` | A new job is created | `toList`
`JOB_MOVED` | An existing job is moved | `fromList`, `toList`
`JOB_APPLICATION_DATE_SET` |  Application date is set for an existing job | `date`
`JOB_FIRST_INTERVIEW_DATE_SET` |  First interview date is set for an existing job | `date`
`JOB_SECOND_INTERVIEW_DATE_SET` |  Second interview date is set for an existing job | `date`
`JOB_OFFER_DATE_SET` |  Offer date is set for an existing job | `date`

## List Events

```shell
curl "https://api.huntr.co/org/events?limit=2"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
        "id": "5b17300b4453e621e676d90e",
        "eventType": "JOB_OFFER_DATE_SET",
        "date": 1528095600,
        "createdAt": 1528246283,
        "member": {
            "id": "5a6bf0758f18344d6f76d118",
            "givenName": "Gracielita",
            "familyName": "Varelita",
            "email": "rennie+grace@huntr.co",
            "createdAt": 1517023349
        },
        "job": {
            "id": "5b088a7b12728af4ec0be9d6",
            "title": "Senior Engineer",
            "description": "A job description",
            "salary": "$90,000",
            "url": "https://linkedin.com/jobs/asdf234adf",
            "location": {
                "lng": "13.384453399999984",
                "lat": "52.53073269999999",
                "url": "https://maps.google.com/?cid=13193077415763177194",
                "id": "1c56fadc63fb73a87bfae16a1096735b2817793b",
                "placeId": "ChIJbTkS9uxRqEcR6u7KfAI6F7c",
                "name": "ResearchGate",
                "address": "Invalidenstraße 115, 10115 Berlin, Germany"
            },
            "deadline": 1527404400,
            "applicationDate": 1527490800,
            "firstInterviewDate": 1527750000,
            "secondInterviewDate": 1527836400,
            "offerDate": 1528095600,
            "createdAt": 1527286395
        }
    },
    {
        "id": "5b1730094453e621e676d905",
        "eventType": "JOB_SECOND_INTERVIEW_DATE_SET",
        "date": 1527836400,
        "createdAt": 1528246281,
        "member": {
            "id": "5a6bf0758f18344d6f76d118",
            "givenName": "Gracielita",
            "familyName": "Varelita",
            "email": "rennie+grace@huntr.co",
            "createdAt": 1517023349
        },
        "job": {
            "id": "5b088a7b12728af4ec0be9d6",
            "title": "Senior Engineer",
            "description": "A job description",
            "salary": "$90,000",
            "url": "https://linkedin.com/jobs/asdf234adf",
            "location": {
                "lng": "13.384453399999984",
                "lat": "52.53073269999999",
                "url": "https://maps.google.com/?cid=13193077415763177194",
                "id": "1c56fadc63fb73a87bfae16a1096735b2817793b",
                "placeId": "ChIJbTkS9uxRqEcR6u7KfAI6F7c",
                "name": "ResearchGate",
                "address": "Invalidenstraße 115, 10115 Berlin, Germany"
            },
            "deadline": 1527404400,
            "applicationDate": 1527490800,
            "firstInterviewDate": 1527750000,
            "secondInterviewDate": 1527836400,
            "offerDate": 1528095600,
            "createdAt": 1527286395
        }
    }
  ],
  "next": "5b1730094453e621e676d905"
}
```

This endpoint retrieves all events for your organization, sorted from most recently created to oldest.

### HTTP Request

`GET https://api.huntr.co/org/events`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`member_id` | String | none | If set, will only return events belonging to member
`job_id` | String | none | If set, will only return events belonging to job
`event_type` | String | none | If set, will only return events of the specific type. See [event types](#event-types) section for possible event name values.
`limit` | Integer | 100 | Defines the maximum number of jobs to return
`next` | String | none | If set, returns the next set of results after the object id provided

# Employers

## Employer Resource

> An employer for which your members have added a job for:

```json
{
    "id": "589b77128bf3dd17f01a3055",
    "name": "Etsy",
    "domain": "etsy.com",
    "foundedYear": 2005,
    "description": "Find handmade, vintage, and unique goods that express who you are.",
    "location": "55 Washington St #512, Brooklyn, NY 11201, USA",
    "jobCount": 4,
    "applicationCount": 2,
    "interviewCount": 1,
    "offerCount": 1
}
```

An employer for which your members have added a job for.

Field | Type | Description
----- | ---- | -----------
`id` | String | Employer's id
`name` | String | Name of the employer
`domain` | String | Website for the employer
`foundedYear` | Integer | Year the employer was founded
`description` | String | Employer description
`location` | String | Full address for the employer
`jobCount` | Integer | Number of jobs added for this employer
`applicationCount` | Integer | Number of applications to this employer
`interviewCount` | Integer | Number of interviews from this employer
`offerCount` | Integer | Number of offers from this employer

## List Employers

```shell
curl "https://api.huntr.co/org/employers?limit=2"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
      {
          "id": "5b088f63b9defaba88d37264",
          "name": "The Art Institutes",
          "domain": "artinstitutes.edu",
          "foundedYear": 1921,
          "description": "The Art Institutes is a system of over 30 nonprofit institutions with a focus on education, student outcomes, and community involvement. Learn more about our programs.\r  ",
          "location": "K&L Gates Center, 210 Sixth Avenue, Pittsburgh, PA 15222, USA",
          "jobCount": 1,
          "applicationCount": 0,
          "interviewCount": 0,
          "offerCount": 0
      },
      {
          "id": "5b088a7bb9defaba88d36f1e",
          "name": "ResearchGate",
          "domain": "researchgate.net",
          "foundedYear": 2008,
          "description": "Find 100+ million publication pages, 15+ million researchers, and 700k+ projects. ResearchGate is where you discover scientific knowledge and share your work.",
          "location": "10115 Berlin, Germany",
          "jobCount": 1,
          "applicationCount": 1,
          "interviewCount": 2,
          "offerCount": 1
      }
  ],
  "next": "5b088a7bb9defaba88d36f1e"
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

# Webhooks

## Receiving Webhooks

Huntr can send webhook events to notify your application any time an [event](#events) happens within your organization.

You can register webhook URLs in your organization's admin dashboard, under the **Developers** tab that we will notify any time a event happens in your organization. When the event occurs—a new job is added, an interview date set, etc.—Huntr creates a **[Event](#events)** object.

The Event object contains all the information about the event that just happened, including the type of event and the data associated with that event. Huntr will send the **Event** via an `HTTP POST` request, to any endpoint URLs that you have defined in your organization's Webhooks settings.

## Responding to a webhook

To acknowledge receipt of a webhook, your endpoint should return a `200` HTTP status code. All other response codes will indicate to Huntr that you did not receive the webhook.
