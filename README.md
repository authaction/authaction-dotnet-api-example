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
