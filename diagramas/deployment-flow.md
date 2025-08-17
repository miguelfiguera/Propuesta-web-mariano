# Flujo de Deployment y Configuración

## Proceso de Setup del Servidor

```plantuml
@startuml
title Proceso de Setup del Servidor

start

if (🖥️ Servidor Ubuntu\nDisponible?) then (No)
  :☁️ Provisionar VPS;
else (Yes)
endif

:🔄 Actualizar Sistema;
:📦 Instalar Dependencias;

rectangle "Installation" {
  :💎 Instalar Ruby 3.2;
  :🚂 Instalar Rails 7+;
  :📗 Instalar Node.js 18;
  :🌐 Instalar Nginx;
  :📚 Instalar Git;
}

:⚙️ Configurar Nginx;
:🔒 Setup SSL/TLS;
:👤 Crear Usuario Deploy;
:🏢 Configurar PxPlus Access;
:🧪 Probar Configuración;
:✅ Servidor Listo;

stop

@enduml
```

## Flujo de Deployment

```plantuml
@startuml
title Flujo de Deployment

participant "👨‍💻 Developer" as Dev
participant "📚 GitHub" as Git
participant "🖥️ Ubuntu Server" as Server
participant "💎 Rails App" as App
participant "🌐 Nginx" as Nginx
participant "🏢 PxPlus" as PxPlus

Dev -> Git : git push origin main
Dev -> Server : SSH connection
Server -> Git : git pull origin main
Server -> Server : bundle install
Server -> Server : npm install & build
Server -> App : Restart Rails app
Server -> Nginx : Reload configuration
App -> PxPlus : Test connection
PxPlus -> App : Connection OK
Server -> Dev : Deployment successful

@enduml
```

## Configuración de Archivos

```plantuml
@startuml
title Configuración de Archivos

package "⚙️ Configuración del Sistema" {
  [⚙️ Systemd Service\n/etc/systemd/system/pxplus_app.service] as SystemdService
  [🌐 Nginx Config\n/etc/nginx/sites-available/pxplus_app] as NginxConfig
  [🔐 Environment Variables\n.env.production] as EnvFile
  [📝 Log Rotation\n/etc/logrotate.d/pxplus_app] as LogRotate
}

package "📁 Aplicación" {
  [📁 /home/deploy/pxplus_app/] as AppDir
  [🐾 Puma Config\nconfig/puma.rb] as PumaConfig
  [📄 Temp Files\n/tmp/pxplus_exchange/] as TempFiles
  [💾 Backups\n/home/deploy/backups/] as BackupDir
}

package "📜 Scripts de Automatización" {
  [🚀 deploy.sh] as DeployScript
  [💾 backup.sh] as BackupScript
  [🧹 cleanup.sh] as CleanupScript
  [🏥 health_check.sh] as HealthCheck
}

SystemdService --> AppDir
NginxConfig --> PumaConfig
EnvFile --> TempFiles
DeployScript --> BackupScript
BackupScript --> CleanupScript
CleanupScript --> HealthCheck

@enduml
```

## Monitoreo y Mantenimiento

```plantuml
@startuml
title Monitoreo y Mantenimiento

package "📝 Logs del Sistema" {
  [🌐 Nginx Logs\n/var/log/nginx/] as NginxLogs
  [💎 Rails Logs\nlog/production.log] as RailsLogs
  [🖥️ System Logs\n/var/log/syslog] as SystemLogs
  [🏢 PxPlus Logs\nCustom location] as PxPlusLogs
}

package "📊 Scripts de Monitoreo" {
  [🏥 Health Check\nApp status, disk space] as HealthCheck
  [📊 Log Analysis\nError patterns, performance] as LogAnalysis
  [💾 Backup Verification\nBackup integrity] as BackupCheck
  [🔒 Security Scan\nFailed logins, file access] as SecurityScan
}

package "🚨 Alertas" {
  [📧 Email Alerts] as EmailAlert
  [📝 Log-based Alerts] as LogAlert
  [💿 Disk Space Alerts] as DiskAlert
  [⚙️ Service Down Alerts] as ServiceAlert
}

NginxLogs --> HealthCheck
RailsLogs --> LogAnalysis
SystemLogs --> BackupCheck
PxPlusLogs --> SecurityScan

HealthCheck --> EmailAlert
LogAnalysis --> LogAlert
BackupCheck --> DiskAlert
SecurityScan --> ServiceAlert

@enduml
```

## Estructura de Directorios en Servidor

```plantuml
@startuml
title Estructura de Directorios en Servidor

folder "🖥️ Ubuntu Server" {
  folder "📁 /home/" {
    folder "👤 /home/deploy/" {
      folder "💎 pxplus_app/" {
        folder "📄 app/"
        folder "⚙️ config/"
        folder "🌐 public/"
        folder "📁 tmp/"
      }
      folder "📜 scripts/" {
        file "🚀 deploy.sh"
        file "💾 backup.sh"
        file "🧹 cleanup.sh"
        file "🏥 health_check.sh"
      }
      folder "💾 backups/"
      folder "📝 logs/"
    }
  }
  
  folder "📁 /tmp/" {
    folder "🔄 pxplus_exchange/" {
      folder "📥 input/"
      folder "📤 output/"
      folder "📦 archive/"
    }
  }
  
  folder "⚙️ /etc/" {
    folder "🌐 nginx/" {
      folder "📋 sites-available/"
    }
    folder "⚙️ systemd/" {
      folder "📄 system/"
    }
  }
}

@enduml
```

## Backup y Recovery Strategy

```plantuml
@startuml
title Backup y Recovery Strategy

package "💾 Backup Types" {
  [💻 Code Backup\nApplication files] as CodeBackup
  [⚙️ Config Backup\nSystem configuration] as ConfigBackup
  [📄 PxPlus Files\nExchange files] as FileBackup
  [📝 Log Backup\nApplication logs] as LogBackup
}

package "📅 Backup Schedule" {
  [📅 Daily\n2:00 AM] as Daily
  [📅 Weekly\nSunday 1:00 AM] as Weekly
  [📅 Monthly\n1st day 0:00 AM] as Monthly
}

package "💽 Storage" {
  [🖥️ Local Storage\n/home/deploy/backups/] as LocalStorage
  [☁️ Remote Storage\nOptional cloud backup] as RemoteStorage
}

package "🔧 Recovery Process" {
  [⏹️ Stop Services] as StopServices
  [📁 Restore Files] as RestoreFiles
  [🔐 Update Permissions] as UpdatePermissions
  [▶️ Start Services] as StartServices
  [✅ Verify Health] as VerifyHealth
}

CodeBackup --> Daily
ConfigBackup --> Weekly
FileBackup --> Daily
LogBackup --> Monthly

Daily --> LocalStorage
Weekly --> LocalStorage
Monthly --> RemoteStorage

LocalStorage --> StopServices
StopServices --> RestoreFiles
RestoreFiles --> UpdatePermissions
UpdatePermissions --> StartServices
StartServices --> VerifyHealth

@enduml
```