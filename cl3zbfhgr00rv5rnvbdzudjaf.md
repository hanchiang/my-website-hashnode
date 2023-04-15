---
title: "API versioning: The inevitable breaking change in software"
datePublished: Mon May 25 2020 12:28:53 GMT+0000 (Coordinated Universal Time)
cuid: cl3zbfhgr00rv5rnvbdzudjaf
slug: api-versioning-inevitable-breaking-change-in-software
canonical: https://www.yaphc.com/api-versioning-inevitable-breaking-change-in-software
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654345439811/E_DJayXqQ.jpeg
tags: work, api-versioning, breaking-changes

---

RESTful APIs are interfaces for computers to communicate with one another, and are ideally set in stone. 

However, even with great design knowledge and practices, demands and requirements of systems evolve over time, which may cause incompatibility with older versions. The use of API versioning helps to manage these incompatibilities, which are also called **breaking changes**.

API versioning is a concept that I have heard of some time ago but did not realise its significance until I needed to implement them. It is also one of the concepts that are rather subjective in nature, with no single superior approach to rule them all.

## Overview

**When is versioning required?**

> Change in an API is inevitable as your knowledge and experience of a system improves. Managing the impact of this change can be quite a challenge when it threatens to break existing client integrations.

It is needed when the application logic changes, which results in a **different response format**, such as in my case of [implementing pagination](https://www.yaphc.com/pagination-deceptively-simple-task).

It is also important to mention that breaking changes are most likely to happen when building APIs that serve a **mobile** application. This is because each build submission of the mobile application captures the snapshot of the application code at that time, therefore it is **unable to adapt** to changes to the APIs from that point onward. This reinforces the importance of understanding the requirements and in order to design a robust solution.

In my current role, certain parts of the system that were designed at the initial phase were done without a full understanding of the business domain and its implications, which is normal in startup environments.

As a result, some APIs were awkward to work with because of reasons like confusing naming conventions, business requirements are not fully met, etc.

## Versioning approaches

There are a few common ways to implement the versioning, which have been discussed in great depth such as the one from [Troy Hunt](https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/), [Mark Nottingham](https://www.mnot.net/blog/2012/12/04/api-evolution):

-   URL path: e.g. _api.example.com/v2/_
-   Query parameters: e.g. _api.example.com?version=2_
-   Header: e.g. _api-version: v2_

### 1. URL path

The simplest of all is to embed the major version in the URL to use the appropriate version of a resource.

Under this design, the version applies to the entire service. If a certain API requires a breaking change, the frontend clients need to adjust the version for **that particular service**.

Deprecating an API would involve keeping the routes live, updating the logic to return an appropriate response.

**Pros**

-   Failures are **detected early** at the API gateway level. Using a URL that does not correspond to a resource will result in an error response without passing the request to backend services.
-   A specific version of a resource means that there is no need for additional logic to determine the **correct code execution path** in the backend services, safeguarding against an outage due to incorrect code routing.
-   No need to store a list of APIs in the database and write custom code to validate

**Cons**

-   A quick and cheap solution that can become messy if it is not maintained properly
-   Require an additional set of entries in API gateway
-   There is a potential for code duplication at the service level if the logic for different versions are not managed properly

## 2. Header

This involves adding a custom HTTP request header to indicate the resource version.

Here, the same version is used for all services. This means that if a service contains an API that needs a version update, frontend client will update a single version that will be used for requests to **all services**.

**Pros**

-   Does not require an additional list of entries in API gateway
-   There is no service-level code duplication

**Cons**

-   Extra care needs to be taken to ensure that the proper code path is executed for different APIs with different breaking versions, since the condition to check for will be different. Otherwise, it may result in an outage for that API or even a crash in the entire service.

## Implementation workflow

After some discussion, the decision was to go with the header approach, because it is a more structured method that allows us to maintain **code quality** and implement a centralised logic to handle the versions. The tradeoff is that the documentation of various APIs versions needs to be very detailed, and the logic to determine which version of the code to execute needs to be done carefully.

**Implementation overview**

Each HTTP request will receive an _api-version_ entry in the header. Following that are several processing steps:

1.  Validate the _api-version_ input. An empty input is allowed.
2.  If the list of API versions is not saved in memory, retrieve the latest copy.
3.  If the input is not specified, set the initial API version in the header. Otherwise, check that the input is found in the list of valid versions before setting it in the header. An invalid version will be responded with an error.

**Usage in services**

Using it in the services is a matter of a few simple but crucial lines of if-else code that triggers the appropriate code according to the version.

To illustrate the importance of the conditional versioning logic, let's say version 1 of a search hotels API expects to receive the following query string: _name_, _pricePointMin_, _pricePointMax_

`api.example.com/search?name=lhotel&pricePointMin=2&pricePointMax=5`

While version 2 requires the following query string: _name_, _pricePoints_

`api.example.com/search?name=1hotel&pricePoints=2,4,5`

In the case of executing code for version 2 using version 1's request parameters and vice versa, the best outcome is an error response to clients which is not too damaging, the worst would be a crash of the entire service.

## Conclusion

Business needs and requirements **evolve over time**, and the software will follow suit. For this particular topic, my personal opinion is that the URL approach is a quick way to get things done, and the header approach is suitable for meeting the needs of organisation that is scaling or has achieved scale.

Regardless of the approach, it is important to have clear documentation to align expectations among team members, as well as a strategy for deprecating older APIs so that clients using older versions won't be left with a broken application.