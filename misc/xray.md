# X-Ray

- Adds traceability to your AWS services
	- Useful for distributed and microservice architectures
- Visualizes relationships between services
- Trace message pathways
- Find performance bottlenecks
- Reveal service exceptions/errors
- Applications must implement Xray SDK in order to take advantage of them

## Xray Data Flow
1. Trace requests that come to your applcation/service (as well as data about the request and the service)
2. Record the trace
3. View service map to see trace data, such as latencies, HTTP statuses, and metadata for services
4. Finally, analyze the issues (i.e. high latency, lots of 4xx HTTP responses)

## Components
- Xray Daemon
	- Listens for traffic on UDP port 2000 (port can be configured)
	- Gathers segment data (more on this later) and relays to Xray API
	- Needs to be authorized to publish trace data to Xray API
		- Can be done via IAM role when daemon is deployed in EC2 environment
		- Can supply environment variables when daemon is deployed in non-EC2 environment
	- Available for Linux, Windows, and OSX
- Xray SDK (code-based sdks like for Java, NodeJS, Python, etc)
- Xray API
- Xray console
	- Visualizes the traces going through your services
	- Can see avg latency and failure rates for each segment
	- Segment will have the 4 colors
		- Green - successful
		- Orange/yellow - client error
		- Red - server fault
		- Purple - throttle errors
- Other clients - integrated via Xray SDK, API, or CLI
	- Will have to define trace segments manually

## Concepts
- Segments: portions of trace that correspond to a single service, which collects the following data
	- Host and hostname, alias/IP address
	- Request: HTTP method, path, client address, user agent
	- Response: status and body
	- Time elapsed (includes subsegments)
- Subsegments: records downstream calls from the point of view of the service that calls it
	- Xray uses subsegments to identify downstream servies that _don't_ send segments, and then creates an interest for them on your service graph
	- This is useful in case you need to analyze a downstream service call
- Traces: end-end-path of a request through your app
	- Multiple segments form a trace
	- Each trace has a UNIQUE trace id
	- As data flows thru AWS services, the trace ID is also passed along. Your apps then use that trace ID to populate more info into Xray
- Sampling: how often and how many requests are traced
	- Can configure time period and percentage of requests to trace within that time period
- Filters: filter expressions to help sort and sift thru traces
	- There are special filter keywords that help you out:
		- `ok` - status 2xx
		- `error` - status 4xx
		- `fault` - status 5xx
		- And many many more...
- Annotations: business data added to trace, and _is indexed for filtered_
- Metadata: business data added to trace, and _is **not indexed** for filtered_
- **NOTE:** annotations are key-value pairs, and metadata is a nested object structure (like a typical JSON object)
- Exceptions: an error message or stack trace

## IAM Security

`AWSXrayWriteOnlyAccess` role grants write permission to Xray API with the following policies
- `xray:PutTraceSegments`: uploading segment data
- `xray:PutTelemetryRecords` uploading telemetry data

`AWSXrayReadOnlyAccess` role grants read permission to X-ray api with the following policies
- `xray:BatchGetTraces`
- `xray:GetServiceGraph`
- `xray:GetTraceGraph`
- `xray:GetTraceSummaries`

## Pricing

Perpetual Free Tier
- 100K traces recorded per month for free
- 1M traces retrieved/scanned per month for free

Additional Charges
- $5 per 1M additional traces
- $0.50 per 1M additional traces retrieved/scanned

## Timing
Trace data is generally available for retrieval and filtering within 30s of the trace data being received by the service

Xray stores data for past 30 days
