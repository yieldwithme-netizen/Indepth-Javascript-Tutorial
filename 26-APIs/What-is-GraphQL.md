# What is GraphQL

## Definition

**GraphQL** is a query language and runtime for APIs developed by Facebook. It allows clients to request exactly the data they need, no more or no less. Unlike REST, GraphQL has a single endpoint and the client specifies the data structure in the query.

## GraphQL vs REST

```javascript
// REST: Multiple endpoints
GET /api/users/123
GET /api/users/123/posts
GET /api/users/123/followers

// GraphQL: Single endpoint
POST /graphql
{
  "query": `
    query {
      user(id: 123) {
        name
        email
        posts {
          title
          content
        }
        followers {
          name
        }
      }
    }
  `
}
```

## Schema Definition

```javascript
const { ApolloServer, gql } = require('apollo-server');

// Define schema using SDL (Schema Definition Language)
const typeDefs = gql`
  type User {
    id: ID!
    name: String!
    email: String!
    posts: [Post!]!
    followers: [User!]!
  }

  type Post {
    id: ID!
    title: String!
    content: String!
    author: User!
    comments: [Comment!]!
  }

  type Comment {
    id: ID!
    text: String!
    author: User!
  }

  type Query {
    users: [User!]!
    user(id: ID!): User
    posts: [Post!]!
    post(id: ID!): Post
  }

  type Mutation {
    createUser(name: String!, email: String!): User!
    updateUser(id: ID!, name: String, email: String): User!
    deleteUser(id: ID!): Boolean!
    createPost(title: String!, content: String!, authorId: ID!): Post!
  }
`;
```

## Resolvers

```javascript
// Mock data
const users = [
  { id: 1, name: 'John', email: 'john@example.com' },
  { id: 2, name: 'Jane', email: 'jane@example.com' }
];

const posts = [
  { id: 1, title: 'First Post', content: 'Hello World', authorId: 1 },
  { id: 2, title: 'Second Post', content: 'GraphQL is awesome', authorId: 2 }
];

// Resolvers define how to fetch data
const resolvers = {
  Query: {
    users: () => users,
    user: (_, { id }) => users.find(u => u.id === parseInt(id)),
    posts: () => posts,
    post: (_, { id }) => posts.find(p => p.id === parseInt(id))
  },
  
  User: {
    posts: (parent) => posts.filter(p => p.authorId === parent.id),
    followers: (parent) => users.filter(u => u.id !== parent.id)
  },
  
  Post: {
    author: (parent) => users.find(u => u.id === parent.authorId)
  },
  
  Mutation: {
    createUser: (_, { name, email }) => {
      const newUser = { id: users.length + 1, name, email };
      users.push(newUser);
      return newUser;
    },
    
    updateUser: (_, { id, name, email }) => {
      const user = users.find(u => u.id === parseInt(id));
      if (!user) throw new Error('User not found');
      if (name) user.name = name;
      if (email) user.email = email;
      return user;
    },
    
    deleteUser: (_, { id }) => {
      const index = users.findIndex(u => u.id === parseInt(id));
      if (index === -1) return false;
      users.splice(index, 1);
      return true;
    },
    
    createPost: (_, { title, content, authorId }) => {
      const newPost = { 
        id: posts.length + 1, 
        title, 
        content, 
        authorId: parseInt(authorId) 
      };
      posts.push(newPost);
      return newPost;
    }
  }
};
```

## Client Queries

```javascript
// Fetching specific fields
const GET_USER = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      name
      email
      posts {
        title
      }
    }
  }
`;

// With variables
const result = await client.query({
  query: GET_USER,
  variables: { id: '1' }
});

// Creating data
const CREATE_USER = gql`
  mutation CreateUser($name: String!, $email: String!) {
    createUser(name: $name, email: $email) {
      id
      name
      email
    }
  }
`;

const result = await client.mutate({
  mutation: CREATE_USER,
  variables: { name: 'New User', email: 'new@example.com' }
});
```

## Error Handling

```javascript
const resolvers = {
  Query: {
    user: async (_, { id }) => {
      try {
        const user = await fetchUser(id);
        if (!user) {
          throw new Error(`User with id ${id} not found`);
        }
        return user;
      } catch (error) {
        throw new ApolloError(error.message, 'USER_NOT_FOUND');
      }
    }
  }
};
```

## Common Mistakes

1. **Over-fetching in resolvers**: Use DataLoader for N+1 problem
2. **No query complexity limits**: Prevent expensive queries
3. **Missing authentication**: Validate user permissions
4. **Ignoring caching**: Use persisted queries
5. **Not versioning schemas**: Use schema evolution

## Related Topics

- [[What-is-REST]]
- [[What-is-CORS]]
- [[Handle-CORS]]

## Quick Revision

- GraphQL uses a single endpoint with queries
- Schema defines types and operations
- Resolvers fetch data for each field
- Clients request exactly the data they need
- Use DataLoader to solve N+1 problem
- Good for complex data requirements
