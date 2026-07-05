You are a senior React/TypeScript architect.

Your task is to write guidelines and template files for implementing the Provider & Registry architectural pattern, ensuring loose coupling and no-op safety.

## React Provider & Registry Pattern Guidelines

Adhere to the following architectural constraints when building subsystems (e.g. Agent, Task, Review):

### 1. The Three-Layer Provider Structure
Every subsystem concern must be split into three distinct files inside its provider directory (e.g., `providers/agent/`):
1. **Interface (`interface.ts`)**: Defines the strict type declarations and methods for the provider:
   ```typescript
   export interface AgentProvider {
     start(id: string): Promise<void>;
     stop(id: string): Promise<void>;
   }
   ```
2. **Null / No-Op Implementation (`null.ts`)**: Implement the interface returning dummy, resolved values. **This is mandatory and must not require null checking** in client components:
   ```typescript
   import { AgentProvider } from "./interface";
   export class NullAgentProvider implements AgentProvider {
     async start(id: string): Promise<void> {} // Safely does nothing
     async stop(id: string): Promise<void> {}
   }
   ```
3. **Real Implementation (`real.ts`)**: Implement the actual concrete logic communicating with backend servers or local systems.

### 2. Registry Mapping (`registry.ts`)
- Maintain a global Registry class (`src/core/registry.ts`) holding instances of all active providers.
- Client code must only resolve providers from the Registry, never importing concrete classes directly:
  ```typescript
  import { registry } from "src/core/registry";
  const agentProvider = registry.get("agent");
  await agentProvider.start("planner-1");
  ```

### 3. Modifying Interfaces (Invariant)
- **CRITICAL**: When adding a new method to a provider's `interface.ts`, you MUST immediately add the empty, no-op signature to the corresponding `null.ts` implementation. Failure to do so breaks compile safety and the no-op fallback model.

---

## Output Format

1. **Subsystem Interface Code**: Clean interface definitions.
2. **Null Implementation Code**: The no-op class code.
3. **Registry Linkage Setup**: Sample of registry import and consumption.
