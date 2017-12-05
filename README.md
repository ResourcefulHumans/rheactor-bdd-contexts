# rheactor-bdd-contexts

[![Build Status](https://travis-ci.org/ResourcefulHumans/rheactor-bdd-contexts.svg?branch=master)](https://travis-ci.org/ResourcefulHumans/rheactor-bdd-contexts)
[![monitored by greenkeeper.io](https://img.shields.io/badge/greenkeeper.io-monitored-brightgreen.svg)](http://greenkeeper.io/) 
[![npm version](https://img.shields.io/npm/v/@resourcefulhumans/rheactor-bdd-contexts.svg)](https://www.npmjs.com/package/@resourcefulhumans/rheactor-bdd-contexts)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)
[![semantic-release](https://img.shields.io/badge/semver-semantic%20release-e10079.svg)](https://github.com/semantic-release/semantic-release)
[![Code Climate](https://codeclimate.com/github/ResourcefulHumans/rheactor-bdd-contexts/badges/gpa.svg)](https://codeclimate.com/github/ResourcefulHumans/rheactor-bdd-contexts)

[Yadda](https://github.com/acuminous/yadda) BDD contexts for [RHeactor applications](https://github.com/RHeactor).


Use `@thisonly` annotation to specify single test feature to run.
# Examples


## Given

- "$value" is the $header header

	```feature
	Given "application/vnd.resourceful-humans.networhk.v2+json; charset=utf-8" is the Content-Type header
	```

- "$value" is the $header header

	```feature
	the Authorization header is empty
	```

- this is the request body\n$json

	```feature
	Given this is the request body
	--------------
	"email": "mike.doe-{time}@example.com",
	"password": "serious garage excellent making"
	--------------
	```

- this JSON is the request body\n$json
	
	Example missing

- the request body is empty

	No Example

## When

- I $method to $endpoint

	```feature
	When I POST to {loginEndpoint}
	```

- I $method $endpoint
	
	```feature
	When I GET {loginEndpoint}
	```
- I store "$node" as "$storage"
	
	Get value from response and located by `$node` using [Lodash path](https://lodash.com/docs/4.17.4#property) (example: `model.subproperty`)
	and store it in variable defined by `$storage`.

	```feature
	And I store "$version" as "contributionVersion"
	```

	From response
	```javascript
	{
		"$version": 2,
		// other properties
	}
	```
	it will store `2` in `contributionVersion` variable.

- I store "$node" of the ([0-9]+)[a-z]+ item as "$storage"

	When response has `items` (typical list response), use this to pull nth element and get value from it.

	```feature
	And I store "$id" of the 1st item as "task"
	```

	Response
	```javascript
	{
	    "$context": "https://github.com/ResourcefulHumans/rheactor-models#List",
	    "$contextVersion": 1,
	    "items": [
	    {
	    	"$id": "http://127.0.0.1:8081/api/task/1",
	        // ...
	       
	    },
	    // ...
	    ]
	}
	```
	It will store `http://127.0.0.1:8081/api/task/1` in variable `task`.

- I store the link to the list "$context" as "$storage"
	
	When response has `$links` array you can use this to locate one by `subject` with `$context` and it's href will be stored in variable. That link **must** be list.
	If not use other selector.

	```feature
	And I store the link to the list "https://github.com/ResourcefulHumans/netwoRHk/wiki/JsonLD#Commitment" as "commitments"
	```

	Response
	```javascript
	{
	    "$context": "https://github.com/ResourcefulHumans/netwoRHk/wiki/JsonLD#Contribution",
	    "$contextVersion": 1,
	    "$links": [
	    {
	        "$context": "https://github.com/ResourcefulHumans/rheactor-models#Link",
	        "$contextVersion": 1,
	        "subject": "https://github.com/ResourcefulHumans/netwoRHk/wiki/JsonLD#Commitment",
	        "href": "http://127.0.0.1:8081/api/netwoRHk/2/search/commitment?contribution=1",
	        "list": true
	    },
	    //...
	    ]
	    //...
	}
	```
	
	It will store `http://127.0.0.1:8081/api/netwoRHk/2/search/commitment?contribution=1` to `commitments`.

- I store the link to the list "$context" of the ([0-9]+)[a-z]+ item as "$storage"

	Same like previous but it pulls from nth item from `items` array.

	```feature
	And I store the link to the list "https://github.com/ResourcefulHumans/netwoRHk/wiki/JsonLD#ContributionRelation" of the 2nd item as "secondRelatedContributions"
	```

- I store the link to "$relation" as "$storage" `I store the link to "([^"]+)" as "([^"]+)"`
	
	Match response `$links` where `rel` is equal to `$relation`.

- I store the link to "$relation" of "$node" as "$storage" (I store the link to "([^"]+)" of "([^"]+)" as "([^"]+)")

	Same as previous but pulled from `$node` value of response.

	```feature
	And I store the link to "update-date" of "checkpoints.items[0]" as "updateCheckpointDate"
	```

- I store the link of "$relatedContext" as "$storage"

	Match response `$links` where `subject` is equal to `$relatedContext`.

	> No usage in netwoRHk code

- I store the link to "$relation" of the ([0-9]+)[a-z]+ item as "$storage"
	
	Same like previous but nth element of `items` array

	```feature
	And I store the link to "update-status" of the 2nd item as "updateTaskStatus"
	```
- I store the $header header as "$storage"

- I follow the redirect	

## Then

- "$node" of the $num item should reference the "$subject" with id "$id" `"([^"]+)" of the ([0-9]+)[a-z]+ item should reference the "([^"]+)" with id "([^"]+)"`

	```feature
	And "user" of the 1st item should reference the "https://github.com/ResourcefulHumans/rheactor-models#User" with $id "{jwt.sub}"
	```

- "$node" should reference the "$subject" with id "$id" `"([^"]+)" should reference the "([^"]+)" with id "([^"]+)"`
	
	```feature
	And "user" should reference the "https://github.com/ResourcefulHumans/rheactor-models#User" with $id "{jwt.sub}"
	```

- the status code should be $num

	```feature
	Then the status code should be 200
	```

- the $header header should equal "$value"

	```feature
	And the Content-Type header should equal "application/vnd.resourceful-humans.networhk.v2+json; charset=utf-8"
	```

- the $header header should exist

- "$node" should equal "$value"

	```feature
	And "status" should equal "ok"
	```

- "$node" should not equal "$value"

- "$node" should equal $number `"([^"]+)" should equal ([+0-9,.-]+)`

	```feature
	And "checkpoints.items[0].expected" should equal 1000000
	```

- "$node" should be $assertion $number `"([^"]+)" should be ([^ ]+) ([+0-9,.-]+)`
	
	Value is compared using chai `$assertion`.

- "$node" should equal $bool `"([^"]+)" should equal (true|false)`
	
	`$bool` can be true or false string

- "$node" should equal\n$text

	```feature
	And "description" should equal
	--------------
	This one of our most valuable customers!

	**Be 💯 to them!**
	--------------
	```

- "$node" should exist

- "$node" should not exist

- "$node" should match $regexp

#### List

Applies on response that has `items`

- a list of "([^"]+)" with ([0-9]+) of ([0-9]+) items? should be returned

	```feature
	And a list of "https://github.com/ResourcefulHumans/netwoRHk/wiki/JsonLD#Task" with 10 of 16 items should be returned
	```
- a list of "([^"]+)" should be returned

- "([^"]+)" should be a list of "([^"]+)" with ([0-9]+) of ([0-9]+) items?

- I filter the list by $property equals (true|false)

- I filter the list by ([^ ]+) contains "([^"]+)

- "([^"]+)" of the ([0-9]+)[a-z]+ item should equal "([^"]+)"

- "([^"]+)" of the ([0-9]+)[a-z]+ item should equal (true|false)

- "([^"]+)" of the ([0-9]+)[a-z]+ item should equal ([+0-9,.-]+)

#### JWT

- JWT $property should exist

- JWT $property should equal "$value"

- JWT ([^ ]+) should equal (true|false)

- JWT ([^ ]+) should be ([0-9]+) ([a-z]+) in the (future|past)

- I parse JWT token into "$name"

- I parse JWT token from "$storage" into "$name"

#### Debug

- I print the response
