##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 33 - GraphQL @connection

<hr>

# GRAPHQL

Overview:
The GraphQL Transform provides a simple to use abstraction that helps you quickly create backends for your web and mobile applications on AWS. With the GraphQL Transform, you define your application’s data model using the GraphQL Schema Definition Language (SDL) and the library handles converting your SDL definition into a set of fully descriptive AWS CloudFormation templates that implement your data model.


For example you might create the backend for a blog like this:

```js
type Blog @model {
  id: ID!
  name: String!
  posts: [Post] @connection(name: "BlogPosts")
}
type Post @model {
  id: ID!
  title: String!
  blog: Blog @connection(name: "BlogPosts")
  comments: [Comment] @connection(name: "PostComments")
}
type Comment @model {
  id: ID!
  content: String
  post: Post @connection(name: "PostComments")
}
```

The GraphQL Transform simplifies the process of developing, deploying, and maintaining GraphQL APIs. With it, you define your API using the GraphQL Schema Definition Language (SDL) and can then use automation to transform it into a fully descriptive cloudformation template that implements the spec. The transform also provides a framework through which you can define your own transformers as `@directives` for custom workflows.



<hr>

## Add relationships between types:

**`@connection`**

* The `@connection` directive enables you to specify relationships between @model types. Currently, this supports one-to-one, one-to-many, and many-to-one relationships.
* You may implement many-to-many relationships using two one-to-many connections and a joining @model type. 

## Usage

Relationships between types are specified by annotating fields on an `@model` object type with the `@connection` directive.

The fields argument can be provided and indicates which fields can be queried by to get connected objects. The `keyName` argument can optionally be used to specify the name of secondary index (an index that was set up using `@key`) that should be queried from the other type in the relationship.

When specifying a `keyName`, the fields argument should be provided to indicate which field(s) will be used to get connected objects. If `keyName` is not provided, then `@connection` queries the target table’s primary index.


## Has one (one-to-one)

```typescript
type Project @model {
  id: ID!
  name: String
  team: Team @connection
}

type Team @model {
  id: ID!
  name: String!
}
```

* You can also define the field you would like to use for the connection by populating the first argument to the fields array and matching it to a field on the type:

```typescript
type Project @model {
  id: ID!
  name: String
  teamID: ID!
  team: Team @connection(fields: ["teamID"])
}

type Team @model {
  id: ID!
  name: String!
}

```

* ***A Has One @connection can only reference the primary index of a model (ie. it cannot specify a “keyName” as described below in the Has Many section).***

## Has many (one-to-many || many-to-one)

```typescript
type Post @model {
  id: ID!
  title: String!
  comments: [Comment] @connection(keyName: "byPost", fields: ["id"])
}

type Comment @model
  @key(name: "byPost", fields: ["postID", "content"]) {
  id: ID!
  postID: ID!
  content: String!
}
```

* Note how a one-to-many connection needs an @key that allows comments to be queried by the postID and the connection uses this key to get all comments whose postID is the id of the post was called on.

### Belongs to

* You can make a connection bi-directional by adding a many-to-one connection to types that already have a one-to-many connection. In this case you add a connection from Comment to Post since each comment belongs to a post:

```typescript
type Post @model {
  id: ID!
  title: String!
  comments: [Comment] @connection(keyName: "byPost", fields: ["id"])
}

type Comment @model
  @key(name: "byPost", fields: ["postID", "content"]) {
  id: ID!
  postID: ID!
  content: String!
  post: Post @connection(fields: ["postID"])
}

```

## Many-to-many connections

* You can implement many to many using two 1-M @connections, an @key, and a joining @model. For example:

```typescript
type Post @model {
  id: ID!
  title: String!
  editors: [PostEditor] @connection(keyName: "byPost", fields: ["id"])
}

// Create a join model and disable queries as you don't need them 
// and can query through Post.editors and User.posts

type PostEditor
  @model(queries: null)
  @key(name: "byPost", fields: ["postID", "editorID"])
  @key(name: "byEditor", fields: ["editorID", "postID"]) {
  id: ID!
  postID: ID!
  editorID: ID!
  post: Post! @connection(fields: ["postID"])
  editor: User! @connection(fields: ["editorID"])
}

type User @model {
  id: ID!
  username: String!
  posts: [PostEditor] @connection(keyName: "byEditor", fields: ["id"])
}

```

* This case is a bidirectional many-to-many which is why two @key calls are needed on the PostEditor model. You can first create a Post and a User, and then add a connection between them with by creating a PostEditor object

>Note that neither the User type nor the Post type have any identifiers of connected objects. The connection info is held entirely inside the PostEditor objects.

## Limit

* The default number of nested objects is 100. You can override this behavior by setting the limit argument. For example:

```typescript
type Post @model {
  id: ID!
  title: String!
  comments: [Comment] @connection(limit: 50)
}

type Comment @model {
  id: ID!
  content: String!
}

```

## Generates

In order to keep connection queries fast and efficient, the GraphQL transform manages global secondary indexes (GSIs) on the generated tables on your behalf when using @connection

>***Note After you have pushed a @connection directive you should not try to change it. If you try to change it, the DynamoDB UpdateTable operation will fail. Should you need to change a @connection, you should add a new @connection that implements the new access pattern, update your application to use the new @connection, and then delete the old @connection when it’s no longer needed.***



<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://docs.amplify.aws/cli/graphql-transformer/connection#belongs-to)
###### [resource2 (queries examples)](https://docs.amplify.aws/cli/graphql-transformer/dataaccess)
<!-- ###### [resource3]() -->
<!-- ###### [resource4]() -->