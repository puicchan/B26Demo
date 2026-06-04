# Modernization Plan: ZavaPayGateway — Modernize and Deploy to Azure

**Project**: ZavaPayGateway

---

## Technical Framework

- **Language**: Java 8
- **Framework**: Apache Struts 1.3.10 (legacy MVC)
- **Build Tool**: Gradle 7.6 (WAR packaging)
- **Database**: Microsoft SQL Server (password-based JDBC authentication)
- **Key Dependencies**: javax.servlet 3.1.0, mssql-jdbc 12.8.1, Apache Struts 1.3.10
- **Container**: Tomcat 9 (Dockerfile exists)

---

## Overview

This migration modernizes the ZavaPayGateway payment processing application and deploys it to Azure. The application currently runs as a Java 8 Struts 1.x WAR on Tomcat with password-based SQL Server authentication and hardcoded plaintext credentials in its properties file. The new architecture will:

- Remove hardcoded plaintext database credentials and store secrets securely in Azure Key Vault for centralized secret management
- Scan and remediate known CVE vulnerabilities in legacy dependencies (Apache Struts 1.3.10 and others) to ensure a secure deployment baseline
- Deploy the containerized application to Azure Container Apps, leveraging the existing Dockerfile for cloud-native hosting with managed scaling and infrastructure

The migration follows a sequential approach: secure secrets first, remediate CVEs, then deploy to Azure Container Apps.

---

## Migration Impact Summary

| Application       | Original Service         | New Azure Service         | Authentication     | Comments                          |
|-------------------|--------------------------|---------------------------|--------------------|-----------------------------------|
| ZavaPayGateway    | Plaintext credentials    | Azure Key Vault           | Managed Identity   | DB password and config secrets    |
| ZavaPayGateway    | Tomcat (self-hosted)     | Azure Container Apps      | N/A                | Deploy using existing Dockerfile  |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: Yes — deploy to Azure Container Apps with new infrastructure provisioned via Bicep
- [x] Q: Should the plan include integration testing? → A: No — integration testing not explicitly requested; skipped
- [x] Q: Should the plan include security/CVE remediation? → A: Yes — include security/CVE scan and remediation (default)
- [x] Q: Which Azure deployment target? → A: Azure Container Apps (default)
