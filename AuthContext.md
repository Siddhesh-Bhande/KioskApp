# Authentication Context Documentation

## Overview

The `AuthContext` provides authentication state management and token handling functionality across the application. It implements JWT-based authentication with automatic token refresh capabilities and persistent authentication state.

## Component Features

- JWT token management
- Automatic token refresh
- Persistent authentication state
- Token verification
- User session management
- Secure logout handling
- Loading state management

## Context State

The context manages several key states:

- `user`: Current authenticated user information
- `token`: JWT authentication token
- `isLoading`: Authentication verification status
- `refreshAttempts`: Counter for token refresh attempts
- `isAuthenticated`: Boolean indicating authentication status

## Core Types

### User Interface

```typescript
interface User {
  id: number; // Unique user identifier
  email: string; // User's email address
  name: string; // User's full name
  role: string; // User's system role
  project_role: string; // User's project-specific role
}
```

### JWT Payload Interface

```typescript
interface JwtPayload {
  exp: number; // Token expiration timestamp
  sub: string; // Subject identifier
  user_id: number; // User's ID
  email: string; // User's email
  [key: string]: any; // Additional custom claims
}
```

### Context Interface

```typescript
interface AuthContextType {
  user: User | null;
  token: string | null;
  login: (token: string, user: User) => void;
  logout: () => void;
  isAuthenticated: boolean;
  getToken: () => string | null;
  refreshToken: () => Promise<string | null>;
  isLoading: boolean;
}
```

## Core Functions

### `isTokenExpired`

**Purpose**: Checks if the current JWT token is expired
**Usage**: Called before token usage and during verification
**Logic**:

- Decodes JWT token using jose library
- Compares expiration time with current time
- Includes 5-minute buffer for edge cases
- Returns boolean indicating expiration status

### `verifyToken`

**Purpose**: Validates the current authentication token
**Usage**: Automatically called on initial load
**Logic**:

- Checks for token existence
- Verifies token expiration
- Makes API call to validate token
- Handles token refresh if needed
- Manages refresh attempts
- Triggers logout on validation failure

### `login`

**Purpose**: Handles user authentication
**Usage**: Called when user successfully logs in
**Logic**:

- Resets refresh attempts counter
- Updates token and user state
- Persists authentication data in localStorage
- Updates context state

### `logout`

**Purpose**: Manages user logout process
**Usage**: Called on user logout or authentication failure
**Logic**:

- Clears token and user state
- Removes stored authentication data
- Resets refresh attempts
- Redirects to login page
- Prevents back navigation to authenticated state

### `getToken`

**Purpose**: Retrieves current authentication token
**Usage**: Used for authenticated API requests
**Logic**:

- Checks token expiration
- Initiates token refresh if needed
- Returns current token
- Manages refresh attempts

### `refreshToken`

**Purpose**: Renews expired authentication token
**Usage**: Automatically called when token expires
**Logic**:

- Makes API call to refresh endpoint
- Updates token in state and storage
- Handles refresh failures
- Returns new token or null

## API Integration

### Token Verification API

**Endpoint**: `API_ENDPOINTS.AUTH.VERIFY_TOKEN`
**Method**: POST
**Headers**:

- Content-Type: application/json
- Authorization: Bearer token
  **Response**: Token validity status

### Token Refresh API

**Endpoint**: `API_ENDPOINTS.AUTH.REFRESH_TOKEN`
**Method**: POST
**Headers**:

- Content-Type: application/json
- Authorization: Bearer token
  **Response**: New access token

## Error Handling

- Token verification failures
- Refresh attempt limits
- API error handling
- Network error management
- Invalid token handling
- Unauthorized access handling

## Security Features

1. Automatic token expiration handling
2. Refresh token mechanism
3. Maximum refresh attempts limit
4. Secure token storage
5. Forced logout on authentication failures
6. Prevention of back navigation after logout

## Usage Example

```typescript
import { useAuth } from "../contexts/AuthContext";

function AuthenticatedComponent() {
  const { user, isAuthenticated, logout } = useAuth();

  if (!isAuthenticated) {
    return <Navigate to="/login" />;
  }

  return (
    <div>
      <p>Welcome, {user?.name}</p>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

## Provider Implementation

```typescript
import { AuthProvider } from "../contexts/AuthContext";

function App() {
  return (
    <AuthProvider>
      <YourAppComponents />
    </AuthProvider>
  );
}
```

## Dependencies

- React (createContext, useState, useContext, useEffect, useRef)
- jwt-decode (jose)
- axios
- localStorage API

## Best Practices Implemented

1. Persistent authentication state
2. Automatic token refresh
3. Secure token handling
4. Type safety with TypeScript
5. Error boundary implementation
6. Loading state management
7. Proper cleanup on unmount
8. Centralized authentication logic
