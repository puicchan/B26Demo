# Targets

Approved target technologies for Java modernization. Defines *what is approved* — not implementation steps.

## Target Data Services

| Service | Use When |
|---------|----------|
| Azure Key Vault | Storing and retrieving secrets (database passwords, API keys, certificates, connection secrets) |

## Target Integration Services

| Service | Use When |
|---------|----------|
| Azure Monitor | Receiving exported telemetry (traces, metrics, logs) from applications |
| OpenTelemetry Collector | Aggregating and forwarding telemetry to OTLP-compatible backends |

## Target Libraries

Source → target mappings for Java.

| Category | Source | Target | Notes |
|----------|--------|--------|-------|
| Authentication | Any custom/legacy auth | `com.azure:azure-identity` (`DefaultAzureCredentialBuilder`) | Required for all Azure-authenticated calls |
| Observability | Any custom/legacy instrumentation | `io.opentelemetry.javaagent:opentelemetry-javaagent` | Attach via `-javaagent:opentelemetry-javaagent.jar` at startup |
| Health Endpoints | Custom health checks / none | `org.springframework.boot:spring-boot-starter-actuator` | Expose `/actuator/health`; recommend liveness and readiness sub-paths |
| Secret Management | Inline secrets / config-file secrets | `com.azure:azure-security-keyvault-secrets` (`SecretClient`) | Requires `DefaultAzureCredentialBuilder` for authentication |
| Database ORM | Raw JDBC / dynamic SQL | JPA (`@Entity`, repository pattern) | Use alongside `PreparedStatement` for raw SQL |
