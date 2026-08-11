## Validation Rule

Use validation mainly at application boundaries.

Any data coming from outside the application, such as:

- Request body
- Query parameters
- URL parameters
- External APIs or services

should be validated before being used.

Internal server-side data that is already controlled by our application usually does not need repeated validation unless there is a specific reason, such as data migrations, background jobs, or other processes where the data source may not be fully trusted.

The goal is to validate where it provides value: protecting the application boundary without adding unnecessary complexity throughout the codebase.
