# Arquitectura Frontend - React + Inertia.js

## Estructura de Componentes

```mermaid
graph TD
    subgraph AppStructure["🏠 App Structure"]
        App[App.tsx<br/>Main Application]
        Layout[Layout.tsx<br/>Main Layout]
    end

    subgraph Authentication["🔐 Authentication"]
        AuthProvider[AuthProvider]
        LoginForm[LoginForm]
        ProtectedRoute[ProtectedRoute]
    end

    subgraph PaymentModule["💳 Payment Module"]
        PaymentList[PaymentList]
        PaymentForm[PaymentForm]
        PaymentCard[PaymentCard]
        PaymentDetails[PaymentDetails]
    end

    subgraph DatabaseModule["🗄️ Database Module"]
        QueryBuilder[QueryBuilder]
        ResultsTable[ResultsTable]
        QueryHistory[QueryHistory]
    end

    subgraph ReportsModule["📊 Reports Module"]
        ReportGenerator[ReportGenerator]
        ReportViewer[ReportViewer]
        ReportList[ReportList]
    end

    subgraph SharedComponents["🔧 Shared Components"]
        Button[Button]
        Input[Input]
        Modal[Modal]
        Loading[Loading]
        ErrorBoundary[ErrorBoundary]
    end

    App --> Layout
    Layout --> AuthProvider
    AuthProvider --> ProtectedRoute
    ProtectedRoute --> PaymentList
    ProtectedRoute --> QueryBuilder
    ProtectedRoute --> ReportGenerator

    PaymentList --> PaymentCard
    PaymentCard --> PaymentDetails
    PaymentForm --> Input
    PaymentForm --> Button

    QueryBuilder --> ResultsTable
    ReportGenerator --> ReportViewer

    Layout --> ErrorBoundary
    Layout --> Loading
```

## Mobile-First Responsive Design

```mermaid
graph LR
    subgraph Mobile["📱 Mobile (320px+)"]
        MobileNav[🍔 Hamburger Menu]
        MobileCards[📱 Card Layout]
        MobileStack[📚 Stacked Forms]
    end
    
    subgraph Tablet["📱 Tablet (768px+)"]
        TabletSidebar[📑 Collapsible Sidebar]
        TabletGrid[🔲 Grid Layout]
        TabletTabs[📂 Tab Navigation]
    end
    
    subgraph Desktop["🖥️ Desktop (1024px+)"]
        DesktopSidebar[📋 Fixed Sidebar]
        DesktopTable[🗂️ Table Layout]
        DesktopMultiColumn[📊 Multi-column]
    end
    
    MobileNav -->|Breakpoint| TabletSidebar
    MobileCards -->|Breakpoint| TabletGrid
    MobileStack -->|Breakpoint| TabletTabs
    
    TabletSidebar -->|Breakpoint| DesktopSidebar
    TabletGrid -->|Breakpoint| DesktopTable
    TabletTabs -->|Breakpoint| DesktopMultiColumn
```

## State Management Flow

```mermaid
stateDiagram-v2
    [*] --> AppInit
    AppInit --> CheckAuth
    
    state CheckAuth {
        [*] --> ValidateToken
        ValidateToken --> TokenValid
        ValidateToken --> TokenInvalid
        TokenValid --> Authenticated
        TokenInvalid --> Unauthenticated
    }
    
    Authenticated --> Dashboard
    Unauthenticated --> LoginPage
    
    state Dashboard {
        [*] --> LoadingData
        LoadingData --> DataLoaded
        LoadingData --> LoadingError
        DataLoaded --> UserInteraction
        UserInteraction --> PerformAction
        PerformAction --> LoadingData
    }
    
    LoginPage --> Authentication
    Authentication --> CheckAuth
    
    state PerformAction {
        [*] --> ValidateInput
        ValidateInput --> SendRequest
        SendRequest --> ProcessResponse
        ProcessResponse --> UpdateUI
        UpdateUI --> [*]
    }
```

## Inertia.js Data Flow

```mermaid
sequenceDiagram
    participant User as 👤 User
    participant React as ⚛️ React Component
    participant Inertia as 🔄 Inertia.js
    participant Rails as 💎 Rails Controller
    participant Action as ⚡ Action
    participant PxPlus as 🏢 PxPlus
    
    User->>React: Click/Submit
    React->>Inertia: Inertia.post(url, data)
    Inertia->>Rails: HTTP Request
    Rails->>Action: Execute Action
    Action->>PxPlus: Command Execution
    PxPlus->>Action: Response
    Action->>Rails: Parsed JSON
    Rails->>Inertia: Inertia Response
    Inertia->>React: Update Props
    React->>User: Updated UI
```

## Component Props Flow

```mermaid
graph TD
    subgraph ServerSide["🖥️ Server-Side (Rails)"]
        Controller[🎮 Controller]
        Action[⚡ Action]
        Data[📊 Data from PxPlus]
    end
    
    subgraph ClientSide["🎨 Client-Side (React)"]
        PageComponent[📄 Page Component]
        ChildComponents[👶 Child Components]
        LocalState[🏪 Local State]
        
        subgraph SharedState["🌐 Shared State"]
            AuthContext[🔐 Auth Context]
            ThemeContext[🎨 Theme Context]
        end
    end
    
    Data --> Action
    Action --> Controller
    Controller -->|Inertia Props| PageComponent
    PageComponent -->|Props| ChildComponents
    ChildComponents -->|State Updates| LocalState
    
    AuthContext -->|Auth Data| PageComponent
    ThemeContext -->|Theme Data| PageComponent
```

## TailwindCSS Design System

```mermaid
graph LR
    subgraph DesignTokens["🎨 Design Tokens"]
        Colors[🎨 Colors<br/>Primary, Secondary<br/>Success, Error]
        Typography[📝 Typography<br/>Headings, Body<br/>Sizes, Weights]
        Spacing[📏 Spacing<br/>Margins, Padding<br/>Grid System]
        Shadows[🌓 Shadows<br/>Elevation Levels]
    end
    
    subgraph Components["🧩 Components"]
        Buttons[🔘 Buttons<br/>Primary, Secondary<br/>Sizes, States]
        Forms[📝 Forms<br/>Inputs, Selects<br/>Validation States]
        Cards[🃏 Cards<br/>Content Containers<br/>Actions, Headers]
        Navigation[🧭 Navigation<br/>Menus, Breadcrumbs<br/>Tabs, Pagination]
    end
    
    subgraph Responsive["📱 Responsive"]
        Mobile[📱 Mobile First<br/>320px+ Base]
        Tablet[📱 Tablet<br/>768px+ md:]
        Desktop[🖥️ Desktop<br/>1024px+ lg:]
    end
    
    Colors --> Buttons
    Typography --> Forms
    Spacing --> Cards
    Shadows --> Navigation
    
    Buttons --> Mobile
    Forms --> Mobile
    Cards --> Mobile
    Navigation --> Mobile
    
    Mobile --> Tablet
    Tablet --> Desktop
```