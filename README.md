# HRS - Hands-on Spec-Driven Development

This is the companion application for the session
**[Hands-on Spec-Driven Development](https://www.agenticdays.com/session/1244188-hands-on-spec-driven-development)**
at **Agentic Engineering Days Zurich 2026** (20 November 2026, 10:20–12:00, Studio 1, Marriott Zurich).

You build a hotel reservation system (HRS) by driving the implementation from specifications with the
**AI Unified Process** and an AI agent (Claude Code recommended; Copilot CLI, Codex or others work too).

## Start Your Environment Now

Choose one. Both work equally well. The setup runs in the background while we talk; the first lab starts at 10:45.

### Codespaces

1. Open [`github.com/martinelli-workshops/sdd-lab-agenticdays`](https://github.com/martinelli-workshops/sdd-lab-agenticdays)
2. **Code → Codespaces → Create codespace on main**
3. Java, Maven, the AI Unified Process plugins and Playwright are installed for you
4. You work on your own branch `workshop/<github-user>`

### Local

1. Install **JDK 25** and **Claude Code** (Copilot CLI or Codex also work)
2. `git clone` the repository
3. Open `claude` in the project and accept the AI Unified Process plugins it offers
4. Check: `./mvnw spring-boot:run`

## Predefined Requirements

The session is short, so the first steps are already done. The `docs` folder contains:

- [`docs/vision.md`](docs/vision.md): the vision of the hotel owners
- [`docs/requirements.md`](docs/requirements.md): the requirements catalog (functional requirements,
  non-functional requirements, constraints)
- [`docs/glossary.md`](docs/glossary.md): the domain terms

The technical layers are prepared too: [`docs/architecture/stack.md`](docs/architecture/stack.md) records which
AI Unified Process layer plugin builds and tests each layer (see [AI Unified Process Plugins](#ai-unified-process-plugins)).

Read them, then continue with the next AI Unified Process steps:

1. `/use-case-diagram`: derive the use cases from the requirements
2. `/entity-model`: design the entity model
3. `/use-case-spec UC-001`: specify a use case
4. `/use-case-implement UC-001`: implement it layer by layer (Flyway migration, jOOQ repository, Spring service,
   Vaadin view)
5. `/use-case-test UC-001`: test it (Browserless tests for the view, Playwright browser test), then
   `/use-case-coverage UC-001`

## AI Unified Process Plugins

The lab uses the AI Unified Process layer plugins from the
[AI Unified Process marketplace](https://github.com/ai-unified-process/marketplace). They are configured for the
project in `.claude/settings.json` (Claude Code) and `.github/copilot/settings.json` (Copilot CLI), so the agent
offers to install them when you open the repository:

| Plugin             | Layer                | Skills                                       |
|--------------------|----------------------|----------------------------------------------|
| `aiup-core`        | Specification, orchestration | `/use-case-diagram`, `/entity-model`, `/use-case-spec`, `/use-case-implement`, `/use-case-test`, `/use-case-coverage`, ... |
| `aiup-flyway`      | Migration            | `flyway-schema`                              |
| `aiup-jooq`        | Persistence          | `jooq-persistence`, `jooq-persistence-test`  |
| `aiup-spring-boot` | Service              | `spring-service`, `spring-service-test`      |
| `aiup-vaadin`      | UI, E2E              | `vaadin-flow-ui`, `vaadin-flow-ui-test`, `vaadin-e2e-test` |

To install them manually in Claude Code:

```bash
claude plugin marketplace add ai-unified-process/marketplace
claude plugin install aiup-core@ai-unified-process-marketplace
claude plugin install aiup-flyway@ai-unified-process-marketplace
claude plugin install aiup-jooq@ai-unified-process-marketplace
claude plugin install aiup-spring-boot@ai-unified-process-marketplace
claude plugin install aiup-vaadin@ai-unified-process-marketplace
```

The combined stack plugin `aiup-vaadin-jooq` is not used in this lab; disable it if you have it installed.

## Tech Stack

- **Java 25** with **Spring Boot 4.1**
- **Vaadin 25** for the UI (Java-based views)
- **jOOQ** for type-safe SQL and data access
- **Flyway** for database migrations
- **H2** as the database (embedded, no external setup required)
- **Vaadin Browserless Testing** for server-side UI tests
- **Playwright** with **Drama Finder** for browser-based end-to-end tests

## Running the Application

The application uses an embedded H2 database — no Docker or external database setup required.

```bash
./mvnw spring-boot:run
```

Or run `HrsApplication.main()` directly from your IDE.

## Build and Code Generation

The Maven build uses a two-step pipeline during `generate-sources`:

1. **Flyway** runs database migrations against a file-based H2 database
2. **jOOQ** generates type-safe Java code from the H2 schema

```bash
./mvnw compile
```

## Running Tests

Unit tests (Browserless) are run by **Surefire** with `mvnw test`. Integration tests (Playwright) use the `*IT` suffix
and are run by the **Failsafe** plugin, which is bound to the `integration-test` and `verify` phases.

**Unit tests only:**

```bash
./mvnw test
```

**All tests (unit + integration):**

```bash
./mvnw verify
```

The `verify` phase ensures that the Failsafe plugin picks up all `*IT` classes (e.g. Drama Finder `AbstractBasePlaywrightIT` subclasses)
and reports their results correctly. Always use `verify` instead of `integration-test` directly, so that test
failures are properly detected.
