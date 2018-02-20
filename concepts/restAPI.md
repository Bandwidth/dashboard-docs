# RESTful API and Environments {#top}

## Overview

* [API Methods - REST](#api-methods)
* [Usage of REST with the Bandwidth Phone Number API](#rest-usage)
* [Base URLs and Environments](#base-urls)
* [REST URLs and Parameters](#rest-urls)

## API Methods - REST {#api-methods}

Bandwidth API’s support REST web service technologies.
The Bandwidth API’s are designed to provide customers with a simple way of interfacing with the number intelligence platform following standard industry practices.  Wikipedia (reference http://en.wikipedia.org/wiki/Representational_state_transfer) defines REST as:

> A RESTful web service (also called a RESTful web API) is a simple web service implemented using HTTP and the principles of REST. It is a collection of resources, with four defined aspects:

> * A base URI for the web service, such as http://example.com/resources/
> * An Internet media type of the data supported by the web service. This is often JSON, XML or YAML but can be any other valid Internet media type.
> * A set of operations supported by the web service using HTTP methods (e.g., GET, PUT, POST, or DELETE).
The API must be hypertext driven.


## Usage of REST with the Bandwidth Phone Number API {#rest-usage}

Usage of the REST model for APIs involves using the tools provided by the REST model to create a consistent API experience.  Bandwidth’s Phone Number APIs use:

### GET
<code class="get">GET</code>  for querying static resources, often paginating the results to manage large result sets that arise from dealing with large data sets such as Telephone Numbers
* Search available number inventory
  * REST HTTP <code class="get">GET</code>
* Reporting
  * REST HTTP <code class="get">GET</code>

### POST and PUT
<code class="post">POST</code> for creating resources, and <code class="put">PUT</code> for updating resources.  In many cases <code class="post">POST</code> results in the return of an identifier that is used in the subsequent management of that resource.

* Ordering numbers
  * REST HTTP <code class="post">POST</code>
* SIP Peer Management
  * REST HTTP <code class="post">POST</code> and <code class="put">PUT</code>

### DELETE
DELETE is seldom used.  In most cases, resources are persisted as history records and cannot be deleted.

##  Base URLs and Environments {#base-urls}

All resources are located at:
* Base URL for Interop – `https://test.dashboard.bandwidth.com/api`
* Base URL for Production – `https://dashboard.bandwidth.com/api`

The payload/content of the REST messages is encoded in XML.  To provide information to the endpoints on how to interpret the content, a HTTP Header value is used to identify the content as XML content.  To achieve this the header “Content-Type” must be set as “application/xml” for all messages.  The “Authorization” header is used to include the [Basic-encoded credentials](security.md)

API Calls to the Base URL will return an empty document (no content), and not perform any authentication or other processing.  A REST resource is required for any action on the part of the server.

## REST URLs and Parameters {#rest-urls}

Bandwidth attempts to maintain syntactic consistency in the specification and execution of the Bandwidth Dashboard API.  Some of the guidelines that serve as goals are:

* Backwards compatibility – Bandwidth remains committed to backwards compatibility in its API implementation, occasionally at the expense of API complexity or additional query parameters.
* Date flexibility – for most API calls that have dates as query parameters, the format yyyy-mm-dd or yy-mm-dd will be supported
* Case insensitivity – in many but not all cases elements of the REST URI are case insensitive.  Some resource names and identifiers are case sensitive by their nature, and some have not yet been converted to case insensitive parsing.
* Providing generic, robust resource descriptors for URIs remains a focus of the ongoing development, and will be attempted wherever changes can be made that do not violate our commitment to backwards compatibility.

