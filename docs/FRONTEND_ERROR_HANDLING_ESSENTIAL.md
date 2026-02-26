# Frontend Error Handling - Essential Changes

> **Note**: To be implemented after gaining more experience with React/Next.js patterns

## Overview

The frontend has basic error handling but needs essential improvements for production readiness:

- Standardized error classes with user-friendly messages
- Automatic retry for transient failures
- Error boundaries to prevent full app crashes
- Enhanced error display component

## Essential Changes

### 1. Standardized Error Class

**File**: `src/lib/api.ts`

Add a custom error class that provides structured error information:

```typescript
export class MTGVAPIError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public type?: string,
    public details?: any
  ) {
    super(message);
    this.name = 'MTGVAPIError';
  }
}

// Update handleError in mtgvAPI
private handleError(error: any): never {
  if (error.response) {
    const { status, data } = error.response;
    throw new MTGVAPIError(
      data.error || 'An error occurred',
      status,
      data.type,
      data.details
    );
  }

  if (error.request) {
    throw new MTGVAPIError(
      'Unable to reach the server. Please check your internet connection.',
      0,
      'NetworkError'
    );
  }

  throw new MTGVAPIError(
    error.message || 'An unexpected error occurred',
    500,
    'UnknownError'
  );
}
```

Map backend error types to user-friendly messages:

```typescript
const ERROR_MESSAGES = {
  'NetworkError': 'Unable to connect to the server. Please check your internet connection.',
  'ValidationError': 'The information provided is invalid. Please check your input.',
  'NotFoundError': 'The requested item could not be found.',
  'RateLimitError': 'Too many requests. Please wait a moment and try again.',
  'DatabaseError': 'We\'re experiencing technical difficulties. Please try again shortly.',
  'default': 'Something went wrong. Please try again.',
};

export function getUserFriendlyMessage(error: MTGVAPIError): string {
  return ERROR_MESSAGES[error.type || 'default'] || ERROR_MESSAGES.default;
}
```

### 2. Basic Retry Mechanism

**File**: `src/lib/retry.ts`

Add automatic retry for transient failures (network errors, 5xx responses):

```typescript
export async function withRetry<T>(
  fn: () => Promise<T>,
  options: {
    maxRetries?: number;
    delay?: number;
    shouldRetry?: (error: any) => boolean;
  } = {}
): Promise<T> {
  const { maxRetries = 3, delay = 1000, shouldRetry } = options;

  let lastError: any;

  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error;

      if (shouldRetry && !shouldRetry(error)) {
        throw error;
      }

      if (attempt === maxRetries - 1) {
        throw error;
      }

      // Exponential backoff
      await new Promise(resolve =>
        setTimeout(resolve, delay * Math.pow(2, attempt))
      );
    }
  }

  throw lastError;
}
```

Apply to critical API calls:

```typescript
export async function createCardPackage(data) {
  return withRetry(
    () => mtgvAPI.post('/card_package', data),
    {
      maxRetries: 3,
      shouldRetry: (error) => {
        // Only retry network errors or server errors
        return error.statusCode === 0 || error.statusCode >= 500;
      },
    }
  );
}
```

### 3. Error Boundary Component

**File**: `src/components/ErrorBoundary.tsx`

Catch React rendering errors to prevent full app crashes:

```typescript
'use client';

import React from 'react';

interface ErrorBoundaryState {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends React.Component<
  { children: React.ReactNode },
  ErrorBoundaryState
> {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error boundary caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Something went wrong</h2>
          <p>Please refresh the page to continue.</p>
          <button onClick={() => window.location.reload()}>
            Refresh Page
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}
```

Wrap the app in layout.tsx:

```typescript
<ErrorBoundary>
  {children}
</ErrorBoundary>
```

### 4. Enhanced Error Display

**File**: `src/components/ErrorDisplay.tsx` (already exists)

Update to show user-friendly messages and retry option:

```typescript
export interface ErrorDisplayProps {
  error: MTGVAPIError | Error | string;
  onRetry?: () => void;
}

export function ErrorDisplay({ error, onRetry }: ErrorDisplayProps) {
  const message = error instanceof MTGVAPIError
    ? getUserFriendlyMessage(error)
    : error instanceof Error
    ? error.message
    : error;

  const canRetry = error instanceof MTGVAPIError &&
    (error.statusCode === 0 || error.statusCode >= 500);

  return (
    <div className="error-display" role="alert">
      <div className="error-content">
        <h3>Error</h3>
        <p>{message}</p>
      </div>
      {canRetry && onRetry && (
        <button onClick={onRetry}>Try Again</button>
      )}
    </div>
  );
}
```

## Implementation Checklist

- [ ] Create MTGVAPIError class in api.ts
- [ ] Add user-friendly error message mapping
- [ ] Create retry utility function in retry.ts
- [ ] Apply retry to critical API calls (createCardPackage, updateCardPackage)
- [ ] Create ErrorBoundary component
- [ ] Wrap app in ErrorBoundary in layout.tsx
- [ ] Update ErrorDisplay component with user-friendly messages
- [ ] Add retry button to ErrorDisplay for retryable errors
- [ ] Test error handling with network failures and server errors
- [ ] Update component error handling to use new error types

## Testing

Test these scenarios:

1. Network failure (disconnect internet) - should show connection message and retry
2. 500 server error - should retry automatically then show error
3. 400 validation error - should show validation message, no retry
4. React component crash - should show error boundary, not crash app
5. Rate limit (429) - should show rate limit message

## Future Enhancements (Post-MVP)

These can be added later as needed:

- Toast notification system for non-blocking errors
- Network status detection (online/offline indicator)
- Integration with error tracking service (Sentry)
- Advanced retry strategies (circuit breaker pattern)
- Error analytics and monitoring

## Summary

These essential changes provide:

- ✅ Clear, user-friendly error messages
- ✅ Automatic retry for transient failures
- ✅ Protection against full app crashes
- ✅ Consistent error handling across the app

Total implementation time: ~4-6 hours after learning React/Next.js patterns.
