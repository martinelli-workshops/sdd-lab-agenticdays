# Stack

**For the agent:** read before writing code. This file is the AI Unified Process record of the technical layers,
independent of how the team documents its architecture. The Layers table says which layer skill builds and tests
each technical layer: `/use-case-implement` runs the Skill column bottom-up, `/use-case-test` runs the Test skill
column from the integration layer up, and `/use-case-coverage` searches for the markers in the last table.

**Last updated:** 2026-11-09

## Layers

| Layer       | Technology                        | Skill              | Test skill              | Location                                             |
|-------------|-----------------------------------|--------------------|-------------------------|------------------------------------------------------|
| Migration   | Flyway, H2                        | `flyway-schema`    | –                       | `src/main/resources/db/migration`                    |
| Persistence | jOOQ                              | `jooq-persistence` | `jooq-persistence-test` | `ch.martinelli.edu.hrs.<feature>.domain`             |
| Client      | –                                 | –                  | –                       | –                                                    |
| Service     | Spring Boot 4.1                   | `spring-service`   | `spring-service-test`   | `ch.martinelli.edu.hrs.<feature>.domain`             |
| API         | –                                 | –                  | –                       | –                                                    |
| UI          | Vaadin Flow 25                    | `vaadin-flow-ui`   | `vaadin-flow-ui-test`   | `ch.martinelli.edu.hrs.<feature>.ui`                 |
| E2E         | Playwright for Java, Drama Finder | –                  | `vaadin-e2e-test`       | `src/test/java`, in the package of the view under test |

**Integration layer:**

| Use case offered through | Tested in | Test skill            |
|--------------------------|-----------|-----------------------|
| UI                       | UI        | `vaadin-flow-ui-test` |
| Service                  | Service   | `spring-service-test` |

A layer the project does not have is marked `–` in every column (a UI layer in a REST service). The Integration
layer table keeps only the rows of the entry layers the project has — a REST service drops the UI row — and its
Test skill column names what the Layers table names for that layer (`–` while the skill is not installed). A use
case is tested in the first row its `**Layers:**` line names, or in the first row when it has no such line. That layer's tests run in process against the real database
and exercise every layer below it, so `/use-case-test` runs its test skill and the ones above it and writes no tests
of their own for the layers below unless a run names them (`/use-case-test UC-001 persistence`).

The Vaadin Flow views call the service layer directly; the project has no REST API and calls no external system.

**E2E scope:** Use cases and test cases

`Use cases and test cases`: `/use-case-test` adds a browser test to every use case offered through the UI, and
`/test-case-implement` automates the TC-* journeys. `Test cases only`: browser tests are written for the journeys
alone, and a use case is tested in its integration layer. A project without an E2E layer drops the line.

## Module layout

Flat single Maven module, root package `ch.martinelli.edu.hrs`, packaged by feature:

- `ch.martinelli.edu.hrs.<feature>.domain`: services and jOOQ repositories of the feature
- `ch.martinelli.edu.hrs.<feature>.ui`: the Vaadin Flow views of the feature
- `ch.martinelli.edu.hrs.db`: the jOOQ classes generated from the Flyway migrations during `generate-sources`
  (never edited by hand)
- Tests live in the same package under `src/test/java`. Browserless tests extend
  `ch.martinelli.edu.hrs.core.ui.HrsBrowserlessTest`, browser tests extend Drama Finder's `AbstractBasePlaywrightIT`.
  The `@UseCase` and `@TestCase` annotations are in `ch.martinelli.edu.hrs.usecase`.
- Test data: repeatable Flyway migrations `src/test/resources/db/migration/R__uc<id>_<slug>.sql`.

## Commands

| Purpose            | Command          |
|--------------------|------------------|
| Build              | `./mvnw compile` |
| Unit tests         | `./mvnw test`    |
| Integration tests  | `./mvnw verify`  |
| End-to-end tests   | `./mvnw verify`  |

`./mvnw test` runs the `*Test` classes (Surefire), `./mvnw verify` additionally the `*IT` browser tests (Failsafe).

## Markers

Each layer skill keeps its own rows here: on every run it checks that the rows for its layer match its marker
reference and corrects them. Do not edit the rows by hand; rerun the layer skill instead.

| Layer       | Skill                 | Kind | Marker to grep for |
|-------------|-----------------------|------|--------------------|
| All         | –                     | Code | `// UC-XXX BR-YYY:` directly above the method, query condition, or validator that enforces the rule |
| Migration   | flyway-schema         | Code | `-- UC-XXX BR-YYY:` directly above the `CHECK`, `UNIQUE`, index, or trigger that enforces the rule |
| Migration   | flyway-schema         | Name | `<location>/V*__*.sql`, named after the tables they change, never after a use case — search them by table and column |
| Persistence | jooq-persistence      | Code | `// UC-XXX BR-YYY:` directly above the jOOQ `.where(...)`/`.and(...)` condition or query that enforces the rule |
| Persistence | jooq-persistence      | Name | `<Aggregate>Repository` (`@Repository` with a `DSLContext`, or the adapter implementing that outbound port in a hexagonal layout), named after the aggregate root, never a use case |
| Persistence | jooq-persistence-test | Test | `@UseCase(id = "UC-XXX", scenario = "…", businessRules = {"BR-YYY"}, requirements = {"NFR-XXX"})` on every repository test method |
| Persistence | jooq-persistence-test | Name | `UC<id><Name>RepositoryTest`, e.g. `UC001RegisterPersonRepositoryTest`; test data in `src/test/resources/db/migration/R__uc<id>_<slug>.sql` |
| Service     | spring-service        | Code | `// UC-XXX BR-YYY:` directly above the service method or guard clause that enforces the rule |
| Service     | spring-service        | Code | `UC-XXX` in the Javadoc of the public service method that realizes a step or an alternative flow |
| Service     | spring-service        | Name | `<Entity>Service` classes annotated `@Service`; domain exceptions `<Reason>Exception` |
| Service     | spring-service-test   | Test | `@UseCase(id = "UC-XXX", scenario = "…", businessRules = {"BR-YYY"}, requirements = {"NFR-XXX"})` on every service test method |
| Service     | spring-service-test   | Name | `UC<id><Name>ServiceTest`, e.g. `UC001RegisterGuestServiceTest` |
| UI          | vaadin-flow-ui        | Code | `UC-XXX` in the view's class Javadoc (`/** UC-XXX <Use Case Name>. */`) |
| UI          | vaadin-flow-ui        | Code | `// UC-XXX BR-YYY:` directly above the `Binder` validator or UI guard that enforces the rule |
| UI          | vaadin-flow-ui        | Name | `@Route` view class `<Name>View` named after the use case (`ManagePersonsView` for "Manage Persons") |
| UI          | vaadin-flow-ui-test   | Test | `@UseCase(id = "UC-XXX", scenario = "…", businessRules = {"BR-YYY"}, requirements = {"NFR-XXX"})` on every Browserless test method |
| UI          | vaadin-flow-ui-test   | Name | `UC<id><Name>Test`, e.g. `UC001ManagePersonsTest`, extending `SpringBrowserlessTest` or the project's subclass of it |
| E2E         | vaadin-e2e-test       | Test | `@DisplayName("UC-XXX: …")` on the class and one `@Nested` `@DisplayName("A<n>: …")` per alternative flow |
| E2E         | vaadin-e2e-test       | Test | `@TestCase(id = "TC-XXX", useCases = {…})` on the class, `@DisplayName("TC-XXX: …")`, and one `// Step <n>: <name>` comment per Flow row |
| E2E         | vaadin-e2e-test       | Name | `UC<id><Name>IT` for a use case, `TC<id><Name>IT` for a test case journey |
