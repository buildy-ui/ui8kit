# Monorepo Setup with Git Submodule (Turbo + Vite SWC + Bun)

This manual shows how to create a fresh monorepo that uses a Git submodule for UI libraries, run a working Vite app (Hello World) that imports `@ui8kit/core/Button`, and maintain or remove the submodule cleanly.

Important: Cloning a repository that contains submodules will produce an empty directory for the submodule until you run:

```bash
git submodule update --init --recursive
```

## Prerequisites

- Git 2.40+
- Bun 1.1.30 (recommended)
- macOS/Linux/Windows with terminal access

## Target Structure

```
your-monorepo/
  apps/
    dash/                   # Vite + React (SWC) app
  packages/
    @ui8kit/                # Git submodule: https://github.com/buildy-ui/ui8kit
      core/
      theme/
      hooks/
      form/
      flow/
      chat/
      brain/
```

## 1) Initialize the Monorepo

```bash
mkdir your-monorepo && cd your-monorepo
git init

echo node_modules> .gitignore
echo dist>> .gitignore
echo build>> .gitignore

mkdir -p apps/dash packages
```

## 2) Root Config Files

Create `package.json` in the repository root (aligns with provided example):

```json
{
  "name": "crm",
  "version": "0.0.1",
  "private": true,
  "license": "MIT",
  "type": "module",
  "author": {
    "name": "buildy",
    "url": "https://buildy.tw"
  },
  "workspaces": [
    "apps/*",
    "packages/*",
    "packages/@ui8kit/*"
  ],
  "scripts": {
    "dev": "bunx turbo run dev",
    "build": "bunx turbo run build",
    "test": "bunx turbo run test",
    "lint": "bunx turbo run lint",
    "dev:dash": "bunx turbo run dev --filter=./apps/dash",
    "build:dash": "bunx turbo run build --filter=./apps/dash"
  },
  "packageManager": "bun@1.1.30",
  "devDependencies": {
    "turbo": "^2.5.6"
  }
}
```

Create `turbo.json` in the repository root:

```json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "dev": {
      "cache": false,
      "persistent": true
    },
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**"]
    },
    "test": {
      "dependsOn": ["build"],
      "outputs": []
    },
    "lint": {
      "outputs": []
    }
  }
}
```

Create base `tsconfig.json` in the repository root (paths align with submodule contents):

```json
{
  "$schema": "https://json.schemastore.org/tsconfig",
  "display": "Base TypeScript Config",
  "compilerOptions": {
    "strict": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": true,
    "moduleResolution": "bundler",
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "preserveWatchOutput": true,
    "composite": true,
    "incremental": true,
    "baseUrl": ".",
    "paths": {
      "@ui8kit/core": ["./packages/@ui8kit/core/src"],
      "@ui8kit/core/*": ["./packages/@ui8kit/core/src/*"],
      "@ui8kit/theme": ["./packages/@ui8kit/theme/src"],
      "@ui8kit/theme/*": ["./packages/@ui8kit/theme/src/*"],
      "@ui8kit/hooks": ["./packages/@ui8kit/hooks/src"],
      "@ui8kit/hooks/*": ["./packages/@ui8kit/hooks/src/*"],
      "@ui8kit/form": ["./packages/@ui8kit/form/src"],
      "@ui8kit/form/*": ["./packages/@ui8kit/form/src/*"],
      "@ui8kit/flow": ["./packages/@ui8kit/flow/src"],
      "@ui8kit/flow/*": ["./packages/@ui8kit/flow/src/*"],
      "@ui8kit/chat": ["./packages/@ui8kit/chat/src"],
      "@ui8kit/chat/*": ["./packages/@ui8kit/chat/src/*"],
      "@ui8kit/brain": ["./packages/@ui8kit/brain/src"],
      "@ui8kit/brain/*": ["./packages/@ui8kit/brain/src/*"]
    }
  },
  "exclude": ["node_modules", "dist", "build"]
}
```

## 3) Add the `@ui8kit` Submodule

Place the UI kit as a Git submodule at `packages/@ui8kit`:

```bash
git submodule add https://github.com/buildy-ui/ui8kit.git packages/@ui8kit
git submodule update --init --recursive
```

Notes:

- If you cloned without `--recurse-submodules`, `packages/@ui8kit` will be empty until you run the update command above.
- To clone and populate submodules in one step, use: `git clone --recurse-submodules <repo-url>`.

## 4) Create the Vite App in `apps/dash`

Create `apps/dash/package.json` (aligned with the provided example):

```json
{
  "name": "@buildy/dash",
  "private": true,
  "version": "0.0.1",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "dev:crud": "cross-env VITE_ITEM_SCHEMA=crud vite",
    "dev:qdrant": "cross-env VITE_ITEM_SCHEMA=qdrant vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "@ui8kit/core": "workspace:*",
    "@ui8kit/form": "workspace:*",
    "@ui8kit/hooks": "workspace:*",
    "@ui8kit/theme": "workspace:*",
    "@ui8kit/flow": "workspace:*",
    "@ui8kit/chat": "workspace:*",
    "@dqbd/tiktoken": "^1.0.22",
    "@xyflow/react": "^12.8.4",
    "lucide-react": "^0.525.0",
    "openai": "^5.19.1",
    "react-day-picker": "^9.8.1",
    "react-resizable-panels": "^3.0.4",
    "react-router-dom": "^7.9.1",
    "zod": "^4.1.5"
  },
  "devDependencies": {
    "@types/react": "^19.1.0",
    "@types/react-dom": "^19.1.0",
    "@vitejs/plugin-react-swc": "^3.11.0",
    "autoprefixer": "^10.4.18",
    "class-variance-authority": "^0.7.0",
    "cross-env": "^10.0.0",
    "postcss": "^8.4.35",
    "react": "^19.1.0",
    "react-dom": "^19.1.0",
    "tailwind-merge": "^2.2.0",
    "tailwindcss": "^3.4.1",
    "typescript": "^5.5.3",
    "vite": "^5.4.1"
  }
}
```

Create `apps/dash/tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@ui8kit/core": ["../../packages/@ui8kit/core/src"],
      "@ui8kit/core/*": ["../../packages/@ui8kit/core/src/*"],
      "@ui8kit/theme": ["../../packages/@ui8kit/theme/src"],
      "@ui8kit/theme/*": ["../../packages/@ui8kit/theme/src/*"],
      "@ui8kit/hooks": ["../../packages/@ui8kit/hooks/src"],
      "@ui8kit/hooks/*": ["../../packages/@ui8kit/hooks/src/*"],
      "@ui8kit/form": ["../../packages/@ui8kit/form/src"],
      "@ui8kit/form/*": ["../../packages/@ui8kit/form/src/*"],
      "@ui8kit/flow": ["../../packages/@ui8kit/flow/src"],
      "@ui8kit/flow/*": ["../../packages/@ui8kit/flow/src/*"],
      "@ui8kit/chat": ["../../packages/@ui8kit/chat/src"],
      "@ui8kit/chat/*": ["../../packages/@ui8kit/chat/src/*"]
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

Create `apps/dash/vite.config.ts`:

```ts
import { defineConfig, loadEnv } from 'vite'
import react from '@vitejs/plugin-react-swc'
import path from 'path'

export default defineConfig(({ mode }) => {
  // loadEnv is imported but not used in this configuration, which is acceptable.
  // The 'mode' parameter is also not directly used in the returned config object, which is fine.

  return {
    plugins: [react()],
    resolve: {
      preserveSymlinks: true,
      dedupe: ['react', 'react-dom'],
      alias: {
        '@': path.resolve(__dirname, './src'),
        '@ui8kit/core': path.resolve(__dirname, '../../packages/@ui8kit/core/src'),
        '@ui8kit/theme': path.resolve(__dirname, '../../packages/@ui8kit/theme/src'),
        '@ui8kit/hooks': path.resolve(__dirname, '../../packages/@ui8kit/hooks/src'),
        '@ui8kit/form': path.resolve(__dirname, '../../packages/@ui8kit/form/src'),
        '@ui8kit/flow': path.resolve(__dirname, '../../packages/@ui8kit/flow/src'),
        '@ui8kit/chat': path.resolve(__dirname, '../../packages/@ui8kit/chat/src'),
      }
    },
    server: { port: 5000 }
      })(),
})
```

Create `apps/dash/src/main.tsx`:

```tsx
import React from 'react'
import { createRoot } from 'react-dom/client'
import App from './App'

createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

Create a simple Hello World using `@ui8kit/core/Button` at `apps/dash/src/App.tsx`:

```tsx
import { Button } from '@ui8kit/core'

export default function App() {
  return (
    <div style={{ padding: 24 }}>
      <h1>Hello World</h1>
      <Button>Click me</Button>
    </div>
  )
}
```

Create `apps/dash/index.html`:

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Dash</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
  </html>
```

## 5) Install and Run

From the monorepo root:

```bash
bun install

# If submodule dir is empty after clone:
git submodule update --init --recursive

# Start all dev tasks (Turbo)
bun run dev

# Or start only the dash app
bun run dev:dash
```

Open `http://localhost:5000` and you should see a page with a "Hello World" title and an `@ui8kit/core` `<Button>`.

## 6) Using the Submodule Like a Local Directory

- The submodule is used as if it were a regular directory inside the repo.
- Vite resolves the sources directly with `preserveSymlinks: true` and alias mapping to `../../packages/@ui8kit/*/src`.
- You can import components like normal:

```ts
import { Button } from '@ui8kit/core'
```

- From the app, the submodule path is reachable via `../../packages/@ui8kit/*`. From the repo root, it is `./packages/@ui8kit/*`.

## 7) Daily Submodule Operations

- Update submodule to the latest on its default branch:

```bash
cd packages/@ui8kit
git fetch
git checkout main
git pull --ff-only
cd ../../
git add packages/@ui8kit
git commit -m "chore(submodule): bump @ui8kit to latest"
```

- Pull monorepo and update submodules to recorded commits:

```bash
git pull --recurse-submodules
git submodule update --init --recursive
```

- See submodule status:

```bash
git submodule status --recursive
```

## 8) Remove the Submodule Cleanly

If you need to remove `packages/@ui8kit` completely from Git tracking and your working tree:

```bash
# 1) De-initialize from Git config
git submodule deinit -f packages/@ui8kit

# 2) Remove the submodule entry from the index and working tree
git rm -f packages/@ui8kit

# 3) Remove the internal Git data for the submodule
rm -rf .git/modules/packages/@ui8kit/

# 4) (Optional) Clean any remaining references in .gitmodules (if present)
sed -i.bak "/packages\/@ui8kit/,+2d" .gitmodules || true

# Commit the removal
git commit -m "chore: remove @ui8kit submodule"
```

What this accomplishes:
- Removed the submodule config from `.git/config` (via deinit)
- Deleted `.git/modules/packages/@ui8kit/` (internal Git data)
- Cleaned the Git index (removed submodule entry)
- Removed the working directory `packages/@ui8kit` (via `git rm`)

After this, `git submodule update --init --recursive` will work again, and the submodule is fully removed from version control.

## 9) Critical Notes and Best Practices for Git Submodules

- Clone correctly: Prefer `git clone --recurse-submodules` to avoid empty submodule directories.
- Commit the pointer: The parent repo tracks a specific commit of the submodule. When you update the submodule, commit the updated pointer in the parent repo.
- Avoid duplicate React copies: Ensure Vite `resolve.dedupe` includes `react` and `react-dom`, and set `preserveSymlinks: true`.
- Use workspace ranges: In the app `package.json`, depend on `@ui8kit/*` as `workspace:*` to use local sources directly.
- Align TS and Vite resolution: Mirror TS `paths` and Vite `alias` so IDE and bundler resolve the same sources.
- CI: Add `git submodule update --init --recursive` before install/build steps.
- Security: Submodules reference external repos. Pin to trusted branches/commits and review updates.
- Reproducibility: Use fixed Bun version (`bun@1.1.30`) and leverage Turbo for caching.

## 10) Quick Start (All Steps Together)

```bash
# 0) New repo
mkdir your-monorepo && cd your-monorepo && git init

# 1) Add configs (root package.json, turbo.json, tsconfig.json)
#    Create apps/dash and packages directories

# 2) Add submodule
git submodule add https://github.com/buildy-ui/ui8kit.git packages/@ui8kit
git submodule update --init --recursive

# 3) Create Vite app files under apps/dash (package.json, tsconfig.json, vite.config.ts, index.html, src/*)

# 4) Install and run
bun install
bun run dev:dash
```

## License

MIT


