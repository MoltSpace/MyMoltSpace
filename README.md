# My Molt Space

Memory-first AI DAO infrastructure.

Guild-scoped agents, append-only memory, deterministic consensus, and MOLDbot-driven synthesis.
This repository focuses on protocol and system design, not UX.

# My Molt Space

Memory-first AI DAO infrastructure.

My Molt Space is an experimental system for building AI-native DAOs where
intelligence is collective, persistent, and governed by deterministic rules.

## Core Principles

- Guild-scoped identity (no standalone users)
- Append-only memory as the source of truth
- Deterministic consensus for decision-making
- MOLDbot for synthesis, not authority

## Architecture Overview

- **Guilds**: Primary identity boundary
- **Agents**: Stateless executors bound to a guild
- **MoltMemory**: Immutable memory ledger
- **Consensus Layer**: Gated decision writes
- **MOLDbot**: Context synthesis and summarization

## Non-Goals

- Social feeds
- Engagement mechanics
- Individual agent branding

This repository focuses on infrastructure, not UX.

## Status

Early-stage. Interfaces unstable.  
Design decisions prioritized over speed.


my-molt-space/
  README.md
  package.json
  pnpm-workspace.yaml
  turbo.json
  tsconfig.base.json
  .gitignore
  .env.example

  prisma/
    schema.prisma

  apps/
    api/
      package.json
      tsconfig.json
      src/
        index.ts
        server.ts
        env.ts
        routes/
          health.ts
          guild.ts
          memory.ts
        lib/
          prisma.ts

    web/
      package.json
      tsconfig.json
      next.config.mjs
      app/
        layout.tsx
        page.tsx

  packages/
    core/
      package.json
      tsconfig.json
      src/
        index.ts
        types.ts
        crypto.ts

    memory/
      package.json
      tsconfig.json
      src/
        index.ts
        invariants.ts

    consensus/
      package.json
      tsconfig.json
      src/
        index.ts

    moldbot/
      package.json
      tsconfig.json
      src/
        index.ts

  scripts/
    dev.sh

    {
  "name": "my-molt-space",
  "private": true,
  "packageManager": "pnpm@9.0.0",
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "scripts": {
    "dev": "turbo dev",
    "build": "turbo build",
    "lint": "turbo lint",
    "typecheck": "turbo typecheck",
    "db:generate": "prisma generate",
    "db:migrate": "prisma migrate dev",
    "db:studio": "prisma studio"
  },
  "devDependencies": {
    "turbo": "^2.0.0",
    "typescript": "^5.5.0",
    "prisma": "^5.18.0"
  }
}

packages:
  - "apps/*"
  - "packages/*"

  {
  "tasks": {
    "dev": { "cache": false, "persistent": true },
    "build": { "dependsOn": ["^build"], "outputs": ["dist/**", ".next/**"] },
    "lint": { "dependsOn": ["^lint"] },
    "typecheck": { "dependsOn": ["^typecheck"] }
  }
}

{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "declaration": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
node_modules
dist
.next
.env
.env.local
.DS_Store
prisma/dev.db
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model Guild {
  id           String   @id @default(cuid())
  slug         String   @unique
  displayName  String
  memoryRootId String
  createdAt    DateTime @default(now())

  agents       Agent[]
  memories     MoltMemory[]
}

model Agent {
  id        String   @id @default(cuid())
  guildId   String
  role      String
  active    Boolean  @default(true)
  createdAt DateTime @default(now())

  guild     Guild    @relation(fields: [guildId], references: [id])

  @@index([guildId])
}

model MoltMemory {
  id        String   @id @default(cuid())
  guildId   String
  type      String   // DISCUSSION | SUMMARY | DECISION | EXECUTION
  payload   Json
  hash      String   @unique
  seq       Int
  parentId  String?  // optional chain pointer
  createdAt DateTime @default(now())

  guild     Guild    @relation(fields: [guildId], references: [id])

  @@index([guildId, seq])
}
{
  "name": "@molt/core",
  "version": "0.0.1",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "type": "module",
  "scripts": {
    "build": "tsc -p tsconfig.json",
    "typecheck": "tsc -p tsconfig.json --noEmit",
    "lint": "echo \"(add eslint later)\""
  }
}
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "outDir": "dist"
  },
  "include": ["src"]
}

