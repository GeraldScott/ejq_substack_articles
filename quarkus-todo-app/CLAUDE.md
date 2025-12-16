# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Development Commands

```bash
# Run in dev mode with live reload (auto-provisions PostgreSQL via Dev Services)
./mvnw quarkus:dev

# Run tests
./mvnw test

# Package the application
./mvnw package

# Build native executable (requires GraalVM)
./mvnw package -Dnative

# Build native executable using container (no GraalVM required)
./mvnw package -Dnative -Dquarkus.native.container-build=true
```

Dev mode automatically provisions a PostgreSQL database via Quarkus Dev Services - no manual database setup needed.

## Architecture

This is a full-stack Quarkus Todo application using:
- **Quarkus 3.24.4** with Java 21
- **Hibernate ORM with Panache** for persistence (Active Record pattern)
- **Qute** for server-side HTML templating
- **PostgreSQL** database (auto-provisioned in dev mode)

### Endpoints

- **REST API** (`/todos`): JSON CRUD operations via `TodoResource`
- **Web UI** (`/page/todos`): HTML interface via `PageResource` using Qute templates

### Key Files

- `Todo.java`: Panache entity (extends `PanacheEntity` for Active Record pattern)
- `TodoResource.java`: REST API endpoints returning JSON
- `PageResource.java`: HTML page rendering using Qute template injection
- `templates/todos.html`: Qute template for the web interface

### Panache Active Record Pattern

Entities extend `PanacheEntity` and use static methods directly on the entity class:
```java
Todo.listAll();
Todo.findById(id);
todo.persist();
Todo.deleteById(id);
```

### Qute Template Integration

Templates are injected by name matching the file in `src/main/resources/templates/`:
```java
@Inject
Template todos;  // matches templates/todos.html
```
