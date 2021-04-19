##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 13 - Related Resources and Integration Testing

#### ______________________________________________________

# Working with Relationships in Spring Data REST:

## One-to-One Relationship:

### The Data Model

Let's define two entity classes Library and Address having a one-to-one relationship, using the `@OneToOne` annotation. The association is owned by the Library end of the association:

```
@Entity
public class Library {

    @Id
    @GeneratedValue
    private long id;

    @Column
    private String name;

    @OneToOne
    @JoinColumn(name = "address_id")
    @RestResource(path = "libraryAddress", rel="address")
    private Address address;
    
    // standard constructor, getters, setters
}
```

```
@Entity
public class Address {

    @Id
    @GeneratedValue
    private long id;

    @Column(nullable = false)
    private String location;

    @OneToOne(mappedBy = "address")
    private Library library;

    // standard constructor, getters, setters
}
```

The `@RestResource` annotation is **optional** and can be used to customize the endpoint.

***We must be careful to have different names for each association resource***.

The association name defaults to the property name and can be customized using the rel attribute of `@RestResource` annotation:
```
@OneToOne
@JoinColumn(name = "secondary_address_id")
@RestResource(path = "libraryAddress", rel="address")
private Address secondaryAddress;
```

If we were to add the `secondaryAddress` property above to the Library class, we would have two resources named address, and we would encounter a **conflict**.

We can *resolve* this by specifying a different value for the rel attribute or by ***omitting*** the RestResource annotation so that the resource name defaults to `secondaryAddress`.

#### _____________________

### The Repositories

* In order to expose these entities as resources, let's create two repository interfaces for each of them, by extending the CrudRepository interface:

`public interface LibraryRepository extends CrudRepository<Library, Long> {}`
`public interface AddressRepository extends CrudRepository<Address, Long> {}`

#### _____________________

# One-to-Many Relationship:

## The Data Model

* To exemplify a one-to-many relationship, let's add a new Book entity that will represent the “many” end of a relationship with the Library entity:
```
@Entity
public class Book {

    @Id
    @GeneratedValue
    private long id;
    
    @Column(nullable=false)
    private String title;
    
    @ManyToOne
    @JoinColumn(name="library_id")
    private Library library;
    
    // standard constructor, getter, setter
}
```

* Let's add the relationship to the Library class as well:

```
public class Library {
 
    //...
 
    @OneToMany(mappedBy = "library")
    private List<Book> books;
 
    //...
 
}
```

## The Repository

* We also need to create a BookRepository:

`public interface BookRepository extends CrudRepository<Book, Long> { }`

#### _____________________

# Many-to-Many Relationship

* A many-to-many relationship is defined using @ManyToMany annotation, to which we can add @RestResource.

## The Data Model

* To create an example of a many-to-many relationship, let's add a new model class Author that will have a many-to-many relationship with the Book entity:
```
@Entity
public class Author {

    @Id
    @GeneratedValue
    private long id;

    @Column(nullable = false)
    private String name;

    @ManyToMany(cascade = CascadeType.ALL)
    @JoinTable(name = "book_author", 
      joinColumns = @JoinColumn(name = "book_id", referencedColumnName = "id"), 
      inverseJoinColumns = @JoinColumn(name = "author_id", 
      referencedColumnName = "id"))
    private List<Book> books;

    //standard constructors, getters, setters
}
```

* Let's add the association in the Book class as well:

```
public class Book {
 
    //...
 
    @ManyToMany(mappedBy = "books")
    private List<Author> authors;
 
    //...
}
```

## The Repository

* Let's create a repository interface to manage the Author entity:

`public interface AuthorRepository extends CrudRepository<Author, Long> { }`



#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.