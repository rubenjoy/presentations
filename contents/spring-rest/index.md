## Training outcome

- Introduced to REST and RESTful Web services
- Introduced to Spring Data REST Repository
- Hands on simple project with Spring Data REST Repository
- Introduced to Spring Data REST Controller
- Hands on simple project with Spring Data REST Controller


## Agenda

- REST
- RESTful Web Service
- RESTful Web Service with Spring Data REST

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

---

## RESTful Web Service - Security

- Validation – Validate all inputs on the server. Protect your server against SQL or NoSQL injection attacks
- Session Based Authentication – Use session based authentication to authenticate a user whenever a request is made to a Web Service method
- No Sensitive Data in the URL – Never use username, password or session token in a URL, these values should be passed to Web Service via the POST method
- Restriction on Method Execution – Allow restricted use of methods like GET, POST and DELETE methods. The GET method should not be able to delete data
- Validate Malformed XML/JSON – Check for well-formed input passed to a web service method
- Throw generic Error Messages – A web service method should use HTTP error messages like 403 to show access forbidden, etc

---

## HTTP Code

- __200__ OK - show success
- __201__ CREATED - when a resource is successfully created using POST or PUT request. Returns link to the newly created resource using the location header
- __204__ NO CONTENT - when response body is empty, ex: a DELETE request
- __304__ NOT MODIFIED – used to reduce network bandwidth usage in case of conditional GET requests. Response body should be empty. Headers should have date, location, etc
- __400__ BAD REQUEST – states that an invalid input is provided. ex: validation error, missing data
- __401__ UNAUTHORIZED – states that user is using invalid or wrong authentication token
- __403__ FORBIDDEN – states that the user is not having access to the method being used. For example, Delete access without admin rights
- __404__ NOT FOUND – states that the method is not available
- __409__ CONFLICT – states conflict situation while executing the method. For example,
adding duplicate entry
- __500__ INTERNAL SERVER ERROR – states that the server has thrown some exception while executing the method

---

## RESTful Web Service with Spring Data REST

- Resource: Domain objects or model
- Message construction: Spring REST Repository and/or Spring REST Controller

---

## Domain objects or model

- POJO representation of domain
- Can be a simple POJO class or database entity
- Example:

```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```
Which will be returned as JSON Object in Response body:
```json
{
    "id": 1,
    "content": "Hello, World!"
}
```

---

## Entity

- Java representation for database relations
- @Entity - annotate a class as Entity
- @Table - define database table name
- @Id - define table Id
- @Column - define column name for field
- @OneToMany - define relations one to many between entity

Example:
```java
@Entity
@Table(name="employee")
public class Employee {
    @Id
    @Column(name="emp_id")
    @GeneratedValue(strategy= GenerationType.AUTO)
    private long empId;
    @Column(name="first_name", nullable = false)
    private String firstName;
    @Column(name="last_name", nullable = false)
    private String lastName;
    @Column(name="gender", nullable = false)
    private String gender;
    @Column(name="dob")
    private Date dob;
    @OneToMany(mappedBy="employee", cascade = CascadeType.ALL)
    private Set<OfficeLocation> officeLocations;
}
```

---

## Spring REST Repository

- Repository: A mechanism for encapsulating storage, retrieval, and search behavior which emulates a collection of objects
- Spring REST Repository Type
  - CRUD Repository
  - PagingAndSortingRepository
    - CRUD Repository + paging and sorting capability   
  - Persistence technology-specific abstractions
    - ex: JpaRepository or MongoRepository

---

## CRUD Repository

- Provides sophisticated CRUD functionality for the entity class that is being managed

![alt text](images/crudrepository.png "CRUD Repository")

1. Saves the given entity
2. Returns the entity identified by the given id
3. Returns all entities
4. Returns the number of entities
5. Deletes the given entity
6. Indicates whether an entity with the given id exists

---

## Spring REST Repository resources

- Fundamentals  
- The collection resource
- The item resource
- The association resource
- The search resource
- The query method resource

---

## Fundamentals

```java
public interface EmployeeRepository extends PagingAndSortingRepository<Employee,Long> {
    
}
```

- Exposes a collection resource at /employees
- Exposes an item resource for each of the items managed by the repository under the URI template /employees/{id}
- Default status codes:
  - 200 OK - for plain GET requests
  - 201 Created - for POST and PUT requests that create new resources
  - 204 No Content - for PUT, PATCH, and DELETE
- Resource discoverability
  - Spring Data REST uses HAL to render responses
  - HAL defines links to be contained in a property of the returned document
  - ex: GET http://localhost:8080 will return
```json
{
  "_links": {
    "employees": {
      "href": "http://localhost:8080/employees{?page,size,sort}",
      "templated": true
    }
  }
}
```

---

## The collection resource

- Spring Data REST exposes a collection resource named after the uncapitalized, pluralized version of the domain class the exported repository is handling
- Supported HTTP Methods:
  - GET - Returns all entities the repository servers through its findAll(…) method
  - HEAD - Returns whether the collection resource is available
  - POST - Creates a new entity from the given request body
- Example:
GET http://localhost:8080/employees will return
```json
{
  "_embedded": {
    "employees": [
      {
        "empId": 502,
        "firstName": "Cal",
        "lastName": "Supreme",
        "gender": "Male",
        "dob": "1989-03-09",
        "_links": {
          "self": {
            "href": "http://localhost:8080/employees/502"
          },
          "employee": {
            "href": "http://localhost:8080/employees/502"
          },
           "officeLocations": {
             "href": "http://localhost:8080/employees/502/officeLocations"
           }
        }
      }//,
      // ...
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/employees"
    },
    "profile": {
      "href": "http://localhost:8080/profile/employees"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 3,
    "totalPages": 1,
    "number": 0
  }
}
```

---

## The item resource

- Spring Data REST exposes a resource for individual collection items as sub-resources of the collection resource
- Supported HTTP methods
  - GET - Returns a single entity
  - HEAD - Returns whether the item resource is available
  - PUT - Replaces the state of the target resource with the supplied request body
  - PATCH - Similar to PUT but partially updating the resources state
  - DELETE - Deletes the resource exposed
- Example:
GET http://localhost:8080/employees/502 will return
```json
{
  "empId": 502,
  "firstName": "Cal",
  "lastName": "Supreme",
  "gender": "Male",
  "dob": "1989-03-09",
  "_links": {
      "self": {
        "href": "http://localhost:8080/employees/502"
      },
      "employee": {
        "href": "http://localhost:8080/employees/502"
      },
      "officeLocations": {
        "href": "http://localhost:8080/employees/502/officeLocations"
      }
    }
  }
```

---

## The association resource

- Spring Data REST exposes sub-resources of every item resource for each of the associations the item resource has
- Supported HTTP methods
  - GET - Returns the state of the association resource
  - PUT - Binds the resource pointed to by the given URI(s) to the resource
  - POST - Only supported for collection associations. Adds a new element to the collection
  - DELETE - Unbinds the association
- Example:
GET http://localhost:8080/employees/502/officeLocations will return
```json
{
  "_embedded": {
    "officeLocations": [
      {
        "locId": "l_502_1",
        "startDate": "2017-04-12",
        "endDate": null,
        "officeLocation": "Bandung",
        "_links": {
          "self": {
            "href": "http://localhost:8080/officeLocations/l_502_1"
          },
          "officeLocation": {
            "href": "http://localhost:8080/officeLocations/l_502_1"
          },
          "employee": {
            "href": "http://localhost:8080/officeLocations/l_502_1/employee"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/employees/502/officeLocations"
    }
  }
}
```

---

## The search resource

- The search resource returns links for all query methods exposed by a repository
- The path and name of the query method resources can be modified using @RestResource on the method declaration
- Supported HTTP methods
  - GET - Returns a list of links pointing to the individual query method resources
  - HEAD - Returns whether the search resource is available. A 404 return code indicates no query method resources available at all
- Example:
GET http://localhost:8080/employees/search will return
 ```json
{
  "_links": {
    "findByName": {
      "href": "http://localhost:8080/employees/search/findByName{?name,page,size,sort}",
      "templated": true
    },
    "self": {
      "href": "http://localhost:8080/employees/search"
    }
  }
}
 ```
 ---
 
 ## The query method resource
 
 - The query method resource executes the query exposed through an individual query method on the repository interface
 - Supported HTTP methods
   - GET - Returns the result of the query execution
   - HEAD - Returns whether a query method resource is available
 - Example:
 GET http://localhost:8080/employees/search/findByName?name=cal will return
 ```json
 {
   "_embedded": {
     "employees": [
       {
         "empId": 502,
         "firstName": "Cal",
         "lastName": "Supreme",
         "gender": "Male",
         "dob": "1989-03-09",
         "_links": {
           "self": {
             "href": "http://localhost:8080/employees/502"
           },
           "employee": {
             "href": "http://localhost:8080/employees/502"
           }
         }
       }
     ]
   },
   "_links": {
     "self": {
       "href": "http://localhost:8080/employees/search/findByName?name=cal"
     }
   },
   "page": {
     "size": 20,
     "totalElements": 1,
     "totalPages": 1,
     "number": 0
   }
 }
```

---

## Spring REST Controller

- If you need custom behavior that is not supported by Spring REST Repository
- If you need a different response body with the domain object

Example:

```java
@RestController
public class EmployeeController {
    
    @RequestMapping(value = "employees/{empId}/score", method = RequestMethod.GET)
    AbstractMap.SimpleEntry<String, Double> getEmployeeByFilter(@PathVariable long empId) {
        Employee employee = verifyEmployee(empId);
        double result = employeeService.calcualateScore(employee);
        return new AbstractMap.SimpleEntry<String, Double>("score", result);
    }
}
```

- @RestController - annotate a class as REST Controller
- @RequestMapping - define path/URI and HTTP method if needed
- @PathVariable - URI Template Patterns
- @RequestParam - define GET parameter
- @RequestBody - define request body

---

## Exercise

TODO

---

## References

- https://www.tutorialspoint.com/restful/restful_web_services_tutorial.pdf
- http://docs.spring.io/spring-data/rest/docs/current/reference/html/
- http://docs.spring.io/spring-framework/docs/current/spring-framework-reference/html/mvc.html
 
