# Quantum Ledger Database (QLDB)

- Ledger database
	- Useful for financial cases
	- Ex) Recording financial data over a period of time with cryptography (SHA256)
- Contains a **journal** which is an immutable transaction log that can only be appended to (cannot delete, can only insert at the end)
	- Can easily see how data changes over time (if it does)
	- Helps to maintain data integrity, prevents people from manipulating previous data entries, and helps prevent fraudulent activity
- Scaling is managed by AWS
- Integration with Amazon Kinesis

## Components

Amazon Ion Documents
- Self-describing, data serialization format (superset of JSON)
- Can store unstructured and structured data
- Any change to the document is tracked
- Each time a change is committed, a sequence number is added to act as an identifier. A hash of a signature is also produced for verification purposes

Storage
- Journal storage
	- Used to hold history of changes made to ledger database
	- Immutable changes via Ion documents
- Indexed storage
	- For povisioning tables and indexes within ledger
	- Optimized for querying
