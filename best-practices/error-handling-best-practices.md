
# ✅ Error Handling Best Practices

---

## 📌 Why Exception Handling is Critical

- **Root Cause Analysis**:  
  Preserves the original error context and enables tracing back to the source.

- **Better Debugging**:  
  Allows understanding the sequence of events leading to the error.

- **Production Stability**:  
  Prevents cascading failures by properly propagating errors.

---

## 📌 Importance of Exception Chaining

- Maintains the complete context of the original error when throwing new exceptions.
- Helps developers identify the root cause quickly.
- Modern JavaScript/Node.js supports `Error.cause` (Node.js 16+).

---

## 📚 Best Practices

---

### 1. Always Chain Exceptions

- When rethrowing an error, include the original error for context.

**Example in JavaScript (NestJS):**

```typescript
try {
    someFunction();
} catch (error) {
    throw new Error(`An error occurred in someFunction: ${error.message}`, { cause: error });
}
````

**NestJS Custom Exception Example:**

```typescript
import { HttpException, HttpStatus } from '@nestjs/common';

try {
    someServiceOperation();
} catch (error) {
    throw new HttpException(`Service error: ${error.message}`, HttpStatus.INTERNAL_SERVER_ERROR, { cause: error });
}
```

---

### 2. Avoid Swallowing Exceptions

* **Never silently catch errors without logging or handling them.**

**❌ Bad Example:**

```typescript
try {
    riskyOperation();
} catch (error) {
    // Do nothing 😞
}
```

**✅ Good Example:**

```typescript
try {
    riskyOperation();
} catch (error) {
    console.error("Error occurred:", error);
    throw error; // Re-throw if necessary
}
```

---

### 3. Log Before Rethrowing Exceptions

* Log errors immediately when they occur to capture local context.

**Example with Winston in NestJS:**

```typescript
import { Logger } from '@nestjs/common';
const logger = new Logger('AppService');

try {
    someFunction();
} catch (error) {
    logger.error("Error in someFunction", {
        message: error.message,
        stack: error.stack,
        context: { functionName: "someFunction" }
    });
    throw error;
}
```

---

### 4. Preserve the Original Error

* Always include the original stack trace and message when throwing a new error.
* Use modern error properties like `Error.cause` (Node.js 16+).

**Example in NestJS:**

```typescript
import { HttpException, HttpStatus } from '@nestjs/common';

try {
    someOperation();
} catch (error) {
    console.error('Error occurred:', error);
    throw new HttpException(`Operation failed: ${error.message}`, HttpStatus.INTERNAL_SERVER_ERROR, { cause: error });
}
```

---

### 5. Graceful Error Handling

* Handle known errors gracefully while logging unknown errors for investigation.

**Example:**

```typescript
try {
    processData();
} catch (error) {
    if (error instanceof KnownError) {
        logger.warn("Handled known error", { error: error.message });
    } else {
        logger.error("Unexpected error occurred", { stack: error.stack });
        throw error; // Re-throw unknown errors for proper handling
    }
}
```

---

## 🚫 Common Pitfalls to Avoid

* **Silent Errors**:
  Never catch errors without logging or handling them.

* **Swallowing Errors**:
  Always rethrow or handle the error appropriately after logging.

* **Losing Context**:
  Avoid throwing generic errors without attaching the original cause.

* **Incorrect Error Types**:
  Use proper HTTP status codes and error types for client and server errors.

---

> 📢 **Pro Tip**: Combine this with our [Logging Best Practices](./logging-best-practices.md) for complete observability.



