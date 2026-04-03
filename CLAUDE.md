# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

Commonly used commands for development:

- **Start development server**: `npm run dev`  
  Runs Next.js dev server with turbopack at http://localhost:3000

- **Run in background**: `npm run dev:daemon`  
  Starts server and writes logs to logs.txt

- **Build for production**: `npm run build`  
  Creates optimized production build

- **Start production server**: `npm run start`  
  Runs the built application

- **Lint code**: `npm run lint`  
  Executes ESLint on TypeScript/JavaScript files

- **Run tests**: `npm run test`  
  Executes Vitest test suite

- **Run single test**: `npx vitest run path/to/test-file.test.{ts,tsx}`  
  Run specific test file

- **Setup project**: `npm run setup`  
  Installs dependencies, generates Prisma client, runs database migrations

- **Reset database**: `npm run db:reset`  
  Forces database reset (use with caution - deletes all data)

## Code Architecture & Structure

### High-Level Organization

```
src/
├── app/                   # Next.js App Router
│   ├── layout.tsx         # Root layout with fonts and globals
│   ├── page.tsx           # Home page (chat interface)
│   ├── [projectId]/       # Project-specific pages
│   │   └── page.tsx       # Editor interface for specific project
│   └── api/               # API routes
│       └── chat/route.ts  # Chat endpoint for AI generation
├── components/            # React components
│   ├── ui/                # Shadcn/ui primitives
│   ├── editor/            # File system editor components
│   ├── chat/              # Chat interface components
│   └── auth/              # Authentication components
├── lib/                   # Core libraries and utilities
│   ├── prisma.ts          # Prisma client singleton
│   ├── file-system.ts     # Virtual file system implementation
│   ├── transform/         # JSX/TSX transformation utilities
│   ├── contexts/          # React contexts (file-system, chat, auth)
│   ├── tools/             # Low-level utilities (str-replace, etc.)
│   ├── prompts/           # AI prompt templates
│   ├── provider.ts        # AI provider configuration
│   └── auth.ts            # Authentication utilities
├── actions/               # Server actions (Next.js 15)
│   ├── create-project.ts  # Project creation
│   ├── get-project.ts     # Fetch project data
│   └── get-projects.ts    # List user projects
├── hooks/                 # Custom React hooks
│   └── use-auth.ts        # Authentication hook
├── middleware.ts          # Next.js middleware
└── components.json        # Shadcn/ui configuration
```

### Key Architectural Patterns

1. **Virtual File System**: 
   - Files exist only in memory/storage, not on disk
   - Implemented in `src/lib/file-system.ts` with Prisma persistence
   - Provides file tree manipulation, content storage, and retrieval

2. **AI Component Generation**:
   - Uses Vercel AI SDK with Anthropic Claude
   - Prompt templates in `src/lib/prompts/generation.tsx`
   - Streaming responses handled in `src/app/api/chat/route.ts`
   - Transforms JSX/TSX strings to executable components via `src/lib/transform/`

3. **Editor Interface**:
   - Built with Monaco Editor (`@monaco-editor/react`)
   - File tree navigation in `src/components/editor/FileTree.tsx`
   - Code editing in `src/components/editor/CodeEditor.tsx`
   - Live preview in `src/components/preview/PreviewFrame.tsx`

4. **State Management**:
   - React Context for file system (`src/lib/contexts/file-system-context.tsx`)
   - Chat state (`src/lib/contexts/chat-context.tsx`)
   - Authentication state (`src/lib/contexts/auth-context.tsx` implied)

5. **Authentication**:
   - Supports anonymous usage and registered users
   - Session management via `jose` library
   - Protected routes via middleware

### Data Flow

1. User describes component in chat interface
2. Message sent to `/api/chat` route
3. Route calls AI provider with generation prompt
4. AI returns component code (JSX/TSX)
5. Code transformed and stored in virtual file system
6. Editor updates with new file
7. Preview frame renders component live

### Testing Approach

- Unit tests with Vitest and React Testing Library
- Tests located alongside source files (`*.test.tsx`/`*.test.ts`)
- Key test areas:
  - File system utilities (`src/lib/__tests__/`)
  - Editor components (`src/components/editor/__tests__/`)
  - Chat interface (`src/components/chat/__tests__/`)
  - Transformation logic (`src/lib/transform/__tests__/`)

### Environment Configuration

- `.env` file for environment variables
- Required: `ANTHROPIC_API_KEY` for AI features
- Optional without API key: falls back to static code generation
- Database: SQLite (managed by Prisma, file stored in `prisma/dev.db`)

### Styling & UI

- Tailwind CSS v4 for utility-first styling
- Shadcn/ui primitives in `src/components/ui/` (radix-based)
- Custom animations via `tw-animate-css`
- Dark/light mode support via Tailwind
- Global styles in `src/app/globals.css`

## Common Development Tasks

### Adding New Components
1. Create component in `src/components/ui/` following Shadcn/ui patterns
2. Export from `src/components/ui/index.ts` (if creating barrel)
3. Use in editor/preview components as needed

### Modifying AI Generation
1. Adjust prompts in `src/lib/prompts/generation.tsx`
2. Modify transformation logic in `src/lib/transform/jsx-transformer.ts`
3. Update AI model/provider in `src/lib/provider.ts`

### Database Changes
1. Edit `prisma/schema.prisma`
2. Run `npx prisma generate` to update client
3. Run `npx prisma migrate dev` to apply migrations
4. Update corresponding Prisma client usage

### Testing Specific Features
- File system: `npx vitest run src/lib/__tests__/file-system.test.ts`
- Editor: `npx vitest run src/components/editor/__tests__/file-tree.test.tsx`
- Chat: `npx vitest run src/components/chat/__tests__/ChatInterface.test.tsx`

## Important Files to Understand First

1. `src/app/page.tsx` - Main chat interface entry point
2. `src/lib/file-system.ts` - Core virtual file system logic
3. `src/lib/prompts/generation.tsx` - AI prompt engineering
4. `src/app/api/chat/route.ts` - AI response handling
5. `src/components/editor/` - Editor UI components
6. `src/components/preview/PreviewFrame.tsx` - Component rendering

This architecture enables rapid iteration on AI-generated components while maintaining type safety and persistence across sessions.