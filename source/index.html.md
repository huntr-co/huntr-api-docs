---
title: Huntr for Organizations - API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - changelog

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
            "isActive": true,
            "advisor": {
                "id": "64befb78b1ad13706cad0745",
                "fullName": "Alice Advisor",
                "givenName": "Alice",
                "familyName": "Advisor",
                "email": "alice@huntr.co",
                "createdAt": 1666980452,
                "lastSeenAt": 1689792002,
            }
        },
        {
            "id": "5b037898b7f93473edc1c43a",
            "givenName": "Nicole",
            "familyName": "Haylock",
            "email": "nicoleh@huntr.co",
            "createdAt": 1526954136,
            "isActive": false,
            "advisor": {
                "id": "64befb996efb7b393bfba5fb",
                "fullName": "Bob Advisor",
                "givenName": "Bob",
                "familyName": "Advisor",
                "createdAt": 1666980452,
                "lastSeenAt": 1689792002,
                "email": "bob@huntr.co",
            }
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

# Organization Invitations

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
        "listTemplates": [
          {
            "id": "5fc9686050c1de39c8412af7",
            "name": "Wishlist"
          },
          {
            "id": "5fc9686050c1de39c8412af8",
            "name": "Applied"
          },
          {
            "id": "5fc9686050c1de39c8412af9",
            "name": "Phone Interview"
          },
          {
            "id": "5fc9686050c1de39c8412af2",
            "name": "On Site Interview"
          },
          {
            "id": "5fc9686050c1de39c8412afa",
            "name": "Offer"
          },
          {
            "id": "5fc9686050c1de39c8412afa",
            "name": "Rejected"
          }
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
              "listTemplates": [
                {
                  "id": "5fc9686050c1de39c8412af7",
                  "name": "Wishlist"
                },
                {
                  "id": "5fc9686050c1de39c8412af8",
                  "name": "Applied"
                },
                {
                  "id": "5fc9686050c1de39c8412af9",
                  "name": "Phone Interview"
                },
                {
                  "id": "5fc9686050c1de39c8412af2",
                  "name": "On Site Interview"
                },
                {
                  "id": "5fc9686050c1de39c8412afa",
                  "name": "Offer"
                },
                {
                  "id": "5fc9686050c1de39c8412afa",
                  "name": "Rejected"
                }
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
  "advisorEmail": "john@huntr.co",
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
        "listTemplates": [
          {
            "id": "5fc9686050c1de39c8412af7",
            "name": "Wishlist"
          },
          {
            "id": "5fc9686050c1de39c8412af8",
            "name": "Applied"
          },
          {
            "id": "5fc9686050c1de39c8412af9",
            "name": "Phone Interview"
          },
          {
            "id": "5fc9686050c1de39c8412af2",
            "name": "On Site Interview"
          },
          {
            "id": "5fc9686050c1de39c8412afa",
            "name": "Offer"
          },
          {
            "id": "5fc9686050c1de39c8412afa",
            "name": "Rejected"
          }
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
`advisorEmail` | no | String | Email address of the Huntr advisor you'd like to assign to this member. Advisor must already have an advisor account under your organization with Huntr.

# Advisors
## Advisor Resource

> An advisor of your organization:

```json
{
    "id": "64befb78b1ad13706cad0745",
    "fullName": "Alice Advisor",
    "givenName": "Alice",
    "familyName": "Advisor",
    "email": "alice@huntr.co",
    "createdAt": 1666980452,
    "lastSeenAt": 1689792002,
}
```

An advisor within your organization

Field | Type | Description
----- | ---- | -----------
id | String | Advisors's Huntr id
givenName | String | Advisor's given name
familyName | String | Advisor's family name
fullName | String | Advisor's full name
email | String | Advisor's email
createdAt | Unix timestamp | Date the advisor signed up for Huntr
lastSeenAt | Unix timestamp | Last date the advisor was seen using Huntr

## List Advisors

```shell
curl "https://api.huntr.co/org/advisors"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "id": "64befb78b1ad13706cad0745",
            "fullName": "Alice Advisor",
            "givenName": "Alice",
            "familyName": "Advisor",
            "email": "alice@huntr.co",
            "createdAt": 1666980452,
            "lastSeenAt": 1689792002
        }
        {
            "id": "64befb78b1ad13706cad0746",
            "fullName": "Bob Advisor",
            "givenName": "Bob",
            "familyName": "Advisor",
            "createdAt": 1666980452,
            "lastSeenAt": 1689792002,
            "email": "bob@huntr.co",
        }
    ]
}
```

This endpoint retrieves all advisors for your organization.

### HTTP Request

`GET https://api.huntr.co/org/advisors`


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
    "lastSeenAt": 1526984421,
    "memberFieldValues": [
        {
            "fieldId": "5b3f908c99c94b6177d55a28",
            "value": "Montreal"
        }
    ],
    "memberFieldValuesKeyedById": {
      "5b3f908c99c94b6177d55a28": {
        "fieldId": "5b3f908c99c94b6177d55a28",
        "value": "Montreal"
      }
    },
    "advisor": {
        "id": "64befb78b1ad13706cad0745",
        "fullName": "Alice Advisor",
        "givenName": "Alice",
        "familyName": "Advisor",
        "email": "alice@huntr.co",
        "createdAt": 1666980452,
        "lastSeenAt": 1689792002,
    }
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
memberFieldValuesKeyedById | Object | The same elements seen as memberFieldValues, but keyed by fieldId for fast access.
lastSeenAt | Unix timestamp | Last date the member was seen using Huntr (this includes logging in, saving jobs from extension, mobile, etc)
advisor | Object | [Advisor](#advisors) who this job seeker is assigned to

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
            "memberFieldValues": [],
            "advisor": {
                "id": "64befb78b1ad13706cad0745",
                "fullName": "Alice Advisor",
                "givenName": "Alice",
                "familyName": "Advisor",
                "email": "alice@huntr.co",
                "createdAt": 1666980452,
                "lastSeenAt": 1689792002
            }
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
            "memberFieldValues": [],
            "advisor": {
                "id": "64befb996efb7b393bfba5fb",
                "fullName": "Bob Advisor",
                "givenName": "Bob",
                "familyName": "Advisor",
                "email": "bob@huntr.co",
                "createdAt": 1666980452,
                "lastSeenAt": 1689792002,
            }
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
    "memberFieldValues": [],
    "advisor": {
        "id": "64befb78b1ad13706cad0745",
        "fullName": "Alice Advisor",
        "givenName": "Alice",
        "familyName": "Advisor",
        "email": "alice@huntr.co",
        "createdAt": 1666980452,
        "lastSeenAt": 1689792002,
    }
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
choices | String | false | Choices found on member fields of fieldType `MULTIPLE_CHOICE` and `MULTIPLE_SELECT`

## Types of Member Fields

### This is a list of all the types of fields currently available.

`memberField.fieldType` | Description
----------------------- | -----------
`MULTIPLE_CHOICE` | Represents a multiple choice field for which each job seeker may only have a single value for. Like "Cohort" or "Campus"
`MULTIPLE_SELECT` | Represents a multiple choice field for which each job seeker may have multiple values for. Like "Security Certifications" or "Specialties"
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
`choices` | only when `fieldType == 'MULTIPLE_CHOICE'` or `fieldType == 'MULTIPLE_SELECT'` | Array | List of strings representing choices in a field of type MULTIPLE_CHOICE or MULTIPLE_SELECT

# Board Templates

## Board Template Resource

> A board template:

```json
{
    "id": "5a6e4a567e42789e6e65c986",
    "name": "Tech Jobs",
    "listTemplates": [
      {
        "id": "5fc9686050c1de39c8412af7",
        "name": "Wishlist"
      },
      {
        "id": "5fc9686050c1de39c8412af8",
        "name": "Applied"
      },
      {
        "id": "5fc9686050c1de39c8412af9",
        "name": "Phone Interview"
      },
      {
        "id": "5fc9686050c1de39c8412af2",
        "name": "On Site Interview"
      },
      {
        "id": "5fc9686050c1de39c8412afa",
        "name": "Offer"
      },
      {
        "id": "5fc9686050c1de39c8412afa",
        "name": "Rejected"
      }
    ],
    "isDefault": true
}
```

A template to use when creating a new board for your members. Board templates are used when you want your job seekers' boards to have specific stages. Say you want your job seekers' board to have the following stages `['Wishlist', 'Applied', 'Interview', 'Offer']`. First you create a board template where `listTemplates == [{name: 'Wishlist'}, {name: 'Applied'}, {name: 'Interview'}, {name: 'Offer'}]` (the order of the stages is the same order that will be followed when a new board is created). Then you can create a new [Organization Invitation](#create-organization-invitations) in which `boardTemplateId` is the id of the Board Template you just created.

Field | Type | Found in all records | Description
----- | ---- | -------------- | -----------
id | String | true | Board Template id
name | String | true | Board template name
listTemplates | Array | true | Template used to create each stage in a board created with this template
isDefault | Boolean | true | If true, it means the template is the default within the organization

### Deprecated board template fields

These fields will be supported until the end of Q1 2021. See the **Replacement field** column to see what field is replacing the deprecated fields.

Field | Type | Replacement field | Description
----- | ---- | ----------------- | -----------
`listNames` | Array | `listTemplates` | Names of each stage in a board created with this template

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
            "isDefault": false,
            "listTemplates": [
              {
                "id": "5fc9686050c1de39c8412ad8",
                "name": "Applied"
              },
              {
                "id": "5fc9686050c1de39c8412ad9",
                "name": "Wishlist"
              },
              {
                "id": "5fc9686050c1de39c8412ad2",
                "name": "Interview"
              },
              {
                "id": "5fc9686050c1de39c8412ada",
                "name": "Offer"
              }
            ]
        },
        {
            "id": "5a6e4a567e42789e6e65c986",
            "name": "Tech Jobs",
            "isDefault": true,
            "listTemplates": [
              {
                "id": "5fc9686050c1de39c8412af7",
                "name": "Wishlist"
              },
              {
                "id": "5fc9686050c1de39c8412af8",
                "name": "Applied"
              },
              {
                "id": "5fc9686050c1de39c8412af9",
                "name": "Phone Interview"
              },
              {
                "id": "5fc9686050c1de39c8412af2",
                "name": "On Site Interview"
              },
              {
                "id": "5fc9686050c1de39c8412afa",
                "name": "Offer"
              },
              {
                "id": "5fc9686050c1de39c8412afa",
                "name": "Rejected"
              }
            ]
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
        "isPartner": true,
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
`jobPost` | Object | [Job Post](#job-posts) from which this job was created, if any
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
                "isPartner": true,
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
                "isPartner": false,
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
        "isPartner": true,
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

# Job Posts

## Job Post Resource

> A job post in your organization's portal:

```json
{
    "id": "601da7c59ca97b1604508a1c",
    "title": "Illustrator",
    "isRemote": false,
    "salary": {
      "actual": "90000",
      "currency": "USD",
      "interval": "ANNUAL"
    },
    "jobType": "FULL_TIME",
    "applicationInstructions": "Apply through original post link or email pete@notion.so",
    "url": "https://notion.so/jobs",
    "postDate": 1612556211,
    "createdAt": 1612556229,
    "location": {
        "name": "Brooklyn",
        "address": "Brooklyn, NY, USA",
        "country": "United States",
        "lat": "40.6781784",
        "lng": "-73.9441579"
    },
    "locations": [
      {
          "name": "Brooklyn",
          "address": "Brooklyn, NY, USA",
          "country": "United States",
          "lat": "40.6781784",
          "lng": "-73.9441579"
      }
    ],
    "jobPostStatus": {
        "name": "Open"
    },
    "employerId": "5bbfe1c6ed9dc0f2dbb9fe76",
    "employer": {
        "id": "5bbfe1c6ed9dc0f2dbb9fe76",
        "name": "Notion",
        "domain": "notion.so",
        "description": "A new tool that blends your everyday work apps into one. It's the all-in-one workspace for you and your team",
        "location": "3000 20th St, San Francisco, CA 94110, USA",
        "isPartner": false
    },
    "creatorMember": {
        "id": "5b2bbb5837fbc835bbe32154",
        "givenName": "Rennie",
        "familyName": "Dev 32",
        "email": "rennie+dev32@huntr.co",
        "createdAt": 1529592664,
        "isActive": true,
        "memberFieldValues": []
    },
    "ownerMember": {
        "id": "5b2bbb5837fbc835bbe32154",
        "givenName": "Rennie",
        "familyName": "Dev 32",
        "email": "rennie+dev32@huntr.co",
        "createdAt": 1529592664,
        "isActive": true,
        "memberFieldValues": []
    },
    "tags": [{
        "id": "6375bf49678aa421b84e8743",
        "name": "🔥 Hot Jobs",
        "description": "High demand job posts",
        "color": "#FF0000",
    }],
    "htmlDescription": "<h3>About <strong>Us:</strong></h3><p>We make software that anyone can mold and shape to take on every challenge — from taking personal notes to running large companies. We've been building together since 2016 and are trusted by customers including Nike, Airbnb, Slack, Samsung, and more. We're excited to be growing a team as diverse and creative as the millions of people we reach worldwide, and a company where everyone can thrive.</p><h3>About The Role:</h3><p>Do you want to help define what data means at Notion? We are looking to hire our first data engineer to guide the team with a vision for what our data tooling and infrastructure should look like as we scale, and then to build and operate those systems over time. With your background in data processing and warehousing, you'll partner with other members of the Data Team to evolve the infrastructure that allows Notion to make high quality decisions that are driven by data.</p><h3>What You'll Do:</h3><ul><li>You'll design and set up the tools that allow other members of the data team to easily write effective data pipelines.</li><li>You'll articulate best practices around logging frameworks and implement the changes to make those practices a reality.</li><li>You'll implement alerting systems to track data quality and consistency.</li><li>You'll create tools for data science team members that enable them to produce insights quickly and with high impact.</li><li>You'll help the Data Science team apply and generalize statistical and econometric models efficiently across large datasets.</li></ul><h3>What We're Looking For:</h3><ul><li><strong>Team player:</strong> For you, work isn't a solo endeavor. You have worked cross-functionally to establish the right overarching data architecture for a company's needs, to build data pipelines, and to provide guidance on best data practices for the business.</li><li><strong>Data tools experience:</strong> You have experience managing relational databases and authoring queries (SQL) and using workflow management technologies (e.g. Airflow, Luigi)</li><li><strong>Thoughtful problem-solving:</strong> For you, problem-solving starts with a clear and accurate understanding of the context. You can decompose tricky problems and work towards a clean solution, by yourself or with teammates. You're comfortable asking for help when you get stuck.</li><li><strong>Pragmatic and business-oriented:</strong> You care about business impact and prioritize projects accordingly. You're not just going after cool stuff—you understand the balance between craft, speed, and the bottom line.</li><li><strong>Put users first:</strong> You think critically about the implications of what you're building, and how it shapes real people's lives. You understand that reach comes with responsibility for our impact—good and bad.</li><li><strong>Not ideological about technology:</strong> To you, technologies and programming languages are about tradeoffs. You may be opinionated, but you're not ideological and can learn new technologies as you go.</li><li><strong>Empathetic communication:</strong> You communicate nuanced ideas clearly, whether you're explaining technical decisions in writing or brainstorming in real time. In disagreements, you engage thoughtfully with other perspectives and compromise when needed.</li></ul><h3>Bonus Points:</h3><ul><li>You've built out data infrastructure from, or nearly from, scratch at a fast-growing startup.</li><li>If you have led or managed a Data Engineering Team.</li></ul>"
}
```

A job post in your organization's portal. Job posts are sourced and saved by your advisors through the Advisor Portal or the Huntr chrome extension. Learn more about the job portal feature [here](https://blog.huntr.co/job-portal).

Field | Type | Description
----- | ---- | -----------
`id` | String | Job post's id
`title` | String | Job post title  
`htmlDescription` | String | Html Job post description
`applicationInstructions` | String | Instructions on how to apply to this job post
`url` | String | URL for the original job post
`employerId`  | String | Id of the job post [Employer](#employers)
`jobType` | String | Job type, see [Job Types](#job-types)
`isRemote` | Boolean | Defines if the job is a remote opportunity
`salary` | Object | Job post salary
`location` | Object | Object representing job post's location. If a job has multiple locations, the first one is returned
`locations` | Array | Array of objects representing multiple locations for a job post
`jobPostStatus` | Object | Object representing job post's status
`creatorMember` | Object | [Advisor](#members) who created the job post
`ownerMember` | Object | [Advisor](#members) who owns the job post
`employer`  | Object | [Employer](#employers) for the job post
`tags` | Array | An array of [Tags](#tags)
`postDate` | Unix timestamp | Date the job post was posted, your staff can change this date from the advisor portal
`createdAt` | Unix timestamp | Date the job post was created by the member

### Deprecated job post fields

Field | Type | Replacement field | Description
----- | ---- | ----------------- | -----------
`location` | Object | `locations` | Object representing job post's location. If a job has multiple locations, the first one is returned

## Job Types

There 4 job types:

Job Type | Description
---------- | -----------
`FULL_TIME` |  Represents a full time job
`PART_TIME` | Represents a part time job
`CONTRACTOR` | Represents a contractor job
`INTERNSHIP` | Represents an internship

## List Job Posts

```shell
curl "https://api.huntr.co/org/job-posts?limit=2"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
      {
        "id": "6037116c9c299b95bfd431d8",
        "title": "Engineer",
        "isRemote": true,
        "htmlDescription": "<p>Job Description</p><ul><li>Item one</li><li>Item two</li><li>Item three</li></ul>",
        "salary": {
            "actual": "85000",
            "currency": "USD",
            "interval": "ANNUAL"
        },
        "jobType": "FULL_TIME",
        "applicationInstructions": "Send email to joe@priceline.com",
        "url": "https://priceline.com/jobs",
        "postDate": 1614221661,
        "createdAt": 1614221676,
        "location": {
            "name": "Priceline",
            "address": "800 Connecticut Ave, Norwalk, CT 06854, USA",
            "country": "United States",
            "lat": "41.0930242",
            "lng": "-73.4539645"
        },
        "locations": [{
            "name": "Priceline",
            "address": "800 Connecticut Ave, Norwalk, CT 06854, USA",
            "country": "United States",
            "lat": "41.0930242",
            "lng": "-73.4539645"
        }],
        "jobPostStatus": {
            "name": "Draft"
        },
        "employer": {
            "id": "59b5f5f10143bacaa3eaa3ff",
            "name": "Priceline.com",
            "domain": "priceline.com",
            "foundedYear": 1998,
            "description": "Deep Discounts on Hotels, Flights and Rental Cars.  Get Exclusive Savings with Priceline.com.",
            "location": "800 Connecticut Ave, Norwalk, CT 06854, USA",
            "isPartner": false
        },
        "ownerMember": {
            "id": "5b2bbb5837fbc835bbe32154",
            "givenName": "Rennie",
            "familyName": "Dev 32",
            "email": "rennie+dev32@huntr.co",
            "createdAt": 1529592664,
            "isActive": true,
            "memberFieldValues": []
        },
        "creatorMember": {
            "id": "5b2bbb5837fbc835bbe32154",
            "givenName": "Rennie",
            "familyName": "Dev 32",
            "email": "rennie+dev32@huntr.co",
            "createdAt": 1529592664,
            "isActive": true,
            "memberFieldValues": []
        },
        "employerId": "59b5f5f10143bacaa3eaa3ff"
      },
      {
        "id": "6037114e9c299b95bfd43158",
        "title": "Product Manager",
        "isRemote": false,
        "jobType": "FULL_TIME",
        "postDate": 1614221578,
        "createdAt": 1614221646,
        "location": {
            "name": "Slack",
            "address": "500 Howard St, San Francisco, CA 94105, USA",
            "country": "United States",
            "lat": "37.7885052",
            "lng": "-122.396428"
        },
        "locations": [{
            "name": "Slack",
            "address": "500 Howard St, San Francisco, CA 94105, USA",
            "country": "United States",
            "lat": "37.7885052",
            "lng": "-122.396428"
        }],
        "jobPostStatus": {
            "name": "Open"
        },
        "employer": {
            "id": "592ed813b67c6168772b19ec",
            "name": "Slack",
            "domain": "slack.com",
            "description": "Slack brings all your communication together in one place. It’s real-time messaging, archiving and search for modern teams.",
            "location": "San Francisco, CA, USA",
            "isPartner": false
        },
        "ownerMember": {
            "id": "5b2bbb5837fbc835bbe32154",
            "givenName": "Rennie",
            "familyName": "Dev 32",
            "email": "rennie+dev32@huntr.co",
            "createdAt": 1529592664,
            "isActive": true,
            "memberFieldValues": []
        },
        "creatorMember": {
            "id": "5b2bbb5837fbc835bbe32154",
            "givenName": "Rennie",
            "familyName": "Dev 32",
            "email": "rennie+dev32@huntr.co",
            "createdAt": 1529592664,
            "isActive": true,
            "memberFieldValues": []
        },
        "employerId": "592ed813b67c6168772b19ec",
        "tags": [{
            "id": "6375bf49678aa421b84e8743",
            "name": "🔥 Hot Jobs",
            "description": "High demand job posts",
            "color": "#FF0000",
        }]
      }
    ],
    "next": "6037114e9c299b95bfd43158"
}
```

This endpoint lists all job posts added by advisors/staff from your organization to your organization's internal job portal, jobs are sorted by the date they were created, starting from the most recently created.

### HTTP Request

`GET https://api.huntr.co/org/job-posts`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`limit` | Integer | 100 | Defines the maximum number of job posts to return
`next` | String | none | If set, returns the next set of results after the object id provided

## Retrieve a Job Post

```shell
curl "https://api.huntr.co/org/job-posts/601da7c59ca97b1604508a1c"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "id": "601da7c59ca97b1604508a1c",
    "title": "Illustrator",
    "isRemote": false,
    "htmlDescription": "<h3>About <strong>Us:</strong></h3><p>We make software that anyone can mold and shape to take on every challenge — from taking personal notes to running large companies. We've been building together since 2016 and are trusted by customers including Nike, Airbnb, Slack, Samsung, and more. We're excited to be growing a team as diverse and creative as the millions of people we reach worldwide, and a company where everyone can thrive.</p><h3>About The Role:</h3><p>Do you want to help define what data means at Notion? We are looking to hire our first data engineer to guide the team with a vision for what our data tooling and infrastructure should look like as we scale, and then to build and operate those systems over time. With your background in data processing and warehousing, you'll partner with other members of the Data Team to evolve the infrastructure that allows Notion to make high quality decisions that are driven by data.</p><h3>What You'll Do:</h3><ul><li>You'll design and set up the tools that allow other members of the data team to easily write effective data pipelines.</li><li>You'll articulate best practices around logging frameworks and implement the changes to make those practices a reality.</li><li>You'll implement alerting systems to track data quality and consistency.</li><li>You'll create tools for data science team members that enable them to produce insights quickly and with high impact.</li><li>You'll help the Data Science team apply and generalize statistical and econometric models efficiently across large datasets.</li></ul><h3>What We're Looking For:</h3><ul><li><strong>Team player:</strong> For you, work isn't a solo endeavor. You have worked cross-functionally to establish the right overarching data architecture for a company's needs, to build data pipelines, and to provide guidance on best data practices for the business.</li><li><strong>Data tools experience:</strong> You have experience managing relational databases and authoring queries (SQL) and using workflow management technologies (e.g. Airflow, Luigi)</li><li><strong>Thoughtful problem-solving:</strong> For you, problem-solving starts with a clear and accurate understanding of the context. You can decompose tricky problems and work towards a clean solution, by yourself or with teammates. You're comfortable asking for help when you get stuck.</li><li><strong>Pragmatic and business-oriented:</strong> You care about business impact and prioritize projects accordingly. You're not just going after cool stuff—you understand the balance between craft, speed, and the bottom line.</li><li><strong>Put users first:</strong> You think critically about the implications of what you're building, and how it shapes real people's lives. You understand that reach comes with responsibility for our impact—good and bad.</li><li><strong>Not ideological about technology:</strong> To you, technologies and programming languages are about tradeoffs. You may be opinionated, but you're not ideological and can learn new technologies as you go.</li><li><strong>Empathetic communication:</strong> You communicate nuanced ideas clearly, whether you're explaining technical decisions in writing or brainstorming in real time. In disagreements, you engage thoughtfully with other perspectives and compromise when needed.</li></ul><h3>Bonus Points:</h3><ul><li>You've built out data infrastructure from, or nearly from, scratch at a fast-growing startup.</li><li>If you have led or managed a Data Engineering Team.</li></ul>",
    "salary": {
        "actual": "90000",
        "currency": "USD",
        "interval": "ANNUAL"
    },
    "jobType": "FULL_TIME",
    "applicationInstructions": "Apply through original post link or email pete@notion.so",
    "url": "https://notion.so/jobs",
    "postDate": 1612556211,
    "createdAt": 1612556229,
    "location": {
        "name": "Brooklyn",
        "address": "Brooklyn, NY, USA",
        "country": "United States",
        "lat": "40.6781784",
        "lng": "-73.9441579"
    },
    "locations": [{
        "name": "Brooklyn",
        "address": "Brooklyn, NY, USA",
        "country": "United States",
        "lat": "40.6781784",
        "lng": "-73.9441579"
    }],
    "jobPostStatus": {
        "name": "Open"
    },
    "employer": {
        "id": "5bbfe1c6ed9dc0f2dbb9fe76",
        "name": "Notion",
        "domain": "notion.so",
        "description": "A new tool that blends your everyday work apps into one. It's the all-in-one workspace for you and your team",
        "location": "3000 20th St, San Francisco, CA 94110, USA",
        "isPartner": false
    },
    "ownerMember": {
        "id": "5b2bbb5837fbc835bbe32154",
        "givenName": "Rennie",
        "familyName": "Dev 32",
        "email": "rennie+dev32@huntr.co",
        "createdAt": 1529592664,
        "isActive": true,
        "memberFieldValues": []
    },
    "creatorMember": {
        "id": "5b2bbb5837fbc835bbe32154",
        "givenName": "Rennie",
        "familyName": "Dev 32",
        "email": "rennie+dev32@huntr.co",
        "createdAt": 1529592664,
        "isActive": true,
        "memberFieldValues": []
    },
    "employerId": "5bbfe1c6ed9dc0f2dbb9fe76",
    "tags": [{
        "id": "6375bf49678aa421b84e8743",
        "name": "🔥 Hot Jobs",
        "description": "High demand job posts",
        "color": "#FF0000",
    }]
}
```

This endpoint retrieves a specific job post.

### HTTP Request

`GET https://api.huntr.co/org/job-posts/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the job post to retrieve

## Create Job Post

```shell
curl --location --request POST 'https://api.huntr.co/org/job-posts' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Full Stack Engineer",
    "jobType": "FULL_TIME",
    "jobPostStatusName": "Open",
    "applicationInstructions": "Send resume to info@huntr.co",
    "htmlDescription": "<b>Description</b><p>An html job description</p>",
    "url": "https://huntr.co/jobs/1",
    "isRemote": false,
    "locations": [{"address": "Seattle, WA, United States"}],
    "postDate": 1614911232,
    "salary": {
        "min": "90000",
        "max": "120000",
        "currency": "USD",
        "interval": "ANNUAL"
    },
    "employer": {
        "name": "Huntr",
        "domain": "huntr.co"
    },
    "tagIds": ["6375bf49678aa421b84e8743"],
    "memberEntries": [
      {"email": "johns@gmail.com"},
      {"memberId": "5b0379b5712bb674fd0d561e"}
    ]
}'
```

> The above command returns JSON structured like this:

```json
{
    "id": "6041976280901dae754f0851",
    "title": "Full Stack Engineer",
    "isRemote": false,
    "htmlDescription": "<b>Description</b><p>An html job description</p>",
    "salary": {
        "min": 90000,
        "max": 120000,
        "currency": "USD",
        "interval": "ANNUAL"
    },
    "jobType": "FULL_TIME",
    "applicationInstructions": "Send resume to info@huntr.co",
    "url": "https://huntr.co/jobs/1",
    "postDate": 1614911232,
    "createdAt": 1614911330,
    "location": {
        "name": "Seattle, King County, Washington, USA",
        "address": "Seattle, WA, United States",
        "lat": "47.6038321",
        "lng": "-122.3300624"
    },
    "locations": [{
        "name": "Seattle, King County, Washington, USA",
        "address": "Seattle, WA, United States",
        "lat": "47.6038321",
        "lng": "-122.3300624"
    }],
    "jobPostStatus": {
        "name": "Open"
    },
    "employer": {
        "id": "584917a6e660b2d8a5b46bd1",
        "name": "Huntr",
        "domain": "huntr.co",
        "isPartner": false
    },
    "employerId": "584917a6e660b2d8a5b46bd1",
    "tags": [{
        "id": "6375bf49678aa421b84e8743",
        "name": "🔥 Hot Jobs",
        "description": "High demand job posts",
        "color": "#FF0000",
    }]
}
```

This endpoint creates a single Job Post. If the job post status is `Open`, then this job will be published in your organization's job portal. See the `memberEntries` key if you'd like to share the newly created job post with a list of job seekers in one request.

### HTTP Request

`POST https://api.huntr.co/org/job-posts`

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
`title` | yes | String | Job title
`jobType` | yes | String | A [Job Type](#job-types) name
`jobPostStatusName` | yes | String | One of `Open`, `Closed` or `Draft`
`employer` | yes | Object | Required, unless the `employerId` field is included. Object is of type `{"name": "Google", "domain": "google.com", "description": "American multinational technology company"}`. If object is included, then it requires the `domain` key. If we find an employer in the Huntr database with this `domain`, we will link the job post to that employer; if not, then a new employer will be created and you will be able to edit the employer details from your organization's portal.
`employerId` | yes | String | Id for existing Huntr employer. Required, unless an `employer` object is included.
`applicationInstructions` | no | String | Application instructions
`url` | no | String | Url for the location of the original job post
`htmlDescription` | no | String | Job description in HTML format
`isRemote` | no | Boolean | Defines if this is a remote opportunity or not
`postDate` | no | Unix Timestamp | Timestamp denoting job post date.
`location` | no | Object | Location of the opportunity. Object of type `{"name": "Huntr HQ", "address": "Seattle, WA, USA", "lat": "47.6038321", "lng": "-122.3300624"}`. If object is included, it requires either the `address` key or both `lat` and `lng` keys.
`locations` | no | Array | Array of locations of the opportunity. Location object is of type `{"name": "Huntr HQ", "address": "Seattle, WA, USA", "lat": "47.6038321", "lng": "-122.3300624"}`. Each included object requires either the `address` key or both `lat` and `lng` keys.
`salary` | no | Object | A [Salary](#salary) object
`tagIds` | no | Array | Array of [Tag](#tags) ids for this job post
`memberEntries` | no | Array | Array of objects specifying the job seekers you want to share this new Job Post with. Each entry is an object with either an `email` or `memberId` key corresponding to the member you want to send the job post to:  `{"email": "john@example.com"}` or `{"memberId": "5b0379b5712bb674fd0d561e"}`. If received, our system will queue up an asynchronous job to deliver this job post for each member included in `memberEntries`. The jobs should run and finish a few seconds after your request is received. Each delivery of the job post to a job seeker triggers a `JOB_CREATED` action you can listen to via our [Webhooks](#webhooks); the [Action](#actions) body for the webhook will contain a `jobPost` key matching the job post that was just created.

## Send Job Posts to Job Seekers

```shell
curl --location --request POST 'https://api.huntr.co/org/job-posts/send-to-members' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jobPostIds": ["6041976280901dae754f0851", "6041976280901dae754f0852"],
    "memberEntries": [
      {"email": "johns@gmail.com"},
      {"memberId": "5b0379b5712bb674fd0d561e"}
    ]
}'
```

> The above command sends 2 job posts to 2 job seekers and returns a `200 OK` status response

This endpoint sends multiple job posts to job seekers' Huntr boards in the form of a new [Job](#jobs) card in each board. You can see all the job seekers that a job post has been shared with via the **Pipeline** tab of the Job Post details page in your advisor dashboard.

This endpoint does not run synchronously, it only queues up asynchronous jobs to deliver the job posts to each member included in `memberEntries`. Thus, a `200` status response does not imply successful delivery, it only means that your job post delivery requests have been successfully queued up. The jobs should run and finish a few seconds after your request is received. Each delivery of the job post to a job seeker triggers a `JOB_CREATED` action you can listen to via our [Webhooks](#webhooks). The [Action](#actions) body for the webhook will contain a `jobPost` key matching the job posts in the `jobPostIds` key of the original `job-posts/send-to-members` request.

### HTTP Request

`POST https://api.huntr.co/org/job-posts/send-to-members`

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
`jobPostIds` | yes | Array | array of ids for the job posts you want to send
`memberEntries` | yes | Array | Array of objects specifying the job seekers you want to share these job posts with. Each entry is an object with either an `email` or `memberId` key corresponding to the member you want to send the job post to:  `{"email": "john@example.com"}` or `{"memberId": "5b0379b5712bb674fd0d561e"}`.

# Tags
## Tag Resource

```json
{
  "id": "6375bf49678aa421b84e8743",
  "name": "🔥 Hot Jobs",
  "description": "High demand job posts",
  "color": "#FF0000",
  "targetObjects": ["JOB_POST"]
}
```

Tags are keywords or identifiers which can be applied to records within Huntr.

Field | Type | Description
----- | ---- | -----------
`id` | String | id for the tag
`name` | String | Display name for the tag
`description` | String | An internal description for what the tag describes
`color` | String | Hex color for displaying this tag
`targetObjects` | Array | Array of [Target Objects](#target-objects) this tag can be applied to

## Create Tag

```shell
curl --location --request POST 'https://api.huntr.co/org/tags' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "🔥 Hot Jobs",
    "description": "High demand job posts",
    "color": "#FF0000",
    "targetObjects": ["JOB_POST"]
}'
```

> The above command returns JSON structured like this:

```json
{
  "id": "6375bf49678aa421b84e8743",
  "name": "🔥 Hot Jobs",
  "description": "High demand job posts",
  "color": "#FF0000",
  "targetObjects": ["JOB_POST"]
}
```

This endpoint creates a new tag.

### HTTP Request

`POST https://api.huntr.co/org/tags`

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
`name` | yes | String | Tag name
`description` | no | String | Internal tag description
`color` | yes | String | Display color for this tag
`targetObjects` | yes | Array | Array of [Target Objects](#target-objects) this tag can be applied to

## List Tags

```shell
curl "https://api.huntr.co/org/tags"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
[
    {
        "id": "6375bf49678aa421b84e8743",
        "name": "🔥 Hot Jobs",
        "description": "High demand job posts",
        "color": "#FF0000",
        "targetObjects": ["JOB_POST"]
    },
    {
        "id": "6375bf2a678aa421b84e872e",
        "name": "💎 Exclusive",
        "description": "Organization exclusive jobs",
        "color": "#A0A0FF",
        "targetObjects": ["JOB_POST"]
    }
]

```

This endpoint lists all tags created by your organization

### HTTP Request

`GET https://api.huntr.co/org/tags`


## Target Objects

Target Objects refer to the types resources a tag can be applied to. There is currently only one type of target object

Target Object | Description
---------- | -----------
`JOB_POST` |  Tag can be applied to [Job Posts](#job-posts)

# Salary

## Salary Resource

> A salary object:

```json
{
    "actual": 120000,
    "currency": "USD",
    "interval": "ANNUAL"
}
```

This resource represents a salary. There's two possible salary formats:

1. A specific salary, in which case we would use the `actual` key.
2. A salary range, in which case we would use the `min` and `max` keys.

Parameter | Type | Description
--------- | ---- | -----------
`actual` | Number | Represents a specific salary amount
`min` | Number | Represents the minimum value in a salary range
`max` | Number | Represents the maximum value in a salary range
`currency` | String | One of [ISO 4217 currency codes](https://en.wikipedia.org/wiki/ISO_4217)
`interval` | String | One of `ANNUAL`, `MONTHLY`, `WEEKLY`, `DAILY` or `HOURLY`

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
      "isPartner": true,
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
`jobPost` | Object | false | [Job Post](#job-posts) linked to activity, if any
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
                "isPartner": true,
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
                "isPartner": false,
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
      "isPartner": true,
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
`jobs` | Array | false |  [Jobs](#jobs) linked to this action; only applies to Contact and Document related actions, since a contact and a document can be linked to multiple jobs
`jobPostId` | String | false |  id for job post linked to this action
`jobPostIds` | Array | false |  array of ids for job posts linked to this action
`jobPost` | Object | false |  [Job Post](#job-posts) linked to this action
`employerId` | String | false |  id for [Employer](#employers) linked to this action
`employer` | Object | false |  [Employer](#employers) linked to this action
`employers` | Array | false |  [Employers](#employers) linked to this action; only applies to Contact actions since a contact can be linked to multiple employers
`activityId` | String | false |  id for Activity linked to this action
`activity` | Object | false |  [Activity](#activities) linked to this action; only applies to Activity actions
`activityCategory` | Object | false |  [Activity Category](#activity-categories) linked to this action; only applies to Activity actions
`noteId` | String | false |  id for note linked to this action
`note` | Object | false |  [Note](#notes) linked to this action; only applies to Note actions
`contactId` | String | false |  id for contact linked to this action
`contact` | Object | false |  [Contact](#contacts) linked to this action; only applies to Contact actions
`documentId` | String | false |  id for document linked to this action
`document` | Object | false |  [Document](#documents) linked to this action; only applies to Document actions
`fromList` | Object | false | List from which job was moved in a `JOB_MOVED` action
`toList` | Object | false | List to which a job was moved or added to in `JOB_MOVED` and `JOB_CREATED` actions
`member` | Object | false | [Member](#members) created in `MEMBER_CREATED` actions
`organizationInvitation` | Object | false | [Organization Invitation](#organization-invitations) that was accepted on `MEMBER_CREATED` actions.
`organizationEmployerRequest` | Object | false | [Organization Employer Request](#organization-employer-requests) related to Employer portal actions like `ORGANIZATION_EMPLOYER_REQUEST_CREATED` `ORGANIZATION_EMPLOYER_REQUEST_VERIFIED` `ORGANIZATION_EMPLOYER_REQUEST_APPROVED` `ORGANIZATION_EMPLOYER_REQUEST_DECLINED`.
`organizationEmployerPartnership` | Object | false | [Organization Employer Partnership](#organization-employer-partnerships) related to Employer portal actions like `ORGANIZATION_EMPLOYER_PARTNERSHIP_CREATED`.
`update` | Object | false | Entry used to update the resource, only shown in Update actions

## Action Types

There are multiple action types:

Action Type | Description | Extra fields
---------- | ----------- | -----------
`MEMBER_CREATED` |  A new member is created. i.e: job seeker has accepted invitation | `member` `organizationInvitation` `organizationInvitationId`
`MEMBER_UPDATED` |  Details for a member have been updated. i.e: A custom field or advisor update for a member | `member` `update`
`MEMBER_DELETED` |  A member is deleted. i.e: job seeker has been deleted | `member`
`MEMBER_DEACTIVATED` |  A member is deactivated. i.e: job seeker has been deactivated | `member`
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
`ORGANIZATION_COMPANY_CREATED` |  An employer is saved through the '+ Company' Advisor portal modal | `employer`
`ORGANIZATION_COMPANY_UPDATED` |  An employer's info is updated through the Advisor portal | `employer` `update`
`JOB_POST_CREATED` | A new job post is created | `jobPost` `employer`
`JOB_POST_UPDATED` | A job post is updated | `jobPost` `employer` `update`
`JOB_POST_DELETED` | A job post is deleted | `jobPostId` `employer`
`BULK_JOB_POST_UPDATED` | Multiple job posts are updated | `jobPostIds` `update`
`GOAL_CREATED` |  A new goal is created | `goal`
`GOAL_UPDATED` |  A goal is updated | `goal` `update`
`GOAL_ENROLLMENT_CREATED` |  A job seeker is enrolled into a goal | `goal` `goalEnrollment`
`GOAL_ENROLLMENT_CONCLUDED` |  A job seeker is unenrolled from a goal | `goal` `goalEnrollment` `update`
`GOAL_PROGRESS_UPDATED` |  Triggered every time there is progress from a job seeker towards a goal | `goal` `goalEnrollment` `goalProgress` `update`
`GOAL_PROGRESS_COMPLETED` |  Triggered when a job seeker has completed a goal | `goal` `goalEnrollment` `goalProgress` `update`
`GOAL_PROGRESS_INCOMPLETE` | Triggered if a job seeker did not complete the goal on time and the end date for the goal has passed | `goal` `goalEnrollment` `goalProgress` `update`
`GOAL_PROGRESS_IN_PROGRESS` |  Triggered on the start date of a goal progress record | `goal` `goalEnrollment` `goalProgress` `update`
`GOAL_PROGRESS_UNENROLLED` |  Triggered after an advisor unenrolls a job seeker from a goal, thus marking all upcoming goal progress records as unenrolled | `goal` `goalEnrollment` `goalProgress` `update`
`DOCUMENT_CREATED` |  A new document is created | `document` `jobs`
`DOCUMENT_UPDATED` |  A document is updated | `document` `jobs` `update`
`DOCUMENT_DELETED` |  A document is deleted | `documentId`
`ORGANIZATION_EMPLOYER_REQUEST_CREATED` |  Triggered immediately after a new employer request is created. When first triggered, request is still not linked to an employer in our DB. | `organizationEmployerRequest`
`ORGANIZATION_EMPLOYER_REQUEST_VERIFIED` | Triggered after employer verifies their email and claims or joins their corresponding employer account. We suggest listening to this action instead of `ORGANIZATION_EMPLOYER_REQUEST_CREATED` for new request notifications. | `organizationEmployerRequest` `employer`
`ORGANIZATION_EMPLOYER_REQUEST_APPROVED` |  An employer request is approved by a team member | `organizationEmployerRequest` `employer`
`ORGANIZATION_EMPLOYER_REQUEST_DECLINED` |  An employer request is declined by a team member | `organizationEmployerRequest` `employer`
`ORGANIZATION_EMPLOYER_PARTNERSHIP_CREATED` | First user from a new employer has joined your portal | `organizationEmployerPartnership` `employer`

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
            "isPartner": true,
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
            "isPartner": true,
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
            "isPartner": false,
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

# Documents

## Document Resource

> A document object:

```json
{
  "id": "622bc9862a2ee4f9779277eb",
  "title": "Resume",
  "name": "MyResume.pdf",
  "url": "https://huntr-documents.s3.amazonaws.com/8b33e720-46f7-45b9-be76-b163be75fbb6/MyResume.pdf",
  "extension": "pdf",
  "documentCategoryName": "Resume"
}
```

This resource represents a document.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the document
`title` | String | none | Document's title
`name` | String | none | Document's file name
`url` | String | none | URL where document file is hosted. This url is protected, thus the document is not accessible.
`extension` | String | none | Document's file extension
`documentCategoryName` | String | none | Document category name

# Goals

## Goal Resource

> A goal object:

```json
{
  "id": "61bf6eef5fda60b9fb7f3978",
  "name": "Log 5 activities of type Phone Call each wee",
  "description": "Job seekers have to  complete 5 activities of type Phone Call each week for 4 weeks from the moment they are enrolled into this goal.\n\nWeekly goal intervals begin on Monday of every week.\n\nJob seekers will be enrolled manually by advisors.",
  "instructions": "<p>These are some instructions! They have a <a href=\"https://google.com\" target=\"_blank\">link too</a></p>",
  "status": "ACTIVE",
  "scheduleType": "RECURRING",
  "objectiveEntity": "Activity",
  "objectiveTargetType": "NUMERIC",
  "objectiveEntityConditions": [
    {
      "operator": "IS_IN",
      "key": "_activityCategory",
      "value": [
        "5e448f3bf2b856c29af11281"
      ]
    },
    {
      "operator": "EQUAL",
      "key": "completed",
      "value": true
    }
  ],
  "objectiveNumericTarget": 5,
  "enrollmentType": "MANUAL",
  "enrollmentTriggers": [],
  "enrollmentStartDate": 1639935727,
  "ownerMember": {
      "id": "5e6a7bbdc56bc6669e3da86c",
      "givenName": "Ruipo",
      "familyName": "Fauli",
      "email": "rennie+thinkful20@huntr.co",
      "createdAt": 1584036797,
      "isActive": true
  }
}
```

This resource represents a goal you've designed for your job seekers. Goals are like a blueprint of an objective you want your job seekers to accomplish. Our goals feature works with this goal resource along with [Goal Enrollment](#goal-enrollments) and [Goal Progress](#goal-progresses) resources. While goals tell Huntr what you expect your job seekers to do, goal enrollments represent a job seeker's participation in a goal, and goal progress records represent a job seeker's performance towards a goal for a specific time window (i.e: performance towards a goal for a specific monthly or week interval). These three items provide everything you need to design goals and effectively track job seeker performance towards those goals.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the goal
`name` | String | none | name of the goal
`description` | String | none | private internal description of the goal
`instructions` | String | none | public job seeker facing instructions of the goal
`status` | String | none | status of the goal
`scheduleType` | String | none | either `RECURRING` or `ONE_TIME`
`objectiveEntity` | String | none | either `Activity`, `Job` or `Contact`
`objectiveEntityConditions` | Array | none | For `Activity` entity types, details the Activity Category ids that count towards to goal.
`objectiveNumericTarget` | Number | none | Number of entities that must be saved or logged to complete the goal
`enrollmentType` | String | none | one of `MANUAL`, `ON_SIGNUP` or `ON_TRIGGER`
`enrollmentTriggers` | String | none | for `ON_TRIGGER` enrollmend, details the action that triggers the enrollment
`enrollmentStartDate` | Unix timestamp | none | date when auto enrollments should start
`oneTimeGoalDueDaysCount` | String | none | for `ONE_TIME` goals, details the number of days the job seeker has to complete this goal.

## List Goals

```shell
curl "https://api.huntr.co/org/goals"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "data": [{
        "id": "642711341daf29393e1b2351",
        "name": "Weekly Application Goal",
        "description": "Job seekers have to  complete 5 activities of type Apply each week for 12 week(s) from the moment they are enrolled into this goal.\n\nJob seekers will be enrolled manually by advisors.",
        "instructions": "<p>Each week, you must complete 5 job applications.</p>",
        "status": "ACTIVE",
        "scheduleType": "RECURRING",
        "objectiveEntity": "Activity",
        "objectiveTargetType": "NUMERIC",
        "objectiveEntityConditions": [
            {
                "operator": "IS_IN",
                "key": "_activityCategory",
                "value": [
                    "635c14b97370b10fbb987f10"
                ]
            },
            {
                "operator": "EQUAL",
                "key": "completed",
                "value": true
            }
        ],
        "objectiveNumericTarget": 5,
        "enrollmentType": "MANUAL",
        "enrollmentTriggers": [],
        "enrollmentStartDate": 1680281939,
        "goalRecurrence": {
            "byMonth": [],
            "byMonthDay": [],
            "byYearDay": [],
            "byWeekNumber": [],
            "byWeekDay": [],
            "byHour": [],
            "byMinute": [],
            "bySecond": [],
            "frequency": 2,
            "start": "2023-03-27T00:00:00.000Z",
            "interval": 1,
            "count": 12,
            "timezone": "America/Los_Angeles"
        },
        "firstGoalInterval": {
            "id": "642711541eaf29393e1b2354",
            "nextIntervalId": "642711541daf29393e1b2355",
            "intervalIndex": 1,
            "timezone": "America/Los_Angeles",
            "goalId": "642711541daf29393e1b2351",
            "start": 1679900400,
            "end": 1680505200
        },
        "currentGoalInterval": {
            "id": "642711541eaf29393e1b2355",
            "nextIntervalId": "642711541daf29393e1b2356",
            "previousIntervalId": "642711541daf29393e1b2354",
            "intervalIndex": 2,
            "timezone": "America/Los_Angeles",
            "goalId": "642711541daf29393e1b2351",
            "start": 1680505200,
            "end": 1681110000
        }
    },
    {
        "id": "642bd7b0089c281e8d21344e",
        "name": "Interview Goal",
        "description": "Job seekers have to  complete 1 activities of type Interview each month for 6 month(s) from the moment they are enrolled into this goal.\n\nJob seekers will be enrolled manually by advisors.",
        "instructions": "<p>Keep interviewing! Don't stop until you find that dream job 😊</p>",
        "status": "ACTIVE",
        "scheduleType": "RECURRING",
        "objectiveEntity": "Activity",
        "objectiveTargetType": "NUMERIC",
        "objectiveEntityConditions": [
            {
                "operator": "IS_IN",
                "key": "_activityCategory",
                "value": [
                    "635c14b97771b10fbb987f10"
                ]
            },
            {
                "operator": "EQUAL",
                "key": "completed",
                "value": true
            }
        ],
        "objectiveNumericTarget": 1,
        "enrollmentType": "MANUAL",
        "enrollmentTriggers": [],
        "enrollmentStartDate": 1680594864,
        "goalRecurrence": {
            "byMonth": [],
            "byMonthDay": [],
            "byYearDay": [],
            "byWeekNumber": [],
            "byWeekDay": [],
            "byHour": [],
            "byMinute": [],
            "bySecond": [],
            "frequency": 1,
            "start": "2023-04-01T00:00:00.000Z",
            "interval": 1,
            "count": 6,
            "timezone": "America/Los_Angeles"
        },
        "firstGoalInterval": {
            "id": "642bd7b0089c281e8d213451",
            "nextIntervalId": "642bd7b0089c281e8d213452",
            "intervalIndex": 1,
            "timezone": "America/Los_Angeles",
            "goalId": "642bd7b0089c281e8d21344e",
            "start": 1680332400,
            "end": 1682924400
        },
        "currentGoalInterval": {
            "id": "642bd7b0089c281e8d213451",
            "nextIntervalId": "642bd7b0089c281e8d213452",
            "intervalIndex": 1,
            "timezone": "America/Los_Angeles",
            "goalId": "642bd7b0089c281e8d21344e",
            "start": 1680332400,
            "end": 1682924400
        }
    }]
}
```

This endpoint retrieves all goals for your organization. By default, only active goals are returned.

### HTTP Request

`GET https://api.huntr.co/org/goals`

### Query Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`includeInactive` | Boolean | false | If true, will return inactive goals alongside active goals.

## Retrieve a Goal

```shell
curl "https://api.huntr.co/org/goals/642711341daf29393e1b2351"
  -H "Authorization: Bearer <ORG_ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
    "id": "642711341daf29393e1b2351",
    "name": "Weekly Application Goal",
    "description": "Job seekers have to  complete 5 activities of type Apply each week for 12 week(s) from the moment they are enrolled into this goal.\n\nJob seekers will be enrolled manually by advisors.",
    "instructions": "<p>Each week, you must complete 5 job applications.</p>",
    "status": "ACTIVE",
    "scheduleType": "RECURRING",
    "objectiveEntity": "Activity",
    "objectiveTargetType": "NUMERIC",
    "objectiveEntityConditions": [
        {
            "operator": "IS_IN",
            "key": "_activityCategory",
            "value": [
                "635c14b97370b10fbb987f10"
            ]
        },
        {
            "operator": "EQUAL",
            "key": "completed",
            "value": true
        }
    ],
    "objectiveNumericTarget": 5,
    "enrollmentType": "MANUAL",
    "enrollmentTriggers": [],
    "enrollmentStartDate": 1680281939,
    "goalRecurrence": {
        "byMonth": [],
        "byMonthDay": [],
        "byYearDay": [],
        "byWeekNumber": [],
        "byWeekDay": [],
        "byHour": [],
        "byMinute": [],
        "bySecond": [],
        "frequency": 2,
        "start": "2023-03-27T00:00:00.000Z",
        "interval": 1,
        "count": 12,
        "timezone": "America/Los_Angeles"
    },
    "firstGoalInterval": {
        "id": "642711541eaf29393e1b2354",
        "nextIntervalId": "642711541daf29393e1b2355",
        "intervalIndex": 1,
        "timezone": "America/Los_Angeles",
        "goalId": "642711541daf29393e1b2351",
        "start": 1679900400,
        "end": 1680505200
    },
    "currentGoalInterval": {
        "id": "642711541eaf29393e1b2355",
        "nextIntervalId": "642711541daf29393e1b2356",
        "previousIntervalId": "642711541daf29393e1b2354",
        "intervalIndex": 2,
        "timezone": "America/Los_Angeles",
        "goalId": "642711541daf29393e1b2351",
        "start": 1680505200,
        "end": 1681110000
    }
}
```

This endpoint retrieves a specific goal.

### HTTP Request

`GET https://api.huntr.co/org/goals/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the goal to retrieve

# Goal Enrollments

## Goal Enrollment Resource

> A goal enrollment object:

```json
{
  "id": "61bf669a887ee3b7bff62fe1",
  "goalId": "61a1653e0a60f10199537f84",
  "status": "ACTIVE",
  "ownerMember": {
      "id": "5e6a7bbdc56bc6669e3da86c",
      "givenName": "Ruipo",
      "familyName": "Fauli",
      "email": "rennie+thinkful20@huntr.co",
      "createdAt": 1584036797,
      "isActive": true
  }
}
```

This resource represents a job seeker's participation in a goal. Any job seeker participating in a goal will have a corresponding Goal Enrollment record.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the goal enrollment
`goalId` | String | none | id for the goal
`status` | String | none | status of the goal enrollment.
`ownerMember` | Object | [Member](#members) who is participating in the goal

## Create Goal Enrollment

```shell
curl --location --request POST 'https://api.huntr.co/org/goals/642711341daf29393e1b2351/enrollments' \
--header 'Authorization: Bearer <ORG_ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "memberId": "64090fa941a8885a417d0c99",
}'
```

> The above command returns JSON structured like this:

```json
{
    "id": "642da8d2aad714656f293a14",
    "goalId": "642711341daf29393e1b2351",
    "status": "ACTIVE",
    "startingGoalIntervalId": "642711541eaf29393e1b2354",
    "ownerMember": {
        "id": "64090fa941a8885a417d0c99",
        "fullName": "Peter Huntr",
        "givenName": "Peter",
        "familyName": "Huntr",
        "email": "peter-huntr@huntr.co",
        "createdAt": 1678315433,
        "isActive": true,
        "lastSeenAt": 1678316577,
    },
    "startingGoalInterval": {
        "id": "642711541eaf29393e1b2354",
        "nextIntervalId": "642da8c7aad714656f29389f",
        "intervalIndex": 1,
        "timezone": "America/Los_Angeles",
        "goalId": "642da8c7aad714656f29389b",
        "start": 1680505200,
        "end": 1681110000
    }
}
```

This endpoint creates a new goal enrollment for an organization member.

### HTTP Request

`POST https://api.huntr.co/org/goals/<ID>/enrollments`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the goal to enroll the member in

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
`memberId` | no | String | [Member](#members) id of member to be enrolled in this goal. Either `memberId` or `memberEmail` must be set.
`memberEmail` | no | String | [Member](#members) email of member to be enrolled in this goal. Either `memberId` or `memberEmail` must be set.
`startingGoalIntervalId` | no | String | For `RECURRING` goals, the id of the first goal interval for this member. Defaults to the current interval if not provided
`oneTimeGoalStart` | no | Unix timestamp | For `ONE_TIME` goals, specifies the start time for this member. Defaults to now if not provided.

# Goal Progresses

## Goal Progress Resource

> A goal progress object:

```json
{
  "id": "61bf669a887ee3b7bff62fe3",
  "goalId": "61a1653e0a60f10199537f84",
  "goalEnrollmentId": "61bf669a887ee3b7bff62fe1",
  "status": "IN_PROGRESS",
  "progressIndex": 1,
  "objectiveNumericCount": 2,
  "objectiveNumericTarget": 20,
  "objectiveEntity": "Job",
  "objectiveActivityIds": [],
  "objectiveJobIds": [
    "61bf66a70ab6e63cef099798",
    "61bf66db0ab6e63cef0997b5"
  ],
  "objectiveContactIds": [],
  "start": 1639890000,
  "end": 1647752399
}
```

This resource represents a job seeker's performance towards a goal for a specific time window. For `RECURRING` goals, there will be multiple Goal Progress records for each job seeker enrollment, each one representing the job seeker's performance towards a different time window (week or month). For `ONE_TIME` goals, there is only 1 goal progress record per job seeker enrollment.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the goal progress
`goalId` | String | none | id for the goal this goal progress record is related to
`goalEnrollmentId` | String | none | id for the goal enrollment this goal progress record is related to
`status` | String | none | one of `UPCOMING`, `IN_PROGRESS`, `COMPLETE`, `INCOMPLETE`, `UNENROLLED`
`progressIndex` | Number | none | For a given job seeker, represents the goal progress index in a list of goal progress records sorted by start date.
`objectiveNumericCount` | Number | none | current progress number for this goal
`objectiveNumericTarget` | Number | none | target number for this goal
`objectiveEntity` | String | none | one of  `Activity`, `Job`, `Contact`
`objectiveActivityIds` | Array | none | ids for logged activities that count towards this goal progress record, only applies to goals with objectiveEntity of type `Activity`
`objectiveJobIds` | Array | none | ids for saved jobs that count towards this goal progress record, only applies to goals with objectiveEntity of type `Job`
`objectiveContactIds` | Array | none | ids for saved contacts that count towards this goal progress record, only applies to goals with objectiveEntity of type `Contact`
`start` | Unix timestamp | none | Start date for this goal progress record
`end` | Unix timestamp | none | End date for this goal progress record

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
    "isPartner": true,
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
`isPartner` | Boolean | Set to `true` if the Employer has been marked as a partner for your organization
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
          "isPartner": true,
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
          "isPartner": false,
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
`limit` | 100 | Defines the maximum number of employers to return
`isPartner` | none | Set this parameter to `true` if you want to retrieve only your employers marked as partners
`next` | none | If set, returns the next set of results after the object id provided

# Organization Employer Requests

## Organization Employer Request Resource

> An employer request object:

```json
{
    "id": "648111fee7093f3013aeaefd",
    "approvalStatus": "APPROVED",
    "fromEmail": "karen.salgado@dropbox.com",
    "utmTags": {
        "utm_source": "facebook",
        "utm_medium": "social",
        "utm_campaign": "fb-2023"
    },
    "createdAt": 1686180350,
    "decidedAt": 1686180501
}
```

This resource represents a request from a potential employer partner to join your talent portal.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the request
`approvalStatus` | String | `PENDING` | either `PENDING`, `APPROVED` or `REJECTED`
`fromEmail` | String | none | Requester's email
`utmTags` | Object | `{}` | Any utm tags found in the url the employer used to reach your portal's signup form
`createdAt` | Unix timestamp | none | Date the request was created.
`decidedAt` | Unix timestamp | none | Date the request decision was made (either approved or rejected).

# Organization Employer Partnerships

## Organization Employer Partnership Resource

> An employer partnership object:

```json
{
    "id": "647fc0f471dbf904d034305e",
    "employerCanPostJobs": true,
    "employerCanBrowseCandidates": true,
    "employerId": "5eb4719153a6366028ad14e6",
    "createdAt": 1686094068
}
```

This resource represents an employer who has joined your talent portal. This is not to be confused with the Employer's `isPartner` field, which can be set to true by your team even for employers who have not signed up for your portal. In short, every employer who has joined your portal, will have a corresponding Organization Employer Partnership record.

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
`id` | String | none | id for the partnership
`employerCanPostJobs` | Boolean | none | Defines whether this employer can post jobs into your portal
`employerCanBrowseCandidates` | Boolean | none | Defines whether this employer can view your candidate profiles
`employerId` | String | none | id of the corresponding [Employer](#employers)
`createdAt` | Unix timestamp | true | Date the partnership was created.

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

## Secure your webhooks (Optional)

Huntr will sign the webhook events it sends to your endpoints by including a signature in each event’s `x-huntr-hmac-sha256` header. This allows you to verify that the events were sent by Huntr, not by a third party. Signatures for each webhook event are created using the HMAC SHA-256 algorithm.

### Validating Huntr signature

To validate the signature you received from Huntr, you will generate the signature yourself using your Webhook's secret and compare that signature with the signature you receive in the webhook payload.

1. Retrieve the Webhook secret through your advisor portal. Head to to the Developers tab -> Click Webhooks -> Click `Reveal` for the Webhook record you are working with.
2. Using the HMAC SHA-256 algorithm, create a hash (using the retrieved secret from step 1 as a key) of the entire received payload.
3. Encode the hash in base64 format.
4. Compare the created value with the signature you received in the `x-huntr-hmac-sha256` header from Huntr.

You can find some code samples on how this verification might work for Python and Node in our [huntr-webhook-verification-examples](https://github.com/huntr-co/huntr-webhook-verification-examples) repo.

### Common issues when validating signature

1. **You are looking at the wrong header**. Check the headers for the specific framework or environment you are working on. While Huntr submits the header name as `x-huntr-hmac-sha256`, certain frameworks will change case or append extra text into headers. Logging all the request headers for inspection can help find the exact key for the signature header within your framework.
2. **Payload used to create hash is incorrect**. The body/payload you are providing the hmac algorithm might not exactly match the body that the Huntr system used to encode the hmac signature in the first place. Huntr creates the hmac algorithm with a stringified version of the request body (i.e: the payload). You might have to 'stringify' the request body in certain frameworks and not in others depending on how each framework encodes the request object.
3. **Don't forget to encode the HMAC hash in base64 format**. You might be properly calculating the hmac, but might have forgotten to encode that result to base64, or you might be encoding to 'hex' or another value.
4. **Use HMAC SHA-256 algorithm**. There are a number of SHA variants available with HMAC, make sure that your are using the 'SHA-256' variant.

## Webhooks Changelog

### May 2020
Before May 2020, Webhooks were powered by the now deprecated [Event](#events-deprecated) resource. Webhooks are now powered by the [Action](#actions) resource. To make the transition from Event to Action Webhooks easier, Webhook messages will contain all of the old Event fields until the end of 2020, alongside the new fields introduced in the Action resource.
