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

## Fields

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

## Arguments

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

## Aliases

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

## Variables

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

required variables
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