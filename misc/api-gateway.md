# API Gateway

Allows you to define and model a RESTful API without having to manage a lot of code associated with it (i.e. writing a REST server in C, Java, Python, etc).

Allows you to map API endpoints via **mapping templates**

Integrates with Lambda functions
- Here, "method" refers to `HTTP_METHOD /foo/bar`
- A method can invoke a lambda
- Can also map multiple methods to invoke the same lambda

Resources can have dynamic query parameters
- Example: `GET /items/{id}`, where `{id}` defines the query parameter `id`

## Components

Models
- Defines object schemas (one supported schema format is JSON schema)

Resources
- Defines a route or endpoint (i.e. `/items/`)
- Does not specify an HTTP method (that's next...)
- Resources can have **child resources**; useful for dynamic query params (i.e. `/items/{id}`)

Methods
- Responsible for bulk of the business logic in API Gateway
- For a given resource, defines an HTTP method to perform some action
	- So the resource `/items` could have 1 or more methods, say GET, POST, etc.
- When a method is created, you can mock responses from it by modifying the "Integration Response"
	- Can hard-code some static responses
	- Can generate responses based off model schemas that you have defined in your API Gateway

## Usage Plans, API Keys, and More

Can specify usage plans to limit/throttle your API

Methods on resources have the option to require API Keys

Can add API keys to your gateway, and assign usage plans to each API key
- Use case: Alice may need to use the API more often than Bob does. So Alice's API key may have a more generous usage plan, while Bob's API key might have a more strict usage plan

Can throttle for different stages (i.e. dev, test, prod)

Can configure CORS, so that your API only makes HTTP requests to certain domains or origins

Can enable caching
- Max cache size
- Whether to encrypt cached data
- How long cache lives for (TTL)
- Whether it requires authorization and how to handle unauthorized requests
