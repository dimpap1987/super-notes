- [[Api]]
- [[Security]]

## Overview
- Authorization framework (not authentication)
- Delegated access to resources
- Industry standard (RFC 6749)
- Enables third-party access without sharing credentials

## Grant Types

### Authorization Code (Recommended)
- Server-side applications
- Most secure flow
- Redirects user to authorization server
- Exchanges code for tokens

### Client Credentials
- Machine-to-machine communication
- No user involved
- Direct token request

### Refresh Token
- Obtain new access tokens
- Extends session without re-authentication

### Implicit (Deprecated)
- Legacy flow, security concerns
- Not recommended

### Password (Legacy)
- Direct username/password
- Not recommended for production

## Key Components
- **Resource Owner** - user who owns the data
- **Client** - application requesting access
- **Authorization Server** - issues tokens
- **Resource Server** - API with protected resources

## Tokens
- **Access Token** - short-lived (15-60 min), used for API calls
- **Refresh Token** - long-lived, used to get new access tokens
- **ID Token** - (OpenID Connect) user identity information

## Flow Example
```
1. Client redirects user to authorization server
2. User authenticates and grants permission
3. Authorization server redirects with code
4. Client exchanges code for access token
5. Client uses access token for API calls
6. When expired, use refresh token for new access token
```

## Best Practices
- Use HTTPS only
- Store tokens securely
- Use short-lived access tokens
- Implement token revocation
- Validate tokens on resource server
- Use PKCE for mobile/public clients

