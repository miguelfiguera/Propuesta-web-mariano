# Diagramas del Proyecto

Este repositorio contiene diagramas del proyecto en dos formatos:

## 📊 PlantUML

Los diagramas PlantUML se renderizan automáticamente usando el proxy de PlantUML:

![Arquitectura](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/chepino/PropuestaMariano/main/diagrams/plantuml/arquitectura.puml)

## 🧜‍♀️ Mermaid

Los diagramas Mermaid se renderizan nativamente en GitHub. Ver [`diagrams/mermaid/`](./diagrams/mermaid/)

## 📁 Estructura

- **PlantUML:** [`diagrams/plantuml/`](./diagrams/plantuml/) - Renderizados automáticamente con proxy
- **Mermaid:** [`diagrams/mermaid/`](./diagrams/mermaid/) - Renderizados nativamente en GitHub

Para usar PlantUML en tus propios archivos:
```markdown
![Nombre](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/USUARIO/REPO/main/ARCHIVO.puml)
```