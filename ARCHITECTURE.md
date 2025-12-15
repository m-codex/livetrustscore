# SourceCheck Pro Architecture

This document provides a high-level overview of the technical architecture for the SourceCheck Pro application.

## Tech Stack

- **Frontend**: React, Next.js (App Router)
- **Language**: TypeScript
- **Styling**: Hero UI and Tailwind CSS
- **Backend**: Next.js Route Handlers (server-side)
- **Database**: Supabase (PostgreSQL)

## Project Structure

The project follows a standard Next.js `src` directory structure:

- `src/app/`: Contains the application's pages and API routes.
  - `src/app/page.tsx`: The main user-facing page.
  - `src/app/api/`: Contains all backend API endpoints.
    - `src/app/api/analyze/route.ts`: The core endpoint for URL analysis.
- `src/components/`: For shared, reusable React components.
- `src/lib/`: For shared utilities, helper functions, and business logic.
  - `src/lib/analysis/`: Specifically for the four analysis modules that will be developed.
- `src/types/`: To store custom TypeScript type definitions.
- `public/`: For static assets like images and fonts.

## Conventions

### File Naming

- **Components**: `PascalCase.tsx` (e.g., `UserProfile.tsx`)
- **Pages & Layouts**: `kebab-case/page.tsx` or `kebab-case/layout.tsx`
- **API Endpoints**: `kebab-case/route.ts`
- **Libraries & Utilities**: `camelCase.ts` (e.g., `cn.ts`)
- **Type Definitions**: `PascalCase.ts` (e.g., `AnalysisResult.ts`)

### Component Architecture

- **Server Components by Default**: To optimize for performance, all components should be Server Components unless they explicitly require client-side interactivity.
- **`"use client"` for Interactivity**: Only add the `"use client"` directive to components that need to use hooks (`useState`, `useEffect`, etc.) or attach event listeners.
- **Isolate Client Logic**: Keep Client Components as small and focused as possible. Pass data down from Server Components as props.

### Folder Responsibilities

- **`src/app`**: Core routing, pages, and layouts. Each route has its own folder.
- **`src/components`**: Shared, reusable UI components. Can be either Server or Client Components.
- **`src/lib`**: Non-component code, such as utility functions, data-fetching logic, and core business logic.
- **`src/types`**: Shared TypeScript type definitions and interfaces.

## High-Level Data Flow

The intended flow of data through the application is as follows:

1.  **Client (Browser)**: The user enters a URL into the form on the homepage (`src/app/page.tsx`) and submits it.
2.  **API Request**: The client sends a `POST` request containing the URL to the backend API endpoint at `/api/analyze`.
3.  **Backend (Next.js Route Handler)**: The `route.ts` handler at `src/app/api/analyze/` receives the request.
4.  **Analysis Pipeline (Future Implementation)**:
    - The backend will first scrape and sanitize the content of the provided URL.
    - It will then execute four distinct analysis modules located in `src/lib/analysis/`.
    - Each module will return a score or a set of metrics.
5.  **Score Calculation**: The results from the four modules are aggregated to calculate a final **LiveTrust Score** (0-100).
6.  **Database Storage (Future Implementation)**: The URL, the final score, and the breakdown from each analysis module are stored in a Supabase (PostgreSQL) database.
7.  **API Response**: The backend sends a JSON response to the client, containing the final LiveTrust Score and the detailed analysis results.
8.  **Client (Browser)**: The frontend receives the response and dynamically visualizes the score and the analysis breakdown for the user.

This architecture is designed to be scalable and maintainable, separating the frontend presentation, backend logic, and core analysis services.
