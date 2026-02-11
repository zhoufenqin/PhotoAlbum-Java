# PhotoAlbum Application Architecture Diagram

**Generated:** 2026-02-11  
**Application:** Photo Album  
**Version:** 1.0.0  
**Framework:** Spring Boot 2.7.18  
**Language:** Java 8  

---

## Application Architecture Overview

```mermaid
graph TB
    subgraph "Client Layer"
        Browser[Web Browser]
    end
    
    subgraph "Application Container - Docker"
        subgraph "Presentation Layer"
            HomeCtrl[HomeController<br/>Upload & Gallery]
            DetailCtrl[DetailController<br/>Photo Details & Delete]
            PhotoCtrl[PhotoFileController<br/>Photo Streaming]
            Thymeleaf[Thymeleaf<br/>Template Engine]
        end
        
        subgraph "Business Layer"
            PhotoService[PhotoService<br/>Interface]
            PhotoServiceImpl[PhotoServiceImpl<br/>Business Logic]
        end
        
        subgraph "Data Access Layer"
            PhotoRepo[PhotoRepository<br/>JPA Repository]
            Hibernate[Hibernate ORM]
        end
        
        subgraph "Domain Model"
            Photo[Photo Entity<br/>UUID, Binary Data, Metadata]
            UploadResult[UploadResult DTO]
        end
    end
    
    subgraph "Data Layer - Oracle Container"
        OracleDB[(Oracle Database 23ai<br/>Free Edition)]
        PhotosTable[PHOTOS Table<br/>ID, PHOTO_DATA BLOB,<br/>Metadata]
    end
    
    subgraph "Infrastructure"
        Docker[Docker Compose<br/>Orchestration]
    end
    
    %% Client to Presentation
    Browser -->|HTTP Requests| HomeCtrl
    Browser -->|HTTP Requests| DetailCtrl
    Browser -->|HTTP Requests| PhotoCtrl
    
    %% Presentation to Templates
    HomeCtrl -->|Renders| Thymeleaf
    DetailCtrl -->|Renders| Thymeleaf
    Thymeleaf -->|HTML Response| Browser
    
    %% Controllers to Service
    HomeCtrl -->|Calls| PhotoServiceImpl
    DetailCtrl -->|Calls| PhotoServiceImpl
    PhotoCtrl -->|Calls| PhotoServiceImpl
    
    %% Service Implementation
    PhotoServiceImpl -.->|Implements| PhotoService
    PhotoServiceImpl -->|Uses| PhotoRepo
    PhotoServiceImpl -->|Creates/Uses| Photo
    PhotoServiceImpl -->|Creates| UploadResult
    
    %% Repository to ORM
    PhotoRepo -->|JPA Queries| Hibernate
    PhotoRepo -->|CRUD Operations| Photo
    
    %% ORM to Database
    Hibernate -->|JDBC Connection| OracleDB
    OracleDB -->|Stores| PhotosTable
    
    %% Docker orchestration
    Docker -.->|Manages| Browser
    Docker -.->|Manages| OracleDB
    
    style Browser fill:#e1f5ff
    style HomeCtrl fill:#fff4e1
    style DetailCtrl fill:#fff4e1
    style PhotoCtrl fill:#fff4e1
    style PhotoServiceImpl fill:#e8f5e9
    style PhotoRepo fill:#f3e5f5
    style OracleDB fill:#ffebee
    style Docker fill:#f5f5f5
```

---

## Technology Stack

```mermaid
graph LR
    subgraph "Frontend Technologies"
        HTML[HTML5]
        Thyme[Thymeleaf Templates]
        CSS[CSS/Bootstrap]
    end
    
    subgraph "Backend Framework"
        SB[Spring Boot 2.7.18]
        SBWEB[Spring Web MVC]
        SBJPA[Spring Data JPA]
        SBVAL[Spring Validation]
    end
    
    subgraph "Runtime & Build"
        Java8[Java 8 JDK]
        Maven[Apache Maven]
        Tomcat[Embedded Tomcat]
    end
    
    subgraph "Data & ORM"
        JPA[JPA Specification]
        Hibernate[Hibernate 5.6.x]
        OJDBC[Oracle JDBC Driver ojdbc8]
    end
    
    subgraph "Database"
        Oracle[Oracle Database 23ai Free]
    end
    
    subgraph "Utilities"
        CommonsIO[Apache Commons IO 2.11.0]
        ImageIO[Java ImageIO]
    end
    
    subgraph "Deployment"
        DockerEngine[Docker Engine]
        DockerCompose[Docker Compose]
    end
    
    HTML --> Thyme
    Thyme --> SBWEB
    SBWEB --> SB
    SBJPA --> SB
    SBVAL --> SB
    SB --> Tomcat
    Tomcat --> Java8
    Maven --> Java8
    SBJPA --> JPA
    JPA --> Hibernate
    Hibernate --> OJDBC
    OJDBC --> Oracle
    SB --> CommonsIO
    SB --> ImageIO
    DockerCompose --> DockerEngine
    
    style SB fill:#6db33f
    style Java8 fill:#f89820
    style Oracle fill:#f80000
    style DockerEngine fill:#2496ed
```

---

## Data Flow Diagram

```mermaid
sequenceDiagram
    participant User as Web Browser
    participant HC as HomeController
    participant PS as PhotoServiceImpl
    participant PR as PhotoRepository
    participant DB as Oracle Database
    
    Note over User,DB: Photo Upload Flow
    User->>HC: POST /upload with multipart file
    HC->>PS: uploadPhoto(MultipartFile)
    PS->>PS: Validate file type and size
    PS->>PS: Extract image dimensions
    PS->>PS: Create Photo entity with BLOB data
    PS->>PR: save(Photo)
    PR->>DB: INSERT with BLOB data
    DB-->>PR: Photo saved with ID
    PR-->>PS: Photo entity
    PS-->>HC: UploadResult
    HC-->>User: Redirect to home with message
    
    Note over User,DB: Photo Retrieval Flow
    User->>HC: GET /
    HC->>PS: getAllPhotos()
    PS->>PR: findAllOrderByUploadedAtDesc()
    PR->>DB: SELECT * ORDER BY uploaded_at DESC
    DB-->>PR: List of Photos (without BLOB)
    PR-->>PS: List of Photos
    PS-->>HC: List of Photos
    HC-->>User: Gallery page with thumbnails
    
    Note over User,DB: Photo Display Flow
    User->>User: Click photo thumbnail
    User->>PhotoFileController: GET /photo/{id}
    PhotoFileController->>PS: getPhotoById(id)
    PS->>PR: findById(id)
    PR->>DB: SELECT with BLOB data
    DB-->>PR: Photo with binary data
    PR-->>PS: Photo entity
    PS-->>PhotoFileController: Photo
    PhotoFileController-->>User: Binary image stream with cache headers
```

---

## Component Relationships

```mermaid
graph TD
    subgraph "MVC Pattern"
        V[View Layer<br/>Thymeleaf Templates]
        C[Controller Layer<br/>@Controller classes]
        M[Model Layer<br/>Photo Entity]
    end
    
    subgraph "Service Pattern"
        S[Service Interface<br/>PhotoService]
        SI[Service Implementation<br/>@Service @Transactional]
    end
    
    subgraph "Repository Pattern"
        R[Repository Interface<br/>JpaRepository]
        RQ[Custom Native Queries<br/>Oracle-specific SQL]
    end
    
    V <-->|Model & View| C
    C -->|Delegates| SI
    SI -.->|Implements| S
    SI -->|CRUD & Queries| R
    R -->|Extends| RQ
    SI <-->|Uses| M
    R <-->|Maps| M
    
    style V fill:#e1f5ff
    style C fill:#fff4e1
    style SI fill:#e8f5e9
    style R fill:#f3e5f5
    style M fill:#fce4ec
```

---

## Deployment Architecture

```mermaid
graph TB
    subgraph "Docker Host"
        subgraph "photoalbum-network Bridge Network"
            AppContainer[Application Container<br/>photoalbum-app<br/>Port 8080<br/>Spring Boot with Tomcat]
            DBContainer[Database Container<br/>oracle-db<br/>Port 1521<br/>Oracle 23ai Free]
        end
        
        Volume1[Docker Volume<br/>oracle-data<br/>Persistent DB storage]
    end
    
    Users[End Users] -->|HTTP Port 8080| AppContainer
    AppContainer -->|JDBC oracle-db:1521| DBContainer
    DBContainer -->|Mounts| Volume1
    
    AppContainer -.->|Depends on| DBContainer
    AppContainer -.->|Health Check| DBContainer
    
    style AppContainer fill:#e8f5e9
    style DBContainer fill:#ffebee
    style Volume1 fill:#fff9c4
```

---

## Security & Configuration Notes

**⚠️ Current Security Status:**
- ❌ No authentication or authorization
- ❌ All endpoints publicly accessible
- ❌ No Spring Security dependency
- ❌ Database credentials in application.properties

**Configuration:**
- Profiles: default, docker, test
- Database: Oracle with Hibernate auto-DDL (ddl-auto=create in docker profile)
- File Upload: Max 5MB per file, image/* MIME types only
- JVM Settings: Xmx512m, Xms256m

---

## Key Dependencies

| Dependency | Version | Purpose |
|------------|---------|---------|
| Spring Boot | 2.7.18 | Application framework |
| Spring Data JPA | (inherited) | Data access |
| Hibernate | 5.6.x | ORM implementation |
| Oracle JDBC | ojdbc8 | Database driver |
| Thymeleaf | (inherited) | Template engine |
| Commons IO | 2.11.0 | File utilities |
| H2 Database | (test) | In-memory testing |

---

## Architecture Patterns Applied

1. **MVC (Model-View-Controller)**: Separation of presentation, business logic, and data
2. **Service Layer Pattern**: Business logic encapsulation with @Service
3. **Repository Pattern**: Data access abstraction with Spring Data JPA
4. **DTO Pattern**: UploadResult for data transfer
5. **Dependency Injection**: Spring's IoC container for component wiring
6. **Transaction Management**: @Transactional for database operations

---

## Identified Architecture Issues

### Critical
- 🔴 **No Security Layer**: Missing authentication and authorization
- 🔴 **Java 8 EOL**: Runtime version reached end of life
- 🔴 **Database Schema Management**: ddl-auto=create drops data on restart

### High
- 🟠 **BLOB Storage**: Photos stored in database, not scalable
- 🟠 **Spring Boot 2.7**: End of support reached
- 🟠 **Hardcoded Config**: Credentials in properties files

### Medium
- 🟡 **Oracle-Specific SQL**: Reduces database portability
- 🟡 **No Health Checks**: Missing Actuator endpoints
- 🟡 **No Structured Logging**: Console output only

---

## Modernization Architecture Target

```mermaid
graph TB
    subgraph "Proposed Cloud Architecture"
        LB[Load Balancer]
        
        subgraph "AKS/EKS Cluster"
            Pod1[App Pod 1]
            Pod2[App Pod 2]
            Pod3[App Pod N]
        end
        
        CloudDB[(Managed PostgreSQL<br/>Azure/AWS)]
        BlobStorage[Object Storage<br/>Azure Blob / S3]
        Cache[(Redis Cache)]
        Secrets[Key Vault /<br/>Secrets Manager]
        Auth[OAuth2 Provider<br/>Azure AD / Cognito]
        Monitor[Application Insights /<br/>CloudWatch]
    end
    
    Users[End Users] --> LB
    LB --> Pod1
    LB --> Pod2
    LB --> Pod3
    
    Pod1 --> Auth
    Pod2 --> Auth
    Pod3 --> Auth
    
    Pod1 --> Cache
    Pod2 --> Cache
    Pod3 --> Cache
    
    Pod1 --> CloudDB
    Pod2 --> CloudDB
    Pod3 --> CloudDB
    
    Pod1 --> BlobStorage
    Pod2 --> BlobStorage
    Pod3 --> BlobStorage
    
    Pod1 -.->|Reads Secrets| Secrets
    Pod2 -.->|Reads Secrets| Secrets
    Pod3 -.->|Reads Secrets| Secrets
    
    Pod1 -.->|Metrics & Logs| Monitor
    Pod2 -.->|Metrics & Logs| Monitor
    Pod3 -.->|Metrics & Logs| Monitor
    
    style LB fill:#e1f5ff
    style Pod1 fill:#e8f5e9
    style Pod2 fill:#e8f5e9
    style Pod3 fill:#e8f5e9
    style CloudDB fill:#f3e5f5
    style BlobStorage fill:#fff4e1
    style Cache fill:#ffebee
    style Secrets fill:#fce4ec
    style Auth fill:#e8f5e9
    style Monitor fill:#fff9c4
```

---

## Summary

**Current State:**
- Traditional monolithic Spring Boot application
- MVC architecture with clear layer separation
- Docker containerized but not cloud-optimized
- Single Oracle database with BLOB storage
- No security, limited observability

**Strengths:**
- ✅ Clean architecture with proper separation of concerns
- ✅ Already containerized with Docker
- ✅ Stateless application design
- ✅ Standard Spring Boot patterns

**Modernization Needs:**
- Upgrade runtime (Java 17, Spring Boot 3.x)
- Implement security (Spring Security + OAuth2)
- Migrate to cloud object storage
- Add observability (metrics, logs, tracing)
- Externalize configuration
- Replace Oracle-specific queries

**Recommended Next Steps:**
1. Review comprehensive assessment report (report.json)
2. Plan modernization in phases (see assessment report)
3. Start with critical security and runtime updates
4. Gradually migrate to cloud-native services
