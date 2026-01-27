# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

UIGen is an AI-powered React component generator web application that uses Anthropic Claude to generate React components with live preview. Users can chat with the AI to create, modify, and iterate on UI components rendered in a live preview frame.

## Development Commands

```bash
npm run dev          # Start Next.js dev server with Turbopack
npm run dev:daemon   # Start dev server in background (logs to logs.txt)
npm run build        # Production build
npm run start        # Start production server
npm run lint         # Run ESLint
npm run test         # Run Vitest tests
npm run setup        # Install deps, generate Prisma client, run migrations
npm run db:reset     # Reset Prisma database (warning: destructive)
```

## Architecture

### Core Patterns

- **Server Actions**: All data mutations go through `src/actions/*.ts` for type-safe server-side operations
- **Path Aliases**: Use `@/*` imports (configured in tsconfig.json) - maps to `src/` directory
- **AI Integration**: Uses Vercel AI SDK (`@ai-sdk/anthropic`) for Claude integration with streaming responses

### Key Directories

- `src/app/` - Next.js App Router pages and API routes
- `src/components/` - React components organized by feature (auth, chat, editor, preview, ui)
- `src/lib/` - Core utilities:
  - `auth.ts` - JWT authentication
  - `file-system.ts` - Virtual file system implementation
  - `contexts/` - React contexts (FileSystem, Chat)
  - `tools/` - File manipulation tools for AI
  - `prompts/` - AI prompt templates

### Database

- Uses SQLite with Prisma ORM
- Schema defined in `prisma/schema.prisma` with User and Project models
- Run `npm run setup` after pulling changes to sync migrations

### AI Tool Usage

The AI has access to file manipulation tools defined in `src/lib/tools/`. These tools allow the AI to:
- Create, read, update, and delete files in the virtual file system
- Execute tests and report results
- When prompting for component modifications, consider what files need to be updated and in what order

## Tech Stack

- Next.js 15 with App Router
- TypeScript 5
- React 19 with Tailwind CSS v4 and shadcn/ui
- Vercel AI SDK with Anthropic Claude
- Monaco Editor for code editing
- Babel for in-browser JSX transformation (live preview)
