# Queries and Mutations

## Operation Name

explicit use of operation type and name
```gql
# query is the operation type
# HeroNameAndFriends is the operation name
query HeroNameAndFriends {
	hero {
		name
		friends {
			name
		}
	}
}
```

query shorthand syntax
```gql
{
	hero {
		name
		friends {
			name
		}
	}
}
```

it is encouraged to be explicit because:
- it provides more context and verbosity
- allows the use of variables

## Query

basic fields query
```gql
# query
{
	hero {
		name
	}
}

# response
{
	"data": {
		"hero": {
			"name": "R2-D2"
		}
	}
}
```

basic argument query
```gql
# query
{
	# id argument with string value of "1000"
	human(id: "1000") {
		name
		height
	}
}

# response
{
	"data": {
		"human": {
			"name": "Luke Skywalker",
			"height": 1.72
		}
	}
}
```

basic argument query with Enumeration type
```gql
# query
{
	human(id: "1000") {
		name
		height(unit: FOOT) # FOOT enum used here
	}
}

# response
{
	"data": {
		"human": {
			"name": "Luke Skywalker",
			"height": 5.643044 # server resolves the data based on the type
		}
	}
}
```

basic use of alias to query multiple records of the same field
```gql
# query
{
	# alias hero to empireHero
	empireHero: hero(episode: EMPIRE) {
		name
	}
	# alias hero to jediHero
	jediHero: hero(episode: JEDI) {
	    name
	}
}

# response
{
	"data": {
		# response will use the query alias
	    "empireHero": {
		    "name": "Luke Skywalker"
	    },
	    "jediHero": {
		    "name": "R2-D2"
	    }
	}
}
```

basic use of variables
```gql
# declare $episode variable
query HeroNameAndFriends($episode: Episode) {
	# use $episode variable
	hero(episode: $episode) {
		name
		friends {
			name
		}
	}
}

# input variable
{
	"episode": "JEDI"
}

# response
{
	"data": {
	    "hero": {
		    "name": "R2-D2",
		    "friends": [
		        {
			        "name": "Luke Skywalker"
		        },
		        {
			        "name": "Han Solo"
		        },
		        {
			        "name": "Leia Organa"
		        }
		    ]
	    }
	}
}
```

default variables
```gql
# $episode variable has default JEDI enum member
query HeroNameAndFriends($episode: Episode = JEDI) {
	hero(episode: $episode) {
	    name
	    friends {
		    name
	    }
	}
}
```

required and non-nullable variables
```gql
# ! makes the $episode variable required and cannot be null
query HeroNameAndFriends($episode: Episode!) {
	hero(episode: $episode) {
	    name
	    friends {
		    name
	    }
	}
}
```

## Directives

- @include(if: Boolean) Only include this field in the result if the argument is true.
- @skip(if: Boolean) Skip this field if the argument is true.

basic use of directives
```gql
query Hero($episode: Episode, $withFriends: Boolean!) {
	hero(episode: $episode) {
	    name
		# @include(if: Boolean) is a directive that will only include
		# the field if the value is true
	    friends @include(if: $withFriends) {
		    name
	    }
	}
}

# input variable
{
	"episode": "JEDI",
	"withFriends": true
}

# response
{
	"data": {
	    "hero": {
		    "name": "R2-D2",
			# friends array is included as the directive is true
		    "friends": [
		        {
			        "name": "Luke Skywalker"
		        },
		        {
			        "name": "Han Solo"
			    },
		        {
			        "name": "Leia Organa"
		        }
		    ]
	    }
	}
}
```

defining new directives
```gql
# directive on enums
directive @external on FIELD_DEFINITION | OBJECT
# directive with arguments
directive @requires(fields: FieldSet!) on FIELD_DEFINITION
# recursive directive
directive @key(fields: FieldSet!, resolvable: Boolean = true) repeatable on OBJECT | INTERFACE
```
## Fragments

basic use of fragments to reuse query constructs
```gql
# query
{
	leftComparison: hero(episode: EMPIRE) {
		...comparisonFields # use fragment
	}
	rightComparison: hero(episode: JEDI) {
		...comparisonFields
	}
}

# fragment definition
fragment comparisonFields on Character {
	name
	appearsIn
	friends {
		name
	}
}

# response
{
	"data": {
		"leftComparison": {
		    "name": "Luke Skywalker",
		    "appearsIn": [
		        "NEWHOPE",
		        "EMPIRE",
		        "JEDI"
		    ],
		    "friends": [
				{
				    "name": "Han Solo"
		        },
		        {
			        "name": "Leia Organa"
		        },
		        {
			        "name": "C-3PO"
		        },
		        {
			        "name": "R2-D2"
		        }
		    ]
	    },
		"rightComparison": {
			"name": "R2-D2",
		    "appearsIn": [
		        "NEWHOPE",
		        "EMPIRE",
		        "JEDI"
		    ],
		    "friends": [
		        {
			        "name": "Luke Skywalker"
		        },
		        {
			        "name": "Han Solo"
		        },
		        {
			        "name": "Leia Organa"
		        }
			]
	    }
	}
}
```

using variables inside fragments
```gql
# query
# $first variable is defined here
query HeroComparison($first: Int = 3) {
	leftComparison: hero(episode: EMPIRE) {
	    ...comparisonFields
	}
	rightComparison: hero(episode: JEDI) {
		...comparisonFields
	}
}

fragment comparisonFields on Character {
	name
	# $first variable is used here
	friendsConnection(first: $first) {
	    totalCount
	    edges {
		    node {
		        name
		    }
	    }
	}
}

# response
{
	"data": {
	    "leftComparison": {
		    "name": "Luke Skywalker",
		    "friendsConnection": {
			    "totalCount": 4,
		        "edges": [
				    {
					    "node": {
				            "name": "Han Solo"
			            }
				    },
			        {
				        "node": {
				            "name": "Leia Organa"
			            }
			        },
			        {
				        "node": {
				            "name": "C-3PO"
			            }
			        }
		        ]
		    }
	    },
	    "rightComparison": {
			"name": "R2-D2",
		    "friendsConnection": {
		        "totalCount": 3,
		        "edges": [
			        {
			            "node": {
				            "name": "Luke Skywalker"
			            }
			        },
			        {
				        "node": {
				            "name": "Han Solo"
				        }
			        },
			        {
				        "node": {
				            "name": "Leia Organa"
			            }
			        }
		        ]
		    }
	    }
	}
}
```

## Inline Fragments

basic use of inline fragments to resolve union types
```gql
query HeroForEpisode($ep: Episode!) {
	hero(episode: $ep) {
	    name
		# if returned type is Droid return primaryFunction field
	    ... on Droid {
		    primaryFunction
	    }
		# if returned type is Human return height field
	    ... on Human {
		    height
	    }
	}
}

# input variable
{
	"ep": "EMPIRE"
}

# response
{
  "data": {
    "hero": {
      "name": "Luke Skywalker",
	  # height is return as the type is Human
      "height": 1.72
    }
  }
}
```

## Meta fields

basic use of meta fields to get the name of the object type
```gql
# query
{
	search(text: "an") {
	    __typename # this is a meta field query
	    ... on Human {
			name
	    }
	    ... on Droid {
		    name
	    }
	    ... on Starship {
		    name
	    }
	}
}

# response
{
	"data": {
	    "search": [
		    {
		        "__typename": "Human",
		        "name": "Han Solo"
		    },
		    {
		        "__typename": "Human",
		        "name": "Leia Organa"
		    },
		    {
		        "__typename": "Starship",
		        "name": "TIE Advanced x1"
		    }
	    ]
	}
}
```

## Mutations
>While query fields are executed in parallel, mutation fields run in series, one after the other.

basic mutation
```gql
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
	# if mutation returns an object type, the updated state can be queried
	createReview(episode: $ep, review: $review) {
	    stars
	    commentary
	}
}

# input variable
{
	"ep": "JEDI",
	"review": {
	    "stars": 5,
	    "commentary": "This is a great movie!"
	}
}

# response
{
	# updated data
	"data": {
	    "createReview": {
		    "stars": 5,
		    "commentary": "This is a great movie!"
		}
	}
}
```

# Schema and Types

## Object types and fields

basic GraphQL schema components
```gql
# Character is an object type and can have nested fields
type Character {
	name: String! # name is a scalar type and cannot have nested fields
	appearsIn: [Episode!]! # appearsIn is an array of Episode objects
}
```

arguments
```gql
type Starship {
	id: ID!
	name: String!
	# any field can have zero or more arguments
	length(unit: LengthUnit = METER): Float
}
```

## Types

default scalar types out of the box
- Int: A signed 32‐bit integer.
- Float: A signed double-precision floating-point value.
- String: A UTF‐8 character sequence.
- Boolean: true or false.
- ID: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an ID signifies that it is not intended to be human‐readable.

defining custom scalar type
```gql
scalar Date
```

enumeration types
```gql
enum Episode {
	NEWHOPE
	EMPIRE
	JEDI
}
```

lists and non-null
```gql
type Character {
	name: String! # ! is used to mark a field as non-null
	appearsIn: [Episode]! # the list cannot be null but can contain no element
}
```

union
```gql
union SearchResult = Human | Droid | Starship
```

input type
```gql
input ReviewInput {
	stars: Int!
	commentary: String
}
```

extend type
```gql
extend type Character {
	bio: String!
}
```

## Interfaces

basic interface
```gql
# interface declaration
interface Character {
	id: ID!
	name: String!
	friends: [Character]
	appearsIn: [Episode]!
}

# concrete implementation
type Human implements Character {
	id: ID!
	name: String!
	friends: [Character]
	appearsIn: [Episode]!
	# extra fields
	starships: [Starship]
	totalCredits: Int
}
 
type Droid implements Character {
	id: ID!
	name: String!
	friends: [Character]
	appearsIn: [Episode]!
	# extra fields
	primaryFunction: String
}
```

# Introspection

- Query, Character, Human, Episode, Droid - These are the ones that we defined in our type system.
- String, Boolean - These are built-in scalars that the type system provided.
- __Schema, __Type, __TypeKind, __Field, __InputValue, __EnumValue, __Directive - These all are preceded with a double underscore, indicating that they are part of the introspection system.

# Best Practices

serving over http
```
# GraphQL should be placed after authentication middleware
Req --> Authentication --> GraphQL

# example GET request
http://myapi/graphql?query={me{name}}

# example POST request body
{
	"query": "...",
	"operationName": "...", # optional. required if more than one operation
	"variables": { "myVariable": "someValue", ... } # optional
}

# example response
{
	"data": { ... },
	"errors": [ ... ]
}
```

JSON (with gzip)
```
# client need to send this header
Accept-Encoding: gzip
```

separate authorization logic to business logic layer
```ts
// Authorization logic lives inside postRepository
const postRepository = require('postRepository');
 
const postType = new GraphQLObjectType({
  name: 'Post',
  fields: {
    body: {
      type: GraphQLString,
      resolve(post, args, context, { rootValue }) {
        return postRepository.getBody(context.user, post)
      }
    }
  }
})
```

pagination - [Relay](https://facebook.github.io/relay/)
```gql
# offset-based pagination
{
	hero {
		name
		# return the first two items
		friends(first: 2, offset: 1) {
			name
		}
	}
}

# cursor-based pagination
{
	hero {
		name
		# return the next 2 item after cursor from last item
		# cursors are opaque therefore should be base64 encoded
		friendsConnection(first: 2, after: $cursor) {
			# list of edges contains a node for the data
			# and a cursor to the next data
			# edges can be replaced by querying the field
			# directly to avoid indirection
			edges {
				node {
					name
				}
				cursor
			}
			# pageInfo tells whether the current node has more data after
			pageInfo {
		        endCursor
		        hasNextPage
		    }
		}
	}
}
```

global object identification
```gql
{
	# node is a generic interface to fetch any object
	node(id: "4") {
		# by passing the id we can get any type using an inline fragment
	    id
	    ... on User {
			# ...and then get the specific type fields
		    name
	    }
	}
}
```

batching - [DataLoader](https://github.com/facebook/dataloader)

[GraphQL Federation](https://www.apollographql.com/docs/federation/subgraph-spec/)
[Apollo Sandbox](https://studio.apollographql.com/sandbox/explorer)
[Why I'm over GraphQL](https://bessey.dev/blog/2024/05/24/why-im-over-graphql/)