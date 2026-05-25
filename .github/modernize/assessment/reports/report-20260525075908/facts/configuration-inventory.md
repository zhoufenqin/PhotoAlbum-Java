# Configuration & Externalized Settings Inventory

The project configuration is driven by Spring property files plus Docker Compose environment overrides, with a small number of runtime profiles and upload-related custom properties. Sensitive values are currently embedded in local config examples and compose environment sections.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Spring default config | Properties file | `src/main/resources/application.properties` | Primary runtime settings |
| Spring docker profile config | Properties file | `src/main/resources/application-docker.properties` | Overrides for docker profile |
| Docker Compose | Compose YAML | `docker-compose.yml` | Defines service env vars and startup dependencies |
| Maven build config | Build file | `pom.xml` | Build/dependency configuration and Java version |
| PowerShell setup env output | Script-generated env file | `.env` created by `azure-setup.ps1` | Deployment helper environment values |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| default Maven build | Automatic | Build executable Spring Boot jar | `spring-boot-maven-plugin` |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| default | Implicit (no `spring.profiles.active`) | `application.properties` | Oracle datasource, DEBUG logging, upload limits |
| docker | `SPRING_PROFILES_ACTIVE=docker` | `application.properties` + `application-docker.properties` | Docker datasource/logging values |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `server.port` | `8080` | default,docker | application properties |
| `server.servlet.encoding.charset` | `UTF-8` | default,docker | application properties |
| `server.servlet.encoding.enabled` | `true` | default,docker | application properties |
| `server.servlet.encoding.force` | `true` | default,docker | application properties |
| `spring.datasource.url` | Oracle JDBC URL | default,docker (+env override in compose) | application properties + compose env |
| `spring.datasource.username` | `photoalbum` | default,docker (+env override) | application properties + compose env |
| `spring.datasource.password` | `[MASKED]` | default,docker (+env override) | application properties + compose env |
| `spring.datasource.driver-class-name` | `oracle.jdbc.OracleDriver` | default,docker | application properties |
| `spring.jpa.database-platform` | `org.hibernate.dialect.OracleDialect` | default,docker | application properties |
| `spring.jpa.hibernate.ddl-auto` | `create` | default,docker | application properties |
| `spring.jpa.show-sql` | `true` | default,docker | application properties |
| `spring.jpa.properties.hibernate.format_sql` | `true` | default,docker | application properties |
| `spring.servlet.multipart.max-file-size` | `10MB` | default,docker | application properties |
| `spring.servlet.multipart.max-request-size` | `50MB` | default,docker | application properties |
| `app.file-upload.max-file-size-bytes` | `10485760` | default,docker | application properties |
| `app.file-upload.allowed-mime-types` | `image/jpeg,image/png,image/gif,image/webp` | default,docker | application properties |
| `app.file-upload.max-files-per-upload` | `10` | default,docker | application properties |
| `logging.level.com.photoalbum` | `DEBUG` (default), `INFO` (docker) | default,docker | application properties |
| `logging.level.org.springframework.web` | `DEBUG` (default), `WARN` (docker) | default,docker | application properties |
| `logging.level.org.hibernate.SQL` | `DEBUG` (docker only) | docker | application-docker.properties |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| photoalbum-java-app (Dockerfile) | `JAVA_OPTS="-Xmx512m -Xms256m"` | 256m-512m heap declared | 1 (compose default) |
| oracle-db | Container defaults | Not explicitly set in compose | 1 (compose default) |

## Startup Dependency Chain

1. `oracle-db` starts and must become healthy via container healthcheck.
2. `photoalbum-java-app` waits on `oracle-db` with `depends_on: condition: service_healthy`.
3. Spring Boot app initializes datasource and begins serving on port 8080.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `spring.datasource.password` / `SPRING_DATASOURCE_PASSWORD` | Database password | application properties / compose env (`[MASKED]`) |
| `ORACLE_PASSWORD` | Oracle admin password | compose env (`[MASKED]`) |
| `APP_USER_PASSWORD` | Oracle app user password | compose env (`[MASKED]`) |
| `POSTGRES_PASSWORD` (azure setup output) | Database password | generated `.env` (`[MASKED]`) |

### Secrets Provisioning Workflow

Current local workflow sets secrets directly in property files and compose environment variables, with optional `.env` generation by Azure setup scripts. No managed identity, centralized secret store, or RBAC-based secret retrieval flow is configured in this repository.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No explicit feature flags detected | N/A | N/A |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Spring Boot | 2.7.18 | `pom.xml` parent |
| Java target | 1.8 | `pom.xml` properties |
| Maven compiler source/target | 8 / 8 | `pom.xml` properties |
| commons-io | 2.11.0 | `pom.xml` dependency |
| Docker build image | `maven:3.9.6-eclipse-temurin-8` | `Dockerfile` |
| Docker runtime image | `eclipse-temurin:8-jre` | `Dockerfile` |
| Oracle container image | `gvenzl/oracle-free:latest` | `docker-compose.yml` |
