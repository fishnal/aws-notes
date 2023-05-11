# Snowball

* Securely transfer large amounts of data (i.e. petabytes) to or from AWS
* Snowball appliance comes in 50TB and 80TB devices
* Snowball is dust, water and tamper resistant
* Built for high speed transfer

## Encryption & Tracking
* End to end tracking via an "E-ink" shipping label
* Can be tracked through text messages or AWS console
* HIPAA compliant, and thus allows transfer of protected health data
* **Data removal**: After your data is transferred, AWS deletes your data according to NIST standards

## Data Aggregation
* You can split data transfer across multiple snowballs if needed (i.e. transferring 400TB of data)

## Transfer Process
1. Create an export job
2. Receive snowball devices
3. Connect snowballs to your network
4. Transfer data via Snowball Client
5. Once transfer is completed, return snowball

## Pricing
* Data transferred into AWS is free
* Cost associated per transfer job created + shipping costs
* 50TB snowball costs $200
* 80TB snowball costs $250
* Can keep snowball for up to 10 days
* Cost associated for transfers out of S3
