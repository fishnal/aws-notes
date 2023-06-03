# Cognito

Authentication and user-management service
- Integrates with 3rd party identity providers like Apple, Facebook, Google, and Amazon
- Allows you to federate identities from active directory services
- Supports MFA
- Customizable UIs that your users will see (i.e. branded logos)

**2 core components:** user pools and identity pools

## User Pools

**User pools** serve to create and maintain a user directory for applications
- Manages signing up for new users and signing in for existing users
- Can configure what information users must enter when signing up
	- Information can be pre-defined (like emails or full names) and can also be custom fields
	- Example: can enforce passwords meet certain requirements
- Supports MFA
- Can have users sign in with social media
	- Requires you to setup a developer account on the external service in order to obtain API keys
- Supports SAML identity providers
	- IF using SAML, you must own a domain name
- Integrates with Lambdas to trigger functions based on user flow (i.e. user signed up, user signed in, user logged out)
- Can import CSV list of users

### User Pool Authentication Flow

1. Client sends `InitiateAuth` API call to Cognito
2. Cognito responds with a **challenge**
	1. Usually a captcha, but can be configured to other challenges
	2. Can also create your own challenge
3. Client sends `RespondsToAuthChallenge` API call back to Cognito
4. Cognito evaluates challenge
	1. Failure: resend challenge
	2. Success: send token

## Identity Pools

**Identity pools**, also known as **federated identities,** provide temporary access to AWS credentials for users and _guests_
- Works well with user pools
- Similar to user pools, can federate with public providers
- **Authenticated identities** are just users who have logged in
- **Unauthenticated identities** are users who have not logged in (aka guests) but still have _some access_ to AWS
	- For example, a guest is only allowed to list S3 buckets, but cannot list S3 objects or read/write to the bucket. But a logged-in user would be able to

Overall, useful when you want permissions for guests

Can also control level of access for authenticated identities via _roles_

### Identity Pool Authentication Flow

1. Client signs in with Cognito user pools
2. When identity provider validates user's credentials, the provider sends an **identity token** to Cognito user pool
3. Cognito _normalizes_ the id token into a _standard token_ called a **Cognito User Pool (CUP) token**
	1. Cognito only stores CUP tokens; it does not store id token
4. CUP tokens are not accepted as an authentication for all services. For example, Lambda accepts CUP, but S3 does not. So you must use an _identity pool to authenticate_
5. CUP token is sent to identity pool, which creates an **STS token** based off the CUP token. You can then use the STS token to access services that don't support CUP

## Integrating Cognito into Apps

**Recommended** to integrate Cognito into your app by using **AWS Amplify Framework**
- Don't have to do this, but strongly recommended

When using Amplify framework, must specify
- Cognito region
- Which user pools and identity pools to use
- Any other information your authentication process needs

**Cognito Auth Plugin** also sends events through **Amplify Hub** that you can subscribe to
- For example, user signed in, user signed up, etc.

## Cognito Sync

**Note**: AWS is currently merging Cognito Sync **into AWS AppSync**

Helps sync user data across different platforms and applications
- Stores into a **dataset**: simple dictionary with max size of 1MB
- An identity pool has a max of 20 datasets
- Generic read/writes are performed locally
- When data needs to be synced, the entire dataset is synchronized

## AppSync
- Fully managed service for developing serverless GraphQL and Pub/Sub APIs
- Has all features of Cognito Sync plus
	- Lets multiple users access the same data
	- Synchronize and collaborate in real time on shared data
- Uses GraphQL to allow clients to fetch, change, and subscribe to data all from a single endpoint
- GraphSQL subscriptions allow real-time updates across all connected clients
- Integrates with IAM roles and Cognito for authentication and authorization
- Configure CloudWatch and Xray for management and governance
- Supports **resolvers** - allows you to implement custom business logic for to access a data source
- Can use a Lambda data source as a **proxy** to your data source
