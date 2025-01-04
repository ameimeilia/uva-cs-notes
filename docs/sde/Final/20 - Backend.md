---
layout: default
title: Backend
parent: Final Notes
nav_order: 11
---
## Backend
- rule: don’t implement your own database from scratch
- Frontend - client-side
	- the screens the user sees
	- any local storage
- Backend - server-side
	- remote database
	- remote services

## “Off-the-shelf” Backend
- backend as a service
	- ex. AWS, Azure, etc.
- Firebase
	- popular back-end OTS software, especially in mobile development
		- supports user management, data storage, file storage, etc.
	- business model has a limited start-up plan and pay-per-data-usage plan

## Firebase Tools
- authentication: authenticate and manage users
- cloud firestore: cloud noSQL database
- cloud storage: cloud file management
	- ex. storing image files as opposed to storing ordered data
- cloud functions: **serverless** functions available through cloud calls
	- offload
	- don’t have to manage the servers
	- servers accessed through API calls
- firebase hosting: global web hosting for static content

## Cost of Backend
- operational costs
	- physical server(s), electricity, internet data usage/bandwith
- logistical costs
	- monitoring, security, building your own APIs or HTML interfaces, database management/backup

## Monolith
- entire application on one code-base that can be deployed to one or more servers
- advantages
	- scaling entire application is easy: buy more servers and load the same application on each
	- no “redirection cost” which can occur with microservices
	- easier debugging: don’t need to find source of “cascading failure” since every service is in the same code base
	- reduced monitoring overhead
- disadvantages
	- cannot scale individual services, since all services are on the same server
	- reliability: an error that causes a crash on one server can crash them all
	- services can end up tightly coupled, making them harder to separate/change over time

## Microservice
- have individual services hosted on their own server(s)
- advantages:
	- each service is smaller code base/may have its own database → less “accidental coupling,” development is easier
	- “server requests as function calls”
	- indirection costs: data and time
	- scaling: if particular services have high demand, they can be scaled up individually to more servers
- disadvantages
	- monitoring: need to be able to see which services are functional and which aren’t
	- debugging: each server may have it’s own logs; if several services go down at nearly the same time, identifying the source can be hard
	- infrastructure costs: each service can have its own costs

## Scaling
- horizontal scaling: servers full → add more servers
- vertical scaling: servers full → upgrade existing servers to make them more powerful
- bottlenecks:
	- ex. microservice where only authentication services are at capacity → logged in users are final but new users can’t log in
- database scalability
	- relational database have significant horizontal scaling issues
	- typically, NoSQL more scalable → inherently distributed
- ideal:
	- start with a monolith and relational database

## MVC
![[Screenshot 2024-12-12 at 9.13.40 PM.png | center | 350]]

## HTTPRequests
- HTTPRequests: from client to server (or one server to another)
	- GET: retrieve some information
		- generally no message body
	- POST: store new information on the server
		- “input” data typically included in a request body as a hash-map
- less common:
	- PUT: similar to POST, but for changing existing information
	- DELETE: remove information
		- not supported in older HTML forms

## HTTPResponse
- Codes: 3 digit number, first digit denotes “type” of response
	- 1xx: informational responses, request was received and is processing
	- 2xx: successful response
	- 3xx: redirect, further action needs to be taken to complete request
	- 4xx: client error in HTTPRequest
	- 5xx: server error
- Common ones to know:
	- 200: success
	- 201: created, successful POST-PUT
	- 400: bad request
	- 401: unauthorized, page requires authentication but none was provided by the request
	- 403: forbidden, request authentication permissions not authorized
	- 404: not found, requested resource not recognized
	- 408: timeout, request has been idle too long with no response
	- 500: internal server error, error on server side
	- 502: bad gateway, gateway server had an error
	- 503: service unavailable, service cannot handle requests

## REST
- representation state transfer
- 5 components:
	1. statelessness: no state information stored in active memory
	2. idempotency:  output of identical requests is itself identical
	3. addressable resources
	4. well-defined operations in request: in HTTP, GET, POST, PUT, DELETE
	5. structure data is response: HTML, JSON, XML, etc
