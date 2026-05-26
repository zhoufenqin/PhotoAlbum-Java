# Configuration & Externalized Settings Inventory

This project externalizes configuration through Spring Boot property files, Docker Compose environment variables, and a multi-stage Dockerfile, with separate defaults for local runtime, Docker runtime, and tests. Secrets are present as inline properties or container environment variables rather than an external secret store, and no Kubernetes, `.env`, or remote configuration repository was detected.

## Configuration Sources

| Source | Type | Path/Location | Notes |
| --- | --- | --- | --- |
| Maven project descriptor | Build configuration | `pom.xml` | Defines Spring Boot parent `2.7.18`, Java 8 target, dependencies, and Spring Boot Maven plugin; no Maven `<profiles>` section is declared. |
| Spring default runtime config | Application properties | `src/main/resources/application.properties` | Base runtime settings for server port, Oracle datasource, JPA, multipart upload, application upload validation, and logging. |
| Spring Docker runtime config | Profile-specific application properties | `src/main/resources/application-docker.properties` | Docker profile overrides datasource/JPA/logging settings; some values are later overridden again by Compose environment variables. |
| Spring test runtime config | Test profile properties | `src/test/resources/application-test.properties` | Activated by `@ActiveProfiles("test")`; switches datasource to in-memory H2 and adjusts JPA/logging for tests. |
| Container orchestration config | Docker Compose | `docker-compose.yml` | Defines `oracle-db` and `photoalbum-java-app`, environment variables, health checks, ports, volumes, network, restart policy, and startup dependency. |
| Container build/runtime config | Dockerfile | `Dockerfile` | Multi-stage Maven build, runtime base image, exposed port, and default `JAVA_OPTS` heap settings. |
| Database bootstrap scripts | SQL/shell init scripts | `oracle-init/01-create-user.sql`, `oracle-init/02-verify-user.sql`, `oracle-init/create-user.sh`, `oracle-init/healthcheck.sql` | Seed Oracle user credentials/privileges and provide DB readiness checks used during container startup. |
| External config / secret store | Not detected | n/a | No `bootstrap.*`, `.env*`, Kubernetes manifests, Helm charts, Spring Cloud Config, Vault, Key Vault, or other remote config/secret-store references were found. |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
| --- | --- | --- | --- |
| default | Automatic (no `-P` flag required) | Standard JAR build for the Spring Boot application. | `spring-boot-maven-plugin`; Spring Boot starters for web, Thymeleaf, validation, and JPA; `ojdbc8` runtime dependency; `commons-io:2.11.0`; test-only `h2`; optional `spring-boot-devtools`. |
| docker-image build stage | `docker build` / `docker-compose up --build` | Builds the application inside the container image before producing a slim runtime image. | Build image `maven:3.9.6-eclipse-temurin-8`; runtime image `eclipse-temurin:8-jre`; Maven commands `dependency:go-offline` and `clean package -DskipTests`. |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
| --- | --- | --- | --- |
| default | Automatic when no Spring profile is set | `src/main/resources/application.properties` | Oracle datasource points to `jdbc:oracle:thin:@oracle-db:1521/FREEPDB1`; SQL logging enabled; multipart and upload validation defaults enabled. |
| docker | `SPRING_PROFILES_ACTIVE=docker` in `docker-compose.yml` | `src/main/resources/application.properties` + `src/main/resources/application-docker.properties` + Compose env overrides | Docker profile changes datasource URL to `jdbc:oracle:thin:@oracle-db:1521:XE`, reduces app logging, adds `logging.level.org.hibernate.SQL=DEBUG`; Compose then overrides datasource URL/user/password for the container back to `FREEPDB1` and injected credentials. |
| test | `@ActiveProfiles("test")` in `src/test/java/com/photoalbum/PhotoAlbumApplicationTests.java` | `src/main/resources/application.properties` + `src/test/resources/application-test.properties` | Replaces Oracle datasource with `jdbc:h2:mem:testdb`, uses H2 driver/dialect, sets `ddl-auto=create-drop`, disables SQL output, and adds `app.file-upload.upload-path=target/test-uploads`. |

## Properties Inventory

### photo-album application

| Property Key | Default | Profiles | Source |
| --- | --- | --- | --- |
| `server.port` | `8080` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `server.servlet.encoding.charset` | `UTF-8` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `server.servlet.encoding.enabled` | `true` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `server.servlet.encoding.force` | `true` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `spring.datasource.url` | `jdbc:oracle:thin:@oracle-db:1521/FREEPDB1` | default; docker profile file sets `jdbc:oracle:thin:@oracle-db:1521:XE`; test sets `jdbc:h2:mem:testdb`; Docker container override uses `SPRING_DATASOURCE_URL=jdbc:oracle:thin:@oracle-db:1521/FREEPDB1` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties`, `docker-compose.yml` |
| `spring.datasource.username` | `photoalbum` | default, docker; test overrides to `sa`; Docker container override uses `SPRING_DATASOURCE_USERNAME=photoalbum` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties`, `docker-compose.yml` |
| `spring.datasource.password` | `[MASKED]` | default, docker; test overrides to empty string; Docker container override uses `SPRING_DATASOURCE_PASSWORD=[MASKED]` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties`, `docker-compose.yml` |
| `spring.datasource.driver-class-name` | `oracle.jdbc.OracleDriver` | default, docker; test overrides to `org.h2.Driver` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `spring.jpa.database-platform` | `org.hibernate.dialect.OracleDialect` | default, docker; test overrides to `org.hibernate.dialect.H2Dialect` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `spring.jpa.hibernate.ddl-auto` | `create` | default, docker; test overrides to `create-drop` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `spring.jpa.show-sql` | `true` | default, docker; test overrides to `false` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `spring.jpa.properties.hibernate.format_sql` | `true` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `spring.servlet.multipart.max-file-size` | `10MB` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `spring.servlet.multipart.max-request-size` | `50MB` | default, docker | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `app.file-upload.max-file-size-bytes` | `10485760` | default, docker, test | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `app.file-upload.allowed-mime-types` | `image/jpeg,image/png,image/gif,image/webp` | default, docker, test | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties`; consumed via `@Value` in `PhotoServiceImpl` |
| `app.file-upload.max-files-per-upload` | `10` | default, docker, test | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `app.file-upload.upload-path` | `target/test-uploads` | test only | `src/test/resources/application-test.properties` |
| `logging.level.com.photoalbum` | `DEBUG` | default and test; docker overrides to `INFO` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties`, `src/test/resources/application-test.properties` |
| `logging.level.org.springframework.web` | `DEBUG` | default; docker overrides to `WARN` | `src/main/resources/application.properties`, `src/main/resources/application-docker.properties` |
| `logging.level.org.hibernate.SQL` | `DEBUG` | docker only | `src/main/resources/application-docker.properties` |

### docker-compose service overrides

| Property Key | Default | Profiles | Source |
| --- | --- | --- | --- |
| `SPRING_PROFILES_ACTIVE` | `docker` | docker-compose runtime | `docker-compose.yml` |
| `SPRING_DATASOURCE_URL` | `jdbc:oracle:thin:@oracle-db:1521/FREEPDB1` | docker-compose runtime | `docker-compose.yml` |
| `SPRING_DATASOURCE_USERNAME` | `photoalbum` | docker-compose runtime | `docker-compose.yml` |
| `SPRING_DATASOURCE_PASSWORD` | `[MASKED]` | docker-compose runtime | `docker-compose.yml` |
| `ORACLE_PASSWORD` | `[MASKED]` | docker-compose runtime | `docker-compose.yml` |
| `APP_USER` | `photoalbum` | docker-compose runtime | `docker-compose.yml` |
| `APP_USER_PASSWORD` | `[MASKED]` | docker-compose runtime | `docker-compose.yml` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
| --- | --- | --- | --- |
| `photoalbum-java-app` | Docker `ENTRYPOINT` runs `java $JAVA_OPTS -jar app.jar`; default `JAVA_OPTS="-Xmx512m -Xms256m"`; Compose also injects `SPRING_PROFILES_ACTIVE=docker` and datasource env vars. | App heap capped at `512m` with `256m` initial heap via `JAVA_OPTS`; no Compose/Kubernetes memory or CPU limit is declared. | `1` container in `docker-compose.yml` |
| `oracle-db` | Oracle image startup with init scripts mounted at `/container-entrypoint-initdb.d`; no JVM flags exposed by this project. | No container memory limit is declared; README states Docker should have at least `4GB` RAM available for Oracle startup. | `1` container in `docker-compose.yml` |

## Startup Dependency Chain

1. `photoalbum-java-app` → waits for → `oracle-db` via Docker Compose `depends_on` with `condition: service_healthy`.
2. `oracle-db` readiness is gated by the container `healthcheck.sh` command, which in turn uses the mounted Oracle health-check SQL before Compose allows the application to start.
3. `oracle-db` also runs the mounted `oracle-init` scripts during container bootstrap to create and verify the application database user before the Java service begins normal database access.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
| --- | --- | --- |
| `spring.datasource.password` | Spring datasource password | Inline in `src/main/resources/application.properties` and `src/main/resources/application-docker.properties` as `[MASKED]` |
| `SPRING_DATASOURCE_PASSWORD` | Container environment variable for datasource password | `docker-compose.yml` environment entry `[MASKED]` |
| `ORACLE_PASSWORD` | Oracle admin/bootstrap password | `docker-compose.yml` environment entry `[MASKED]` |
| `APP_USER_PASSWORD` | Oracle application user password | `docker-compose.yml` environment entry `[MASKED]` |
| `system/[MASKED]@//localhost:1521/XE` | Embedded credential in bootstrap shell script | `oracle-init/create-user.sh` contains masked inline connection credential |
| `CREATE USER photoalbum IDENTIFIED BY [MASKED]` | Embedded SQL credential | `oracle-init/01-create-user.sql` stores the application user password inline as `[MASKED]` |

### Secrets Provisioning Workflow

Secrets are provisioned locally and in containers from checked-in files rather than an external secret store. During Docker startup, `docker-compose.yml` injects `ORACLE_PASSWORD`, `APP_USER`, and `APP_USER_PASSWORD` into the Oracle container, the Oracle init scripts create or verify the application user with those credentials, and the application container receives `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, and `SPRING_DATASOURCE_PASSWORD` so Spring Boot can bind the datasource at startup. For non-Docker local runs, the same datasource password is read directly from `application.properties`; tests switch to H2 and use an empty password. No managed identity, Vault, Key Vault, `.env`, or Kubernetes Secret workflow was found.

## Feature Flags

| Flag Name | Default | Controlled By |
| --- | --- | --- |
| None detected | n/a | No feature-flag framework, `@ConditionalOnProperty`, `@ConditionalOnExpression`, or custom toggle properties were found in the application source. |

## Framework & Runtime Versions

| Component | Version | Source |
| --- | --- | --- |
| Java language target | `1.8` / compiler source-target `8` | `pom.xml` |
| Spring Boot parent | `2.7.18` | `pom.xml` |
| Spring Boot Maven Plugin | Inherited from Spring Boot `2.7.18` parent | `pom.xml` |
| Spring Boot starters (`web`, `thymeleaf`, `data-jpa`, `validation`, `json`, `test`, `devtools`) | Versions managed by Spring Boot `2.7.18` | `pom.xml` |
| Oracle JDBC driver (`ojdbc8`) | Version not explicitly pinned in `pom.xml` (managed by Spring Boot dependency management) | `pom.xml` |
| H2 test database | Version not explicitly pinned in `pom.xml` (managed by Spring Boot dependency management) | `pom.xml` |
| Commons IO | `2.11.0` | `pom.xml` |
| Docker build image | `maven:3.9.6-eclipse-temurin-8` | `Dockerfile` |
| Docker runtime image | `eclipse-temurin:8-jre` | `Dockerfile` |
| Oracle database container image | `gvenzl/oracle-free:latest` (floating tag) | `docker-compose.yml` |
| Docker Compose network driver | `bridge` | `docker-compose.yml` |
