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

# Organization Invitation

## Organization Invitation Resource

> An invitation sent to a member to join your Huntr Organization as a job seeker:

```json
{
    "id": "5e6a9d1f62593578ce9de841",
    "toEmail": "test@example.com",
    "boardName": "Job Search 2020",
    "to": {
        "id": "5e6a7c99c56bc6669e3da876",
        "givenName": "Cristobal",
        "familyName": "Hackett",
        "email": "test@example.com",
        "createdAt": 1584045343
    },
    "boardTemplate": {
        "id": "5a6e4a567e42789e6e65c986",
        "name": "Tech Jobs",
        "listNames": [
            "Wishlist",
            "Applied",
            "Phone Interview",
            "On Site Interview",
            "Offer",
            "Rejected"
        ]
    },
    "memberFieldValues": [
      {
          "fieldId": "5b3f908c99c94b6177d55a28",
          "value": "Seattle"
      }
    ],
    "createdAt": 1539800736
}
```

Field | Type | Description
----- | ---- | -----------
id | String | Invitation id
toEmail | String | Email to which invitation was sent to
boardName | String | Name to give the board created when member accepts invite
to | Object | If the user with this email already has a Huntr account this object will show his/her user account information
email | String | Member's email
boardTemplate | Object | The board template to use to define the board stages when member accepts invite
memberFieldValues | Array | Array of member field values to assign to the member when invitation is accepted. Each object in the array is of type `{fieldId: "5b3f908c99c94b6177d55a28", value: "Montreal"}`, where `fieldId` represents the `id` of an existing [Member Field](#member-fields)
createdAt | Unix timestamp | Date the invitation was created

## List Organization Invitations

```shell
curl "https://api.huntr.co/org/organization-invitations"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json

{
  "data": [
      {
          "id": "5bc77e60856b6201a7f84d99",
          "toEmail": "johns@gmail.com",
          "boardName": "Fall 2020",
          "memberFieldValues": [],
          "createdAt": 1539800672
      },
      {
          "id": "5bc77ea0856b6201a7f84d9a",
          "toEmail": "peterj@gmail.com",
          "boardName": "Fall 2020",
          "memberFieldValues": [],
          "createdAt": 1539800736
      },
      {
          "id": "5ca53f5687967fd925e3aff4",
          "toEmail": "joshg@gmail.com",
          "boardName": "Fall 2020",
          "boardTemplate": {
              "id": "5a6e4c3c7e42789e6e65c987",
              "name": "Fall 2020 Cohort 1",
              "listNames": [
                  "Wishlist",
                  "Applied",
                  "Interview",
                  "Offer",
                  "Rejected"
              ]
          },
          "memberFieldValues": [
              {
                  "fieldId": "5b3f908c99c94b6177d55a28",
                  "value": "Seattle"
              }
          ],
          "createdAt": 1554333526
      }
  ]
}
```

This endpoint retrieves all pending organization invitations.

### HTTP Request

`GET https://api.huntr.co/org/organization-invitations`

## Create Organization Invitation

```shell
curl --location --request POST 'https://api.huntr.co/org/organization-invitations' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
	"email": "test@example.com",
	"boardName": "Job Search 2020",
  "boardTemplateId": "5a6e4a567e42789e6e65c986",
  "memberFieldValues": [
    {"fieldId": "5b3f908c99c94b6177d55a28", "value": "Vancouver"},
    {"fieldId": "5b3f9cf499c94b6177d55a2a", "value": "Web Design"}
  ]
}'
```

> The above command returns JSON structured like this:

```json
{
    "id": "5ec478c20f3c3c8443328eb5",
    "toEmail": "test@example.com",
    "boardName": "Job Search 2020",
    "boardTemplate": {
        "id": "5a6e4a567e42789e6e65c986",
        "name": "Tech Jobs",
        "listNames": [
            "Wishlist",
            "Applied",
            "Phone Interview",
            "On Site Interview",
            "Offer",
            "Rejected"
        ]
    },
    "memberFieldValues": [
        {
            "fieldId": "5b3f908c99c94b6177d55a28",
            "value": "Vancouver"
        },
        {
            "fieldId": "5b3f9cf499c94b6177d55a2a",
            "value": "Web Design"
        }
    ],
    "createdAt": 1589934274
}
```

This endpoint creates and sends a single Organization Invitation.
The person invited through this endpoint will receive an email invitation
to join your organization under Huntr.

To prevent duplicate invitations from being sent, requests with an email that has an existing pending invitation from your organization will return an error and no invitation will be created.

### HTTP Request

`POST https://api.huntr.co/org/organization-invitations`

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
`email` | yes | String | An email address for the job seeker your want to invite
`boardName` | no | String | Name of board to be created for member
`boardTemplateId` | no | String | Id of the board template to use when creating a board for member, if none is submitted it will default to Organization's default board template, or Huntr's default template if no template has been set as a default.
`memberFieldValues` | no | Array | Array of member field values to assign to the member when invitation is accepted. Each object in the array is of type `{fieldId: "5b3f908c99c94b6177d55a28", value: "Montreal"}`, where `fieldId` represents the `id` of an existing [Member Field](#member-fields)

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
    "isActive": true,
    "memberFieldValues": [
        {
            "fieldId": "5b3f908c99c94b6177d55a28",
            "value": "Montreal"
        }
    ]
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
memberFieldValues | Array | Array of assigned member field values. Each object in the array is of type `{fieldId: "5b3f908c99c94b6177d55a28", value: "Montreal"}`, where `fieldId` represents the `id` of an existing [Member Field](#member-fields)

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
            "isActive": true,
            "memberFieldValues": []
        },
        {
            "id": "5a9f699edd2f935a4e5b263a",
            "givenName": "Rennie",
            "familyName": "Haylock",
            "email": "rennie@huntr.co",
            "createdAt": 1520396702,
            "isActive": true,
            "memberFieldValues": []
        },
        {
            "id": "5a9b3d392bd2e22e37dee4f0",
            "givenName": "John",
            "familyName": "Smith",
            "email": "johns@huntr.co",
            "createdAt": 1520123193,
            "isActive": true,
            "memberFieldValues": []
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
`email` | String | none | If set, it will return the single member that matches the provided email address.
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
    "isActive": true,
    "memberFieldValues": []
}
```

This endpoint retrieves a specific member.

### HTTP Request

`GET https://api.huntr.co/org/members/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
ID | String | The ID of the member to retrieve

## Update a Job Seeker's member field value

```shell
curl --location --request PUT 'https://api.huntr.co/org/members/5a9b3d392bd2e22e37dee4f0/member-fields/5b3f908c99c94b6177d55a28' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
	"value": "Seattle"
}'
```

> The above command returns JSON structured like this:

```json
{
    "id": "5a9b3d392bd2e22e37dee4f0",
    "givenName": "John",
    "familyName": "Smith",
    "email": "johns@huntr.co",
    "createdAt": 1589931899,
    "isActive": true,
    "memberFieldValues": [
        {
            "fieldId": "5b3f908c99c94b6177d55a28",
            "value": "Seattle"
        },
        {
            "fieldId": "5b3f9cf499c94b6177d55a2a",
            "value": "Web Design"
        },
        {
            "fieldId": "5b3fb247d9d5bd9d6de8f655",
            "value": "YVR-001"
        }
    ]
}
```

This endpoint updates or sets a specific member's field value.

### HTTP Request

`PUT https://api.huntr.co/org/members/<memberId>/member-fields/<fieldId>`

Where `<fieldId>` is the `id` for an existing [Member Field](#member-fields)

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
`value` | Mixed | The value to give the member field


# Member Fields

## Member Field Resource

> A member field:

```json
{
   "id": "5b3f908c99c94b6177d55a28",
   "name": "Location",
   "fieldType": "MULTIPLE_CHOICE",
   "choices": [
       "Vancouver",
       "Seattle",
       "New York",
       "Montreal",
       "Miami"
   ]
}
```

A member field is a custom field created by someone in your organization which describes your members and is used to filter and/or group your members in different areas of your advisor dashboard. For example: If your members are in different cities, you could create a 'location' custom field.

Field | Type | In all fields | Description
----- | ---- | -------------- | -----------
id | String | true | Member field id
name | String | true | Member field name
fieldType | String | true | Field type
choices | String | false | Choices found on member fields of fieldType `MULTIPLE_CHOICE`

## Types of Member Fields

### This is a list of all the types of fields currently available.

`memberField.fieldType` | Description
----------------------- | -----------
`MULTIPLE_CHOICE` | Represents a multiple choice field type like "Locations" or "Specialties"
`TEXT` | Represents a text field type like "Nickname"
`NUMBER` | Represents a number field type like "Age"
`DATE` | Represents a date field type like "Graduation Date"

## List Member Fields

```shell
curl "https://api.huntr.co/org/member-fields"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "5b3f908c99c94b6177d55a28",
            "name": "Location",
            "fieldType": "MULTIPLE_CHOICE",
            "choices": [
                "Vancouver",
                "Seattle",
                "New York",
                "Montreal",
                "Miami"
            ]
        },
        {
            "id": "5b3f960c99c94b6177d55a29",
            "name": "Graduation",
            "fieldType": "DATE"
        },
        {
            "id": "5b3f9cf499c94b6177d55a2a",
            "name": "Program",
            "fieldType": "MULTIPLE_CHOICE",
            "choices": [
                "Web Design",
                "iOS",
                "Data Science"
            ]
        },
        {
            "id": "5b3fb247d9d5bd9d6de8f655",
            "name": "Cohort",
            "fieldType": "MULTIPLE_CHOICE",
            "choices": [
                "YVR-001",
                "TRT-001",
                "YVR-002",
                "MTR-001"
            ]
        },
        {
            "id": "5b3ff25b0ec3828665ff7c81",
            "name": "Status",
            "fieldType": "MULTIPLE_CHOICE",
            "choices": [
                "In program",
                "Searching",
                "Placed"
            ]
        },
        {
            "id": "5b40d89ae582264d4368a2bc",
            "name": "Gender",
            "fieldType": "MULTIPLE_CHOICE",
            "choices": [
                "Female",
                "Male"
            ]
        },
        {
            "id": "5b43ee4a8308d2400b9cced3",
            "name": "Nickname",
            "fieldType": "TEXT"
        }
    ]
}
```

This endpoint retrieves all member fields for your organization.

### HTTP Request

`GET https://api.huntr.co/org/member-fields`

## Create new member field

```shell
curl --location --request POST 'https://api.huntr.co/org/member-fields' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
	"name": "Location",
	"fieldType": "MULTIPLE_CHOICE",
  "choices": ["London", "Paris", "New York", "San Francisco"]
}'
```

> The above command returns JSON structured like this:

```json
{
    "id": "5ec44b9cc482170b3129e569",
    "name": "Locations",
    "fieldType": "MULTIPLE_CHOICE",
    "choices": [
        "London",
        "Paris",
        "New York",
        "San Francisco"
    ]
}
```

This endpoint creates a new Member Field. After creating the new field, you will
be able to view the field from your advisor dashboard and will be able to send
organization invitations with the `memberFieldValues` referring to the newly created field.

To prevent duplicate fields from being created, requests with a field name
that matches an existing Member Field from your organization, will return an error
message and the field will not be created.

### HTTP Request

`POST https://api.huntr.co/org/member-fields`

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
`name` | yes | String | Name of the new field, must be unique (i.e: you cannot have two fields with the same name)
`fieldType` | yes | String | One of [type of member fields](#types-of-member-fields)
`choices` | only when `fieldType == 'MULTIPLE_CHOICE'` | Array | List of strings representing choices in a field of type MULTIPLE_CHOICE

# Board Templates

## Board Template Resource

> A board template:

```json
{
    "id": "5a6e4a567e42789e6e65c986",
    "name": "Tech Jobs",
    "listNames": [
        "Wishlist",
        "Applied",
        "Phone Interview",
        "On Site Interview",
        "Offer",
        "Rejected"
    ],
    "isDefault": true
}
```

A template to use when creating a new board for your members. Board templates are used when you want your job seekers' boards to have specific stages. Say you want your job seekers' board to have the following stages `['Wishlist', 'Applied', 'Interview', 'Offer']`. First you create a board template where `listNames == ['Wishlist', 'Applied', 'Interview', 'Offer']` (the order of the stages is the same order that will be followed when a new board is created). Then you can create a new [Organization Invitation](#create-organization-invitation) in which `boardTemplateId` is the id of the Board Template you just created.

Field | Type | Found in all records | Description
----- | ---- | -------------- | -----------
id | String | true | Board Template id
name | String | true | Board template name
listNames | Array | true | Names to give each stage in a board created with this template
isDefault | Boolean | true | If true, it means the template is the default within the organization

## Default Board Stages

In cases where there are no existing Board Templates for your organization. Huntr will create boards with the following stages (in the order shown):

`[ "wishlist", "applied", "interview", "offer", "rejected" ]`

## List Board Templates

```shell
curl "https://api.huntr.co/org/board-templates"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "5a6bd300f92659747fe49a53",
            "name": "Spring 2018",
            "listNames": [
                "Applied",
                "Wishlist",
                "Interview",
                "Offer"
            ],
            "isDefault": false
        },
        {
            "id": "5a6e4a567e42789e6e65c986",
            "name": "Tech Jobs",
            "listNames": [
                "Wishlist",
                "Applied",
                "Phone Interview",
                "On Site Interview",
                "Offer",
                "Rejected"
            ],
            "isDefault": true
        }
    ]
}
```

This endpoint retrieves all board templates for your organization.

### HTTP Request

`GET https://api.huntr.co/org/board-templates`

# Jobs

## Job Resource

> A job in one of your members' boards:

```json
{
    "id": "5eb0a4338899ba51bf96f645",
    "title": "Senior Engineer",
    "htmlDescription": "<p><strong>Description</strong></p><p>Great opportunity</p>",
    "salary": "$90k",
    "url": "https://linkedin.com/job/123",
    "location": {
        "address": "2015 Main St, Vancouver, BC V5T 3C2, Canada",
        "name": "Mailchimp",
        "placeId": "ChIJG7btXnJxhlQR62EUZuSVsOE",
        "id": "ceb4675ecb0f218b4adf86d215eeffbcb12f5f1f",
        "url": "https://maps.google.com/?cid=16262663062632554987",
        "lat": "49.2668893",
        "lng": "-123.1012246"
    },
    "createdAt": 1588634675,
    "ownerMember": {
        "id": "5e6a7bbdc56bc6669e3da86c",
        "givenName": "Ruipo",
        "familyName": "Fauli",
        "email": "rennie+thinkful20@huntr.co",
        "createdAt": 1584036797,
        "isActive": true
    },
    "creatorMember": {
        "id": "5b2bbb5837fbc835bbe3218b",
        "givenName": "Advisor",
        "familyName": "Two",
        "email": "rennie+advisor2@huntr.co",
        "createdAt": 1529592664,
        "isActive": true
    },
    "employer": {
        "id": "5a016b5221bce9aa2bd70296",
        "name": "MailChimp",
        "domain": "mailchimp.com",
        "foundedYear": 2001,
        "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
        "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
    },
    "list": {
        "name": "Applied"
    },
    "activities": [
        {
            "id": "5eb0abc13fac1580adcf5b19",
            "title": "Follow Up",
            "note": "With pete",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588809403,
            "createdAt": 1588636609
        },
        {
            "id": "5eb0cbafacf21d6980c45d64",
            "title": "Apply",
            "completed": true,
            "createdByWorkflow": false,
            "startAt": 1588748400,
            "completedAt": 1588644783,
            "createdAt": 1588644783
        },
        {
            "id": "5eb0cbb2acf21d6980c45d66",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588834800,
            "createdAt": 1588644786
        },
        {
            "id": "5eb0cbb6acf21d6980c45d68",
            "title": "Offer Received",
            "completed": true,
            "createdByWorkflow": false,
            "startAt": 1590044400,
            "completedAt": 1588644790,
            "createdAt": 1588644790
        },
        {
            "id": "5eb0cbbaacf21d6980c45d6a",
            "title": "Accept Offer",
            "completed": true,
            "createdByWorkflow": false,
            "startAt": 1590562800,
            "completedAt": 1588644794,
            "createdAt": 1588644794
        },
        {
            "id": "5eb0cebdf9e9ac821876aab0",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1589353200,
            "createdAt": 1588645565
        }
    ],
    "interviewActivities": [
        {
            "id": "5eb0cbb2acf21d6980c45d66",
            "title": "Phone interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588834800,
            "createdAt": 1588644786
        },
        {
            "id": "5eb0cebdf9e9ac821876aab0",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1589353200,
            "createdAt": 1588645565
        }
    ],
    "applicationActivity": {
        "id": "5eb0cbafacf21d6980c45d64",
        "title": "Apply",
        "completed": true,
        "createdByWorkflow": false,
        "startAt": 1588748400,
        "completedAt": 1588644783,
        "createdAt": 1588644783
    },
    "offerActivity": {
        "id": "5eb0cbb6acf21d6980c45d68",
        "title": "Offer Received",
        "completed": true,
        "createdByWorkflow": false,
        "startAt": 1590044400,
        "completedAt": 1588644790,
        "createdAt": 1588644790
    },
    "offerAcceptedActivity": {
        "id": "5eb0cbbaacf21d6980c45d6a",
        "title": "Accept Offer",
        "completed": true,
        "createdByWorkflow": false,
        "startAt": 1590562800,
        "completedAt": 1588644794,
        "createdAt": 1588644794
    }
}
```

A job is an instance of a job opportunity that one of your members has added to one of their boards managed by your organization. Each job card in the Huntr UI represent a different Job.

Field | Type | Description
----- | ---- | -----------
`id` | String | Job's id
`title` | String | Job title  
`description` | String | Job description
`htmlDescription` | String | Html Job description
`description` | String | Job description
`salary` | String | Job salary  
`url` | String | URL for the original job post
`location` | Object | Object representing job's location
`deadline` | Unix timestamp | Date of job's application deadline
`activities` | Array | [Activities](#activities) linked to job
`applicationActivity` | Object | [Application Activity](#activities) linked to job
`interviewActivities` | Array | [Interview Activities](#activities) linked to job
`offerActivity` | Object | [Offer Received Activity](#activities) linked to job
`offerAcceptedActivity` | Object | [Offer Accepted Activity](#activities) linked to job
`creatorMember` | Object | [Member](#members) who created the job
`ownerMember` | Object | [Member](#members) who owns the job
`employer`  | Object | [Employer](#employers) for the job
`list`  | Object | Object representing the list the job is in. A list is a stage in a specific Huntr board.
`createdAt` | Unix timestamp | Date the job was created by the member

### Deprecated job fields

These fields will be supported until the end of 2020. See the **Replacement field** column to see what field is replacing the deprecated fields.

Field | Type | Replacement field | Description
----- | ---- | ----------------- | -----------
`applicationDate` | Unix timestamp | `applicationActivity` | Date of application deadline
`firstInterviewDate` | Unix timestamp | `interviewActivities` | Date of first interview
`secondInterviewDate` | Unix timestamp | `interviewActivities` | Date of second interview
`offerDate` | Unix timestamp | `offerActivity` | Date offer was received
`member` | Object | `ownerMember` | Object representing the member who owns the job

## List Jobs

```shell
curl "https://api.huntr.co/org/jobs?limit=2"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "5eb0a4338899ba51bf96f645",
            "title": "Software Developer",
            "htmlDescription": "<p><strong>Description</strong></p><p>Great job!</p>",
            "salary": "$90k",
            "url": "google.com",
            "location": {
                "address": "2015 Main St, Vancouver, BC V5T 3C2, Canada",
                "name": "Mailchimp",
                "placeId": "ChIJG7btXnJxhlQR62EUZuSVsOE",
                "id": "ceb4675ecb0f218b4adf86d215eeffbcb12f5f1f",
                "url": "https://maps.google.com/?cid=16262663062632554987",
                "lat": "49.2668893",
                "lng": "-123.1012246"
            },
            "createdAt": 1588634675,
            "ownerMember": {
                "id": "5e6a7bbdc56bc6669e3da86c",
                "givenName": "Ruipo",
                "familyName": "Fauli",
                "email": "rennie+thinkful20@huntr.co",
                "createdAt": 1584036797,
                "isActive": true
            },
            "creatorMember": {
                "id": "5b2bbb5837fbc835bbe3218b",
                "givenName": "Advisor",
                "familyName": "Two",
                "email": "rennie+advisor2@huntr.co",
                "createdAt": 1529592664,
                "isActive": true
            },
            "employer": {
                "id": "5a016b5221bce9aa2bd70296",
                "name": "MailChimp",
                "domain": "mailchimp.com",
                "foundedYear": 2001,
                "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
                "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
            },
            "list": {
                "name": "Applied"
            },
            "activities": [
                {
                    "id": "5eb0abc13fac1580adcf5b19",
                    "title": "Follow Up",
                    "note": "With pete",
                    "completed": false,
                    "createdByWorkflow": false,
                    "startAt": 1588809403,
                    "createdAt": 1588636609
                },
                {
                    "id": "5eb0cbafacf21d6980c45d64",
                    "title": "Apply",
                    "completed": true,
                    "createdByWorkflow": false,
                    "startAt": 1588748400,
                    "completedAt": 1588644783,
                    "createdAt": 1588644783
                },
                {
                    "id": "5eb0cbb2acf21d6980c45d66",
                    "title": "On Site Interview",
                    "completed": false,
                    "createdByWorkflow": false,
                    "startAt": 1588834800,
                    "createdAt": 1588644786
                },
                {
                    "id": "5eb0cbb6acf21d6980c45d68",
                    "title": "Offer Received",
                    "completed": true,
                    "createdByWorkflow": false,
                    "startAt": 1590044400,
                    "completedAt": 1588644790,
                    "createdAt": 1588644790
                },
                {
                    "id": "5eb0cbbaacf21d6980c45d6a",
                    "title": "Accept Offer",
                    "completed": true,
                    "createdByWorkflow": false,
                    "startAt": 1590562800,
                    "completedAt": 1588644794,
                    "createdAt": 1588644794
                },
                {
                    "id": "5eb0cebdf9e9ac821876aab0",
                    "title": "On Site Interview",
                    "completed": false,
                    "createdByWorkflow": false,
                    "startAt": 1589353200,
                    "createdAt": 1588645565
                }
            ],
            "interviewActivities": [
                {
                    "id": "5eb0cbb2acf21d6980c45d66",
                    "title": "On Site Interview",
                    "completed": false,
                    "createdByWorkflow": false,
                    "startAt": 1588834800,
                    "createdAt": 1588644786
                },
                {
                    "id": "5eb0cebdf9e9ac821876aab0",
                    "title": "On Site Interview",
                    "completed": false,
                    "createdByWorkflow": false,
                    "startAt": 1589353200,
                    "createdAt": 1588645565
                }
            ],
            "applicationActivity": {
                "id": "5eb0cbafacf21d6980c45d64",
                "title": "Apply",
                "completed": true,
                "createdByWorkflow": false,
                "startAt": 1588748400,
                "completedAt": 1588644783,
                "createdAt": 1588644783
            },
            "offerActivity": {
                "id": "5eb0cbb6acf21d6980c45d68",
                "title": "Offer Received",
                "completed": true,
                "createdByWorkflow": false,
                "startAt": 1590044400,
                "completedAt": 1588644790,
                "createdAt": 1588644790
            },
            "offerAcceptedActivity": {
                "id": "5eb0cbbaacf21d6980c45d6a",
                "title": "Accept Offer",
                "completed": true,
                "createdByWorkflow": false,
                "startAt": 1590562800,
                "completedAt": 1588644794,
                "createdAt": 1588644794
            }
        },
        {
            "id": "5eaf7f8e7d33a823fd5a06c1",
            "title": "Front end engineer",
            "location": {},
            "createdAt": 1588559758,
            "ownerMember": {
                "id": "5eae15c3a4e9196db88cb394",
                "givenName": "Rennie",
                "familyName": "Haylock",
                "email": "rennie+dev30@huntr.co",
                "createdAt": 1588467139,
                "isActive": true
            },
            "creatorMember": {
                "id": "5b2bbb5837fbc835bbe3218b",
                "givenName": "Advisor",
                "familyName": "Two",
                "email": "rennie+advisor2@huntr.co",
                "createdAt": 1529592664,
                "isActive": true
            },
            "employer": {
                "id": "5903fe3a0ceb436653b2bf7d",
                "name": "ESPN",
                "domain": "espn.com",
                "description": "Visit ESPN to get up-to-the-minute sports news coverage, scores, highlights and commentary for NFL, MLB, NBA, College Football, NCAA Basketball and more.",
                "location": "935 Middle St, Bristol, CT 06010, USA"
            },
            "list": {
                "name": "offer"
            },
            "activities": [
                {
                    "id": "5eaf7fa5f0cf6b54208282be",
                    "title": "Offer received",
                    "completed": true,
                    "createdByWorkflow": true,
                    "startAt": 1588559758,
                    "completedAt": 1588559781,
                    "createdAt": 1588559781
                }
            ],
            "offerActivity": {
                "id": "5eaf7fa5f0cf6b54208282be",
                "title": "Offer received",
                "completed": true,
                "createdByWorkflow": true,
                "startAt": 1588559758,
                "completedAt": 1588559781,
                "createdAt": 1588559781
            }
        }
    ],
    "next": "5eaf7f8e7d33a823fd5a06c1"
}
```

This endpoint lists all jobs added by members from your organization, jobs are sorted by the date they were created, starting from the most recently created.

### HTTP Request

`GET https://api.huntr.co/org/jobs`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`owner_member_id` | String | none | If set, will only return jobs owned by member
`creator_member_id` | String | none | If set, will only return jobs created by member
`created_after` | Unix timestamp | none | If set, will only return jobs created after `created_after`
`created_before` | Unix timestamp | none | If set, will only return jobs created before `created_before`
`limit` | Integer | 100 | Defines the maximum number of jobs to return
`next` | String | none | If set, returns the next set of results after the object id provided

## Retrieve a Job

```shell
curl "https://api.huntr.co/org/jobs/5eb0a4338899ba51bf96f645"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "id": "5eb0a4338899ba51bf96f645",
    "title": "Software Engineer",
    "htmlDescription": "<p><strong>Description</strong></p><p>Great job!</p>",
    "salary": "$90k",
    "url": "google.com",
    "location": {
        "address": "2015 Main St, Vancouver, BC V5T 3C2, Canada",
        "name": "Mailchimp",
        "placeId": "ChIJG7btXnJxhlQR62EUZuSVsOE",
        "id": "ceb4675ecb0f218b4adf86d215eeffbcb12f5f1f",
        "url": "https://maps.google.com/?cid=16262663062632554987",
        "lat": "49.2668893",
        "lng": "-123.1012246"
    },
    "createdAt": 1588634675,
    "ownerMember": {
        "id": "5e6a7bbdc56bc6669e3da86c",
        "givenName": "Ruipo",
        "familyName": "Fauli",
        "email": "rennie+thinkful20@huntr.co",
        "createdAt": 1584036797,
        "isActive": true
    },
    "creatorMember": {
        "id": "5b2bbb5837fbc835bbe3218b",
        "givenName": "Advisor",
        "familyName": "Two",
        "email": "rennie+advisor2@huntr.co",
        "createdAt": 1529592664,
        "isActive": true
    },
    "employer": {
        "id": "5a016b5221bce9aa2bd70296",
        "name": "MailChimp",
        "domain": "mailchimp.com",
        "foundedYear": 2001,
        "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
        "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
    },
    "list": {
        "name": "Applied"
    },
    "activities": [
        {
            "id": "5eb0abc13fac1580adcf5b19",
            "title": "Follow Up",
            "note": "With pete",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588809403,
            "createdAt": 1588636609
        },
        {
            "id": "5eb0cbafacf21d6980c45d64",
            "title": "Apply",
            "completed": true,
            "createdByWorkflow": false,
            "startAt": 1588748400,
            "completedAt": 1588644783,
            "createdAt": 1588644783
        },
        {
            "id": "5eb0cbb2acf21d6980c45d66",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588834800,
            "createdAt": 1588644786
        },
        {
            "id": "5eb0cbb6acf21d6980c45d68",
            "title": "Offer Received",
            "completed": true,
            "createdByWorkflow": false,
            "startAt": 1590044400,
            "completedAt": 1588644790,
            "createdAt": 1588644790
        },
        {
            "id": "5eb0cbbaacf21d6980c45d6a",
            "title": "Accept Offer",
            "completed": true,
            "createdByWorkflow": false,
            "startAt": 1590562800,
            "completedAt": 1588644794,
            "createdAt": 1588644794
        },
        {
            "id": "5eb0cebdf9e9ac821876aab0",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1589353200,
            "createdAt": 1588645565
        }
    ],
    "interviewActivities": [
        {
            "id": "5eb0cbb2acf21d6980c45d66",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588834800,
            "createdAt": 1588644786
        },
        {
            "id": "5eb0cebdf9e9ac821876aab0",
            "title": "On Site Interview",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1589353200,
            "createdAt": 1588645565
        }
    ],
    "applicationActivity": {
        "id": "5eb0cbafacf21d6980c45d64",
        "title": "Apply",
        "completed": true,
        "createdByWorkflow": false,
        "startAt": 1588748400,
        "completedAt": 1588644783,
        "createdAt": 1588644783
    },
    "offerActivity": {
        "id": "5eb0cbb6acf21d6980c45d68",
        "title": "Offer Received",
        "completed": true,
        "createdByWorkflow": false,
        "startAt": 1590044400,
        "completedAt": 1588644790,
        "createdAt": 1588644790
    },
    "offerAcceptedActivity": {
        "id": "5eb0cbbaacf21d6980c45d6a",
        "title": "Accept Offer",
        "completed": true,
        "createdByWorkflow": false,
        "startAt": 1590562800,
        "completedAt": 1588644794,
        "createdAt": 1588644794
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

# Activities

## Activity Resource

> An activity object:

```json
{
  "id": "5eb0abc13fac1580adcf5b19",
  "title": "Follow Up",
  "note": "",
  "completed": false,
  "createdByWorkflow": false,
  "startAt": 1588809403,
  "createdAt": 1588636609,
  "ownerMember": {
      "id": "5e6a7bbdc56bc6669e3da86c",
      "givenName": "Ruipo",
      "familyName": "Fauli",
      "email": "rennie+thinkful20@huntr.co",
      "createdAt": 1584036797,
      "isActive": true
  },
  "creatorMember": {
      "id": "5b2bbb5837fbc835bbe3218b",
      "givenName": "Advisor",
      "familyName": "Two",
      "email": "rennie+advisor2@huntr.co",
      "createdAt": 1529592664,
      "isActive": true
  },
  "job": {
      "id": "5eb0a4338899ba51bf96f645",
      "title": "Software Engineer",
      "location": {},
      "createdAt": 1588634675
  },
  "employer": {
      "id": "5a016b5221bce9aa2bd70296",
      "name": "MailChimp",
      "domain": "mailchimp.com",
      "foundedYear": 2001,
      "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
      "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
  },
  "activityCategory": {
      "id": "5e448f3bf2b856c29af1128c",
      "name": "Follow Up"
  }
}
```

An activity represents any task and/or event that happens during a job seeker's search. Activities represent real world things like an `On Site Interview` or an `Offer Received`, or even a `Prep for Interview` task. Each activity is clearly classified into an **[Activity Category](#activity-categories)**.

Field | Type | In all events | Description
----- | ---- | ------------- | -----------
`id` | String | true | id for the activity
`title` | String | true | Title of the activity
`note` | String | false | Note about activity
`completed` | Boolean | true | Completion state of activity
`completedAt` | Unix timestamp | false | If `activity.completed` is true, then this is the date when activity was completed
`startAt` | Unix timestamp | false | Date when activity is/was scheduled to start
`endAt` | Unix timestamp | false | Date when activity is/was scheduled to end
`createdAt` | Unix timestamp | true | Date when activity was created
`activityCategory` | Object | true |  **[Activity Category](#activity-categories)** representing the type of Activity
`job` | Object | false |  Job linked to activity
`employer` | Object | false |  If activity is linked to a job, then this is the job's employer
`ownerMember` | Object | true | Member who owns the board where the activity was added. i.e: your job seeker
`creatorMember` | Object | false | Member who created the activity. In most cases, this is your job seeker; but if an advisor created this activity, then it would be your advisor.
`createdByWorkflow` | Boolean | true | If true, it means the Activity was created by the Huntr system as a response to a user action; like a job being moved to a specific stage. See [Workflows](#workflows) for more information

## List Activities

```shell
curl "https://api.huntr.co/org/activities?limit=4"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "5eb0a92378ddef71d0748fb9",
            "title": "Prep for Interviews",
            "note": "",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588635936,
            "createdAt": 1588635939,
            "ownerMember": {
                "id": "5b2bbb5837fbc835bbe32187",
                "givenName": "Nicole",
                "familyName": "Thinkful",
                "email": "rennie+nicoleh@huntr.co",
                "createdAt": 1529592664,
                "isActive": true
            },
            "creatorMember": {
                "id": "5b2bbb5837fbc835bbe3218b",
                "givenName": "Advisor",
                "familyName": "Two",
                "email": "rennie+advisor2@huntr.co",
                "createdAt": 1529592664,
                "isActive": true
            },
            "activityCategory": {
                "id": "5e448f3bf2b856c29af112a7",
                "name": "Prep for Interview"
            }
        },
        {
            "id": "5eb0a4745125d758466dea60",
            "title": "Applied",
            "completed": true,
            "createdByWorkflow": true,
            "startAt": 1588634685,
            "completedAt": 1588634740,
            "createdAt": 1588634740,
            "ownerMember": {
                "id": "5e6a7bbdc56bc6669e3da86c",
                "givenName": "Ruipo",
                "familyName": "Fauli",
                "email": "rennie+thinkful20@huntr.co",
                "createdAt": 1584036797,
                "isActive": true
            },
            "job": {
                "id": "5eb0a4338899ba51bf96f645",
                "title": "No Job Title",
                "location": {},
                "createdAt": 1588634675
            },
            "employer": {
                "id": "5a016b5221bce9aa2bd70296",
                "name": "MailChimp",
                "domain": "mailchimp.com",
                "foundedYear": 2001,
                "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
                "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
            },
            "activityCategory": {
                "id": "5e448f3bf2b856c29af11289",
                "name": "Apply"
            }
        },
        {
            "id": "5eb0a4058899ba51bf96f643",
            "title": "Got to Networking Event",
            "note": "",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588634619,
            "createdAt": 1588634629,
            "ownerMember": {
                "id": "5e6a7bbdc56bc6669e3da86c",
                "givenName": "Ruipo",
                "familyName": "Fauli",
                "email": "rennie+thinkful20@huntr.co",
                "createdAt": 1584036797,
                "isActive": true
            },
            "creatorMember": {
                "id": "5b2bbb5837fbc835bbe3218b",
                "givenName": "Advisor",
                "familyName": "Two",
                "email": "rennie+advisor2@huntr.co",
                "createdAt": 1529592664,
                "isActive": true
            },
            "activityCategory": {
                "id": "5e448f3bf2b856c29af112b3",
                "name": "Networking Event"
            }
        },
        {
            "id": "5eaf7fa5f0cf6b54208282be",
            "title": "Offer received",
            "completed": true,
            "createdByWorkflow": true,
            "startAt": 1588559758,
            "completedAt": 1588559781,
            "createdAt": 1588559781,
            "ownerMember": {
                "id": "5eae15c3a4e9196db88cb394",
                "givenName": "Rennie",
                "familyName": "Haylock",
                "email": "rennie+dev30@huntr.co",
                "createdAt": 1588467139,
                "isActive": true
            },
            "job": {
                "id": "5eaf7f8e7d33a823fd5a06c1",
                "title": "No Job Title",
                "location": {},
                "createdAt": 1588559758
            },
            "employer": {
                "id": "5903fe3a0ceb436653b2bf7d",
                "name": "ESPN",
                "domain": "espn.com",
                "description": "Visit ESPN to get up-to-the-minute sports news coverage, scores, highlights and commentary for NFL, MLB, NBA, College Football, NCAA Basketball and more.",
                "location": "935 Middle St, Bristol, CT 06010, USA"
            },
            "activityCategory": {
                "id": "5e448f3bf2b856c29af1129c",
                "name": "Offer Received"
            }
        }
    ],
    "next": "5eaf7fa5f0cf6b54208282be"
}
```

This endpoint retrieves all activities for your organization, sorted from most recently created to oldest.

### HTTP Request

`GET https://api.huntr.co/org/activities`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`owner_member_id` | String | none | If set, will only return activities belonging to member
`creator_member_id` | String | none | If set, will only return activities executed by member
`job_id` | String | none | If set, will only return activities linked to job
`activity_category_id` | String | none | If set, will only return Activity activities of the specified activity category
`employer_id` | String | none | If set, will only return activities linked to employer
`limit` | Integer | 100 | Defines the maximum number of activities to return
`next` | String | none | If set, returns the next set of results after the object id provided
`created_after` | Unix timestamp | none | If set, will only return activities created after `created_after`
`created_before` | Unix timestamp | none | If set, will only return activities created before `created_before`

# Activity Categories

## Activity Category Resource

> An activity category object:

```json
{
    "id": "5b05ca43f193b66c692c29a0",
    "name": "On Site Interview",
}
```

This resource classifies a specific instance of an **[Activity](#activities)** into one of a discrete set of Activity Categories.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the activity category
`name` | String | none | name for the activity category

## Types of Activity Categories

### This is a list of all the activity categories currently available.

`activityCategory.name` | Description
----------------------- | -----------
`Email` | Job seeker needs to send an email
`Meeting` | Job seeker is attending a meeting
`Phone Call` | Job seeker needs to make a phone call
`Reach Out` | Job seeker needs to reach out to someone
`Get Reference` | Job seeker needs to get a reference(s)
`Prep Cover Letter` | Job seeker needs to prepare their cover letter
`Prep Resume` | Job seeker needs to prepare their resume
`Apply` | Job seeker has applied to a job opportunity
`Follow Up` | Job seeker needs to follow up with someone
`Send Availability` | Job seeker needs to send their availability
`Prep for Interview` | Job seeker needs to prepare for an interview
`Phone Screen` | Job seeker has or completed a phone screen type interview
`Phone Interview` | Job seeker has or completed a phone interview
`Assignment` | Job seeker has or has completed an assignment
`On Site Interview` | Job seeker has or completed an on site interview
`Rejected` | Job seeker's application has been rejected by the employer
`Offer Received` | Job seeker has received an offer
`Decline Offer` | Job seeker declines an offer
`Accept Offer` | Job seeker accepts an offer
`Send Thank You` | Job seeker needs to send a thank you
`Networking Event` | Job seeker needs to or has attended a networking event
`Application Withdrawn` | Job seeker has withdrawn their application
`Other` | Other activities

## Key Activity Categories

Here is a shorter list of activity categories we believe represent important milestones in a job seeker's search:

`activityCategory.name` | Description
----------------------- | -----------
`Apply` | Job seeker has applied to a job opportunity
`Phone Screen` | Job seeker has scheduled or completed a phone screen type interview
`Phone Interview` | Job seeker has scheduled or completed a phone interview
`Assignment` | Job seeker has scheduled or completed an assignment
`On Site Interview` | Job seeker has scheduled or completed an on site interview
`Rejected` | Job seeker's application has been rejected by the employer
`Offer Received` | Job seeker has received an offer
`Decline Offer` | Job seeker has declined an offer
`Accept Offer` | Job seeker has accepted an offer
`Application Withdrawn` | Job seeker has withdrawn their application

## List Activity Categories

```shell
curl "https://api.huntr.co/org/activity-categories"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
        "name" : "Email",
        "id" : "5e448e8df2b856c29af11181"
    },
    {
        "name" : "Meeting",
        "id" : "5e448f3bf2b856c29af11280"
    },
    {
        "name" : "Phone Call",
        "id" : "5e448f3bf2b856c29af11281"
    },
    {
        "name" : "Reach Out",
        "id" : "5e448f3bf2b856c29af11284"
    },
    {
        "name" : "Get Reference",
        "id" : "5e448f3bf2b856c29af11285"
    },
    {
        "name" : "Prep Cover Letter",
        "id" : "5e448f3bf2b856c29af11288"
    },
    {
        "name" : "Apply",
        "id" : "5e448f3bf2b856c29af11289"
    },
    {
        "name" : "Follow Up",
        "id" : "5e448f3bf2b856c29af1128c"
    },
    {
        "name" : "Send Availability",
        "id" : "5e448f3bf2b856c29af1128d"
    },
    {
        "name" : "Phone Screen",
        "id" : "5e448f3bf2b856c29af1128f"
    },
    {
        "name" : "Phone Interview",
        "id" : "5e448f3bf2b856c29af11293"
    },
    {
        "name" : "Assignment",
        "id" : "5e448f3bf2b856c29af11295"
    },
    {
        "name" : "On Site Interview",
        "id" : "5e448f3bf2b856c29af11296"
    },
    {
        "name" : "Rejected",
        "id" : "5e448f3bf2b856c29af11299"
    },
    {
        "name" : "Offer Received",
        "id" : "5e448f3bf2b856c29af1129c"
    },
    {
        "name" : "Prep Resume",
        "id" : "5e448f3bf2b856c29af1129d"
    },
    {
        "name" : "Decline Offer",
        "id" : "5e448f3bf2b856c29af1129f"
    },
    {
        "name" : "Accept Offer",
        "id" : "5e448f3bf2b856c29af112a1"
    },
    {
        "name" : "Other",
        "id" : "5e448f3bf2b856c29af112a3"
    },
    {
        "name" : "Prep for Interview",
        "id" : "5e448f3bf2b856c29af112a7"
    },
    {
        "name" : "Send Thank You",
        "id" : "5e448f3bf2b856c29af112ad"
    },
    {
        "name" : "Networking Event",
        "id" : "5e448f3bf2b856c29af112b3"
    },
    {
        "name" : "Application Withdrawn",
        "id" : "5e9a596e53a63660289cd97c"
    }
  ]
}
```

This endpoint retrieves all activity categories.

# Actions

## Action Resource

> An action object:

```json
{
  "id": "5eb0a4338899ba51bf96f646",
  "actionType": "JOB_CREATED",
  "date": 1588634675,
  "ownerMember": {
      "id": "5e6a7bbdc56bc6669e3da86c",
      "givenName": "Ruipo",
      "familyName": "Fauli",
      "email": "rennie+thinkful20@huntr.co",
      "createdAt": 1584036797,
      "isActive": true
  },
  "creatorMember": {
      "id": "5b2bbb5837fbc835bbe3218b",
      "givenName": "Advisor",
      "familyName": "Two",
      "email": "rennie+advisor2@huntr.co",
      "createdAt": 1529592664,
      "isActive": true
  },
  "job": {
      "id": "5eb0a4338899ba51bf96f645",
      "title": "No Job Title",
      "salary": "$90k",
      "location": {},
      "createdAt": 1588634675
  },
  "employer": {
      "id": "5a016b5221bce9aa2bd70296",
      "name": "MailChimp",
      "domain": "mailchimp.com",
      "foundedYear": 2001,
      "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
      "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
  },
  "toList": {
      "name": "Wishlist"
  },
  "createdByWorkflow": false,
  "createdAt": 1588634675,
  "jobId": "5eb0a4338899ba51bf96f645"
}
```

An action represents CRUD (Create, read, update and delete) type events that
occur within the Huntr application in the context of your organization. Essentially,
whenever your job seekers or advisors create, update or delete a Job, Activity, Contact,
or Note a new Action will be created representing that event:

Field | Type | In all actions | Description
----- | ---- | ------------- | -----------
`id` | String | true | id for the action
`actionType` | String | true | Type of action
`date` | Unix timestamp | true | Date when the action happened
`createdByWorkflow` | Boolean | true | If true, it means the Action was taken by the Huntr system as a response to another user action; like a job being moved to a specific stage. See [Workflows](#workflows) for more information.
`ownerMember` | Object | true | Member who owns the board where the action occurred. i.e: your job seeker
`creatorMember` | Object | true | Member who took the action. In most cases, this is your job seeker; but if an advisor took the action, then it would be your advisor.
`jobId` | String | false |  id for job linked to this action
`job` | Object | false |  [Job](#jobs) linked to this action
`jobs` | Array | false |  [Jobs](#jobs) linked to this action; only applies to Contact actions, since a contact can be linked to multiple jobs
`employer` | Object | false |  [Employer](#employers) linked to this action
`employers` | Array | false |  [Employers](#employers) linked to this action; only applies to Contact actions since a contact can be linked to multiple employers
`activityId` | String | false |  id for Activity linked to this action
`activity` | Object | false |  [Activity](#activities) linked to this action; only applies to Activity actions
`activityCategory` | Object | false |  [Activity Category](#activity-categories) linked to this action; only applies to Activity actions
`noteId` | String | false |  id for note linked to this action
`note` | Object | false |  [Note](#notes) linked to this action; only applies to Note actions
`contactId` | String | false |  id for contact linked to this action
`contact` | Object | false |  [Contact](#contacts) linked to this action; only applies to Contact actions
`fromList` | Object | false | List from which job was moved in a `JOB_MOVED` action
`toList` | Object | false | List to which a job was moved or added to in `JOB_MOVED` and `JOB_CREATED` actions
`update` | Object | false | Entry used to update the resource, only shown in Update actions

## Action Types

There are multiple action types:

Action Type | Description | Extra fields
---------- | ----------- | -----------
`JOB_CREATED` | A new job is created | `job` `employer` `toList`
`JOB_UPDATED` | A job is updated | `job` `employer` `update`
`JOB_DELETED` | A job is deleted | `jobId` `employer`
`JOB_MOVED` | A job is moved from one stage to another | `job` `employer` `fromList` `toList`
`ACTIVITY_CREATED` | A new activity is created | `activity` `activityCategory` `job` `employer`
`ACTIVITY_UPDATED` | An activity is updated | `activity` `activityCategory` `job` `employer` `update`
`ACTIVITY_DELETED` | An activity is deleted | `activityId` `activityCategory` `job` `employer`
`NOTE_CREATED` | A new note is created | `note` `job` `employer`
`NOTE_UPDATED` | A note is updated | `note` `job` `employer`
`NOTE_DELETED` |  A note is deleted | `noteId` `job` `employer`
`CONTACT_CREATED` |  A new contact is created | `contact` `employers` `jobs`
`CONTACT_UPDATED` |  A contact is updated | `contact` `employers` `jobs` `update`
`CONTACT_DELETED` |  A contact is deleted | `contactId`


## List Actions

```shell
curl "https://api.huntr.co/org/actions?limit=3"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
        "id": "5eb0abc13fac1580adcf5b1a",
        "actionType": "ACTIVITY_CREATED",
        "date": 1588636609,
        "ownerMember": {
            "id": "5e6a7bbdc56bc6669e3da86c",
            "givenName": "Ruipo",
            "familyName": "Fauli",
            "email": "rennie+thinkful20@huntr.co",
            "createdAt": 1584036797,
            "isActive": true
        },
        "creatorMember": {
            "id": "5b2bbb5837fbc835bbe3218b",
            "givenName": "Advisor",
            "familyName": "Two",
            "email": "rennie+advisor2@huntr.co",
            "createdAt": 1529592664,
            "isActive": true
        },
        "job": {
            "id": "5eb0a4338899ba51bf96f645",
            "title": "No Job Title",
            "salary": "$90k",
            "location": {},
            "createdAt": 1588634675
        },
        "employer": {
            "id": "5a016b5221bce9aa2bd70296",
            "name": "MailChimp",
            "domain": "mailchimp.com",
            "foundedYear": 2001,
            "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
            "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
        },
        "activity": {
            "id": "5eb0abc13fac1580adcf5b19",
            "title": "Follow Up",
            "note": "With pete",
            "completed": false,
            "createdByWorkflow": false,
            "startAt": 1588809403,
            "createdAt": 1588636609
        },
        "activityCategory": {
            "id": "5e448f3bf2b856c29af1128c",
            "name": "Follow Up"
        },
        "createdByWorkflow": false,
        "createdAt": 1588636609,
        "jobId": "5eb0a4338899ba51bf96f645",
        "activityId": "5eb0abc13fac1580adcf5b19"
    },
    {
        "id": "5eb0a43d8899ba51bf96f647",
        "actionType": "JOB_MOVED",
        "date": 1588634685,
        "ownerMember": {
            "id": "5e6a7bbdc56bc6669e3da86c",
            "givenName": "Ruipo",
            "familyName": "Fauli",
            "email": "rennie+thinkful20@huntr.co",
            "createdAt": 1584036797,
            "isActive": true
        },
        "creatorMember": {
            "id": "5b2bbb5837fbc835bbe3218b",
            "givenName": "Advisor",
            "familyName": "Two",
            "email": "rennie+advisor2@huntr.co",
            "createdAt": 1529592664,
            "isActive": true
        },
        "job": {
            "id": "5eb0a4338899ba51bf96f645",
            "title": "No Job Title",
            "salary": "$90k",
            "location": {},
            "createdAt": 1588634675
        },
        "employer": {
            "id": "5a016b5221bce9aa2bd70296",
            "name": "MailChimp",
            "domain": "mailchimp.com",
            "foundedYear": 2001,
            "description": "MailChimp provides marketing automation for e-commerce businesses. Send beautiful emails, connect your e-commerce store, advertise, and build your brand.\n",
            "location": "675 Ponce De Leon Ave NE #5000, Atlanta, GA 30308, USA"
        },
        "fromList": {
            "name": "Wishlist"
        },
        "toList": {
            "name": "Applied"
        },
        "createdByWorkflow": false,
        "createdAt": 1588634685,
        "jobId": "5eb0a4338899ba51bf96f645"
    },
    {
        "id": "5eae1460180a05fbb45f73af",
        "actionType": "NOTE_CREATED",
        "date": 1588466784,
        "ownerMember": {
            "id": "5eae15c3a4e9196db88cb394",
            "givenName": "Rennie",
            "familyName": "Haylock",
            "email": "rennie+dev30@huntr.co",
            "createdAt": 1588467139,
            "isActive": true
        },
        "creatorMember": {
            "id": "5eae15c3a4e9196db88cb394",
            "givenName": "Rennie",
            "familyName": "Haylock",
            "email": "rennie+dev30@huntr.co",
            "createdAt": 1588467139,
            "isActive": true
        },
        "job": {
            "id": "5d7980458a3fd22bfe291f38",
            "title": "No Job Title",
            "url": "http://localhost:3000/app/track/boards/5cc7ab1f5073b727a3862719/board",
            "location": {
                "address": ""
            },
            "createdAt": 1568243781
        },
        "employer": {
            "id": "5d798045ad35fc0158480b95",
            "name": "The San Diego Foundation",
            "domain": "sdfoundation.org",
            "foundedYear": 1975,
            "description": "The San Diego Foundation is a community resource that improves San Diegans’ quality of life by creating equity and ensuring opportunities in the region.",
            "location": "2508 Historic Decatur Rd #200, San Diego, CA 92106, USA"
        },
        "note": {
            "id": "5eae14603eabef4cb322078e",
            "text": "Great Interview"
        },
        "createdByWorkflow": false,
        "createdAt": 1588466784,
        "jobId": "5d7980458a3fd22bfe291f38",
        "noteId": "5eae14603eabef4cb322078e"
    }
  ],
  "next": "5eae1460180a05fbb45f73af"
}
```

This endpoint retrieves all actions for your organization, sorted from most recently created to oldest.

### HTTP Request

`GET https://api.huntr.co/org/actions`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`action_type` | String | none | If set, will only return actions of the specific type. See [action types](#action-types) section for possible action type name values.
`owner_member_id` | String | none | If set, will only return actions belonging to member
`creator_member_id` | String | none | If set, will only return actions executed by member
`job_id` | String | none | If set, will only return actions linked to job
`activity_id` | String | none | If set, will only return actions for the specified activity
`note_id` | String | none | If set, will only return actions for the specified note
`contact_id` | String | none | If set, will only return actions for the specified contact
`activity_category_id` | String | none | If set, will only return Activity actions of the specified activity category
`employer_id` | String | none | If set, will only return actions linked to employer
`limit` | Integer | 100 | Defines the maximum number of actions to return
`next` | String | none | If set, returns the next set of results after the object id provided
`created_after` | Unix timestamp | none | If set, will only return actions created after `created_after`
`created_before` | Unix timestamp | none | If set, will only return actions created before `created_before`

# Notes

## Note Resource

> A note object:

```json
{
    "id": "5b05ca42f193b66c692c29a0",
    "text": "Interview went well. It lasted about 2 hours and was half technical and half culture fit.",
}
```

This resource represents a note.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the note
`text` | String | none | text in the note

# Contacts

## Contact Resource

> A contact object:

```json
{
    "id": "5b05ca43f193b64c692c29a0",
    "firstName": "Pepe",
    "lastName": "Alava",
    "email": "palava@huntr.co",
    "title": "HR Manager",
    "location": "Seattle, WA USA",
    "profiles": {
      "linkedin" : "pepealava",
      "twitter" : "pepealava",
      "googlePlus" : "pepealava",
      "crunchbase" : "pepealava",
      "facebook" : "pepealava",
      "github" : "pepealava",
      "aboutme" : "pepealava",
      "gravatar" : "pepealava"
    }
}
```

This resource represents a contact.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the note
`firstName` | String | none | Contact's first name
`lastName` | String | none | Contact's last name
`email` | String | none | Contact's email
`title` | String | none | Contact's job title
`location` | String | none | Contact's location
`profiles` | Object | none | Contact's social media profile handles

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

# Events (Deprecated)

<aside class="warning">
Starting in May 2020, the Events resource is being replaced by the Actions resource.
</aside>

## Events Deprecation

The Event resource endpoints will work until the end of 2020, but going forward you
should replace all Event calls for Action calls. Actions offer a more robust
architecture encompassing many more things that occur within the Huntr application
(see the **[Action](#actions)** resource for more info).

## Why we are deprecating Events

Our old Event resource mixed CRUD type events like `JOB_ADDED` with real life events like
`JOB_APPLICATION_DATE_SET` and `JOB_OFFER_DATE_SET`. We have addressed this by creating **[Actions](#actions)**,
which exclusively represent CRUD type events that happen within the application.

Things like `JOB_CREATED`, `JOB_DELETED`, `NOTE_CREATED`, `CONTACT_UPDATED` are now **[Actions](#actions)**.

Events that happen in the real world, like `Apply`, `Phone Interview`, `Prep for Interview`,
`Offer Received`, `Offer Accepted` are now represented through the **[Activity](#activities)** resource.
We like to think of these as things you would put on a to do list or a calendar; they map one
to one with the items show on the 'Activities' section of Huntr boards. Each activity
has an Activity Category representing the type of Activity it is (see the **[Activity](#activities)** resource for more info).

Furthermore, the architecture of our old Event resource limited the number of events
that a job seeker could log. There were only two interview fields for example.
With the new **[Activity](#activities)** + **[Action](#actions)** combo, job seekers can log as many interviews or
other activities as they want. It also opens up the possibility to create
custom Activity Categories for your organization, to track any type of activity.

## Migrating from Events to Actions

You can still listen for both CRUD and real world events through the Action resource
endpoint. Since real world events are Activities, they are accounted for in Actions through the
`ACTIVITY_CREATED` action type, which will contain an `activityCategory` field that
will let you know what kind of activity it represents.


### Here is what the old event types would translate to in the new Action resource:

Event Name | `action.actionType` | `action.activityCategory.name`
---------- | ----------- | -----------
`JOB_ADDED` | `JOB_CREATED` | `null`
`JOB_MOVED` | `JOB_MOVED` | `null`
`JOB_APPLICATION_DATE_SET` | `ACTIVITY_CREATED` |  `Apply`
`JOB_FIRST_INTERVIEW_DATE_SET` | `ACTIVITY_CREATED` |  `Phone Interview`
`JOB_SECOND_INTERVIEW_DATE_SET` | `ACTIVITY_CREATED` |  `On Site Interview`
`JOB_OFFER_DATE_SET` | `ACTIVITY_CREATED` |  `Offer Received`


## Events Resource

<aside class="warning">
Starting in May 2020, the Events resource is being replaced by the Actions resource.
</aside>

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

<aside class="warning">
Starting in May 2020, the Events resource is being replaced by the Actions resource.
</aside>

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

# Workflows

## Workflow Resource

A workflow allows Huntr to automatically complete certain Actions when other actions happen within the application. For now, workflows power the automatic creation of certain activities when a job is moved or created into a specific stage. For example, you will notice that if a job seeker moves a job to a stage called "Apply" then Huntr will automatically create an [Activity](#activities) with the `Apply` [Activity Category](#activity-categories).

## Default workflows

Our current default workflows are triggered for ALL Huntr users when jobs are created or moved onto stages with names that imply a specific stage type.

### The default workflows that power automatic activity creation when jobs are moved onto certain stages are the following:

1. `create` **Apply** Activity `when` job is moved or created onto a stage of type **APPLY**
2. `create` **Phone Screen** Activity `when` job is moved or created onto a stage of type **PHONE_SCREEN**
3. `create` **Phone Interview** Activity `when` job is moved or created onto a stage of type **PHONE_INTERVIEW**
4. `create` **On Site Interview** Activity `when` job is moved or created onto a stage of type **ON_SITE_INTERVIEW**
5. `create` **Offer Received** Activity `when` job is moved or created onto a stage of type **OFFER_RECEIVED**
6. `create` **Accept Offer** Activity `when` job is moved or created onto a stage of type **OFFER_ACCEPTED**
7. `create` **Decline Offer** Activity `when` job is moved or created onto a stage of type **OFFER_DECLINED**


## Stage names to type mapping

All of the default workflows mentioned above depend on the `stage type` inferred from the name of the stage a job is being created or moved onto. Here are the stage names that map to each stage type:

*note: stage name matches are not case sensitive, so a stage named 'APply' and another one named 'apply' will both be classified as a stage of type 'APPLY'.

Stage Type | Stage Names
---------- | -----------
`APPLY` | `applied` `apply` `applying` `applied online`
`PHONE_SCREEN` | `phone screen` `screening call` `phone screen(s)` `phone screening` `tech screen` `screening interview` `technical screen` `technical phone screen`
`PHONE_INTERVIEW` | `phone` `phone interview` `video interview` `hr interview` `hr phone interview` `phone interviews`
`ON_SITE_INTERVIEW` | `interview` `on-site` `on-site interview` `onsite` `interviews` `onsite interview` `on site` `on site interview` `final round interview` `interview 2` `interview 1` `2nd interview` `1st interview` `interviewing` `interviews 3+` `technical interview` `personal interview` `virtual interview` `in person interview` `in-person interview` `in-person interview(s)` `interviewed` `final round` `second interview` `entrevistas` `tech interview` `final interview` `live interview` `interview (1st)` `interview (2nd)` `second round` `first interview` `2nd round` `interview 3` `technical` `3rd interview` `additional onsite` `interview scheduled` `1st round` `in-person` `initial interview` `entrevista`
`OFFER_RECEIVED` | `offer` `offers` `offer received` `offer extended` `offered` `offer letter` `ofertas`
`OFFER_ACCEPTED` | `offer accepted` `accepted` `accept` `accept offer`
`OFFER_DECLINED` | `offer declined` `declined` `decline offer` `i rejected` `rejected by me` `i declined` `declined offer` `rejected offer` `reject offer` `refused` `offer rejected`

## Coming soon

The next step is to allow job seekers and organizations to create their own workflows to accomplish things like recurring Activity creation.

# Webhooks

## Receiving Webhooks

Huntr can send webhook events to notify your application any time an [action](#actions) happens within your organization.

You can register webhook URLs in your organization's admin dashboard, under the **Developers** tab that we will notify any time an action happens in your organization. When the action occurs—a new job is created, an activity is updated, etc.—Huntr creates a **[Action](#actions)** object.

The Action object contains all the information about the event that just happened, including the type of action and the data associated with that action. Huntr will send the **Action** via an `HTTP POST` request, to any endpoint URLs that you have defined in your organization's Webhooks settings.

## Responding to a webhook

To acknowledge receipt of a webhook, your endpoint should return a `200` HTTP status code. All other response codes will indicate to Huntr that you did not receive the webhook.

## Webhooks Changelog

### May 2020
Before May 2020, Webhooks were powered by the now deprecated [Event](#events-deprecated) resource. Webhooks are now powered by the [Action](#actions) resource. To make the transition from Event to Action Webhooks easier, Webhook messages will contain all of the old Event fields until the end of 2020, alongside the new fields introduced in the Action resource.
