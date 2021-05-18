
## AWS Lambda
* [Very detailed tutorial for using AWS Lambda to build a note uploading app](https://serverless-stack.com/chapters/what-is-aws-lambda.html).
  * Setup Lambda Functions and config `serverless.yml`
  * Add S3 Bucket for file uploading (e.g., note attachment)
  * Add DynamoDB to store note content and attachment name
  * Add a Cognito User Pool to sign up users
  * Add a Cognito Identity Pool to manage roles for users. The users could be from a Cognito User Pool or Google/Facebook.
  * Add IAM Auth before using S3 Bucket and API gateways (Lambda + DynamoDB)
  * Add Stripe as a third-party API for billing
  * Add unit-test
  * Handle CORS (cross-origin resource sharing) because the app's domain will not be the same as the endpoints of S3 Bucket, Serverless API and User Pool
    * Preflight OPTIONS requests: For certain types of cross-domain requests (PUT, DELETE, ones with Authentication headers, etc.), your browser will first make a preflight request using the request method `OPTIONS`. These need to respond with the domains that are allowed to access this API and the HTTP methods that are allowed.
    * Respond with CORS headers: For all the other types of requests we need to make sure to include the appropriate CORS headers. These headers, just like the one above, need to include the domains that are allowed.
    * To that end, update `serverless.yml` and each Lambda function.
    * Since API gateway will be reached before the Lambda function. If the API gateway has an error and returns right away, it will not return CORS headers that are set in Lambda function. This early return will make the debug very hard. AWS uses CloudFormation to setup CORS headers when API gateway has an error (4xx, 5xx).
  * Create-react-app.
    * React-bootstrap
    * React-router
    * Not-found page (error 404)
  * AWS Amplify: allow users to login and sign up. It also provides modules to connect to the backend.
    * Authentication
    * Session
    * Redirect on Login and Logout: `useHistory` hook
    * Add a loading button while logging in
    * Sign up with AWS Cognito
  * [AuthenticatedRoute](https://serverless-stack.com/chapters/create-a-route-that-redirects.html): a route that will check if the user is logged in before routing. If the user is not authenticated, then it redirects to the login page. 
  * UnauthenticatedRoute: Here we are checking to ensure that the user is not authenticated before we render the child components. Example child components here would be Login and Signup. And in the case where the user is authenticated, we use the Redirect component to simply send the user to the homepage.
  * Host the App with Netlify, which will handle HTTPs too.
  * [Make the entire backend as an Infrastructure](https://serverless-stack.com/chapters/getting-production-ready.html), or [Infrastructure as Code](https://serverless-stack.com/chapters/what-is-infrastructure-as-code.html): describing our entire infrastructure as code allows us to create multiple environments with ease. For example, you can create a dev environment where you can make and test all your changes as you work on it. And this can be kept separate from your production environment that your users are interacting with.
    * CloudFormation can automatically convert the `serverless.yml` into a template, but it has a few major drawbacks.
    * Instead, use [AWS CDK](https://serverless-stack.com/chapters/what-is-aws-cdk.html). (Cloud Development Kit), released in Developer Preview back in August 2018; allows you to use TypeScript, JavaScript, Java, .NET, and Python to create AWS infrastructure.
  * [Automating deployments](https://serverless-stack.com/chapters/creating-a-ci-cd-pipeline-for-serverless.html), CI/CD or Continuous Integration/Continuous Delivery.
    * Seed
    * CircleCI
    * Travis CI
  * [Creating a CI/CD pipeline for React](https://serverless-stack.com/chapters/creating-a-ci-cd-pipeline-for-react.html)
  * [Setup Error Reporting in React](https://serverless-stack.com/chapters/setup-error-reporting-in-react.html) and [Report API Errors in React](https://serverless-stack.com/chapters/report-api-errors-in-react.html)
    * Sentry
  * [Setup an Error Boundary in React](https://serverless-stack.com/chapters/setup-an-error-boundary-in-react.html). An Error Boundary is a component that allows us to catch any errors that might happen in the child components tree, log those errors, and show a fallback UI.
    * And we should see the error being reported to Sentry as well!
  * [Setup Error Logging in Serverless](https://serverless-stack.com/chapters/setup-error-logging-in-serverless.html)
  * Debug Errors in Lambda Functions
    * [Logic error](https://serverless-stack.com/chapters/logic-errors-in-lambda-functions.html)
    * [Timeout](https://serverless-stack.com/chapters/unexpected-errors-in-lambda-functions.html)
    * Memory error
  * [Errors in API Gateway](https://serverless-stack.com/chapters/errors-in-api-gateway.html)