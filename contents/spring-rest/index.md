## Agenda

- REST
- RESTful Web Service
- RESTful Web Service with Spring REST

---

## REST

- REST stands for REpresentational State Transfer
- REST is a web standards based architecture and uses HTTP Protocol for data communication
- a REST Server simply provides access to resources and the REST client accesses and presents the resources
- REST uses various representations to represent a resource like Text, JSON and XML

---

## HTTP Methods

- GET - Provides a read only access to a resource
- PUT - Used to create a new resource
- DELETE - Used to remove a resource
- POST - Used to update an existing resource or create a new resource
- OPTIONS - Used to get the supported operations on a resource

---

## RESTful Web Service

- Web Service: a collection of open protocols and standards used for exchanging data between applications or systems
- RESTful Web Services: 
  - web services based on REST Architecture
  - use HTTP methods to implement the concept of REST architecture
  - defines a URI (Uniform Resource Identifier), which is a service that provides resource representation such as JSON and a set of HTTP Methods

---

## RESTful Web Service - Resources and Messages

- REST architecture treats every content as a resource
  - ex: Text Files, Html Pages, Images, Videos or Dynamic Business Data
- REST architecture transfer content via HTTP Protocols: HTTP Request and HTTP Response (messages)
- HTTP Request
  - Verb – Indicates the HTTP methods such as GET, POST, DELETE, PUT, etc
  - URI – Uniform Resource Identifier (URI) to identify the resource on the server
  - HTTP Version – Indicates the HTTP version. For example, HTTP v1.1.
  - Request Header – Contains metadata for the HTTP Request message as key-value pairs, ex: client (or browser) type, format supported by the client, format of the message body, cache settings, etc
  - Request Body – Message content or Resource representation

 ![alt text](images/httprequest.png "HTTP Request")

---

## RESTful Web Service - Resources and Messages (Contd.)

- HTTP Response
  - Status/Response Code – Indicates the Server status for the requested resource. ex: 404 means resource not found and 200 means response is ok
  - HTTP Version – Indicates the HTTP version. For example HTTP v1.1.
  - Response Header – Contains metadata for the HTTP Response message as key-value pairs. ex: content length, content type, response date, server type, etc
  - Response Body – Response message content or Resource representation

![alt text](images/httpresponse.png "HTTP Response")

## RESTful Web Service with Spring REST

- Spring REST overview
- Revisit Repository
- REST Controller
- Service

