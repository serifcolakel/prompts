# AI Development Playbook 🚀

> [!NOTE]
> This repository is a curated collection of my technical article summaries (published on [Dev.to](https://dev.to/serifcolakel) and [Medium](https://medium.com/@serifcolakel)), combined with prompt templates and best practices developed during my interactive sessions with AI coding agents. It serves as my personal development playbook and guardrail ruleset.

This repository contains a collection of **reusable prompt templates** designed to standardize and align AI coding agents (such as Claude Code, Cursor, Codex, OpenCode, ChatGPT, etc.) with specific engineering guidelines, architectural styles, and quality standards across my development lifecycle.

By referencing these guides during development, you ensure that AI agents generate code that respects type safety, performance budgets, accessibility rules, and clean architecture boundaries.

---

## 📂 Directory Structure & Playbook Guides

The prompt templates are categorized by engineering domain:

### 📱 `react-native/` (Mobile & React Native)
Mobile-specific components, style guides, and testing parameters:
- `TEST_ID_GUIDE.md`: Rules for applying stable, consistent `testID` props for Maestro and Playwright E2E automation.
- `COMPONENT_REVIEW.md`: SOLID, readability, props interface design, memoization, and hooks guidelines for React Native components.
- `ACCESSIBILITY_GUIDE.md`: Guidelines for `accessibilityLabel/Role/Hint`, VoiceOver/TalkBack testing, dynamic type font scaling, and 44x44 minimum touch targets.
- `PERFORMANCE_GUIDE.md`: Performance rules covering useMemo, useCallback, render optimizations, FlatList/FlashList tuning, and image caching.
- `MAESTRO_GUIDE.md`: Standards for building Maestro E2E test flows in YAML, dynamic locators, assertions, and sub-flows.
- `NAVIGATION_GUIDE.md`: React Navigation & Expo Router guidelines including typed routes/params, universal links, and safe area context.
- `DESIGN_SYSTEM_GUIDE.md`: Spacing grid tokens, colors, typography, theme support (dark/light), and reusable component primitives.
- `MODERN_STYLING_GUIDE.md`: Auditing deprecated styling attributes (e.g. replacing `shadow*` styles with modern cross-platform equivalents) and optimizing layouts.

### 🐹 `golang/` (Go Backend Development)
Go-specific architectural layouts, database integration, and testing patterns:
- `GO_BEST_PRACTICES.md`: Clean/Hexagonal architecture, concurrency patterns (goroutine leaks, channels, sync), context propagation, and memory allocations.
- `GO_TEST_GUIDE.md`: Table-driven tests, subtests execution, parallel testing, mockgen dependencies, benchmarks, and the race detector.
- `GO_ERROR_HANDLING.md`: Sentinel errors, custom error structs, error wrapping (`%w`), panic/recover middleware, and structured `slog` logging.
- `GO_DATABASE_GUIDE.md`: Connection pool tuning, parameterized queries (SQL injection prevention), database transactions, and Goose migrations.
- `CROSS_COMPILATION_GUIDE.md`: Multi-platform compilation configurations (`CGO_ENABLED=0`), linker flag optimization (`-ldflags="-s -w"`), and asset bundling using `go:embed`.
- `PYTHON_TO_GO_MIGRATION.md`: Mapping Python structures to Go types, mapping pip dependencies, executing OS subprocess pipes concurrently, and migrating scripts.
- `GO_CONCURRENT_TESTING.md`: Deterministic testing of concurrent Go routines, channel-based synchronization, and clock/time mocking.
- `GO_GRACEFUL_SHUTDOWN.md`: Trapping OS signals (`SIGINT`, `SIGTERM`), draining HTTP/gRPC servers, closing connection pools, and worker context cancellations.
- `GO_SAGA_PATTERN.md`: Microservices distributed transactions implementation (Choreography vs Orchestration), compensation actions, and idempotency logic.
- `COBRA_CLI_GUIDE.md`: Building CLI tools and daemon processes using the Cobra framework, persistent/local flags, and argument validation.

### 📐 `typescript/` (TypeScript & Core Architecture)
TypeScript configurations, state management, forms, and security:
- `TYPESCRIPT_GUIDE.md`: Strict mode guidelines, banning `any` in favor of `unknown`, utility types, generics, and switch-case exhaustiveness checks.
- `ERROR_HANDLING_GUIDE.md`: Try-catch boundaries, custom error structures, React Error Boundaries, and logging integrations (Sentry, Crashlytics).
- `API_INTEGRATION_GUIDE.md`: Client configuration (Axios/Fetch), request/response interceptors, Zod parsing validation, and exponential backoff retry.
- `STATE_MANAGEMENT_GUIDE.md`: Store selections for Zustand/Redux, selector optimization (preventing re-renders), immutable updates, and MMKV/AsyncStorage caching.
- `FORM_GUIDE.md`: Controlled vs uncontrolled inputs (React Hook Form, Formik), dynamic field arrays, and keyboard focus actions.
- `VALIDATION_GUIDE.md`: Schema definitions (Zod/Yup), type inference, custom refinements (password confirmation check), and localized messages.
- `SECURITY_GUIDE.md`: Secure storage (Keychain vs AsyncStorage), secret management (`.env`), XSS data sanitization, and HTTPS rules.
- `ADVANCED_TYPESCRIPT.md`: Conditional types (`infer`), mapped types, key remapping, template literals, custom type guards, and discriminated union states.
- `RXJS_REACTIVE_GUIDE.md`: Piping stream operations (`switchMap`, `exhaustMap`), BehaviorSubjects, and memory leak prevention using the `takeUntil` pattern.
- `TAURI_IPC_GUIDE.md`: Tauri v2 IPC communication using `invoke`, async backend event emitters (`emit`/`listen`), and keychain security integrations.
- `REACT_PROVIDER_PATTERN.md`: Decoupled Provider & Registry pattern with mandatory interface schemas and Null (no-op) fallbacks.

### 🌿 `git-workflow/` (Git & PR Management)
Branch naming, commit formatting, and repository automation:
- `PULL_REQUEST_GUIDE.md`: Conventional Commit titles, structured PR description templates, and self-review quality checklists.
- `MULTI_PR_MERGE_GUIDE.md`: Stacked branch rebasing, E2E release branch consolidation, conflict markers resolution flow, and merge vs rebase guidelines.
- `GITHUB_GITLAB_MCP_GUIDE.md`: Guidelines for using GitHub & GitLab MCP tools to automate PR/MR reviews, pipeline checks, and discussions.

### 🧪 `testing/` (General E2E & Unit Testing)
- `PLAYWRIGHT_GUIDE.md`: Playwright E2E guidelines, stable locators (`getByRole`, `getByTestId`), wait avoidance, and Page Object Models (POM).
- `JEST_GUIDE.md`: Jest unit/integration testing, AAA (Arrange-Act-Assert) pattern, mock strategies, and custom context rendering wrappers.

### 🔍 `code-quality/` (Review, Refactoring, & Observability)
Code reviews, codebase navigation, and telemetry:
- `CODE_REVIEW.md`: Bug finding checklist, edge cases, performance bottlenecks, TypeScript diagnostics, and memory leak tracking (unmounted listeners/timers).
- `REFACTOR_GUIDE.md`: Refactoring code without behavior/UI modifications, DRY principles, complexity reduction, and magic variable extraction.
- `DOCUMENTATION_GUIDE.md`: Standardizing README layouts, JSDoc comment blocks, props tables, and migration changelogs.
- `I18N_GUIDE.md`: Extraction of hardcoded strings, nested translation keys, dynamic variables, and pluralization guidelines.
- `SENTRY_MCP_GUIDE.md`: Querying production crashes via Sentry MCP, stack trace analysis, and alerting workflows.
- `CODEBASE_MEMORY_MCP_GUIDE.md`: Navigating and indexing codebases via the codebase-memory-mcp graph search tools.
- `SQLITE_MIGRATIONS_GUIDE.md`: Safe SQLite database schemas migrations, idempotent column alterations, WAL modes, and busy timeouts.

### 🎨 `ui-ux/` (UI Polish & Motion)
- `DESIGN_POLISH_GUIDE.md`: Spacing token alignment, typography scale, modern theme palettes, glassmorphism, soft shadows, and skeleton screens.
- `ANIMATION_GUIDE.md`: Timing easing curves (spring vs timing), CSS layout reflow prevention, and Reanimated thread worklets.

### 📋 `product/` (Product Management)
Epics, tickets, and user stories tracking:
- `PRODUCT_OWNER_GUIDE.md`: User stories writing (INVEST model), acceptance criteria (Gherkin format), RICE score prioritization, and grooming checklists.
- `BUSINESS_ANALYST_GUIDE.md`: Functional specs, actors use-case specifications, user flow routing, and system data mapping tables.
- `LINEAR_MCP_GUIDE.md`: Linear ticket transitions, comments sync, and PR linking via the Linear remote MCP server.

### ⚙️ `workflows/` (Development Lifecycles)
- `BACKEND_WORKFLOW.md`: API-first OpenAPI designs, cursor pagination, backwards-compatible migrations, and correlation logs propagation.
- `FRONTEND_WORKFLOW.md`: Feature-driven directories, server state caching (TanStack Query), loading skeletons, and code-splitting.
- `MOBILE_WORKFLOW.md`: Offline-first loading, outbound sync queues, EAS/Fastlane builds, OTA updates constraints, and store reviewer preparations.
- `AGENT_ORCHESTRATION_GUIDE.md`: Sub-process command isolation, stdout/stderr scanner stream caching, and WebSockets sync.
- `GENERATIVE_UI_GUIDE.md`: Generative UI architecture, component registry lookup pattern, schema validations (Zod), and token payload efficiency.
- `AI_AGENT_GOVERNANCE.md`: Command execution safety guardrails, automated test verification hooks, and token-saving analytics with CLI proxies (`rtk`).

---

## 🛠️ How to Use

You can use these guides as reference inputs when instructing your AI assistant, or embed them directly into your agent configuration files (like `.cursorrules`, `.claudecoderc`, or system prompt files).

### 1. Referencing Guides in Chat (Cursor / Claude Code)
When editing or generating code in your IDE, refer the AI assistant directly to the appropriate guide using the `@` symbol or relative paths:
> *"Implement the new API endpoints using the rules in `@API_INTEGRATION_GUIDE.md`."*
> *"Update this card layout using the visual principles in `@DESIGN_POLISH_GUIDE.md`."*

### 2. Standardizing Rules (System Prompts)
Import or reference these guides in your system prompts to ensure that the agent follows these rules by default for every generated output.
