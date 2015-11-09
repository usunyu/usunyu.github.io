---
layout: post
title:  "Consumer-Centric API Design"
---
#### Examples of Abstraction:
##### 1. Good Abstraction
Creating a Notification
{% highlight bash %}
POST /notifications
{
  "user_id": "12",
  "message": "Hello World"
}
{
  "id": "1000",
  "user_id": "12",
  "message": "Hello World",
  "medium": "email",
  "created": "2013-01-06T21:02:00Z"
}
{% endhighlight %}

##### 2. Bad Abstraction
Getting User Preference
{% highlight bash %}
GET /get_user_preferences/12
{
  "notification_preference": 1
}
{% endhighlight %}

Sending a Text or Email (Two Endpoints)
{% highlight bash %}
POST /users/12/send_{medium}
{
  "message": "Hello World",
  "sent": "2013-01-06T21:02:00Z"
}
{% endhighlight %}

#### Anatomy of an HTTP Message:
##### 1. HTTP Request
{% highlight bash %}
POST /v1/animal HTTP/1.1
Host: api.example.org
Accept: application/json
Content-Type: application/json
Content-Length: 24
{
  "name": "Gir",
  "animal_type": "12"
}
{% endhighlight %}

##### 1. HTTP Response
{% highlight bash %}
HTTP/1.1 200 OK
Date: Wed, 18 Dec 2013 06:08:22 GMT
Content-Type: application/json
Access-Control-Max-Age: 1728000
Cache-Control: no-cache
{
  "id": "12",
  "created": "2013-12-18T06:08:22Z",
  "modified": null,
  "name": "Gir",
  "animal_type": "12"
}
{% endhighlight %}

#### HTTP Methods:
{% highlight bash %}
• GET (Read)
– Retrieve a specific Resource from the Server
– Retrieve a Collection of Resources from the Server
– Considered Safe: this request should not alter server state
– Considered Idempotent: duplicate subsequent requests should be side-effect free
– Corresponds to a SQL SELECT command

• POST (Create)
– Creates a new Resource on the Server
– Corresponds to a SQL INSERT command

• PUT (Update)
– Updates a Resource on the Server
– Provide the entire Resource
– Considered Idempotent: duplicate subsequent requests should be side-effect free
– Corresponds to a SQL UPDATE command, providing null values for missing columns

• PATCH (Update)
– Updates a Resource on the Server
– Provide only changed attributes
– Corresponds to a SQL UPDATE command, specifying only columns being updated

• DELETE (Delete)
– Destroys a Resource on the Server
– Considered Idempotent: duplicate subsequent requests should be side-effect free
– Corresponds to a SQL DELETE command

• HEAD
– Retrieve metadata about a Resource (just the headers)
– E.g. a hash of the data or when it was last updated
– Considered Safe: this request should not alter server state
– Considered Idempotent: duplicate subsequent requests should be side-effect free

• OPTIONS
– Retrieve information about what the Consumer can do with the Resource
– Considered Safe: this request should not alter server state
– Considered Idempotent: duplicate subsequent requests should be side-effect free
{% endhighlight %}

#### URL Endpoints:
If you were building a fictional API to represent several different Zoo’s, each containing many Animals (with an animal belonging to exactly one Zoo), employees (who can work at multiple Zoos) and keeping track of the species of each animal, you might have the following endpoints:

##### 1. Top-Level Collections
{% highlight bash %}
• https://api.example.com/v1/zoos
• https://api.example.com/v1/animals
• https://api.example.com/v1/animal_types
• https://api.example.com/v1/employees
{% endhighlight %}

##### 2. Specific Endpoints
{% highlight bash %}
• GET /v1/zoos: List all Zoos (perhaps just ID and Name, not too much detail)
• POST /v1/zoos: Create a new Zoo
• GET /v1/zoos/{zoo_id}: Retrieve an entire Zoo resource
• PUT /v1/zoos/{zoo_id}: Update a Zoo (entire resource)
• PATCH /v1/zoos/{zoo_id}: Update a Zoo (partial resource)
• DELETE /v1/zoos/{zoo_id}: Delete a Zoo
• GET /v1/zoos/{zoo_id}/animals: Retrieve a listing of Animals (ID and Name)
• GET /v1/animals: List all Animals (ID and Name)
• POST /v1/animals: Create a new Animal
• GET /v1/animals/{animal_id}: Retrieve an Animal resource
• PUT /v1/animals/{animal_id}: Update an Animal (entire resource)
• PATCH /v1/animals/{animal_id}: Update an Animal (partial resource)
• GET /v1/animal_types: Retrieve a listing (ID and Name) of all Animal Types
• GET /v1/animal_types/{animaltype_id}: Retrieve an entire Animal Type resource
• GET /v1/employees: Retrieve an entire list of Employees
• GET /v1/employees/{employee_id}: Retrieve a specific Employee
• GET /v1/zoos/{zoo_id}/employees: Retrieve a listing of Employees at this Zoo
• POST /v1/employees: Create a new Employee
• POST /v1/zoos/{zoo_id}/employees: Hire an Employee at a specific Zoo
• DELETE /v1/zoos/{zoo_id}/employees/{employee id}: Fire an Employee from a Zoo
{% endhighlight %}

