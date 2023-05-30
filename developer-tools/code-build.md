# CodeBuild

Fully managed build service

Designed to compile source code, run unit tests, and create artifacts that can be deployed

Commonly used in CI/CD pipelines
- CodePipeline can be used to integrate CodeCommit, CodeBuild, and CodeDeploy all together

Publishes events to CloudWatch for monitoring

## Components

**CodeBuild project**
- Define where to fetch source code from (i.e. S3 bucket, a git repo)

**Build environment**
- Uses Docker containers to build your code
- Responsible for compiling, testing, and creating artifacts
- Can use a managed Docker image, or a custom one
	- Restrictions on which custom images are supported will differ based on region
	- Must upload custom images to a container registry (either to ECR or an external registry)

**`Buildspec` file**
- YAML file that tells CodeBuild _how_ to build your code
	- Can also do this by writing build commands directly instead of using `Buildspec`, but this is limited in features
- Can specify commands for each phase; see [below for phases](#buildspec-file)

## Storing Build Artifacts

Can store artifacts in S3 buckets
- Useful if you want to take advantage of S3 bucket triggers

`Buildspec` file allows you to write custom commands, so you can run a `docker push` to upload your artifact to an ECR repository
- Obviously, only if you're building containerized apps

## `Buildspec` file

Required fields
- `version`: version of the buildspec
- `phases`: commands to run for a given phase
	- 4 main phases: `install`, `pre-build`, `build`, and `post-build`
Optional fields
- `run-as`: only for Linux users; which user to run commands as
	- This field can also be specified _per phase_
- `env`: define environment variables
	- Can fetch from Systems Manager Parameter Store and Secrets Manager
- `proxy`: proxy server config
- `batch`: instructions on batching your builds
	- Example: Can specify what happens for your batch build if one or more build tasks fail
- `reports`: can generate test reports
	- Can also choose to create a _report group_ and specify where CodeBuild can find the raw test data
- `artifacts`: where CodeBuild can find output artifact, and how it can prepare to store it in S3
- `cache`: cache content to speed up build times (i.e. common dependencies that don't change)
	- Can pull from local cache or S3
