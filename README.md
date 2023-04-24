# omnia-rdf-proxy
This simple proxy handles requests for the [AWS Neptune](https://aws.amazon.com/neptune/) RDF database. This database exposes an HTTP SPARQL endpoint, that can be found in the AWS console. According to the [documentation](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-sparql-http-rest.html), this endpoint is not public, so this proxy is needed to make it accessible to the outside world.

## Idempotent requests
The proxy is able to handle idempotent requests for the `/update` endpoint. This is done by caching the results of the requests in NGINX. The cache key is the header `Idempotent-Key`. If this header is not present, the request is rejected with a `400 Bad Request` error. The cache is invalidated every 24 hours.

## Authorization
`/update` endpoint returns a `401 Unauthorized` error if the `Authorization` header is not present or if the value is not equal to the one specified in the `UPDATE_API_KEY` env variable.

## Usage
Create an EC2 instance (with Docker) connected to the same VPC as the Neptune database. Clone this repo follow these steps:
- copy the `.env.example` file to `.env` and fill in the values
- run `docker compose up -d`

## Development
Create the `.env` file as above and, in order to spin up a local RDF database, run:
- `docker compose --profile dev up -d`
