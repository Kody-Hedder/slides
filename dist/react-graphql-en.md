# React & GraphQL

## Presentation and code

Presentations available at: https://karuga.eu/courses-presentations

Code available at: https://github.com/marko-knoebl/courses-code

## Your Trainer

Marko Knöbl

- Frontend Web-Development
  - JavaScript
  - React, Angular
- Programming
  - Python, JavaScript

## Introduction of Participants

- current projects
- prior knowledge
- expectations

## Organizational

- duration
- breaks
- lunch
- materials
- questions, feedback?

# GraphQL

## GraphQL

"A query language for your API"

## GraphQL

Can be used for an API that can in turn talk to:

- other APIs
- databases

## GraphQL vs REST

### Advantages of REST

- _widely used_
- _simpler_

### Advantages of GraphQL

- _more flexible_
- _more efficient_

## Advantages of GraphQL

_Flexibility_ of GraphQL:

can describe complex tasks - the server-side API does not have to be adjusted for new use cases

_Efficiency_ of GraphQL:

client can request exactly the data it needs in a single request

[Video: GraphQL is the better REST](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)

## Differences between GraphQL and REST

REST: endpoint (e.g. `/todos`) and method (e.g. `PUT`) are part of the API; a separate endpoint is needed for each type of query

GraphQL: single endpoint (e.g. `/api`), all requests are POST requests

## Differences between GraphQL and REST

REST: complex cases require multiple HTTP requests

GraphQL: data are fetched via a single HTTP request

## Differences between GraphQL and REST

REST: Each endpoint always returns the same set of object fields

GraphQL: the client can request exactly the objects and fields it needs

## Use cases

- API service: e.g. get a random number between 1 and 100
- database query: e.g. get all login names of friends of a specific user

## Example: random number service

query (GraphQL):

```graphql
query {
  random(min: 1, max: 100)
}
```

answer (JSON):

```json
{
  "random": 23
}
```

## Example: get friends of a user

```graphql
query {
  user(login: "john") {
    friends {
      login
    }
  }
}
```

```json
{
  "user": {
    "friends": [
      { "login": "mike" },
      { "login": "stephanie" }
    ]
  }
}
```

## Definining and consuming a GraphQL API

consuming an API:

- query (expressed in the GraphQL language)
- optional query data (in JSON)

defining an API:

- schema
- resolver functions

## Example: random numbers

Example shows use of a random number API which provides parameters to set the quantity and max value of the random numbers

## Example: random numbers

schema definition:

```graphql
type Query {
  rand(max: Int!, quantity: Int!): [Int]!
}
```

resolver function (this depends on the library):

```js
(root, args, context) =>
  Array.from({ length: args.quantity }, () =>
    Math.floor(Math.random() * args.max)
  );
```

## Example: random numbers: fixed query

```graphql
query random {
  rand(max: 10, quantity: 3)
}
```

## Example: random numbers: parametric query

query:

```graphql
query random($max: Int!, $quantity: Int!) {
  rand(max: $max, quantity: $quantity)
}
```

query data:

```json
{
  "max": 10,
  "quantity": 3
}
```

## Try it out

predefined API with posts and users:

https://api.graph.cool/simple/v1/cjmj9v4mk1zs00182rnrzdrai

<!--
source of the predefined API:

howtographql.com - Core Concepts - last "Play" button

try Subscriptions
-->

## Resources

- https://graphql.org/learn/
- https://www.howtographql.com/

# GraphQL vs REST: Example

## GraphQL vs REST

Scenario:

A social media app in which we can view a list of our friends. Clicking on a friend takes us to their most recent posts

## API in REST

```http
GET /users/$myuserid/friends
```

```http
GET /users/$otheruserid/posts
```

## API in GraphQL

```graphql
{
  users(id: "myuserid") {
    friends {
      userid
      name
    }
  }
}
```

## API in GraphQL

```graphql
{
  users(id: "otheruserid") {
    posts {
      date
      title
      body
    }
  }
}
```

## Adding functionality: new post feed

Display new posts of all friends in a feed

## Adding functionality: new post feed

In REST we would either have to send multiple requests to retrieve all posts of friends - or we would have to implement a new endpoint in the API:

```http
GET /postsoffriends/$myuserid
```

## Adding functionality: new post feed

In GraphQL we can implement this with just one request and without creating new endpoints:

```graphql
{
  users(id: "$myuserid") {
    friends {
      posts {
        date
        title
        body
      }
    }
  }
}
```

# GraphQL compared to SQL

## GraphQL compared to SQL: examples

```graphql
query {
  user(login: "my-username") {
    login
    name
  }
}
```

```sql
SELECT login, name
  FROM user
  WHERE login="my-username";
```

## GraphQL compared to SQL

In GraphQL parameters don't have a predefined meaning.

In SQL the clause `WHERE login="my-username` always has the same meaning

In GraphQL the meaning of `login: "my-username"` is up to the implementation on the server

## GraphQL compared to SQL

SQL: relationships between tables (joins) are defined in the query

GraphQL: already knows about relationships when the query is issued → simpler queries

## GraphQL compared to SQL

```graphql
query {
  user(id: "my-username") {
    posts {
      name
    }
  }
}
```

```sql
SELECT name
  FROM user
  INNER JOIN post ON user.id = post.userId
  WHERE user.id = "my-username"
```

(extra code: `INNER JOIN post ON user.id = post.userId`)

## GraphQL compared to SQL

**OpenCRUD** is a more specifc standard that is based on GraphQL. It maps directly to SQL and can be used in place of it.

# GraphQL clients

## GraphQL clients

- _graphql.js_: reference implementation
- _Apollo Client_: easy integration with React, Vue, Angular, ...
- _Relay_: advanced integration with React

# Queries

## GraphQL API examples

from https://github.com/APIs-guru/graphql-apis:

- GitHub (login required)
- Reddit (GraphQL Hub)
- GraphQL Pokémon (second entry!)
- Star Wars
- SpaceX Land
- FakeQL: Mock APIs

## GraphiQL explorer

Graph*i*QL: Browser-based explorer for GraphQL APIs

- see query structure / data structure (click "Docs" in the top right)
- send experimental queries

## Simple GraphQL exercises

- Get a list of titles of all Star Wars films in the database
- Get a list of planets and planet populations from Star Wars
- Get a list of starships grouped by films they appear in

## List of titles of Star Wars films

```graphql
query getTitles {
  allFilms {
    films {
      title
    }
  }
}
```

## List of planets and planet populations

```graphql
query getPlanetsWithPopulations {
  allPlanets {
    planets {
      name
      population
    }
  }
}
```

## List of starships grouped by film

```graphql
query getStarshipsByFilm {
  allFilms {
    films {
      title
      starshipConnection {
        starships {
          name
        }
      }
    }
  }
}
```

## Query parameters

Only query the pokémon with the specified name

```graphql
query getCharmander {
  pokemon(name: "Charmander") {
    name
    weight {
      minimum
      maximum
    }
  }
}
```

## Query parameters

Query the first three pokémon in the database

```graphql
query getFirstThree {
  pokemons(first: 3) {
    name
  }
}
```

Note: specifying a _first_ parameter is possible because it's implemented on the server-side. It's not part of the GraphQL standard. GraphQL _may_ also implement other arbitrary parameters like _orderBy_ or _matchRegex_, but this is all up to the server.

## Query parameters: exercises

- get the name, weight and classification of Pikachu
- get the name and weight of the first three pokémon

## Required and optional parameters

Required parameters are marked with a `!`. These must always be included. Similarly, returned attributes that will always be present (like `id`) will be marked in the same way.

## Variables

query:

```graphql
query getPokemonByName($name: String) {
  pokemon(name: $name) {
    name
    number
    image
  }
}
```

variables:

```json
{
  "name": "Pikachu"
}
```

## Modifying data

https://todo-mongo-graphql-server.herokuapp.com/

(only one query at a time)

## Modifying data

Command that triggers the server's `add` action and returns the id of the new TODO

```graphql
mutation addTodo($title: String!) {
  add(title: $title) {
    id
  }
}
```

```json
{
  "title": "shopping"
}
```

## Modifying data

```graphql
mutation toggleTodo($id: String!) {
  toggle(id: $id) {
    id
    completed
  }
}
```

## Modifying data

```graphql
mutation addOneAndClearCompleted($title: String!) {
  add(title: $title) {
    id
  }
  clearCompleted {
    id
  }
}
```

## Modifying data

Task: write a query that will delete all previous entries and add two new ones

## Modifying data

```graphql
mutation reset {
  toggleAll(checked: true) {
    id
  }
  clearCompleted {
    id
  }
  firstAddition: add(title: "Count to 34") {
    id
  }
  secondAddition: add(title: "Count to 45") {
    id
  }
}
```

## More exercises - optional

- Get all "followers of followers" for a specific GitHub account
- Get the name of a project and number of stars for all GitHub projects of a specific user

## More exercises - solutions

```
query {
  user (login: "marko-knoebl") {
    followers (first: 10) {
      nodes {
        login,
        followers (first: 10) {
          nodes {
            login
          }
        }
      }
    }
  }
}
```

## More exercises - solutions

```
query {
  user (login: "marko-knoebl") {
    id,
    email,
    repositories (first: 100, orderBy: {field: STARGAZERS, direction: DESC}) {
      nodes {
        name,
        stargazers {
          totalCount
        }
      }
    }
  }
}
```

## Task: loading Todos from a GraphQL API

Load todos from `https://5qn401kkl9.lp.gql.zone/graphql`

(admin: https://launchpad.graphql.com/5qn401kkl9)

# Queries - Advanced

## Default variable valuess

```graphql
query getPokemonByName($name: String = "Pikachu") {
  pokemon(name: $name) {
    number
    image
  }
}
```

## Aliases

Task: number of Pikachu and Raichu

## Aliases

This cannot be done the way we know:

```graphql
query getTwo {
  pokemon(name: "Pikachu") {
    number
  }
  pokemon(name: "Raichu") {
    number
  }
}
```

## Aliases

Why does this not work? The result would look like this:

```json
{
  "data": {
    "pokemon": {
      "number": "025"
    },
    "pokemon": {
      "number": "026"
    }
  }
}
```

Note the duplicate key: `pokemon`!

## Aliases

In order to avoid this problem we use aliases:

```graphql
query getTwo {
  pokemon1: pokemon(name: "Pikachu") {
    number
  }
  pokemon2: pokemon(name: "Raichu") {
    number
  }
}
```

## Aliases

response:

```json
{
  "data": {
    "pokemon1": {
      "number": "025"
    },
    "pokemon2": {
      "number": "026"
    }
  }
}
```

## Fragments

Task: get the number, maxHP and image of Pikachu and Raichu

## Fragments

```graphql
query getTwo {
  pokemon1: pokemon(name: "Pikachu") {
    ...essentialData
  }
  pokemon2: pokemon(name: "Raichu") {
    ...essentialData
    id
  }
}

fragment essentialData on Pokemon {
  number
  maxHP
  image
}
```

# Data types

## Data types

With GraphQL the returned data types are always known to the client.

## Data types

available types:

- Boolean
- Int: 32-bit int (signed)
- Float: 64-bit float
- String: UTF-8 character sequence
- ID: unique id serialized as a string
- Object: object with predefined entries
- List: list composed of specific other types

# GraphQL from JavaScript

## Sending queries to the server

Queries are sent to the server via HTTP POST requests

The payload is a JSON object which has a `query` string property (this is also true when sending mutations) and optionally a `variables` property.

## Sending queries to the server

We can try this out from the browser console via fetch (we have to be on the same website):

```js
const requestBody = {
  query:
    'mutation addTodo($title: String!) { add(title: $title) { id } }',
  variables: '{"title": "test"}',
};

const requestBodyStr = JSON.stringify(requestBody);

fetch('https://todo-mongo-graphql-server.herokuapp.com', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: requestBodyStr,
}).then(console.log);
```

## Example: reddit API

```js
const queryTemplate = `
{
  reddit {
    subreddit(name: "javascript") {
      newListings(limit: 2) {
        title
      }
    }
  }
}`;
```

## Example: reddit API

```js
fetch('https://www.graphqlhub.com/graphql', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Accept: 'application/json',
  },
  body: JSON.stringify({ query: queryTemplate }),
})
  .then(r => r.json())
  .then(data => console.log('data returned:', data));
```

# Apollo client

## Apollo Client

https://www.apollographql.com/docs/react/

## Apollo client

advantages over "plain" frontend code:

- automatic sending of queries over the network
- automatic caching
- automatic (re)rendering of React components

## Installation

required packages:

- `apollo-client`
- `apollo-cache-inmemory`
- `apollo-link-http`
- `graphl`
- `graphql-tag`
- `react-apollo` (for use with React)

You can also install `apollo-boost` to install `apollo-client`, `apollo-cache-inmemory` and `apollo-link-http` (and more)

## Apollo client: setup

```js
import { ApolloClient } from 'apollo-client';
import { InMemoryCache } from 'apollo-cache-inmemory';
import { HttpLink } from 'apollo-link-http';
import gql from 'graphql-tag';

const cache = new InMemoryCache();
const link = new HttpLink({
  uri: 'https://api.spacex.land/graphql/',
});

const client = new ApolloClient({
  cache,
  link,
});
```

## Apollo client: example query

```js
// via a tagged template string
const LAUNCHES_QUERY = gql`
  query recentLaunches {
    launchesPast(limit: 10) {
      mission_name
    }
  }
`;

client
  .query({
    query: LAUNCHES_QUERY,
  })
  .then(result => console.log(result));
```

# Apollo client with React

## Connecting React to an Apollo client

An application usually communicates with only one GraphQL API

```js
import { ApolloProvider } from 'react-apollo';
```

```jsx
<ApolloProvider client={client}>
  <App />
</ApolloProvider>
```

## Defining a Query

```js
const LAUNCHES_QUERY = gql`
  query recentLaunches {
    launchesPast(limit: 10) {
      mission_name
    }
  }
`;
```

## useQuery

```js
function RecentLaunches() {
  const { data, loading, error } = useQuery(LAUNCHES_QUERY);
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error!</div>;

  return (
    <div>
      <h1>Launches</h1>
      {data.launchesPast.map(launch => (
        <div>{launch.mission_name}</div>
      ))}
    </div>
  );
}
```

## useQuery: Parameters

```js
const LAUNCHES_QUERY = gql`
  query recentLaunches($numLaunches: ) {
    launchesPast(limit: $numLaunches) {
      mission_name
    }
  }
`;

function RecentLaunches({ numLaunches }) {
  const { data, loading, error } = useQuery(
    LAUNCHES_QUERY,
    { variables: { numLaunches } }
  );
  ...
}
```

## useQuery: polling & refetching

Updating every 5 seconds:

```js
const { data, loading, error } = useQuery(LAUNCHES_QUERY, {
  pollInterval: 5000,
});
```

Updating via `refetch()`:

```js
const { data, loading, error, refetch } = useQuery(
  LAUNCHES_QUERY
);
...
refetch()
```

## useMutation
