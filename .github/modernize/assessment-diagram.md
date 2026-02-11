# Photo Album Application - Architecture Diagram

This diagram represents the current architecture of the Photo Album Java application based on the assessment analysis.

## Application Architecture

```mermaid
C4Context
    title Photo Album Application - System Architecture

    Person(user, "User", "Uploads and views photos")
    
    System_Boundary(app, "Photo Album Application") {
        Container(web, "Web Application", "Spring Boot 2.7.18, Java 8", "Provides photo gallery interface and REST API")
        ContainerDb(db, "Database", "Oracle Database 21c XE", "Stores photo metadata and binary data as BLOBs")
    }

    Rel(user, web, "Uses", "HTTP/HTTPS")
    Rel(web, db, "Reads/Writes", "JDBC")
```

## Application Layers

```mermaid
graph TB
    subgraph "Presentation Layer"
        A[Thymeleaf Templates]
        B[Bootstrap 5 UI]
        C[JavaScript]
    end
    
    subgraph "Controller Layer"
        D[HomeController]
        E[DetailController]
        F[PhotoFileController]
    end
    
    subgraph "Service Layer"
        G[PhotoService]
        H[PhotoServiceImpl]
    end
    
    subgraph "Data Access Layer"
        I[PhotoRepository]
        J[Spring Data JPA]
    end
    
    subgraph "Database Layer"
        K[Oracle Database 21c XE]
        L[PHOTOS Table with BLOB]
    end
    
    A --> D
    B --> D
    C --> D
    D --> G
    E --> G
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> L
```

## Technology Stack

```mermaid
graph LR
    subgraph "Frontend Technologies"
        A1[Thymeleaf 3.0]
        A2[Bootstrap 5.3.0]
        A3[Vanilla JavaScript]
    end
    
    subgraph "Backend Framework"
        B1[Spring Boot 2.7.18]
        B2[Spring MVC]
        B3[Spring Data JPA]
        B4[Hibernate ORM]
    end
    
    subgraph "Runtime"
        C1[Java 8]
        C2[Maven Build Tool]
    end
    
    subgraph "Database"
        D1[Oracle JDBC Driver]
        D2[Oracle Database 21c XE]
    end
    
    subgraph "Additional Libraries"
        E1[Commons IO 2.11.0]
        E2[Spring Validation]
        E3[Spring DevTools]
    end
```

## Data Flow

```mermaid
sequenceDiagram
    participant User
    participant Browser
    participant Controller
    participant Service
    participant Repository
    participant Database

    User->>Browser: Upload Photo
    Browser->>Controller: POST /upload
    Controller->>Service: uploadPhoto(file)
    Service->>Service: Validate file type and size
    Service->>Service: Extract image dimensions
    Service->>Service: Generate UUID
    Service->>Repository: save(photo)
    Repository->>Database: INSERT with BLOB data
    Database-->>Repository: Success
    Repository-->>Service: Photo entity
    Service-->>Controller: UploadResult
    Controller-->>Browser: JSON response
    Browser-->>User: Show success message

    User->>Browser: View Gallery
    Browser->>Controller: GET /
    Controller->>Service: getAllPhotos()
    Service->>Repository: findAllOrderByUploadedAtDesc()
    Repository->>Database: SELECT query
    Database-->>Repository: Photo list with metadata
    Repository-->>Service: List of Photos
    Service-->>Controller: Photo list
    Controller-->>Browser: Render Thymeleaf template
    Browser-->>User: Display gallery

    User->>Browser: View Photo Details
    Browser->>Controller: GET /detail/{id}
    Controller->>Service: getPhotoById(id)
    Service->>Repository: findById(id)
    Repository->>Database: SELECT by ID
    Database-->>Repository: Photo entity
    Repository-->>Service: Optional Photo
    Service-->>Controller: Photo
    Controller-->>Browser: Render detail template
    Browser-->>User: Display photo with metadata
```

## Key Components

### Controllers
- **HomeController**: Gallery view and photo upload
- **DetailController**: Full-size photo view with navigation
- **PhotoFileController**: Photo binary data retrieval

### Services
- **PhotoService**: Business logic interface
- **PhotoServiceImpl**: Photo operations implementation with validation

### Repository
- **PhotoRepository**: Spring Data JPA repository with custom Oracle queries

### Model
- **Photo**: JPA entity with BLOB storage for photo data
- **UploadResult**: Upload operation result wrapper

## Database Schema

```mermaid
erDiagram
    PHOTOS {
        VARCHAR2(36) ID PK "UUID"
        VARCHAR2(255) ORIGINAL_FILE_NAME "Original filename"
        BLOB PHOTO_DATA "Binary photo data"
        VARCHAR2(255) STORED_FILE_NAME "UUID-based filename"
        VARCHAR2(500) FILE_PATH "Relative path"
        NUMBER FILE_SIZE "Size in bytes"
        VARCHAR2(50) MIME_TYPE "Image MIME type"
        TIMESTAMP UPLOADED_AT "Upload timestamp"
        NUMBER WIDTH "Image width"
        NUMBER HEIGHT "Image height"
    }
```

## Configuration

### Application Properties
- Server Port: 8080
- Database: Oracle Database with JDBC connection
- JPA: Hibernate with Oracle dialect, auto-DDL enabled
- File Upload: Max 10MB per file, 50MB per request
- Supported formats: JPEG, PNG, GIF, WebP
- Logging: DEBUG level for application and web components

### Deployment
- Containerization: Docker with docker-compose
- Database Container: Oracle Database 21c Express Edition
- Application Container: Spring Boot JAR with Maven build

## Storage Architecture

### Current Implementation
- **Photo Storage**: BLOB data in Oracle database
- **Benefits**: 
  - No file system dependencies
  - ACID compliance
  - Simplified containerized deployment
  - Easy backup and migration
- **Trade-offs**: 
  - Database size increases with photos
  - Network overhead for binary data

## Assessment Notes

### Framework & Dependencies
- Spring Boot 2.7.18 (maintenance support until 2025)
- Java 8 (legacy version, consider upgrading to Java 11/17/21)
- Oracle Database 21c XE (limited to 2 CPU, 2GB RAM, 12GB storage)
- Using Oracle-specific SQL features (TO_CHAR, ROWNUM, analytical functions)

### Modernization Opportunities
- Upgrade to newer Java LTS version (11, 17, or 21)
- Migrate to Spring Boot 3.x for extended support
- Consider Azure-managed databases (Azure SQL, PostgreSQL, or Cosmos DB)
- Implement Azure Blob Storage for photo files
- Add caching layer (Redis/Azure Cache for Redis)
- Implement CDN for photo delivery
- Add authentication and authorization
- Implement health checks and monitoring

### Cloud Migration Considerations
- **Target Platform**: Azure App Service, Azure Container Apps, or AKS
- **Database Migration**: Azure SQL Database, PostgreSQL, or keep Oracle on Azure VMs
- **Storage Migration**: Azure Blob Storage for photos
- **Observability**: Application Insights integration
- **Security**: Azure Key Vault for secrets, Managed Identity for authentication
