# Charter

## Metadata

| Field | Value |
|-------|-------|
| Rulebook Name | Java Modernization Standards |
| Version | 1.0 |
| Changelog | Initial rulebook from modernization compliance standards |

## Scope

### Covered Applications and Languages

Java applications using Spring Boot are in scope.

### Constraints

Applications must comply with all standards defined in this rulebook to be eligible for production onboarding, platform certification, migration approval, and operational support readiness.

## Principles

- Use cloud-native, credential-free authentication via `DefaultAzureCredentialBuilder` across all environments (local, CI/CD, Azure-hosted).
- Secrets must never be stored in source code, configuration files, or environment-specific repositories.
- All applications must expose health endpoints to support Kubernetes liveness and readiness probes.
- Database access must use JPA and prepared statements to ensure portability and prevent SQL injection.
- Telemetry (traces, metrics, logs) must be emitted and exported to an approved observability backend.
