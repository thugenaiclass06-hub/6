# NUWE Beauty - E-commerce Platform

## Overview

NUWE is a beauty e-commerce platform focused on natural, cloud-like makeup aesthetics. The application is a single-page application (SPA) built with React on the frontend and Express on the backend, featuring a PostgreSQL database for data persistence. The platform showcases beauty products, collects customer reviews, manages newsletter subscriptions, and handles contact form submissions.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework & Build Tools**
- React 18 with TypeScript for type-safe component development
- Vite as the build tool and development server for fast HMR and optimized production builds
- Wouter for lightweight client-side routing (single-page architecture with only a home page and 404 page)

**UI Component System**
- Shadcn UI component library configured with the "new-york" style preset
- Radix UI primitives for accessible, unstyled components
- Tailwind CSS for utility-first styling with custom design tokens
- Theme includes custom color palette (blue-toned with primary color at `hsl(205 65% 55%)`)
- Custom fonts: DM Sans (sans-serif) and Lora (serif for headings)

**State Management & Data Fetching**
- TanStack Query (React Query) for server state management
- Custom query client configured with infinite stale time and disabled auto-refetch
- Form handling with React Hook Form and Zod for validation

**Animation & Interactions**
- Framer Motion for declarative animations
- Embla Carousel for product carousels
- Custom animation utilities via `tw-animate-css`

### Backend Architecture

**Server Framework**
- Express.js with TypeScript running on Node.js
- HTTP server created via Node's native `http` module
- Middleware: JSON body parsing with raw body capture for webhook support
- Custom logging middleware for request/response tracking

**API Design**
- RESTful API endpoints under `/api` prefix:
  - `POST /api/contact` - Contact form submissions
  - `POST /api/newsletter` - Newsletter subscriptions
  - `POST /api/reviews` - Customer review submissions (with approval workflow)
- Zod schema validation for all incoming data
- Consistent error response format with user-friendly messages

**Build & Deployment**
- ESBuild for server-side bundling with selective dependency bundling (allowlist approach)
- Vite for client-side bundling
- Production build outputs to `dist/` directory
- Static file serving from `dist/public`
- SPA fallback routing (all unmatched routes serve index.html)

**Development Environment**
- Vite middleware mode for seamless development experience
- HMR (Hot Module Replacement) on custom path `/vite-hmr`
- Replit-specific plugins for error overlays, cartographer, and dev banners
- Custom meta images plugin for OpenGraph image management

### Data Storage

**Database**
- PostgreSQL via Neon serverless driver (`@neondatabase/serverless`)
- Connection string from `DATABASE_URL` environment variable

**ORM & Schema Management**
- Drizzle ORM for type-safe database queries
- Schema definition in `shared/schema.ts` using Drizzle's declarative API
- Database migrations stored in `migrations/` directory
- Drizzle Kit for schema management and migrations

**Data Models**
1. **Users** - Authentication (username/password)
   - ID (UUID), username (unique), password
   
2. **Contact Messages** - Customer inquiries
   - ID (UUID), name, email, subject, message, timestamp
   
3. **Newsletter Subscriptions** - Email marketing list
   - ID (UUID), email (unique), timestamp
   - Duplicate email checking before insertion
   
4. **Customer Reviews** - Product testimonials
   - ID (UUID), name, email, rating (1-5), content, approval status, timestamp
   - Approval workflow (reviews must be approved before public display)

**Storage Layer**
- Interface-based storage abstraction (`IStorage`)
- `DatabaseStorage` implementation using Drizzle queries
- Methods for creating, reading, and checking entities
- Separate methods for approved vs. all reviews

### External Dependencies

**Third-Party Services**
- Neon Database (PostgreSQL hosting)
- No authentication service integration (local password storage)
- No payment processing integration
- No email service integration (newsletter/contact forms store to database only)

**Asset Management**
- Static assets served from `client/public/` directory
- Generated images stored in `attached_assets/generated_images/`
- Image imports via Vite's asset handling with TypeScript path aliases
- Support for OpenGraph images (png/jpg/jpeg) with automatic URL generation

**Development Tools**
- Replit-specific integrations:
  - Runtime error modal plugin
  - Cartographer (code navigation)
  - Dev banner
  - Deployment URL detection for meta tags

**Key Libraries**
- Validation: Zod with zod-validation-error for friendly error messages
- Date handling: date-fns
- Utilities: clsx + tailwind-merge for className management, nanoid for unique IDs
- Icons: Lucide React

**Font Dependencies**
- Google Fonts: DM Sans and Lora via CDN link in index.html