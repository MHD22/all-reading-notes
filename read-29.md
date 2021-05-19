
##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 29 - Room

<hr>

# Save data in a local database using Room:

* The Room persistence library provides an abstraction layer over SQLite to allow fluent database access while harnessing the full power of SQLite. In particular, Room provides the following benefits:
  * Compile-time verification of SQL queries.
  * Convenience annotations that minimize repetitive and error-prone boilerplate code.
  * Streamlined database migration paths.
Because of these considerations, we highly **recommend** that you use Room instead of using the SQLite APIs directly.


## Primary components

There are three major components in Room:

* The `database` class that holds the database and serves as the main access point for the underlying connection to your app's persisted data.
* `Data entities` that represent tables in your app's database.
* `Data access objects (DAOs)` that provide methods that your app can use to query, update, insert, and delete data in the database.


## Sample implementation:

***Data entity***
The following code defines a User data entity. Each instance of User represents a row in a user table in the app's database.

```java
@Entity
public class User {
    @PrimaryKey
    public int uid;

    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    public String lastName;
}

```

***Data access object (DAO)***

The following code defines a DAO called `UserDao`. UserDao provides the methods that the rest of the app uses to interact with data in the user table.

```java
@Dao
public interface UserDao {
    @Query("SELECT * FROM user")
    List<User> getAll();

    @Query("SELECT * FROM user WHERE uid IN (:userIds)")
    List<User> loadAllByIds(int[] userIds);

    @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
           "last_name LIKE :last LIMIT 1")
    User findByName(String first, String last);

    @Insert
    void insertAll(User... users);

    @Delete
    void delete(User user);
}
```

***Database***

The following code defines an AppDatabase class to hold the database. AppDatabase defines the database configuration and serves as the app's main access point to the persisted data. The database class must satisfy the following conditions:

* The class must be annotated with a @Database annotation that includes an entities array that lists all of the data entities associated with the database.
* The class must be an abstract class that extends RoomDatabase.
* For each DAO class that is associated with the database, the database class must define an abstract method that has zero arguments and returns an instance of the DAO class.

```java
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```

## Usage

After you have defined the data entity, the DAO, and the database object, you can use the following code to create an instance of the database:

```java
AppDatabase db = Room.databaseBuilder(getApplicationContext(),
        AppDatabase.class, "database-name").build();
```

You can then use the abstract methods from the AppDatabase to get an instance of the DAO. In turn, you can use the methods from the DAO instance to interact with the database:

```java
UserDao userDao = db.userDao();
List<User> users = userDao.getAll();
```

<hr>

# Defining data using Room entities:

## Anatomy of an entity:

You define each Room entity as a class that is annotated with @Entity. A Room entity includes fields for each column in the corresponding table in the database, including one or more columns that comprise the primary key.

The following code is an example of a simple entity that defines a User table with columns for ID, first name, and last name:

```java
@Entity
public class User {
    @PrimaryKey
    public int id;

    public String firstName;
    public String lastName;
}
```

>Note: To persist a field, Room must have access to it. You can make sure Room has access to a field either by making it public or by providing getter and setter methods for it.

* If you want the table to have a different name, set the tableName property of the @Entity annotation. 
* If you want a column to have a different name, add the @ColumnInfo annotation to the field and set the name property. 
like following:

```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey
    public int id;

    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    public String lastName;
}
```

>Note: Table and column names in SQLite are case-insensitive.


***Define a primary key***

The most straightforward way of doing this is to annotate a single column with @PrimaryKey:

```java
@PrimaryKey
public int id;
```

***Define a composite primary key***

If you need instances of an entity to be uniquely identified by a combination of multiple columns, you can define a composite primary key by listing those columns in the primaryKeys property of @Entity:

```java
@Entity(primaryKeys = {"firstName", "lastName"})
public class User {
    public String firstName;
    public String lastName;
}
```

### Ignore fields:

If an entity has fields that you don't want to persist, you can annotate them using `@Ignore`, as shown in the following code snippet:

```java
@Entity
public class User {
    @PrimaryKey
    public int id;

    public String firstName;
    public String lastName;

    @Ignore
    Bitmap picture;
}
```

* In cases where an entity inherits fields from a parent entity, it's usually easier to use the `ignoredColumns` property of the @Entity attribute:

```java
@Entity(ignoredColumns = "picture")
public class RemoteUser extends User {
    @PrimaryKey
    public int id;

    public boolean hasVpn;
}

```

for more advanced topics like (provide table search support  "full_text search", index specific columns, and Include AutoValue-based objects ) visit the rosourse 2 link and go dipper in these topics.

<hr>

# Define relationships between objects:

## Create embedded objects:

For instance, your User class can include a field of type Address, which represents a composition of fields named street, city, state, and postCode. To store the composed columns separately in the table, include an Address field in the User class that is annotated with @Embedded, as shown in the following code snippet:

```java
public class Address {
    public String street;
    public String state;
    public String city;

    @ColumnInfo(name = "post_code") public int postCode;
}

@Entity
public class User {
    @PrimaryKey public int id;

    public String firstName;

    @Embedded public Address address;
}
```

>Note: Embedded fields can also include other embedded fields.

## Define one-to-one relationships

* First, create a class for each of your two entities. One of the entities must include a variable that is a reference to the primary key of the other entity.

```java
@Entity
public class User {
    @PrimaryKey public long userId;
    public String name;
    public int age;
}

@Entity
public class Library {
    @PrimaryKey public long libraryId;
    public long userOwnerId;
}
```

In order to query the list of users and corresponding libraries, you must first model the one-to-one relationship between the two entities. 
To do this:

* create a new data class where each instance holds an instance of the parent entity and the corresponding instance of the child entity. 
* Add the `@Relation` annotation to the instance of the child entity, 
* with `parentColumn` set to the name of the primary key column of the parent entity * and `entityColumn` set to the name of the column of the child entity that references the parent entity's primary key.

```java
public class UserAndLibrary {
    @Embedded public User user;
    @Relation(
         parentColumn = "userId",
         entityColumn = "userOwnerId"
    )
    public Library library;
}
```

Finally, add a method to the DAO class that returns all instances of the data class that pairs the parent entity and the child entity. This method requires Room to run two queries, so add the `@Transaction` annotation to this method to ensure that the whole operation is performed atomically.

```java
@Transaction
@Query("SELECT * FROM User")
public List<UserAndLibrary> getUsersAndLibraries();
```

## Define one-to-many relationships:

* First, create a class for each of your two entities. As in the previous example, the child entity must include a variable that is a reference to the primary key of the parent entity.

```java
@Entity
public class User {
    @PrimaryKey public long userId;
    public String name;
    public int age;
}

@Entity
public class Playlist {
    @PrimaryKey public long playlistId;
    public long userCreatorId;
    public String playlistName;
}
```

* In order to query the list of users and corresponding playlists, you must first model the one-to-many relationship between the two entities. 
* To do this, create a new data class where each instance holds an instance of the parent entity and a list of all corresponding child entity instances. 
* Add the `@Relation` annotation to the instance of the child entity, 
* with `parentColumn` set to the name of the primary key column of the parent entity 
* and `entityColumn` set to the name of the column of the child entity that references the parent entity's primary key.

```java
public class UserWithPlaylists {
    @Embedded public User user;
    @Relation(
         parentColumn = "userId",
         entityColumn = "userCreatorId"
    )
    public List<Playlist> playlists;
}
```

* Finally, add a method to the DAO class that returns all instances of the data class that pairs the parent entity and the child entity. This method requires Room to run two queries, so add the @Transaction annotation to this method to ensure that the whole operation is performed atomically.

```java
@Transaction
@Query("SELECT * FROM User")
public List<UserWithPlaylists> getUsersWithPlaylists();
```

## Define many-to-many relationships

* create a third class to represent an `associative` entity (or `cross-reference` table) between the two entities. The `cross-reference` table must have columns for the primary key from each entity in the many-to-many relationship represented in the table.

```java
@Entity
public class Playlist {
    @PrimaryKey public long playlistId;
    public String playlistName;
}

@Entity
public class Song {
    @PrimaryKey public long songId;
    public String songName;
    public String artist;
}

@Entity(primaryKeys = {"playlistId", "songId"})
public class PlaylistSongCrossRef {
    public long playlistId;
    public long songId;
}

```

***The next step depends on how you want to query these related entities.***

* If you want to query playlists and a list of the corresponding songs for each playlist, create a new data class that contains a single `Playlist` object and a list of all of the `Song` objects that the playlist includes.

* If you want to query songs and a list of the corresponding playlists for each, create a new data class that contains a single `Song` object and a list of all of the `Playlist` objects in which the song is included.

In either case, model the relationship between the entities by using the `associateBy` property in the `@Relation` annotation in each of these classes to identify the **cross-reference** entity providing the relationship between the `Playlist` entity and the `Song` entity.

```java
public class PlaylistWithSongs {
    @Embedded public Playlist playlist;
    @Relation(
         parentColumn = "playlistId",
         entityColumn = "songId",
         associateBy = @Junction(PlaylistSongCrossref.class)
    )
    public List<Song> songs;
}

public class SongWithPlaylists {
    @Embedded public Song song;
    @Relation(
         parentColumn = "songId",
         entityColumn = "playlistId",
         associateBy = @Junction(PlaylistSongCrossref.class)
    )
    public List<Playlist> playlists;
}
```

Finally, add a method to the DAO class to expose the query functionality your app needs.

* `getPlaylistsWithSongs`: This method queries the database and returns all of the resulting `PlaylistWithSongs` objects.
* `getSongsWithPlaylists`: This method queries the database and returns all of the resulting `SongWithPlaylists` objects.

These methods each require Room to run two queries, so add the` @Transaction` annotation to both methods to ensure that the whole operation is performed atomically.

```java
@Transaction
@Query("SELECT * FROM Playlist")
public List<PlaylistWithSongs> getPlaylistsWithSongs();

@Transaction
@Query("SELECT * FROM Song")
public List<SongWithPlaylists> getSongsWithPlaylists();
```


>for more advanced details..like: (Define nested relationships) see the `resource 3` at the end of this article.

<hr>

# Accessing data using Room DAOs:

## Anatomy of a DAO:

* The following code is an example of a simple DAO that defines methods for inserting, deleting, and selecting User objects in a Room database:

```java
@Dao
public interface UserDao {
    @Insert
    void insertAll(User... users);

    @Delete
    void delete(User user);

    @Query("SELECT * FROM user")
    List<User> getAll();
}
```

There are two types of DAO methods that define database interactions:

* `Convenience` methods that let you insert, update, and delete rows in your database without writing any SQL code.
* `Query` methods that let you write your own SQL query to interact with the database.

### Convenience methods:

***Insert***

```java
@Dao
public interface UserDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    public void insertUsers(User... users);

    @Insert
    public void insertBothUsers(User user1, User user2);

    @Insert
    public void insertUsersAndFriends(User user, List<User> friends);
}

```

If the @Insert method receives a single parameter, it can return a long value, which is the new rowId for the inserted item. If the parameter is an array or a collection, then the method should return an array or a collection of long values instead, with each value as the rowId for one of the inserted items. 

***Update***

```java
@Dao
public interface UserDao {
    @Update
    public void updateUsers(User... users);
}
```

* If there is no row with the same primary key, Room makes no changes.
* An @Update method can optionally return an int value indicating the number of rows that were updated successfully.

***Delete***

```java
@Dao
public interface UserDao {
    @Delete
    public void deleteUsers(User... users);
}
```

* If there is no row with the same primary key, Room makes no changes.
* A @Delete method can optionally return an int value indicating the number of rows that were deleted successfully.

### Query methods:

* Room validates SQL queries at compile time. This means that if there's a problem with your query, a compilation error occurs instead of a runtime failure.

***Return a subset of a table's columns***

Most of the time, you only need to return a subset of the columns from the table that you are querying. For example, your UI might display just the first and last name for a user instead of every detail about that user. In order to save resources and streamline your query's execution, you should only query the fields that you need.

Room allows you to return a simple object from any of your queries as long as you can map the set of result columns onto the returned object. For example, you can define the following object to hold a user's first and last name:

```java
public class NameTuple {
    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    @NonNull
    public String lastName;
}
```

Then, you can return that simple object from your query method:

```java
@Query("SELECT first_name, last_name FROM user")
public List<NameTuple> loadFullName();
```

***Pass simple parameters to a query***

```java
@Query("SELECT * FROM user WHERE age BETWEEN :minAge AND :maxAge")
public User[] loadAllUsersBetweenAges(int minAge, int maxAge);

@Query("SELECT * FROM user WHERE first_name LIKE :search " +
       "OR last_name LIKE :search")
public List<User> findUserWithName(String search);
```

***Pass a collection of parameters to a query***

```java
@Query("SELECT * FROM user WHERE region IN (:regions)")
public List<User> loadUsersFromRegions(List<String> regions);
```

>For more advanced topics on DAO like (Query multiple tables, Special return types:(Paginated queries with the Paging library,Direct cursor access) ) see the `source 4` at the end.

<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://developer.android.com/training/data-storage/room#java)
###### [resource2](https://developer.android.com/training/data-storage/room/defining-data)
###### [resource3](https://developer.android.com/training/data-storage/room/relationships)
###### [resource4](https://developer.android.com/training/data-storage/room/accessing-data#java)