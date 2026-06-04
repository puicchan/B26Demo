# Modernization Plan: ZavaPayGateway — Struts to Spring Boot on Azure

**Project**: ZavaPayGateway

---

## Technical Framework

- **Language**: Java 8 (target: Java 21)
- **Framework**: Apache Struts 1.3.10 (target: Spring Boot 3.x)
- **Build Tool**: Gradle 7.6
- **Database**: Microsoft SQL Server (via JDBC with password authentication)
- **Key Dependencies**: struts-core 1.3.10, struts-taglib 1.3.10, mssql-jdbc 12.8.1, javax.servlet-api 3.1.0

---

## Overview

> This migration rewrites the ZavaPayGateway payment processing application from the legacy Apache Struts 1.3.10 MVC framework to Java 21 and Spring Boot 3.x, producing a fully modernized application in a new directory. The application currently runs as a WAR deployed on Tomcat 9, uses direct JDBC with password-based SQL Server authentication, and relies on a custom SSO session mechanism backed by a .NET auth gateway. The new architecture will:
>
> - Replace Struts Actions, ActionForms, and struts-config.xml with Spring MVC controllers and Spring Boot auto-configuration, enabling a maintainable, cloud-ready architecture
> - Migrate from a WAR/Tomcat deployment model to a Spring Boot executable JAR with embedded server, simplifying containerization and deployment to Azure Container Apps
> - Remediate all known CVEs in project dependencies to ensure the application is secure before deployment
>
> The migration is structured in three phases: (1) framework rewrite into a new directory, (2) security hardening, and (3) containerized deployment to Azure Container Apps.

---

## Migration Impact Summary

| Application      | Original Service          | New Azure Service         | Authentication        | Comments                                    |
|------------------|---------------------------|---------------------------|-----------------------|---------------------------------------------|
| ZavaPayGateway   | Apache Struts 1.3.10      | Spring Boot 3.x (Java 21) | N/A                   | Full rewrite into new directory             |
| ZavaPayGateway   | Tomcat WAR deployment     | Azure Container Apps      | Managed Identity      | Containerized Spring Boot executable JAR   |

---

## Tasks

### Task 1 — Upgrade: Rewrite Struts to Spring Boot 3.x / Java 21

Rewrite the application from Apache Struts 1.3.10 to Spring Boot 3.x targeting Java 21, placing the new application in a separate directory within the repository. The rewrite preserves all existing functionality (login, payment processing, payment history, health check) and migrates from WAR packaging to executable JAR.

### Task 2 — Security: CVE Scan and Remediation

Scan all project dependencies for known CVEs and upgrade vulnerable dependencies to the minimum patched versions. Verify the project builds and all tests pass after remediation.

### Task 3 — Deployment: Deploy to Azure Container Apps

Containerize the Spring Boot application and deploy it to Azure Container Apps using Azure CLI.

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — the deployment task will provision new Azure resources as needed; no separate IaC task required.
- [x] Q: Should the plan include integration tests? → A: No — integration testing was not explicitly requested; skipped.
- [x] Q: Should the plan include a security scan and CVE remediation task? → A: Yes — included by default.
- [x] Q: Which Azure deployment target should be used? → A: Azure Container Apps (default).
- [ ] Q: The user requested "Java 21 and latest SpringBoot." Spring Boot 4.x (the absolute latest) requires Java 25 and is incompatible with Java 21. The plan targets Spring Boot 3.x (compatible with Java 21). Should the target be changed to Spring Boot 4.x with Java 25 instead?
