# Diagrama de Arquitectura del Sistema

## Arquitectura General

```mermaid
graph TB
    subgraph "Internet"
        User[👤 Usuario]
    end
    
    subgraph "Ubuntu Server"
        subgraph "Web Layer"
            Nginx[🌐 Nginx<br/>Reverse Proxy<br/>Port 80/443]
        end
        
        subgraph "Application Layer"
            Rails[💎 Rails App<br/>Inertia.js + API<br/>Port 3000]
            React[⚛️ React Frontend<br/>TailwindCSS<br/>SSR con Inertia]
        end
        
        subgraph "File System"
            TxtFiles[📄 .txt Files<br/>/tmp/pxplus_exchange]
        end
        
        subgraph "Legacy System"
            PxPlus[🏢 PxPlus 7.71<br/>Existing System]
        end
    end
    
    User -->|HTTPS Requests| Nginx
    Nginx -->|Proxy Pass| Rails
    Rails -->|Inertia.js| React
    Rails -->|Generate .txt| TxtFiles
    Rails -->|Execute Commands| PxPlus
    PxPlus -->|Response .txt| TxtFiles
    TxtFiles -->|Parse to JSON| Rails
```

## Flujo de Datos

```mermaid
sequenceDiagram
    participant U as Usuario
    participant N as Nginx
    participant R as Rails
    participant F as File System
    participant P as PxPlus 7.71
    
    U->>N: Petición HTTPS
    N->>R: Proxy request
    R->>R: Validate JWT
    R->>R: Action.call()
    R->>F: Generate input.txt
    R->>P: Execute command
    P->>F: Write response.txt
    F->>R: Read response
    R->>R: Parse txt → JSON
    R->>U: Inertia.js response
```

## Stack Tecnológico

```mermaid
graph LR
    subgraph "Frontend"
        A[React 18+]
        B[TypeScript]
        C[TailwindCSS]
        D[Inertia.js]
        E[Vite]
    end
    
    subgraph "Backend"
        F[Ruby on Rails 7+]
        G[JWT Auth]
        H[Actions Pattern]
        I[Service Objects]
        J[Puma Server]
    end
    
    subgraph "Infrastructure"
        K[Ubuntu Server]
        L[Nginx]
        M[Let's Encrypt SSL]
        N[GitHub]
    end
    
    subgraph "Legacy"
        O[PxPlus 7.71]
        P[File Exchange]
        Q[Command Execution]
    end
    
    A --> D
    B --> A
    C --> A
    D --> F
    E --> A
    F --> H
    G --> F
    H --> I
    I --> P
    J --> F
    K --> L
    L --> F
    M --> L
    N --> K
    O --> Q
    P --> O
    Q --> I
```