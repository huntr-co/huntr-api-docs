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
