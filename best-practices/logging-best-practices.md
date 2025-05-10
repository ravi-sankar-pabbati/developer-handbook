## ✅ **Best Practices for Logging**

## 1. Logging Stack Traces

- Always log the `stack` property of errors to capture the complete stack trace.
- Ensure stack traces are included in all error logs, especially in production environments.

**Example with Winston in NestJS:**

```typescript
import { Logger } from '@nestjs/common';
const logger = new Logger('AppService');

try {
    someFunction();
} catch (error) {
    logger.error("Error occurred", {
        message: error.message,
        stack: error.stack,
        context: {
            functionName: "someFunction"
        }
    });
}
````

---

## 2. Use Structured Logging

* Prefer JSON format for easy parsing by tools like ELK, Datadog, or Splunk.

**Example:**

```typescript
logger.error("An error occurred", {
    context: {
        userId: "12345",
        operation: "fetchUserData"
    },
    message: "User not found",
    stack: new Error("User not found").stack
});
```

---

## 3. Include Relevant Context

* Log key details such as:

  * User ID
  * Request details (method, path, parameters)
  * Operation metadata (function/module name)

**Example:**

```json
{
    "context": {
        "email": "user@example.com",
        "operation": "createUser",
        "requestId": "abc-123"
    },
    "message": "Validation error",
    "stack": "Error: Validation failed at ..."
}
```

---

## 4. Avoid Sensitive Data

* **Do not log sensitive information** like passwords, tokens, or credit card details.
* If necessary, mask or redact them.

**❌ Bad Example:**

```typescript
logger.error("Failed login attempt", { password: "123456" });
```

**✅ Good Example:**

```typescript
logger.error("Failed login attempt", { password: "******" });
```

---

## 5. Centralized Error Logging

* Use middleware or interceptors to capture and log unhandled errors globally.

**NestJS Exception Filter Example:**

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException, Logger } from '@nestjs/common';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
    private readonly logger = new Logger(AllExceptionsFilter.name);

    catch(exception: unknown, host: ArgumentsHost) {
        const ctx = host.switchToHttp();
        const response = ctx.getResponse();
        const request = ctx.getRequest();
        const status = exception instanceof HttpException ? exception.getStatus() : 500;

        this.logger.error("Unhandled exception", {
            message: (exception as any).message,
            stack: (exception as any).stack,
            path: request.url
        });

        response.status(status).json({
            statusCode: status,
            timestamp: new Date().toISOString(),
            path: request.url
        });
    }
}
```

---

## 6. Standardize Log Schema

* Define a consistent structure for logs to make them easy to parse and analyze.

**Example JSON Schema:**

```json
{
    "timestamp": "2025-01-16T12:34:56.789Z",
    "level": "error",
    "message": "An error occurred",
    "context": {
        "userId": "12345",
        "operation": "fetchData"
    },
    "stack": "Error: Something went wrong at ..."
}
```

---

> 📢 **Pro Tip:** Invest in centralized log management tools like AWS CloudWatch, ELK, Datadog, or Splunk to easily search, visualize, and monitor your logs.

```

---

