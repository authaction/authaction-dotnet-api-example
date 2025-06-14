# AuthAction .NET API Example

This is a sample ASP.NET Core Web API project that demonstrates how to implement JWT authentication using AuthAction's JWKS (JSON Web Key Set) endpoint. The project shows how to secure API endpoints using JWT tokens issued by AuthAction.

## Features

- ASP.NET Core 8.0 Web API
- JWT Authentication with JWKS validation
- Swagger/OpenAPI documentation
- Development and Production environment configurations
- HTTPS support

## Prerequisites

- .NET 8.0 SDK or later
- An AuthAction tenant account
- Your AuthAction API identifier

## Configuration

1. Clone the repository:

```bash
git clone git@github.com:authaction/authaction-dotnet-api-example.git
cd authaction-dotnet-api-example
```

2. Configure AuthAction settings in `appsettings.Development.json`:

```json
{
  "Auth": {
    "Authority": "https://your-authaction-tenant-domain/",
    "Audience": "your-authaction-api-identifier"
  }
}
```

Replace:

- `your-authaction-tenant-domain` with your AuthAction tenant domain
- `your-authaction-api-identifier` with your API identifier in AuthAction

## Running the Application

1. Restore dependencies:

```bash
dotnet restore
```

2. Run the application:

```bash
dotnet run
```

The API will be available at:

- HTTP: http://localhost:5287

### Swagger UI

You can access the Swagger UI documentation at:

- http://localhost:5287/swagger

The Swagger UI provides interactive documentation where you can:

- View all available endpoints
- Test the API directly from the browser
- See request/response schemas
- Try out the authentication

## API Endpoints

### Weather Forecast

- **Endpoint**: `GET /weatherforecast`
- **Authentication**: Required (JWT Bearer token)
- **Response**: Array of weather forecast data

## Testing the API

1. Get a JWT token from your AuthAction tenant

There are two ways to obtain an access token:

### A. Client Credentials Flow (M2M)

For machine-to-machine communication, use the following curl command:

```bash
curl --request POST \
--url https://your-authaction-tenant-domain/oauth2/m2m/token \
--header 'content-type: application/json' \
--data '{"client_id":"your-authaction-m2m-app-clientid","client_secret":"your-authaction-m2m-app-client-secret","audience":"your-authaction-api-identifier","grant_type":"client_credentials"}'
```

Replace:

- `your-authaction-tenant-domain` with your AuthAction tenant domain
- `your-authaction-m2m-app-clientid` with your M2M application client ID
- `your-authaction-m2m-app-client-secret` with your M2M application client secret
- `your-authaction-api-identifier` with your API identifier

### B. Authorization Code Flow (Frontend)

For frontend applications, you can use the authorization code flow:

1. Configure your frontend application in AuthAction Dashboard with:

   - Allowed Callback URLs
   - Allowed Logout URLs
   - Allowed Web Origins

2. Implement the OAuth2 authorization code flow in your frontend application:
   - Redirect users to AuthAction's authorization endpoint
   - Handle the authorization code callback
   - Exchange the code for an access token
   - Use the access token to call this API

Example frontend configuration:

```javascript
// AuthAction configuration
const authConfig = {
  domain: "your-authaction-tenant-domain",
  clientId: "your-frontend-app-clientid",
  audience: "your-authaction-api-identifier",
  redirectUri: "http://localhost:3000/callback",
};
```

The access token obtained from either flow can be used to call the API endpoints.

Note: Make sure your M2M application or Frontend application has been authorized to access the API in the AuthAction Dashboard.

2. Use the token in your requests:

```http
GET http://localhost:5287/weatherforecast
Authorization: Bearer your-jwt-token
```

You can also use the included `authaction-dotnet-api-example.http` file to test the API using REST Client.

## Project Structure

- `Program.cs`: Main application entry point and configuration
- `appsettings.json`: Base configuration
- `appsettings.Development.json`: Development environment configuration
- `Properties/launchSettings.json`: Application launch settings

## Authentication Flow

1. Client obtains a JWT token from AuthAction
2. Client includes the token in the Authorization header
3. API validates the token using AuthAction's JWKS endpoint
4. If valid, the request is processed; if not, returns 401 Unauthorized

## Security Features

- JWT token validation
- HTTPS redirection in production
- Secure configuration management
- Development/Production environment separation

## Development

The project uses the following key packages:

- `Microsoft.AspNetCore.Authentication.JwtBearer`: For JWT authentication
- `Microsoft.IdentityModel.Protocols.OpenIdConnect`: For OpenID Connect support
- `Swashbuckle.AspNetCore`: For API documentation

## Common Issues

#### **Invalid Token Errors**:

- Ensure that the token being used is signed by AuthAction using the `RS256` algorithm and contains the correct issuer and audience claims.
- Verify that the `Authority` and `Audience` properties are correctly set in `appsettings.Development.json`.

#### **Public Key Fetching Errors**:

- If there are issues retrieving the public keys from AuthAction, check the JWKS URI and ensure your application can reach the AuthAction servers.
- The JWKS URI should be: `https://your-authaction-tenant-domain/.well-known/jwks.json`

#### **Unauthorized Access**:

- If requests to the protected route (`/weatherforecast`) are failing, ensure that:
  - The JWT token is being correctly included in the `Authorization` header
  - The token is valid and not expired
  - The token's audience matches your API identifier
  - The token's issuer matches your AuthAction domain
