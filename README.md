# auth-gateway

A lightweight and extensible API gateway focused on authentication and authorization.

## Description

`auth-gateway` provides a central point of entry for your microservices, handling authentication and authorization before routing requests to the appropriate backend service. It simplifies security management by offloading authentication logic from individual services, promoting a consistent and secure environment. This gateway is designed to be easily configured and extended to meet the specific needs of your application.

## Features

*   **Authentication:**
    *   Supports multiple authentication methods (e.g., JWT, API Keys, OAuth 2.0).
    *   Pluggable authentication providers for easy integration with various identity providers.
    *   Authentication middleware for verifying request credentials.
*   **Authorization:**
    *   Role-Based Access Control (RBAC) with fine-grained permission management.
    *   Policy-Based Access Control (PBAC) support for more complex authorization rules.
    *   Authorization middleware to enforce access control policies.
*   **Request Routing:**
    *   Dynamic routing based on request path, headers, or other criteria.
    *   Supports load balancing across multiple backend instances.
    *   Health checks for backend services to ensure high availability.
*   **Rate Limiting:**
    *   Protects backend services from abuse with configurable rate limits.
    *   Supports different rate limiting strategies (e.g., token bucket, leaky bucket).
*   **Logging and Monitoring:**
    *   Comprehensive logging of authentication, authorization, and routing events.
    *   Integration with monitoring tools (e.g., Prometheus, Grafana) for real-time visibility.
*   **Configuration:**
    *   Configuration via YAML or JSON files.
    *   Environment variable support for dynamic configuration.
    *   Hot reloading of configuration changes without restarting the gateway.
*   **Extensibility:**
    *   Plugin architecture for adding custom authentication providers, authorization policies, and request transformations.
    *   Middleware support for intercepting and modifying requests and responses.

## Technologies Used

*   **Language:** Go
*   **Web Framework:** Gin Gonic
*   **Configuration Management:** Viper
*   **Logging:** Zap
*   **Testing:** Go Test, Testify
*  **(Optional) Database:** PostgreSQL (for storing user roles, policies, etc., if needed)
*  **(Optional) Caching:** Redis (for caching authentication tokens, policies, etc., if needed)

## Installation

### Prerequisites

*   Go 1.20 or higher
*   Git

### Steps

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/your-username/auth-gateway.git
    cd auth-gateway
    ```

2.  **Build the application:**

    ```bash
    go build -o auth-gateway main.go
    ```

3.  **Configure the gateway:**

    *   Create a `config.yaml` file in the root directory. An example configuration file (`config.example.yaml`) is provided in the repository.

    ```yaml
    # Example config.yaml
    server:
      port: 8080

    auth:
      jwt:
        secret: "your-secret-key"
        issuer: "auth-gateway"

    routes:
      - path: /api/users
        backend: http://localhost:8081
        methods: [GET, POST]
        authentication: jwt
        authorization: admin_role
      - path: /api/products
        backend: http://localhost:8082
        methods: [GET]
        authentication: api_key
        authorization: read_only
    ```

4.  **Run the application:**

    ```bash
    ./auth-gateway
    ```

### Configuration Details

The `config.yaml` file defines the behavior of the auth-gateway.

*   **`server.port`**: The port on which the gateway will listen for incoming requests.
*   **`auth`**: Contains configuration for different authentication methods:
    *   **`jwt`**: Configuration for JWT authentication.
        *   **`secret`**: The secret key used to sign and verify JWTs.  **Important: Use a strong and randomly generated secret key in production.**
        *   **`issuer`**: The expected issuer of the JWT.
    *   **`api_key`**: (Example - implementation details may vary)
        *   **`header`**: The HTTP header containing the API key.
        *   **`keys`**: A list of valid API keys.  (Alternatively, this could be a database lookup)
*   **`routes`**: A list of route definitions. Each route specifies:
    *   **`path`**: The path that the gateway will route.
    *   **`backend`**: The URL of the backend service.
    *   **`methods`**: The HTTP methods allowed for this route.
    *   **`authentication`**: The authentication method to use (e.g., `jwt`, `api_key`).  Set to `none` to skip authentication.
    *   **`authorization`**: The authorization policy to enforce (e.g., `admin_role`, `read_only`).  Set to `none` to skip authorization.

### Environment Variables

You can use environment variables to override configuration values.  For example, to change the server port:

```bash
SERVER_PORT=9000 ./auth-gateway
```

## Usage

Once the gateway is running, you can send requests to the configured paths. The gateway will authenticate and authorize the requests before routing them to the backend services.

**Example (JWT Authentication):**

1.  Obtain a JWT from your identity provider.
2.  Include the JWT in the `Authorization` header of your request:

    ```
    Authorization: Bearer <your_jwt_token>
    ```

**Example (API Key Authentication):**

1.  Obtain an API key.
2.  Include the API key in the configured header (e.g., `X-API-Key`):

    ```
    X-API-Key: <your_api_key>
    ```

## Contributing

Contributions are welcome! Please submit pull requests with detailed descriptions of your changes.  Follow the established coding style and include unit tests for any new functionality.

## License

[MIT License](LICENSE) (Replace with the actual license file)