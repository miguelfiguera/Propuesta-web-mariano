# Flujo de Integración con PxPlus

## Flujo Principal de Operaciones

```plantuml
@startuml
title Flujo Principal de Operaciones

start

if (🔐 Usuario\nAutenticado?) then (No)
  :📝 Login Form;
  :🔍 Validate Against PxPlus;
  if (✅ Auth Success?) then (No)
    :❌ Login Error;
    stop
  else (Yes)
    :🎫 Generate JWT;
  endif
else (Yes)
endif

:🛡️ Check Permissions;

if (✅ Authorized?) then (No)
  :🚫 403 Forbidden;
  stop
else (Yes)
  :⚡ Execute Action;
endif

:📝 Create .txt File;
:💻 Execute PxPlus Command;

if (⏰ Timeout?) then (Yes)
  :❌ Command Timeout;
else (No)
  :⏱️ Wait for Response;
  :🔄 Parse .txt Response;
  :📋 Convert to JSON;
endif

:🧹 Cleanup Temp Files;
:📤 Send to Frontend;

stop

@enduml
```

## Flujo de Autenticación

```plantuml
@startuml
title Flujo de Autenticación

participant "🎨 Frontend" as F
participant "💎 Rails" as R
participant "📄 File System" as FS
participant "🏢 PxPlus" as P

F -> R : POST /auth/login {email, password}
R -> R : Validate input
R -> FS : Create auth_request.txt
note over FS : "AUTH|email|password|timestamp"
R -> P : Execute auth command
P -> FS : Write auth_response.txt
note over FS : "SUCCESS|user_id|email|role|permissions"
FS -> R : Read response file
R -> R : Parse response
R -> R : Generate JWT token
R -> FS : Cleanup temp files
R -> F : Return {token, user}

@enduml
```

## Flujo de Operaciones CRUD

```plantuml
@startuml
title Operaciones CRUD de Pagos

package "💳 Payment CRUD Flow" {
  rectangle "Create" {
    :Create Payment] as Create
    Create -> :📝 Generate payment_create.txt]
    :📝 Generate payment_create.txt] -> :💻 Execute create_payment command]
    :💻 Execute create_payment command] -> :📋 Parse response] as ParseCreate
  }
  
  rectangle "Read" {
    :Read Payment] as Read
    Read -> :📝 Generate payment_query.txt]
    :📝 Generate payment_query.txt] -> :💻 Execute query_payment command]
    :💻 Execute query_payment command] -> :📋 Parse response] as ParseRead
  }
  
  rectangle "Update" {
    :Update Payment] as Update
    Update -> :📝 Generate payment_update.txt]
    :📝 Generate payment_update.txt] -> :💻 Execute update_payment command]
    :💻 Execute update_payment command] -> :📋 Parse response] as ParseUpdate
  }
  
  rectangle "Delete" {
    :Delete Payment] as Delete
    Delete -> :📝 Generate payment_delete.txt]
    :📝 Generate payment_delete.txt] -> :💻 Execute delete_payment command]
    :💻 Execute delete_payment command] -> :📋 Parse response] as ParseDelete
  }
}

ParseCreate -> :📤 Return JSON to Frontend]
ParseRead -> :📤 Return JSON to Frontend]
ParseUpdate -> :📤 Return JSON to Frontend]
ParseDelete -> :📤 Return JSON to Frontend]

@enduml
```

## Formato de Archivos de Intercambio

```plantuml
@startuml
title Formato de Archivos de Intercambio

package "📥 Input Files (.txt)" {
  [🔐 auth_request.txt\nAUTH|email|password|timestamp] as Auth
  [💳 payment_create.txt\nCREATE_PAYMENT|amount|currency|account] as Payment
  [🔍 query_request.txt\nQUERY|table|fields|conditions] as Query
  [📊 report_request.txt\nGENERATE_REPORT|type|parameters|format] as Report
}

package "💻 PxPlus Commands" {
  [auth_user] as AuthCmd
  [create_payment] as PaymentCmd
  [execute_query] as QueryCmd
  [generate_report] as ReportCmd
}

package "📤 Output Files (.txt)" {
  [✅ auth_response.txt\nSUCCESS|user_id|role|permissions] as AuthResp
  [💳 payment_response.txt\nSUCCESS|payment_id|status|message] as PaymentResp
  [📋 query_response.txt\nDATA|field1|field2|field3\nvalue1|value2|value3] as QueryResp
  [📊 report_response.txt\nREPORT|data|metadata|status] as ReportResp
}

Auth --> AuthCmd
Payment --> PaymentCmd
Query --> QueryCmd
Report --> ReportCmd

AuthCmd --> AuthResp
PaymentCmd --> PaymentResp
QueryCmd --> QueryResp
ReportCmd --> ReportResp

@enduml
```

## Manejo de Errores

```plantuml
@startuml
title Manejo de Errores

start

:🔄 Operation Start;
:💻 Execute Command;

if (⏰ Timeout?) then (Yes)
  :❌ Timeout Error;
else (No)
  if (🔍 Exit Code OK?) then (No)
    :❌ Command Error;
  else (Yes)
    :📖 Read Response;
    
    if (✅ Valid Format?) then (No)
      :❌ Parse Error;
    else (Yes)
      if (🔍 Status = SUCCESS?) then (No)
        :❌ Business Logic Error;
      else (Yes)
        :✅ Success;
        :🧹 Cleanup Files;
        :📤 Return Response;
        stop
      endif
    endif
  endif
endif

:📝 Log Error;
:🧹 Cleanup Files;
:📤 Return Response;

stop

@enduml
```