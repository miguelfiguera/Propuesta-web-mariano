# Arquitectura Frontend - React + Inertia.js

## Estructura de Componentes

```plantuml
@startuml
title Estructura de Componentes

package "🏠 App Structure" {
  [App.tsx\nMain Application] as App
  [Layout.tsx\nMain Layout] as Layout
}

package "🔐 Authentication" {
  [AuthProvider] as AuthProvider
  [LoginForm] as LoginForm
  [ProtectedRoute] as ProtectedRoute
}

package "💳 Payment Module" {
  [PaymentList] as PaymentList
  [PaymentForm] as PaymentForm
  [PaymentCard] as PaymentCard
  [PaymentDetails] as PaymentDetails
}

package "🗄️ Database Module" {
  [QueryBuilder] as QueryBuilder
  [ResultsTable] as ResultsTable
  [QueryHistory] as QueryHistory
}

package "📊 Reports Module" {
  [ReportGenerator] as ReportGenerator
  [ReportViewer] as ReportViewer
  [ReportList] as ReportList
}

package "🔧 Shared Components" {
  [Button] as Button
  [Input] as Input
  [Modal] as Modal
  [Loading] as Loading
  [ErrorBoundary] as ErrorBoundary
}

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

@enduml
```

## Mobile-First Responsive Design

```plantuml
@startuml
title Mobile-First Responsive Design

package "📱 Mobile (320px+)" {
  [🍔 Hamburger Menu] as MobileNav
  [📱 Card Layout] as MobileCards
  [📚 Stacked Forms] as MobileStack
}

package "📱 Tablet (768px+)" {
  [📑 Collapsible Sidebar] as TabletSidebar
  [🔲 Grid Layout] as TabletGrid
  [📂 Tab Navigation] as TabletTabs
}

package "🖥️ Desktop (1024px+)" {
  [📋 Fixed Sidebar] as DesktopSidebar
  [🗂️ Table Layout] as DesktopTable
  [📊 Multi-column] as DesktopMultiColumn
}

MobileNav -right-> TabletSidebar : Breakpoint
MobileCards -right-> TabletGrid : Breakpoint
MobileStack -right-> TabletTabs : Breakpoint

TabletSidebar -right-> DesktopSidebar : Breakpoint
TabletGrid -right-> DesktopTable : Breakpoint
TabletTabs -right-> DesktopMultiColumn : Breakpoint

@enduml
```

## State Management Flow

```plantuml
@startuml
title State Management Flow

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

@enduml
```

## Inertia.js Data Flow

```plantuml
@startuml
title Inertia.js Data Flow

participant "👤 User" as User
participant "⚛️ React Component" as React
participant "🔄 Inertia.js" as Inertia
participant "💎 Rails Controller" as Rails
participant "⚡ Action" as Action
participant "🏢 PxPlus" as PxPlus

User -> React : Click/Submit
React -> Inertia : Inertia.post(url, data)
Inertia -> Rails : HTTP Request
Rails -> Action : Execute Action
Action -> PxPlus : Command Execution
PxPlus -> Action : Response
Action -> Rails : Parsed JSON
Rails -> Inertia : Inertia Response
Inertia -> React : Update Props
React -> User : Updated UI

@enduml
```

## Component Props Flow

```plantuml
@startuml
title Component Props Flow

package "🖥️ Server-Side (Rails)" {
  [🎮 Controller] as Controller
  [⚡ Action] as Action
  [📊 Data from PxPlus] as Data
}

package "🎨 Client-Side (React)" {
  [📄 Page Component] as PageComponent
  [👶 Child Components] as ChildComponents
  [🏪 Local State] as LocalState
  
  package "🌐 Shared State" {
    [🔐 Auth Context] as AuthContext
    [🎨 Theme Context] as ThemeContext
  }
}

Data --> Action
Action --> Controller
Controller -->|Inertia Props| PageComponent
PageComponent -->|Props| ChildComponents
ChildComponents -->|State Updates| LocalState

AuthContext -->|Auth Data| PageComponent
ThemeContext -->|Theme Data| PageComponent

@enduml
```

## TailwindCSS Design System

```plantuml
@startuml
title TailwindCSS Design System

package "🎨 Design Tokens" {
  [🎨 Colors\nPrimary, Secondary\nSuccess, Error] as Colors
  [📝 Typography\nHeadings, Body\nSizes, Weights] as Typography
  [📏 Spacing\nMargins, Padding\nGrid System] as Spacing
  [🌓 Shadows\nElevation Levels] as Shadows
}

package "🧩 Components" {
  [🔘 Buttons\nPrimary, Secondary\nSizes, States] as Buttons
  [📝 Forms\nInputs, Selects\nValidation States] as Forms
  [🃏 Cards\nContent Containers\nActions, Headers] as Cards
  [🧭 Navigation\nMenus, Breadcrumbs\nTabs, Pagination] as Navigation
}

package "📱 Responsive" {
  [📱 Mobile First\n320px+ Base] as Mobile
  [📱 Tablet\n768px+ md:] as Tablet
  [🖥️ Desktop\n1024px+ lg:] as Desktop
}

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

@enduml
```