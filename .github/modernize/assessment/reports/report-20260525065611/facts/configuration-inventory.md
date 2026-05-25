# Configuration & Externalized Settings Inventory

The application has 3 configuration file sources (default, docker, test profiles) and relies entirely on in-process properties and Docker Compose environment variables with no external config server or secret store.

## Configuration Sources

| Source | Type | Path / Location | Notes |
|---|---|---|---|
| `application.properties` | Spring Boot default profile | `src/main/resources/application.properties` | Active in local/IDE runs; connects to Oracle at `oracle-db:1521` |
| `application-docker.properties` | Spring Boot profile: `docker` | `src/main/resources/application-docker.properties` | Activated via `SPRING_PROFILES_ACTIVE=docker`; connects to Oracle XE in Docker Compose network |
| `application-test.properties` | Spring Boot profile: `test` | `src/test/resources/application-test.properties` | Activated by test runner (`@ActiveProfiles("test")`); H2 in-memory DB, `create-drop` DDL |
| `docker-compose.yml` environment section | Docker Compose env vars | `docker-compose.yml` | Injects `SPRING_PROFILES_ACTIVE`, `SPRING_DATASOURCE_URL/USERNAME/PASSWORD` into the container |
| `Dockerfile` | Container build + runtime | `Dockerfile` | Multi-stage build; sets `JAVA_OPTS` env var for JVM heap |

No Spring Cloud Config, Azure App Configuration, Consul, Vault, or external secret stores are configured.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| (default Maven build) | Automatic | Standard build; no conditional profiles defined in `pom.xml` | `spring-boot-maven-plugin` packages the fat JAR |
| Docker build (Dockerfile) | Manual — `docker build` or `docker compose up --build` | Produces a container image from the fat JAR | Build stage: `maven:3.9.6-eclipse-temurin-8`; runtime stage: `eclipse-temurin:8-jre` |

No Maven `<profiles>` blocks are declared in `pom.xml`. The only build-time variation is `-DskipTests` used in the Dockerfile build stage.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides vs Default |
|---|---|---|---|
| `default` (no profile) | Automatic (no active profile set) | `application.properties` | Oracle at `oracle-db:1521/FREEPDB1`, DDL auto = `create`, show SQL = `true` |
| `docker` | `SPRING_PROFILES_ACTIVE=docker` env var (Docker Compose) | `application.properties` + `application-docker.properties` | Oracle at `oracle-db:1521:XE`, logging level WARN for Spring Web, SQL=DEBUG for Hibernate |
| `test` | Spring test annotations (e.g., `@SpringBootTest`) | `application-test.properties` | H2 in-memory (`create-drop`), show SQL = `false`, test upload path `target/test-uploads` |

## Properties Inventory

### photoalbum-java-app

| Property Key | Default | Profile Override | Source |
|---|---|---|---|
| `server.port` | `8080` | docker: `8080` | `application.properties` |
| `server.servlet.encoding.charset` | `UTF-8` | — | `application.properties` |
| `server.servlet.encoding.enabled` | `true` | — | `application.properties` |
| `server.servlet.encoding.force` | `true` | — | `application.properties` |
| `spring.datasource.url` | `jdbc:oracle:thin:@oracle-db:1521/FREEPDB1` | docker: `jdbc:oracle:thin:@oracle-db:1521:XE`; test: `jdbc:h2:mem:testdb`; Docker Compose env: `SPRING_DATASOURCE_URL` | `application.properties` / env var |
| `spring.datasource.username` | `photoalbum` | test: `sa`; Docker Compose env: `SPRING_DATASOURCE_USERNAME` | `application.properties` / env var |
| `spring.datasource.password` | `photoalbum` | test: _(empty)_; Docker Compose env: `SPRING_DATASOURCE_PASSWORD` | `application.properties` / env var |
| `spring.datasource.driver-class-name` | `oracle.jdbc.OracleDriver` | test: `org.h2.Driver` | `application.properties` |
| `spring.jpa.database-platform` | `org.hibernate.dialect.OracleDialect` | test: `org.hibernate.dialect.H2Dialect` | `application.properties` |
| `spring.jpa.hibernate.ddl-auto` | `create` | test: `create-drop` | `application.properties` |
| `spring.jpa.show-sql` | `true` | docker: `true`; test: `false` | `application.properties` |
| `spring.jpa.properties.hibernate.format_sql` | `true` | docker: `true` | `application.properties` |
| `spring.servlet.multipart.max-file-size` | `10MB` | — | `application.properties` |
| `spring.servlet.multipart.max-request-size` | `50MB` | — | `application.properties` |
| `app.file-upload.max-file-size-bytes` | `10485760` (10 MB) | — | `application.properties` |
| `app.file-upload.allowed-mime-types` | `image/jpeg,image/png,image/gif,image/webp` | — | `application.properties` |
| `app.file-upload.max-files-per-upload` | `10` | — | `application.properties` |
| `logging.level.com.photoalbum` | `DEBUG` | docker: `INFO`; test: `DEBUG` | `application.properties` |
| `logging.level.org.springframework.web` | `DEBUG` | docker: `WARN` | `application.properties` |
| `logging.level.org.hibernate.SQL` | _(not set)_ | docker: `DEBUG` | `application-docker.properties` |
| `app.file-upload.upload-path` | _(not set)_ | test: `target/test-uploads` | `application-test.properties` |
| `JAVA_OPTS` | `-Xmx512m -Xms256m` | Container env (Dockerfile) | `Dockerfile ENV` |
| `SPRING_PROFILES_ACTIVE` | _(not set)_ | docker: `docker` (Docker Compose) | `docker-compose.yml` |

## Startup Parameters & Resource Requirements

| Service | JVM / Runtime Options | Memory Allocation | CPU | Instance Count |
|---|---|---|---|---|
| photoalbum-java-app (container) | `-Xmx512m -Xms256m` (via `$JAVA_OPTS`) | No Docker `mem_limit` declared | No CPU limit declared | 1 (no scaling config) |
| oracle-db | N/A (third-party image) | No `mem_limit` declared | No CPU limit declared | 1 |

No Kubernetes resource requests/limits or cloud-managed scaling settings are present in the repository. JVM heap is capped at 512 MB.

## Startup Dependency Chain

```
oracle-db  →  [healthcheck: healthcheck.sh every 30s, timeout 10s, 15 retries, start_period 180s]
    ↓ (condition: service_healthy)
photoalbum-java-app  →  starts Spring Boot, Hibernate DDL auto=create runs schema creation
```

1. **oracle-db** starts and runs its Oracle healthcheck until the database is responsive (up to ~8 minutes: 180s start + 15 × 30s retries).
2. **photoalbum-java-app** starts only after `oracle-db` is marked healthy (`depends_on: condition: service_healthy`).
3. On startup, Hibernate executes `create` DDL — dropping and recreating the `PHOTOS` table every time the application starts.
4. No Spring Boot Actuator health endpoints are exposed; there is no application-level readiness probe.
5. `restart: on-failure` is set on the app container to recover from transient startup errors (e.g., Oracle not yet fully ready).

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|---|---|---|
| `spring.datasource.password` | Database password | Plaintext in `application.properties` and `application-docker.properties`; value: [MASKED] |
| `SPRING_DATASOURCE_PASSWORD` | Database password (container override) | Docker Compose `environment:` section in `docker-compose.yml`; value: [MASKED] |
| `ORACLE_PASSWORD` | Oracle root/admin password | Docker Compose `environment:` for `oracle-db` service; value: [MASKED] |
| `APP_USER_PASSWORD` | Oracle application user password | Docker Compose `environment:` for `oracle-db` service; value: [MASKED] |

### Secrets Provisioning Workflow

All secrets are stored as plaintext values in version-controlled files (`application.properties`, `docker-compose.yml`). No secret rotation, encryption-at-rest, or external secret store (Vault, Azure Key Vault, AWS Secrets Manager) is used. The workflow is:

1. Database credentials are hardcoded in `application.properties` and `application-docker.properties`.
2. Docker Compose passes them as container environment variables, which override Spring Boot properties at runtime.
3. No managed identity, service principal, or RBAC-based secrets provisioning is in place.

This represents a significant security risk for any environment beyond local development.

## Feature Flags

No feature flag frameworks (LaunchDarkly, Unleash, Spring Feature Flags) or `@ConditionalOnProperty` / `@ConditionalOnExpression` beans are used. The only conditional configuration is profile-based (Oracle vs H2 vs Docker database selection).

| Flag Name | Default | Controlled By |
|---|---|---|
| _(none declared)_ | — | — |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Java (compile target) | 8 (1.8) | `pom.xml` `maven.compiler.source/target` |
| Java (Docker build stage) | Eclipse Temurin 8 | `Dockerfile` `FROM maven:3.9.6-eclipse-temurin-8` |
| Java (Docker runtime stage) | Eclipse Temurin 8 JRE | `Dockerfile` `FROM eclipse-temurin:8-jre` |
| Spring Boot | 2.7.18 | `pom.xml` parent |
| Spring Framework | ~5.3.x (managed by Boot 2.7.18) | Spring Boot BOM |
| Spring Data JPA | ~2.7.18 (managed) | Spring Boot BOM |
| Hibernate ORM | ~5.6.x (managed by Boot 2.7.18) | Spring Boot BOM |
| Thymeleaf | ~3.0.x (managed) | Spring Boot BOM |
| Hibernate Validator | ~6.2.x (managed) | Spring Boot BOM |
| Jackson (Databind) | ~2.13.x (managed) | Spring Boot BOM |
| Apache Commons IO | 2.11.0 | `pom.xml` explicit |
| Oracle JDBC (ojdbc8) | Managed by Spring Boot / Oracle BOM | `pom.xml` runtime scope |
| Apache Maven | 3.9.6 (Docker build stage) | `Dockerfile` |
| H2 Database (test) | ~2.x (managed) | Spring Boot BOM (test scope) |
