# Anzii: An Intelligent Spaced Repetition Learning System

> _"Memory is the treasury and guardian of all things."_ — Cicero

## 🚀 Quick Installation

### Prerequisites

- Node.js 18+ and pnpm
- PostgreSQL database (Neon recommended)
- Google Gemini API key

### Setup Instructions

1. **Clone and Install**

   ```bash
   git clone <repository-url>
   cd anzii
   pnpm install
   ```

2. **Environment Configuration**

   ```bash
   cp .env.example .env.local
   ```

   Configure your `.env.local`:

   ```env
   DATABASE_URL="postgresql://username:password@host/database"
   GEMINI_API_KEY="your-gemini-api-key"
   ```

3. **Database Setup**

   ```bash
   pnpm db:generate
   pnpm db:migrate
   ```

4. **Launch Application**

   ```bash
   pnpm build
   pnpm start
   ```

5. **Access the Application**
   - **Landing Page**: http://localhost:3000 (Marketing/showcase page)
   - **Dashboard**: http://localhost:3000/dashboard (Full learning application for authenticated users)

---

## 🎨 Application Architecture

Anzii is structured as a modern web application with clear separation between public marketing content and authenticated user features.

### Routing Structure

```
/ (Landing Page)          - Public marketing page showcasing features
/dashboard               - Authenticated learning dashboard
/create                  - Deck creation interface
/deck/[deckId]          - Individual deck management
```

**Navigation Flow:**

1. **Landing Page** (`/`) - First-time visitors see compelling marketing content
2. **Call-to-Action** - Multiple CTAs redirect users to the dashboard
3. **Dashboard** (`/dashboard`) - Main application where users manage decks and study
4. **Deep Features** - Specialized pages for deck creation and management

### Route Configuration

To ensure maintainability and consistency, all application routes are centralized in `src/lib/routes.ts`:

```typescript
// Centralized route definitions
export const ROUTES = {
  // Public routes
  HOME: "/",
  ABOUT_US: "/about-us",
  FEATURES: "/features", 
  PRICING: "/pricing",
  ROADMAP: "/roadmap",

  // Auth routes
  SIGN_IN: "/sign-in",
  SIGN_UP: "/sign-up",
  SETTINGS: "/settings",

  // Authenticated routes
  DASHBOARD: "/dashboard",
  CREATE: "/create",
  
  // Dynamic routes
  DECK: (deckId: string) => `/deck/${deckId}`,
} as const;
```

**Benefits:**
- **Single Source of Truth**: All routes defined in one place
- **Type Safety**: TypeScript ensures route consistency
- **Easy Refactoring**: Change routes globally by updating the configuration
- **Developer Experience**: Autocomplete and IDE support for all routes

**Usage Examples:**
```typescript
// In components
<Link href={ROUTES.DASHBOARD}>Dashboard</Link>

// In navigation
router.push(ROUTES.CREATE);

// Dynamic routes
const deckUrl = ROUTES.DECK(deckId);

// Server actions
revalidatePath(ROUTES.HOME);
```

### Landing Page

The landing page serves as the primary entry point, featuring a modern, conversion-focused design to showcase the power of AI-enhanced spaced repetition learning.

#### Key Features

- **Responsive Design**: Mobile-first approach with seamless adaptation across all device sizes
- **Interactive Components**: Scroll-triggered animations and parallax effects for engaging user experience
- **Modern UI/UX**: Built with shadcn/ui components following current design trends
- **Conversion Optimized**: Multiple CTAs, social proof, and trust signals strategically placed
- **Performance Focused**: Optimized animations using CSS transforms and Intersection Observer API

#### Landing Page Sections

1. **Hero Section**: Compelling headline with animated background and clear value proposition
2. **Problem/Solution**: Visual comparison between traditional learning and AI-powered spaced repetition
3. **How It Works**: Interactive 4-step process using tabs to explain the learning journey
4. **Features Showcase**: Grid of feature cards with hover animations and benefit highlights
5. **Proven Results**: Animated statistics showing learning improvements and user satisfaction
6. **Testimonials**: Real user feedback with star ratings and professional credentials
7. **Final CTA**: Email capture form with trial benefits and trust indicators

#### Technical Implementation

- **Animations**: Custom scroll-triggered animations with Intersection Observer
- **Parallax Effects**: Smooth parallax scrolling for enhanced visual depth
- **Component Architecture**: Modular design with reusable animated components
- **Performance**: GPU-accelerated animations using `transform3d()` and `will-change`
- **Accessibility**: Respects `prefers-reduced-motion` for users who prefer less animation
- **Navigation**: Seamless routing to dashboard using Next.js App Router

### Dashboard

The dashboard provides the core learning experience with deck management, AI-powered content generation, and spaced repetition study sessions. This is where authenticated users spend most of their time, managing their learning materials and tracking progress.

---

## Abstract

This research presents **Anzii**, a next-generation flashcard application that leverages artificial intelligence and scientifically-proven spaced repetition algorithms to optimize long-term memory retention. By integrating cognitive science principles with modern web technologies, Anzii addresses the fundamental challenge of efficient knowledge acquisition in an increasingly information-dense world.

The system employs a sophisticated implementation of the SM-2 algorithm, enhanced with AI-powered content generation capabilities and an adaptive user interface that responds to individual learning patterns. Through empirical testing of memory consolidation theories, this application demonstrates significant improvements in learning efficiency compared to traditional study methods.

---

## 🧠 Research Methodology

### Spaced Repetition Algorithm Implementation

The core of Anzii's effectiveness lies in its implementation of Hermann Ebbinghaus's forgetting curve theory, operationalized through the SuperMemo SM-2 algorithm:

```mermaid
graph TD
    A[Card Review] --> B{Performance Rating}
    B -->|Easy 5| C[EF += 0.15<br/>Interval *= 2.5]
    B -->|Good 4| D[EF += 0.13<br/>Interval *= 2]
    B -->|Medium 3| E[EF unchanged<br/>Interval *= 1.3]
    B -->|Hard 2| F[EF -= 0.15<br/>Interval = 1 day]
    B -->|Again 1| G[EF -= 0.2<br/>Reset to start]

    C --> H[Schedule Next Review]
    D --> H
    E --> H
    F --> H
    G --> H

    H --> I[Next Review Date = <br/>Current Date + Interval]

    style A fill:#e1f5fe
    style H fill:#f3e5f5
    style I fill:#e8f5e8
```

### System Architecture

The application follows a modular, scalable architecture designed for optimal performance and maintainability:

```mermaid
graph TB
    subgraph "Frontend Layer"
        A[Next.js App Router]
        B[React Components]
        C[Custom Hooks]
        D[Theme System]
    end

    subgraph "Business Logic"
        E[Server Actions]
        F[SRS Algorithm]
        G[AI Integration]
    end

    subgraph "Data Layer"
        H[Drizzle ORM]
        I[PostgreSQL Database]
    end

    subgraph "External Services"
        J[Google Gemini AI]
        K[Neon Database]
    end

    A --> B
    B --> C
    B --> D
    B --> E
    E --> F
    E --> G
    E --> H
    H --> I
    G --> J
    I --> K

    style A fill:#e3f2fd
    style F fill:#fff3e0
    style G fill:#f1f8e9
    style I fill:#fce4ec
```

### User Experience Flow

```mermaid
journey
    title Learning Journey in Anzii
    section Discovery
      Open App: 5: User
      Choose Theme: 4: User
      Browse Decks: 4: User
    section Creation
      Generate AI Deck: 5: User, AI
      Import Markdown: 4: User, AI
      Manual Creation: 3: User
    section Learning
      Start Review: 5: User
      Answer Cards: 4: User
      Rate Difficulty: 5: User, Algorithm
      Optimistic Updates: 5: System
    section Progress
      View Statistics: 4: User
      Track Retention: 5: Algorithm
      Celebrate Growth: 5: User
```

---

## 🎯 Key Innovations

### 1. **AI-Powered Content Generation**

- **Gemini Integration**: Leverages Google's advanced language model for intelligent flashcard creation
- **Context-Aware Processing**: Transforms any text content into optimized question-answer pairs
- **Topic Expansion**: Generates comprehensive decks from simple topic prompts

### 2. **Optimistic UI Architecture**

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Queue
    participant Backend
    participant Database

    User->>Frontend: Rate Card (Hard/Medium/Easy)
    Frontend->>Frontend: Update UI Immediately
    Frontend->>Queue: Add to Pending Reviews
    Note over Frontend: User continues seamlessly
    Queue->>Backend: Batch Process (1s debounce)
    Backend->>Database: Update Card Statistics
    Backend->>Frontend: Sync Complete
    Frontend->>Frontend: Remove "Syncing..." indicator
```

### 3. **Adaptive Theme System**

- **17 Curated Themes**: From minimalist designs to vibrant cyberpunk aesthetics
- **Next.js Themes Integration**: Seamless light/dark mode switching with system preference detection
- **Stack Auth Theming**: Automatic theme sync between your app and authentication components
- **CSS Custom Properties**: Dynamic theme switching without page reloads
- **Accessibility Focus**: High contrast ratios and color-blind friendly palettes

---

## 🔬 Technical Implementation

### Core Technologies

- **Frontend**: Next.js 14, React 18, TypeScript
- **Styling**: Tailwind CSS, Radix UI Components
- **Database**: PostgreSQL with Drizzle ORM
- **AI**: Google Gemini Pro API
- **Deployment**: Vercel with Neon PostgreSQL

### Algorithm Precision

The SM-2 implementation includes several optimizations:

```typescript
// Ease Factor calculation with bounds checking
const newEaseFactor = Math.max(
	1.3,
	card.easeFactor + (0.1 - (5 - rating) * (0.08 + (5 - rating) * 0.02))
);

// Interval progression with exponential growth
const newInterval = rating >= 3 ? Math.ceil(card.interval * newEaseFactor) : 1; // Reset on failure
```

### Performance Optimizations

- **Parallel Processing**: Batch API requests for optimal sync performance
- **Debounced Updates**: 1-second delay prevents excessive server calls
- **Optimistic Updates**: Zero-latency user feedback during reviews

### Component Architecture

The component system follows a clean architecture pattern with strict adherence to coding standards:

#### Clean Code Principles
- **Size Limit**: Components do not exceed 50 lines of code
- **Props Limit**: Components have maximum 3 props for simplicity  
- **Nesting Limit**: JSX nesting does not exceed 3 levels deep
- **State Management**: Multiple useState calls use custom hooks or useReducer

#### Component Organization
- **UI Components** (`/ui`): Base shadcn/ui components for consistent design
- **Layout Components** (`/layout`): Navigation, headers, and page structure
- **Section Components** (`/sections`): Large page sections like landing pages
- **Feature Components** (`/features`): Grouped by domain (AI, deck management, study)
- **Common Components** (`/common`): Shared utilities across features

#### Landing Page Architecture

The landing page has been refactored into modular components for maintainability:

```
src/components/sections/landing/
├── hero-section.tsx          # Hero with CTA
├── stats-section.tsx         # Trust indicators
├── services-section.tsx      # Feature highlights
├── cta-section.tsx          # Call-to-action
├── process-section.tsx      # How it works
├── team-section.tsx         # Team showcase
├── testimonials-section.tsx # Social proof
└── index.ts                 # Barrel exports
```

**Supporting Files:**
- `src/data/landing-data.ts` - Static data extraction
- `src/hooks/useLandingPageState.ts` - State management hook

This modular approach enables:
- **Reusability**: Components can be used across different pages
- **Testability**: Individual sections can be tested in isolation
- **Maintainability**: Easy to update specific sections
- **Performance**: Better code splitting and loading

---

## 📊 Usage Patterns

### Creating Content

1. **AI Generation**: Input topics → AI creates comprehensive decks
2. **Markdown Import**: Paste notes → AI extracts key concepts
3. **Manual Creation**: Traditional question-answer pair entry
4. **JSON Import**: Bulk import from external systems

### Study Sessions

- **Intelligent Scheduling**: Only due cards appear for review
- **Instant Feedback**: Immediate UI response to user interactions
- **Progress Tracking**: Visual indicators show learning momentum
- **Manual Session Control**: Users remain on completion screen and manually choose when to return to dashboard

### Customization

- **Theme Selection**: Choose from 17 carefully crafted color schemes
- **Deck Management**: Organize, rename, and reset progress as needed

---

## 🔧 Troubleshooting

### Common Issues

#### Dependency Override Conflicts

**Error Message:**

```
npm ERR! code EOVERRIDE
npm ERR! Override for @radix-ui/react-slot@^X.X.X conflicts with direct dependency
```

**Cause:** Package overrides can conflict with direct dependencies when versions don't align.

**Solution:** Remove conflicting overrides and clean reinstall:

```bash
# Remove the overrides section from package.json if it conflicts
rm -rf node_modules package-lock.json pnpm-lock.yaml
pnpm install
```

**Prevention:** Only use package overrides when absolutely necessary for compatibility issues. Modern Radix UI packages generally work well together without overrides.

#### Deck Reset Loading State

**Feature:** When users restart deck learning progress, a loading indicator is shown on the restart button to provide immediate feedback.

**Implementation Details:**

- The restart button shows a spinning loader icon and "Restarting..." text during the reset operation
- The button is disabled only during the reset process to prevent multiple operations
- Loading state is tracked per-deck, allowing users to interact with other decks while one is being reset
- Toast notifications confirm successful reset or display error messages
- **Fixed Issue**: Restart button now works correctly when all cards are due for review (e.g., 16/16 cards due)
- Simplified logic ensures restart button is only disabled during active reset operations

**User Experience:**

1. User clicks "Restart" button on a deck
2. Confirmation dialog appears asking to confirm the reset
3. Upon confirmation, button shows loading spinner and "Restarting..." text
4. Button is disabled until the operation completes
5. Success/error toast message appears
6. Button returns to normal state and deck list refreshes

**Process Flow:**

```mermaid
sequenceDiagram
    participant User
    participant DeckList as "DeckList Component"
    participant Hook as "useDeckManagement Hook"
    participant Action as "resetDeckProgress Action"
    participant Database

    User->>DeckList: Click "Restart" button
    DeckList->>User: Show confirmation dialog
    User->>DeckList: Confirm reset
    DeckList->>Hook: Call handleResetDeck(deckId)
    Hook->>Hook: Set resetLoadingDeckId = deckId
    Hook->>DeckList: Update loading state
    DeckList->>User: Show "Restarting..." with spinner
    Hook->>Action: Call resetDeckProgress(deckId)
    Action->>Database: Reset all cards in deck
    Database->>Action: Return success/error result
    Action->>Hook: Return result
    Hook->>Hook: Set resetLoadingDeckId = null
    Hook->>DeckList: Clear loading state
    Hook->>User: Show toast notification
    DeckList->>User: Button returns to normal state
```

---

## 🎓 Conclusion

Anzii represents a synthesis of cognitive science research and modern software engineering, creating a learning tool that adapts to human memory patterns rather than forcing users to adapt to rigid systems. By combining the proven effectiveness of spaced repetition with the creative potential of artificial intelligence, this application offers a glimpse into the future of personalized education technology.

The modular architecture ensures scalability, while the focus on user experience guarantees that learning remains engaging and effective. As Hermann Ebbinghaus demonstrated over a century ago, the key to lasting memory lies not in repetition alone, but in _intelligent_ repetition—and Anzii embodies this principle in every interaction.

---

## 📝 License

MIT License - Feel free to learn, modify, and contribute to the advancement of educational technology.

---

_Built with 🧠 for learners who believe that knowledge, properly organized, can change the world._

## 🏗️ Architecture

### Project Structure

```
anzii/
├── src/
│   ├── app/                          # Next.js 13+ App Router
│   │   ├── (auth)/                   # Auth group routes
│   │   │   ├── sign-in/              # Sign in page
│   │   │   └── sign-up/              # Sign up page
│   │   ├── about-us/                 # About us page
│   │   ├── roadmap/                  # Product roadmap page  
│   │   ├── create/                   # Deck creation page
│   │   ├── dashboard/                # Main dashboard (auth required)
│   │   ├── deck/[deckId]/           # Individual deck study page
│   │   ├── features/                 # Features page
│   │   ├── pricing/                  # Pricing page
│   │   ├── handler/[...stack]/      # Stack Auth handler
│   │   ├── page.tsx                 # Landing page (home)
│   │   ├── layout.tsx               # Root layout with providers
│   │   └── globals.css              # Global styles
│   ├── components/                   # React components
│   │   ├── ui/                      # shadcn/ui components
│   │   ├── layout/                  # Layout components (header, logo)
│   │   ├── sections/                # Page sections (landing, welcome)
│   │   ├── features/                # Feature-specific components
│   │   │   ├── ai/                  # AI-related components
│   │   │   ├── deck/                # Deck management components
│   │   │   ├── study/               # Study session components
│   │   │   └── import/              # Import functionality
│   │   └── common/                  # Shared/utility components
│   ├── lib/                         # Utility functions and configurations
│   ├── hooks/                       # Custom React hooks
│   ├── db/                          # Database schema and utilities
│   ├── ai/                          # AI/ML related code
│   └── types/                       # TypeScript type definitions
├── drizzle/                         # Database migrations
└── docs/                           # Documentation
```

### Component Architecture

The component system follows a clean architecture pattern with strict adherence to coding standards:

#### Clean Code Principles
- **Size Limit**: Components do not exceed 50 lines of code
- **Props Limit**: Components have maximum 3 props for simplicity  
- **Nesting Limit**: JSX nesting does not exceed 3 levels deep
- **State Management**: Multiple useState calls use custom hooks or useReducer

#### Component Organization
- **UI Components** (`/ui`): Base shadcn/ui components for consistent design
- **Layout Components** (`/layout`): Navigation, headers, and page structure
- **Section Components** (`/sections`): Large page sections like landing pages
- **Feature Components** (`/features`): Grouped by domain (AI, deck management, study)
- **Common Components** (`/common`): Shared utilities across features

#### Landing Page Architecture

The landing page has been refactored into modular components for maintainability:

```
src/components/sections/landing/
├── hero-section.tsx          # Hero with CTA
├── stats-section.tsx         # Trust indicators
├── services-section.tsx      # Feature highlights
├── cta-section.tsx          # Call-to-action
├── process-section.tsx      # How it works
├── team-section.tsx         # Team showcase
├── testimonials-section.tsx # Social proof
└── index.ts                 # Barrel exports
```

**Supporting Files:**
- `src/data/landing-data.ts` - Static data extraction
- `src/hooks/useLandingPageState.ts` - State management hook

This modular approach enables:
- **Reusability**: Components can be used across different pages
- **Testability**: Individual sections can be tested in isolation
- **Maintainability**: Easy to update specific sections
- **Performance**: Better code splitting and loading

### Routing Structure

The application uses Next.js 13+ App Router with the following routes:

- `/` - Landing page with marketing content
- `/dashboard` - Main application dashboard (requires authentication)
- `/about-us` - Company information, team, mission, and values
- `/features` - Detailed feature descriptions and comparisons
- `/pricing` - Pricing plans with annual/monthly toggle
- `/roadmap` - Product development roadmap and upcoming features
- `/create` - Deck creation interface
- `/deck/[deckId]` - Study session for specific deck
- `/sign-in` - Authentication sign in
- `/sign-up` - Authentication sign up

### Theme System

The application features a unified design system with:

- **Color Scheme**: Black background with lime green (`#a3e635`) accents
- **Typography**: Responsive text sizing with mobile-first approach
- **Components**: Consistent card styling with rounded corners and shadow effects
- **Theme Integration**: Next-themes integration with Stack Auth theming
- **Responsive Design**: Mobile-first breakpoints (sm, md, lg, xl)

All pages follow the same design language for a cohesive user experience.
