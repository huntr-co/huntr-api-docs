# Changelog

## January 4, 2021
Added validation for the `limit` parameter for requests that use the [List](#pagination) API methods. The maximum `limit` value for those requests is now 500.

## February 25, 2021
- New [Job Post resource](#job-posts)
- New [GET /job-posts](#list-job-posts) endpoint to list job posts
- New [GET /job-posts/<ID>](#retrieve-a-job-post) endpoint to get job post
- New `jobPost` field in [Job](#jobs) resource, if job was saved from a Job Post
- New `jobPost` field in [Activity](#activities) resource, if activity is related to a job that was saved from a Job Post
- New `jobPost` and `jobPostId` fields in [Action](#actions) resource
- Three new [Action Types](#action-types) related to Job Posts: `JOB_POST_CREATED`, `JOB_POST_UPDATED`, `JOB_POST_DELETED`

## March 4, 2021
- New [POST /job-posts](#create-job-post) endpoint to create job posts

## December 20, 2021
- New [Goal](#goals) resource
- New [Goal Enrollment](#goal-enrollments) resource
- New [Goal Progress](#goal-progresses) resource

- Seven new [Action Types](#action-types) related to Goals: `GOAL_CREATED`, `GOAL_UPDATED`, `GOAL_ENROLLMENT_CREATED`, `GOAL_PROGRESS_UPDATED`, `GOAL_PROGRESS_COMPLETED`, `GOAL_PROGRESS_INCOMPLETE`, `GOAL_PROGRESS_IN_PROGRESS`
- Three new [Action Types](#action-types) related to Members: `MEMBER_UPDATED`, `MEMBER_DELETED`, `MEMBER_DEACTIVATED`

## January 28, 2022
- Two new [Action Types](#action-types) related to Goals: `GOAL_ENROLLMENT_CONCLUDED`, `GOAL_PROGRESS_UNENROLLED`
- New potential status value for [Goal Progress](#goal-progresses): `UNENROLLED`

## May 6, 2022
- Three new [Action Types](#action-types) related to Documents: `DOCUMENT_CREATED`, `DOCUMENT_UPDATED`, `DOCUMENT_DELETED`
- New [Document](#documents) resource

## June 15, 2022
- New `MULTIPLE_SELECT` [Member Field Type](#types-of-member-fields)

## Nov 29, 2022
- New [Action Type](#action-types) for bulk updating job posts `BULK_JOB_POST_UPDATED`
- API endpoints for managing [Tags](#tags)
- New locations field in [Job Posts](#job-posts)
- New tags field in [Job Posts](#job-posts)

## Dec 8, 2022
- New API endpoint to [Send Job Posts to Job Seekers](#send-job-posts-to-job-seekers)
- New `memberEntries` body parameter in [Create Job Post](#create-job-post) endpoint to send job post to job seekers

## Jan 27, 2023
- New `advisorEmail` body parameter in [Create Organization Invitation](#create-organization-invitation) endpoint to assign an advisor to newly invited members.

## Feb 15, 2023
- New `memberFieldValuesKeyedById` field in [Member](#member-resource) resource for quick access to member's custom field values.
- New `lastSeenAt` field in [Member](#member-resource).

## Apr 4, 2023
- New API endpoint to [List Goals](#list-goals)
- New API endpoint to [Retrieve a Goal](#retrieve-a-goal)
- New API endpoint to [Enroll Job Seeker in Goal](#create-goal-enrollment)

## July 24, 2023
- New API endpoint to [List Advisors](#list-advisors)
- New [Advisor](#advisor-resource) field in [Member](#member-resource)

## July 31, 2023
- New `boardId`, `listId` and `list.id` fields in [Job](#jobs) resource
- New `boardId`, `fromListId` and `toListId` fields in [Action](#actions) resource

## October 19, 2023
- New API endpoint to [List Internal Member Notes](#list-internal-member-notes)
- New API endpoint to [Create Internal Member Note](#create-internal-member-note)
- New API endpoint to [Delete Internal Member Note](#delete-internal-member-note)

## October 28, 2024
- New `boardIds` field in [Member](#member-resource)

## December 9, 2024
- New [Candidate](#candidate-resource) resource
- New API endpoint to [List Candidates](#list-candidates)
- New API endpoint to [Retrieve a Candidate](#retrieve-a-candidate)
- New API endpoint to [Retrieve Candidate Action Metrics](#retrieve-candidate-action-metrics)
- New [Action Type](#action-types) `CANDIDATE_PROFILE_VIEWED`
- New [Action Type](#action-types) `CANDIDATE_SEARCH_RAN`
- New [Action Type](#action-types) `ORGANIZATION_TALENT_PORTAL_ACCESSED`
