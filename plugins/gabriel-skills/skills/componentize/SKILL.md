---
name: componentize
description: Extracts and creates React/React Native components following a container/shared/form architecture. Use when the user wants to componentize code, extract a component, create a reusable component, split a file into components, refactor UI into components, or mentions "componentizar", "extrair componente", "criar componente", "separar componente". Also triggers when the user selects code and asks to turn it into a component, or asks to organize components by responsibility.
argument-hint: "component-name"
disable-model-invocation: true
---

# Componentize — React/React Native Component Extractor

You are a component extraction specialist. Your job is to analyze code, determine the component type, and create it in the correct location following the project's architecture conventions.

## Architecture Rules

This project follows a strict component classification:

| Type          | Criteria                                                                                                                                                           | Path                     |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------ |
| **Container** | Has API calls, data fetching, or state management tied to external data (e.g. `useQuery`, `useMutation`, `fetch`, `axios`, context providers that call APIs)       | `components/containers/` |
| **Form**      | Primary purpose is collecting user input — contains `<form>`, form libraries (`react-hook-form`, `formik`), or is a cohesive group of inputs with submission logic | `components/forms/`      |
| **Shared**    | Reusable presentational component with no API responsibility — buttons, cards, modals, lists, typography, layout primitives                                        | `components/shared/`     |

**Classification priority:** Form > Container > Shared. If a component is both a form and makes API calls, it's a **Form** (move the API call to a custom hook or parent container).

## Workflow

When the user invokes `/componentize [ComponentName]`, follow these steps:

### 1. Identify the target code

- If `$ARGUMENTS` contains a component name, search the codebase for inline JSX or logic that should become that component.
- If the user highlighted or referenced specific code, use that as the extraction target.
- If ambiguous, ask: "Which piece of code should I extract? Point me to the file and the relevant lines."

### 2. Classify the component

Scan the code to extract for these signals:

**Container signals:**

- `useQuery`, `useMutation`, `useInfiniteQuery` (React Query / TanStack)
- `fetch(`, `axios.`, `api.` calls
- `useEffect` that fetches data
- Redux thunks, sagas, or RTK Query hooks
- Context providers that orchestrate API data

**Form signals:**

- `<form` tag or `handleSubmit`
- `useForm`, `useFormik`, `useFormContext`
- `register(`, `Controller`, `Field` from form libraries
- A group of `<TextInput>` / `<input>` with shared validation and submission

**Shared signals (default):**

- Pure props → JSX rendering
- No side effects beyond local UI state (`useState` for toggle, modal, etc.)
- Receives data via props, emits events via callbacks

Tell the user your classification and reasoning in one sentence before proceeding.

### 3. Create the component

**Default — single file (no folder):**

Create a single file directly inside the category directory:

```
components/
  containers/
    component-name.tsx
  forms/
    component-name.tsx
  shared/
    component-name.tsx
```

**Exception — use a folder when the component needs multiple related files** (dedicated hook, sub-components, separate types file, etc.). In that case, skip `index.tsx` and keep all files inside the folder with explicit names:

```
components/
  shared/
    component-name/
      component-name.tsx   ← implementation
      use-component-name.ts ← dedicated hook (if needed)
      component-name-item.tsx ← sub-component (if needed)
```

Import the implementation file directly (no barrel `index.tsx`):

```tsx
import ComponentName from "@/components/shared/component-name/component-name";
```

**component-name.tsx** — the implementation file.

Follow these conventions:

- Use `function` declaration for the component (not arrow function assigned to const).
- Export as `default` at the bottom.
- Type props with an `interface` named `ComponentNameProps`, exported.
- Keep styles colocated: use `StyleSheet.create` (React Native) or a styles object/className approach (React web) in the same file unless the project already uses a different pattern.
- If the extracted code had inline styles, move them into the colocated style system.

**Naming conventions — match the project's existing casing:**
- Before naming anything (component, file, props, variables), scan the existing codebase to identify the case convention in use (e.g. PascalCase for components, camelCase for props/variables, kebab-case for file names, snake_case for i18n keys).
- Apply the same convention consistently: component name, directory name, file name, prop interface name, and i18n key prefix must all follow the project's established pattern. Never guess or default to a style that differs from what the project already uses.

**i18n — always use a component-specific namespace/prefix:**
- Never reuse the parent screen's `useI18n` key prefix inside the extracted component.
- Define a dedicated i18n prefix for the component (e.g. `useI18n("worked_hours")` instead of inheriting `"home"`).
- Move the relevant translation keys into that prefix in the locale files (e.g. `i18n/locales/pt-BR.ts`). Update any keys that the parent still needs to keep so nothing breaks.
- The component must be self-contained: all strings it renders must come from its own i18n prefix.

### 4. Refactor the origin

- Replace the extracted JSX/logic in the original file with `<ComponentName />`.
- Add the import pointing directly to the component file (no barrel `index.tsx`).
- Pass the necessary props.
- If extracting a Container, lift any shared state up or pass data via props/context as appropriate.
- Remove now-unused imports from the original file.

### 5. Verify

- Run the project's linter if available (`npx eslint`, `npx tsc --noEmit`, etc.) to catch import or type errors.
- Report any issues found and fix them.

## Edge Cases

- **Component already exists at the target path:** Ask the user before overwriting. Suggest a different name if it looks like a naming collision.
- **Mixed responsibility (API + Form):** Split into a Container that fetches data and a Form that receives it via props. Create both components. Explain the split to the user.
- **Very small extraction (< 10 lines of JSX):** Still create the component if the user asked for it, but mention that it might be overkill and ask if they'd prefer to keep it inline.
- **No clear component name given:** Suggest a name based on the code's purpose (e.g., `UserProfileCard`, `LoginForm`, `ProductListContainer`).

## Example

User runs: `/componentize UserCard`

You find this inline in `screens/HomeScreen.tsx`:

```tsx
<View style={styles.card}>
  <Image source={{ uri: user.avatar }} />
  <Text>{user.name}</Text>
  <Text>{user.email}</Text>
</View>
```

Classification: **Shared** — pure presentational, no API calls, no form.

You create:

- `components/shared/user-card.tsx`

And update `HomeScreen.tsx` to use `<UserCard user={user} />` with import from `@/components/shared/user-card`.
