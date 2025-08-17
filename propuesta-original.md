# Propuesta de Arquitectura de Aplicación

## Introducción

El presente documento describe la arquitectura propuesta para una nueva aplicación que integrará un frontend moderno con sistemas heredados basados en PxPlus 7.71. La solución se diseñará con un enfoque pragmático y eficiente, optimizada para desarrollo individual y bajo presupuesto, permitiendo una comunicación directa y efectiva entre las capas de la aplicación.

## Arquitectura General

La arquitectura se compondrá de los siguientes elementos principales:

*   **Frontend:** Se desarrollará una interfaz de usuario responsiva y reactiva utilizando **React** con **Inertia.js**, implementando un enfoque **mobile-first** con **TailwindCSS**. Esta combinación permitirá crear una experiencia de usuario moderna y fluida con renderizado del lado del servidor (SSR).

*   **Backend:** El servidor backend se implementará en **Ruby on Rails** en modo API con **Inertia.js**, sin utilizar ActiveRecord. En su lugar, se implementará un patrón de **Actions/Service Objects** que manejará la comunicación directa con PxPlus mediante ejecución de comandos del sistema.

*   **Servidor Web:** **Nginx** actuará como reverse proxy y servidor de archivos estáticos, manejando las peticiones HTTP y distribuyendo la carga hacia la aplicación Rails.

*   **Comunicación con PxPlus:** La integración con PxPlus 7.71 se realizará mediante **ejecución directa de comandos del sistema**. Rails generará archivos `.txt` con nombres específicos, ejecutará comandos PxPlus pasando estos archivos como argumentos, procesará las respuestas en formato `.txt`, las convertirá a JSON y las enviará al frontend a través de Inertia.js.

## Módulos Funcionales

La aplicación contará con los siguientes módulos clave:

*   **Autenticación y Autorización:** Sistema de autenticación JWT que valida credenciales contra PxPlus 7.71 y mantiene estado de sesión stateless con roles y permisos para cada petición.
*   **CRUD de Pagos:** Módulo para gestión de pagos que utiliza Actions/Service Objects para crear archivos `.txt`, ejecutar comandos PxPlus y procesar respuestas.
*   **CRUD de Consultas a la Base de Datos:** Interfaz para operaciones de consulta que se comunica directamente con PxPlus mediante ejecución de comandos del sistema.
*   **Reportes:** Sistema de generación de reportes que utiliza el mismo patrón de comunicación con PxPlus para obtener datos y formatearlos para visualización.

## Implementación Técnica

### Patrón Actions/Service Objects
La arquitectura sin ActiveRecord se basará en:

*   **Actions:** Clases que encapsulan una operación específica de negocio
*   **File Generators:** Servicios para crear archivos `.txt` con formato específico para PxPlus
*   **Command Executors:** Servicios para ejecutar comandos del sistema con timeouts
*   **Response Parsers:** Servicios para convertir respuestas `.txt` de PxPlus a JSON
*   **Inertia Responders:** Controladores que envían datos JSON al frontend React

### Flujo de Comunicación
1. **Frontend React** envía petición via Inertia.js
2. **Rails Controller** delega a Action correspondiente
3. **Action** genera archivo `.txt` específico
4. **Command Executor** ejecuta comando PxPlus con archivo como argumento
5. **Response Parser** convierte respuesta `.txt` a JSON
6. **Inertia.js** envía JSON al frontend para renderizado

## Desafíos y Adaptaciones

Las principales adaptaciones técnicas incluyen:

*   **Gestión de archivos temporales:** Implementar un sistema robusto para manejo de archivos `.txt` de entrada y salida con nombres específicos.
*   **Ejecución de comandos:** Desarrollar un mecanismo seguro para ejecutar comandos PxPlus con manejo de timeouts y errores.
*   **Parsing personalizado:** Crear parsers específicos para convertir formatos `.txt` de PxPlus 7.71 a JSON estructurado.
*   **Arquitectura stateless:** Implementar autenticación JWT y validación de permisos sin persistencia local.
*   **Responsive Design:** Desarrollar interfaz mobile-first con TailwindCSS que se adapte a diferentes dispositivos.
