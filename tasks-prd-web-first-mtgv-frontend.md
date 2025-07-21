# Task List: Web-First MTGV Frontend

## Design Pattern: Real-Time Package State Management

### Architecture Overview
- **Backend as Source of Truth**: All persistent package state managed by API
- **WebSocket Real-Time Sync**: Frontend connects via WebSocket for immediate updates
- **REST API for Initial Load**: Page reloads restore state from REST endpoints
- **Debounced Updates**: Prevent excessive WebSocket traffic during rapid changes

### State Flow
1. User creates/modifies card list → WebSocket syncs to backend
2. User selects card versions → WebSocket syncs selections to backend  
3. User exports → REST API call (doesn't finalize package)
4. Page reload → REST API loads current state, WebSocket reconnects

### WebSocket Events
- `join-package`: Client joins package room for real-time updates
- `update-card-list`: Sync card list changes to backend
- `update-version-selection`: Sync version selection changes to backend
- `card-list-updated`: Broadcast card list changes to all clients
- `version-selection-updated`: Broadcast version selection changes to all clients

## Relevant Files
- `mtgv-web/next.config.js` - Next.js configuration for image optimization and API routes
- `mtgv-web/tsconfig.json` - TypeScript configuration
- `mtgv-web/tailwind.config.js` - Tailwind CSS configuration for responsive design
- `mtgv-web/src/app/layout.tsx` - Root layout component
- `mtgv-web/src/app/page.tsx` - Main page component
- `mtgv-web/src/components/CardInput.tsx` - Card input interface with autocomplete
- `mtgv-web/src/components/CardInput.test.tsx` - Unit tests for CardInput component
- `mtgv-web/src/components/CardDisplay.tsx` - Card display interface with version selection
- `mtgv-web/src/components/CardDisplay.test.tsx` - Unit tests for CardDisplay component
- `mtgv-web/src/components/CardVersion.tsx` - Individual card version display component
- `mtgv-web/src/components/CardVersion.test.tsx` - Unit tests for CardVersion component
- `mtgv-web/src/lib/api.ts` - API service layer for MTGV API integration
- `mtgv-web/src/lib/api.test.ts` - Unit tests for API service
- `mtgv-web/src/lib/imageCache.ts` - Image caching service for Scryfall images
- `mtgv-web/src/lib/imageCache.test.ts` - Unit tests for image caching
- `mtgv-web/src/lib/validation.ts` - Input validation utilities
- `mtgv-web/src/lib/validation.test.ts` - Unit tests for validation utilities
- `mtgv-web/src/types/index.ts` - TypeScript type definitions
- `mtgv-web/src/hooks/useCardAutocomplete.ts` - Custom hook for card autocomplete
- `mtgv-web/src/hooks/useCardAutocomplete.test.ts` - Unit tests for autocomplete hook
- `mtgv-web/src/hooks/useCardPackage.ts` - Custom hook for card package management
- `mtgv-web/src/hooks/useCardPackage.test.ts` - Unit tests for card package hook
- `mtgv-web/src/constants/config.ts` - Configuration constants (API URLs, etc.)
- `mtgv-web/src/app/api/proxy/route.ts` - API proxy route for MTGV API calls
- `mtgv-web/src/app/api/proxy/route.test.ts` - Unit tests for API proxy
- `mtgv-web/jest.config.js` - Jest testing configuration
- `mtgv-web/.eslintrc.js` - ESLint configuration
- `mtgv-web/.prettierrc.js` - Prettier configuration
- `mtgv-web/public/manifest.json` - PWA manifest for mobile functionality
- `mtgv-web/public/sw.js` - Service worker for caching and offline support
- `mtgv-api/src/middleware/validateParams.js` - Middleware for validating API request parameters (now enforces 100-card limit in card list validation)

### Notes

- Unit tests should typically be placed alongside the code files they are testing (e.g., `CardInput.tsx` and `CardInput.test.tsx` in the same directory).
- Use `npm test` to run tests. Running without a path executes all tests found by the Jest configuration.
- The project will be created in a directory parallel to `mtgv-api` called `mtgv-web`.
- Next.js App Router structure will be used for modern React development.

## Tasks

- [x] 1.0 Project Setup and Configuration
  - [x] 1.1 Initialize Next.js project with TypeScript and Tailwind CSS
  - [x] 1.2 Install and configure essential dependencies (axios, react-hook-form, etc.)
  - [x] 1.3 Set up project structure with src/ directory organization
  - [x] 1.4 Configure Next.js for image optimization and API routes
  - [x] 1.5 Set up Jest testing configuration for Next.js
  - [x] 1.6 Configure TypeScript with proper type definitions
  - [x] 1.7 Set up ESLint and Prettier for code quality
  - [x] 1.8 Configure Tailwind CSS for responsive design

- [x] 2.0 Card Input Interface Implementation
  - [x] 2.1 Create CardInput component with text input field
  - [x] 2.2 Implement card quantity input functionality (default: 1)
  - [x] 2.3 Create useCardAutocomplete hook for card name suggestions
  - [x] 2.4 Integrate autocomplete with MTGV API's MongoDB card database
  - [x] 2.5 Add card list management (add/remove cards from input list)
  - [x] 2.6 Implement input validation for card names and quantities
  - [x] 2.7 Centralize card name sanitization & combine it with input validation
  - [x] 2.8 Add 100-card limit enforcement and validation
  - [x] 2.9 Add loading states and error handling for autocomplete
  - [x] 2.10 Create unit tests for CardInput component and autocomplete hook

- [x] 2.1 Caching Behavior Refactoring
  - [x] 2.1.1 Analyze current caching behavior across the application
  - [x] 2.1.2 Refactor Card model to centralize all card querying by name
  - [x] 2.1.3 Implement unified caching strategy for card lookups
  - [x] 2.1.4 Add caching for autocomplete card lookup functionality
  - [x] 2.1.5 Remove redundant caching implementations
  - [x] 2.1.6 Optimize cache invalidation and TTL strategies
  - [x] 2.1.7 Add cache monitoring and performance metrics
  - [x] 2.1.8 Create unit tests for new caching behavior
  - [x] 2.1.9 Update documentation for caching architecture

- [x] 2.2 Game Selection Simplification
  - [x] 2.2.1 Update API to accept single game type instead of array
  - [x] 2.2.2 Modify CardPackageCreator to handle single game parameter
  - [x] 2.2.3 Update Card model queries to use single game filter
  - [x] 2.2.4 Create GameSelector component with toggle between paper/mtgo/arena
  - [x] 2.2.5 Set paper as default game type
  - [x] 2.2.6 Update API validation to enforce single game selection
  - [x] 2.2.7 Update package cache keys to use single game
  - [x] 2.2.8 Update tests to reflect single game selection
  - [x] 2.2.9 Update documentation for simplified game selection

- [x] 3.0 API Integration and Card Package Creation
  - [x] 3.1 Create API service layer with axios configuration
  - [x] 3.2 Implement API proxy route for MTGV API calls
    - [x] 3.2.1 Implement API proxy route for card package route
    - [x] 3.2.2 Implement API proxy route for random card package route
    - [x] 3.2.3 Implement API proxy route for card package export route
  - [x] 3.3 Implement `/card_package` POST endpoint integration
  - [x] 3.4 Add game type selection (paper, mtgo, arena) with paper default
  - [x] 3.5 Add default selection preferences (oldest, newest, most_expensive, least_expensive)
  - [x] 3.6 Create useCardPackage hook for managing card package state
  - [x] 3.6.1 Implement WebSocket-based real-time package state management
  - [x] 3.6.2 Add backend WebSocket server for package updates
  - [x] 3.6.3 Create package session management (join/leave package rooms)
  - [x] 3.6.4 Implement real-time card list updates via WebSocket
  - [x] 3.6.5 Implement real-time version selection updates via WebSocket
  - [x] 3.6.6 Add debounced updates to prevent excessive WebSocket traffic
  - [x] 3.6.7 Handle WebSocket reconnection and state recovery
  - [x] 3.6.8 Ensure package state persists across page reloads
  - [x] 3.6.9 Create unit tests for WebSocket package management
  - [x] 3.7 Implement API error handling and validation error display
  - [x] 3.8 Add loading states during API calls
  - [x] 3.9 Create unit tests for API service and card package hook

- [ ] 4.0 Card Display Interface and Version Selection
  - [ ] 4.1 Create CardDisplay component for showing card package results
  - [ ] 4.2 Create CardVersion component for individual card version display
  - [ ] 4.3 Implement card image loading from Scryfall with fallback handling
  - [ ] 4.4 Add card version selection functionality (radio buttons or similar)
  - [ ] 4.5 Display card prices and metadata for each version
  - [ ] 4.6 Handle both black-bordered and white-bordered card images appropriately
  - [ ] 4.7 Implement smooth scrolling for large card lists
  - [ ] 4.8 Add visual feedback for selected card versions
  - [ ] 4.9 Create unit tests for CardDisplay and CardVersion components

- [ ] 5.0 Export Functionality Implementation
  - [ ] 5.1 Implement `/card_package/export` POST endpoint integration
  - [ ] 5.2 Add TCGPlayer export format with direct URL forwarding
  - [ ] 5.3 Add text export format with copy-to-clipboard functionality
  - [ ] 5.4 Create export buttons and UI for both export types
  - [ ] 5.5 Handle browser clipboard operations for text export
  - [ ] 5.6 Add export success/error feedback to users
  - [ ] 5.7 Implement export data validation before API calls
  - [ ] 5.8 Create unit tests for export functionality

- [ ] 6.0 Random Package Generation Feature
  - [ ] 6.1 Implement `/card_package/random` GET endpoint integration
  - [ ] 6.2 Add random package count input interface
  - [ ] 6.3 Create random package generation button and UI
  - [ ] 6.4 Handle random package responses identically to created packages
  - [ ] 6.5 Add loading states for random package generation
  - [ ] 6.6 Integrate random package with existing card display interface
  - [ ] 6.7 Create unit tests for random package functionality

- [ ] 7.0 Responsive Design and Mobile Optimization
  - [ ] 7.1 Implement responsive layout for desktop, tablet, and mobile
  - [ ] 7.2 Add touch-friendly interactions for mobile devices
  - [ ] 7.3 Optimize card grid layout for different screen sizes
  - [ ] 7.4 Implement mobile-friendly navigation and UI patterns
  - [ ] 7.5 Add responsive typography and spacing
  - [ ] 7.6 Test responsive design across different devices and browsers
  - [ ] 7.7 Create unit tests for responsive components

- [ ] 8.0 Performance Optimization and Image Caching
  - [ ] 8.1 Implement efficient image caching service using Next.js Image component
  - [ ] 8.2 Configure image optimization and lazy loading
  - [ ] 8.3 Add image preloading for better user experience
  - [ ] 8.4 Implement virtual scrolling for large card lists
  - [ ] 8.5 Optimize image loading to reduce load on Scryfall
  - [ ] 8.6 Add image loading placeholders and error states
  - [ ] 8.7 Monitor and optimize memory usage for large image lists
  - [ ] 8.8 Create unit tests for image caching service

- [ ] 9.0 Deployment and Testing
  - [ ] 9.1 Configure deployment for Render hosting platform
  - [ ] 9.2 Set up environment variables and configuration
  - [ ] 9.3 Test app functionality in production environment
  - [ ] 9.4 Verify API integration and proxy functionality
  - [ ] 9.5 Test responsive design and mobile functionality
  - [ ] 9.6 Test image caching and loading performance
  - [ ] 9.7 Create integration tests for complete user flows
  - [ ] 9.8 Set up monitoring and error tracking
  - [ ] 9.9 **Problem test areas to resolve before MVP launch:**
    - [ ] useCardAutocomplete cache test: Ensure cache hits update suggestions state synchronously and reliably in tests (currently stubbed/skipped)
    - [ ] useCardAutocomplete maxResults test: Ensure client-side limiting is tested reliably (currently stubbed/skipped)

  - [ ] 10.0 Miscellaneous Cleanups
   - [ ] Remove frontend cache monitoring // move it to a better location - admin page with oauth?
   - [ ] card sorting not working properly - especially most & least expensive