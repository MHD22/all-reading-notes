##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 12 - Spring RESTful Routing & Static Files

#### ______________________________________________________

# @RequestMapping Basics

## @RequestMapping — by Path

```
@RequestMapping(value = "/ex/foos", method = RequestMethod.GET)
@ResponseBody
public String getFoosBySimplePath() {
    return "Get some Foos";
}
```

## @RequestMapping — the HTTP Method

The HTTP method parameter has no default. So, if we don't specify a value, it's going to map to any HTTP request.

Here's a simple example, similar to the previous one, but this time mapped to an HTTP `POST` request:

```
@RequestMapping(value = "/ex/foos", method = POST)
@ResponseBody
public String postFoos() {
    return "Post some Foos";
}
```

#### ______________________________________________________

#  RequestMapping and HTTP Headers

## @RequestMapping With the headers Attribute

> The mapping can be narrowed even further by specifying a header for the request:

```
@RequestMapping(value = "/ex/foos", headers = "key=val", method = GET)
@ResponseBody
public String getFoosWithHeader() {
    return "Get some Foos with Header";
}
```

> and even multiple headers via the headers attribute of @RequestMapping:

```
@RequestMapping(
  value = "/ex/foos", 
  headers = { "key1=val1", "key2=val2" }, method = GET)
@ResponseBody
public String getFoosWithHeaders() {
    return "Get some Foos with Header";
}
```


## @RequestMapping Consumes and Produces

Mapping **media types produced by a controller** method is worth special attention.

We can map a request based on its Accept header via the @RequestMapping headers attribute introduced above:

```
@RequestMapping(
  value = "/ex/foos", 
  method = GET, 
  headers = "Accept=application/json")
@ResponseBody
public String getFoosAsJsonFromBrowser() {
    return "Get some Foos with Header Old";
}
```

Starting with Spring 3.1, the `@RequestMapping` annotation now has the produces and consumes attributes, specifically for this purpose:
```
@RequestMapping(
  value = "/ex/foos", 
  method = RequestMethod.GET, 
  produces = "application/json"
)
@ResponseBody
public String getFoosAsJsonFromREST() {
    return "Get some Foos with Header New";
}
```

#### ______________________________________________________

# RequestMapping With Path Variables

## Single @PathVariable

> A simple example with a single path variable:

```
@RequestMapping(value = "/ex/foos/{id}", method = GET)
@ResponseBody
public String getFoosBySimplePathWithPathVariable(
  @PathVariable("id") long id) {
    return "Get a specific Foo with id=" + id;
  }
```

***If the name of the method parameter matches the name of the path variable exactly, then this can be simplified by using @PathVariable with no value:***

```
@RequestMapping(value = "/ex/foos/{id}", method = GET)
@ResponseBody
public String getFoosBySimplePathWithPathVariable(
  @PathVariable String id) {
    return "Get a specific Foo with id=" + id;
}
```

## Multiple @PathVariable

* A more complex URI may need to map multiple parts of the URI to multiple values:

```
@RequestMapping(value = "/ex/foos/{fooid}/bar/{barid}", method = GET)
@ResponseBody
public String getFoosBySimplePathWithPathVariables
  (@PathVariable long fooid, @PathVariable long barid) {
    return "Get a specific Bar with id=" + barid + 
      " from a Foo with id=" + fooid;
}
```

## @PathVariable With Regex

* Regular expressions can also be used when mapping the @PathVariable.

* For example, we will restrict the mapping to only accept numerical values for the id:

```
@RequestMapping(value = "/ex/bars/{numericId:[\\d]+}", method = GET)
@ResponseBody
public String getBarsBySimplePathWithPathVariable(
  @PathVariable long numericId) {
    return "Get a specific Bar with id=" + numericId;
}
```

* This will mean that the following URIs will match:

`http://localhost:8080/spring-rest/ex/bars/1`

* But this will not:

`http://localhost:8080/spring-rest/ex/bars/abc`


#### ______________________________________________________

# RequestMapping With Request Parameters

`@RequestMapping` allows easy mapping of URL parameters with the `@RequestParam` annotation.

* We are now mapping a request to a URI:

`http://localhost:8080/spring-rest/ex/bars?id=100`

```
@RequestMapping(value = "/ex/bars", method = GET)
@ResponseBody
public String getBarBySimplePathWithRequestParam(
  @RequestParam("id") long id) {
    return "Get a specific Bar with id=" + id;
}
```

For more advanced scenarios, `@RequestMapping` can optionally define the parameters as yet another way of narrowing the request mapping:

```
@RequestMapping(value = "/ex/bars", params = "id", method = GET)
@ResponseBody
public String getBarBySimplePathWithExplicitRequestParam(
  @RequestParam("id") long id) {
    return "Get a specific Bar with id=" + id;
}
```

**Even more flexible mappings are allowed. Multiple params values can be set, and not all of them have to be used.**

#### ______________________________________________________


# RequestMapping Corner Cases

## @RequestMapping — Multiple Paths Mapped to the Same Controller Method

Although a single `@RequestMapping` path value is usually used for a single controller method (just good practice, not a hard and fast rule), there are some cases where mapping multiple requests to the same method may be necessary.

In that case, `the value attribute of @RequestMapping does accept multiple mappings`, not just a single one:

```
@RequestMapping(
  value = { "/ex/advanced/bars", "/ex/advanced/foos" }, 
  method = GET)
@ResponseBody
public String getFoosOrBarsByPath() {
    return "Advanced - Get some Foos or Bars";
}
```

## @RequestMapping — Multiple HTTP Request Methods to the Same Controller Method

Multiple requests using different HTTP verbs can be mapped to the same controller method:

```
@RequestMapping(
  value = "/ex/foos/multiple", 
  method = { RequestMethod.PUT, RequestMethod.POST }
)
@ResponseBody
public String putAndPostFoos() {
    return "Advanced - PUT and POST within single method";
}
```

## @RequestMapping — a Fallback for All Requests

* To implement a simple fallback for all requests using a particular HTTP method, for example, for a GET:

```
@RequestMapping(value = "*", method = RequestMethod.GET)
@ResponseBody
public String getFallback() {
    return "Fallback for GET Requests";
}
```

* or even for all requests:

```
@RequestMapping(
  value = "*", 
  method = { RequestMethod.GET, RequestMethod.POST ... })
@ResponseBody
public String allFallback() {
    return "Fallback for All Requests";
}
```

## Ambiguous Mapping Error

* The ambiguous mapping error occurs when Spring evaluates two or more request mappings to be the same for different controller methods. A request mapping is the same when it has the same HTTP method, URL, parameters, headers, and media type.

* For example, this is an ambiguous mapping:

```
@GetMapping(value = "foos/duplicate" )
public String duplicate() {
    return "Duplicate";
}

@GetMapping(value = "foos/duplicate" )
public String duplicateEx() {
    return "Duplicate";
}

```

#### ______________________________________________________

# New Request Mapping Shortcuts

*Spring Framework 4.3 introduced a few new HTTP mapping annotations, all based on @RequestMapping:*

* `@GetMapping`
* `@PostMapping`
* `@PutMapping`
* `@DeleteMapping`
* `@PatchMapping`

These new annotations can improve the readability and reduce the verbosity of the code.

Let's look at these new annotations in action by creating a RESTful API that supports CRUD operations:

```
@GetMapping("/{id}")
public ResponseEntity<?> getBazz(@PathVariable String id){
    return new ResponseEntity<>(new Bazz(id, "Bazz"+id), HttpStatus.OK);
}

@PostMapping
public ResponseEntity<?> newBazz(@RequestParam("name") String name){
    return new ResponseEntity<>(new Bazz("5", name), HttpStatus.OK);
}

@PutMapping("/{id}")
public ResponseEntity<?> updateBazz(
  @PathVariable String id,
  @RequestParam("name") String name) {
    return new ResponseEntity<>(new Bazz(id, name), HttpStatus.OK);
}

@DeleteMapping("/{id}")
public ResponseEntity<?> deleteBazz(@PathVariable String id){
    return new ResponseEntity<>(new Bazz(id), HttpStatus.OK);
}

```




#### ______________________________________________________

# Spring Configuration

The Spring MVC Configuration is simple enough, considering that our FooController is defined in the following package:
```
package org.baeldung.spring.web.controller;

@Controller
public class FooController { ... }
```

#### ______________________________________________________

# CrudRepository, JpaRepository, and PagingAndSortingRepository

## Spring Data Repositories

* Let's start with the `JpaRepository` – which extends `PagingAndSortingRepository` and, in turn, the `CrudRepository`.
* Each of these defines its own functionality:

  * `CrudRepository` provides CRUD functions
  * `PagingAndSortingRepository` provides methods to do pagination and sort records
  * `JpaRepository` provides JPA related methods such as flushing the persistence context and delete records in a batch
And so, because of this inheritance relationship, the JpaRepository contains the full API of CrudRepository and PagingAndSortingRepository.

* When we don't need the full functionality provided by `JpaRepository` and `PagingAndSortingRepository`, we can simply use the `CrudRepository`.

* Let's now have a look at a quick example to understand these APIs better.
We'll start with a simple Product entity:
```
@Entity
public class Product {

    @Id
    private long id;
    private String name;

    // getters and setters
}
```

* And let's implement a simple operation – find a Product based on its name:
```
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    Product findByName(String productName);
}
```


## CrudRepository

* Notice the typical CRUD functionality:

`save(…)` – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
`findOne(…)` – get a single entity based on passed primary key value
`findAll()` – get an Iterable of all available entities in database
`count()` – return the count of total entities in a table
`delete(…)` – delete an entity based on the passed object
`exists(…)` – verify if an entity exists based on the passed primary key value


## JpaRepository

* let's look at each of these methods in brief:

`findAll()` – get a List of all available entities in database
`findAll(…)` – get a List of all available entities and sort them using the provided condition
`save(…)` – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
`flush()` – flush all pending task to the database
`saveAndFlush(…)` – save the entity and flush changes immediately
`deleteInBatch(…)` – delete an Iterable of entities. Here, we can pass multiple objects to delete them in a batch

#### ______________________________________________________

# Downsides of Spring Data Repositories

Beyond all the very useful advantages of these repositories, there are some basic downsides of directly depending on these as well:

1. we couple our code to the library and to its specific abstractions, such as `Page` or `Pageable`; that's of course not unique to this library – but we do have to be careful not to expose these internal implementation details
2. by extending e.g. CrudRepository, we expose a complete set of persistence method at once. This is probably fine in most circumstances as well but we might run into situations where we'd like to gain more fine-grained control over the methods exposed, e.g. to create a ReadOnlyRepository that doesn't include the save(…) and delete(…) methods of CrudRepository

#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.


