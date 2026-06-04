# Policies

Enforceable standards and hard boundaries for Java modernization. Every policy here is validatable against generated artifacts.

## Security Requirements

### Authentication & Authorization

- Use `com.azure:azure-identity` with `DefaultAzureCredentialBuilder` for all Azure service authentication.
- `DefaultAzureCredential` must be used consistently across local development, CI/CD pipelines, and Azure-hosted environments.

### Secrets Management

- All secrets must be stored in and retrieved from Azure Key Vault using `SecretClient`.
- Secrets must not be stored in source code, configuration files, or environment-specific repositories.
- Connection strings must not be used when Azure Entra ID authentication is supported.

## Guardrails (Hard Boundaries)

### Prohibited Technologies

| Technology | Reason | Approved Alternative |
|-----------|--------|---------------------|
| Hardcoded secrets | Security risk; violates secret management policy | Azure Key Vault via `SecretClient` |
| Credentials in source code | Security risk | Azure Key Vault via `SecretClient` |

### Prohibited Patterns

| Pattern | Reason | Approved Alternative |
|---------|--------|---------------------|
| SQL string concatenation using user input | SQL injection risk | `PreparedStatement` with parameterized queries |
| Unsafe dynamic SQL generation | SQL injection risk | `PreparedStatement` with parameterized queries |
| Embedded credentials in JDBC URLs | Security risk | Azure Entra ID authentication via `DefaultAzureCredentialBuilder` |
| Connection strings when Azure Entra ID is supported | Security risk | `DefaultAzureCredentialBuilder` |
| Credentials stored in configuration files | Security risk | Azure Key Vault via `SecretClient` |
| Credentials stored in environment-specific repositories | Security risk | Azure Key Vault via `SecretClient` |

### Required Elements

Every modernized Java application must include:

#### Monitoring

- `opentelemetry-javaagent` attached at startup via `-javaagent:opentelemetry-javaagent.jar`
- Application must emit traces, metrics, and logs (where supported)
- Telemetry must be exported to Azure Monitor, OpenTelemetry Collector, or an OTLP-compatible backend

#### Authentication

- `azure-identity` dependency with `DefaultAzureCredentialBuilder` used for all Azure service authentication

#### Health Endpoints

- `spring-boot-starter-actuator` dependency included
- `management.endpoints.web.exposure.include=health` configured
- `/actuator/health` endpoint exposed
- `/actuator/health/liveness` and `/actuator/health/readiness` endpoints exposed (recommended)

#### Secret Management

- Azure Key Vault `SecretClient` used for all secret retrieval

#### Database Access

- JPA (`@Entity`) used for ORM
- `PreparedStatement` used for raw SQL queries

## Validation & Quality Gates

### Pipeline Gates

- Applications that do not comply with these standards are rejected from:
  - Production onboarding
  - Platform certification
  - Migration approval
  - Operational support readiness

## Coding Style Guidelines

### Java

- Use `@Entity` annotation for JPA-managed domain objects.
- Use `PreparedStatement` with parameterized queries (`?` placeholders) for all raw SQL.
- Do not concatenate user input into SQL strings.
