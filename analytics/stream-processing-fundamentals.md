# Fundamentals of Stream Processing

Benefits of streaming data
- Streaming reduces need for large and expensive shared databases. This is because every stream consumer maintains its own data and state
- Stream processing fits naturally inside a decoupled microservices architecture
- Can provide actionable insights within _milliseconds_ of a recorded event

## Questions to Consider

How important is it to have immediate insights?
- This deals with the **issue of latency and value of data over time**
- For example, a bug ticketing system probably does not need real-time streams
- However, banks, healthcare, and security services do require real-time or near real-time streaming data (i.e. fraudulent transactions, a hospital patient needs immediate assistance, there's a security breach or vulernability)

## Issues with Batch Processing

Batch jobs usually wait until their workload reaches a certain size (i.e. 100 images to process, 30 videos to process).
- A batch may not run because it only has 99 images, but needs 100 in order to start. As a result, _batches start inconsistently_
- While the size of the batch job is uniform, **time period** is inconsistent

## Challenges of Stream Processing
- Streaming applications are usually "high-touch" systems (lots of human interaction) which make the application inconsistent and difficult to automate
- Difficult to set up - streaming apps usually have lots of brittle components
- Expensive to create, maintain, and scale in on-premises datacenters
