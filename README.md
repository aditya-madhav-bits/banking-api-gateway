# API Gateway Service  

![Build](https://img.shields.io/badge/Build-Passing-brightgreen.svg) ![License](https://img.shields.io/github/license/aditya-madhav-bits/api-gateway-service)

The **API Gateway Service** acts as a single entry point for routing, load balancing, and authentication in the banking application ecosystem. Built using Spring Cloud Gateway, this service integrates with Eureka for service discovery and Keycloak for OAuth2-based authentication.  

## Features ✨
- **Dynamic Routing**: Forward requests to underlying microservices based on route configuration.  
- **Load Balancing**: Leverages Eureka for dynamic service discovery.  
- **Authentication and Authorization**: Secures endpoints using Keycloak and JWT-based authentication.  
- **Scalable Architecture**: Supports easy integration of new services.  

## Route Configuration 📡

The following routes are configured in the gateway:  

| **Route ID**           | **Path**               | **Service URI**                |
|-------------------------|------------------------|---------------------------------|
| `user-service`          | `/api/users/**`       | `lb://user-service`            |
| `account-service`       | `/accounts/**`        | `lb://account-service`         |
| `transaction-service`   | `/transactions/**`    | `lb://transaction-service`     |
| `fund-transfer-service` | `/fund-transfers/**`  | `lb://fund-transfer-service`   |

- `lb://` indicates that the service URI is resolved using Eureka.

## Security 🛡️

### Keycloak Integration  
The service uses Keycloak for OAuth2 authentication and JWT validation.  

#### Keycloak Configuration  
- **Keycloak URL**: `http://localhost:8571/`  
- **Realm**: `banking-service`  
- **Client ID**: `banking-service-client`  
- **Redirect URI**: `http://localhost:8571/login/oauth2/code/keycloak`  

The gateway validates tokens and secures routes based on JWTs issued by Keycloak.  

## YAML Configuration 🗂️

### Server Configuration  
- **Port**: `8080`  

### Eureka Client  
The gateway is registered with the Eureka server:  
- **Default Zone**: `http://localhost:8761/eureka`  

### OAuth2 Configuration  
```yaml
spring:
  security:
    oauth2:
      client:
        provider:
          keycloak:
            token-uri: ${app.config.keycloak.url}/realms/${app.config.keycloak.realm}/protocol/openid-connect/token
            authorization-uri: ${app.config.keycloak.url}/realms/${app.config.keycloak.realm}/protocol/openid-connect/auth
            user-info-uri: ${app.config.keycloak.url}/realms/${app.config.keycloak.realm}/protocol/openid-connect/userinfo
        registration:
          banking-service-client:
            client-id: banking-service-client
            client-secret: <client-secret>
            redirect-uri: http://localhost:8571/login/oauth2/code/keycloak
```  

## Installation 🚀

1. Clone the repository:  
   ```bash
   git clone https://github.com/aditya-madhav-bits/api-gateway-service.git
   cd api-gateway-service
   ```  

2. Configure the application:  
   Update the `application.yml` file with the following:  
   - Keycloak URL  
   - Eureka server URL  
   - Client credentials  

3. Build and run the application:  
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```  

4. Access the gateway at `http://localhost:8080`.  

## Testing 🧪
To verify routes and security:  
1. Obtain a JWT from Keycloak.  
2. Use the JWT in the `Authorization` header for API calls:  
   ```bash
   curl -H "Authorization: Bearer <JWT_TOKEN>" http://localhost:8080/api/users/
   ```  
