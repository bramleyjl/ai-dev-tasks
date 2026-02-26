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
- `mtgv-web/src/components/CardList.tsx` - Card display interface with version selection (serves as CardDisplay)
- `mtgv-web/src/components/CardList.test.tsx` - Unit tests for CardList component
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
- `mtgv-web/src/app/api/card_packages/export/route.ts` - Export API route for card packages
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

- [x] 3.5 Caching and State Persistence Issues
  - [x] 3.5.1 Fix API Redis caching for card package data
  - [x] 3.5.2 Fix frontend caching for user selections and package state
  - [x] 3.5.3 Implement proper state persistence across page reloads
  - [x] 3.5.4 Fix user selection reloading issues
  - [x] 3.5.5 Add proper cache invalidation strategies
  - [x] 3.5.6 Test caching behavior with multiple users
  - [x] 3.5.7 Create unit tests for caching functionality

- [x] 4.0 Card Display Interface and Version Selection
  - [x] 4.1 Create CardDisplay component for showing card package results
  - [x] 4.2 Create CardVersion component for individual card version display
  - [x] 4.3 Implement card image loading from Scryfall with fallback handling
  - [x] 4.4 Add card version selection functionality (radio buttons or similar)
  - [x] 4.5 Display card prices and metadata for each version
  - [x] 4.6 Handle both black-bordered and white-bordered card images appropriately
  - [x] 4.7 Implement smooth scrolling for large card lists
  - [x] 4.8 Add visual feedback for selected card versions
  - [x] 4.9 Create unit tests for CardDisplay and CardVersion components

- [x] 5.0 Export Functionality Implementation
  - [x] 5.1 Implement `/card_package/export` POST endpoint integration
  - [x] 5.2 Add TCGPlayer export format with direct URL forwarding
  - [x] 5.3 Add text export format with copy-to-clipboard functionality
  - [x] 5.4 Create export buttons and UI for both export types
  - [x] 5.5 Handle browser clipboard operations for text export
  - [x] 5.6 Add export success/error feedback to users
  - [x] 5.7 Implement export data validation before API calls
  - [x] 5.8 Create unit tests for export functionality

- [~~6.0~~] Random Package Generation Feature (SKIPPED - Not needed for MVP)
  - [~~6.1~~] Implement `/card_package/random` GET endpoint integration
  - [~~6.2~~] Add random package count input interface
  - [~~6.3~~] Create random package generation button and UI
  - [~~6.4~~] Handle random package responses identically to created packages
  - [~~6.5~~] Add loading states for random package generation
  - [~~6.6~~] Integrate random package with existing card display interface
  - [~~6.7~~] Create unit tests for random package functionality

- [x] 7.0 Responsive Design and Mobile Optimization
  - [x] 7.1 Implement responsive layout for desktop, tablet, and mobile
  - [x] 7.2 Add touch-friendly interactions for mobile devices
  - [x] 7.3 Optimize card grid layout for different screen sizes
  - [x] 7.4 Implement mobile-friendly navigation and UI patterns
  - [x] 7.5 Add responsive typography and spacing
  - [x] 7.5.1 organize CSS into named classes for centralized control & easier manual management
  - [x] 7.6 Test responsive design across different devices and browsers

- [x] 8.0 Performance Optimization and Image Caching
  - [x] 8.1 Implement efficient image caching service using Next.js Image component
  - [x] 8.2 Configure image optimization and lazy loading
  - [x] 8.3 Add image preloading for better user experience
  - [x] 8.4 Implement virtual scrolling for large card lists
  - [x] 8.5 Optimize image loading to reduce load on Scryfall
  - [x] 8.6 Add image loading placeholders and error states
  - [x] 8.7 Monitor and optimize memory usage for large image lists
  - [x] 8.8 Create unit tests for image caching service

- [x] 8.5 **Card List Refactoring and Bulk Import**
  - [x] 8.5.1 Refactor CardList to always be visible (even when empty)
  - [x] 8.5.2 Add tabbed interface with 3 tabs: Manual Entry, Free Text Input, Import URL
  - [x] 8.5.3 Implement Free Text tab with deck list parsing and validation
- [x] 8.5.4 Add copy deck list functionality with multiple format options
- [x] 8.5.5 Stub Import URL tab for future implementation
- [x] 8.5.6 Create unit tests for new tabbed interface and free text functionality

- [x] 9.0 **Staging Environment Deployment and Optimization** ✅
  - [x] 9.1 Configure deployment for Render hosting platform
  - [x] 9.2 Set up environment variables and configuration
  - [x] 9.3 Deploy backend API to Render staging environment
  - [x] 9.4 Deploy frontend to Render staging environment
  - [x] 9.5 Verify API integration and proxy functionality
  - [ ] 9.6 Test responsive design and mobile functionality
  - [ ] 9.7 Test image caching and loading performance
  - [ ] 9.8 **Staging Environment Optimization Tasks:**
    - [x] 9.8.1.0 daily cron job for updating render environment card database
    - [x] 9.8.1 Fix remaining Jest test failures (useCardAutocomplete, EditableCardName components)
    - [x] 9.8.2 Optimize database update script memory usage for Render constraints
    - [x] 9.8.3 Implement comprehensive error handling and user feedback (middleware complete, frontend docs created)
    - [x] 9.8.4 Add staging-specific monitoring and logging (request logging, performance monitoring, metrics endpoint)
    - [ ] 9.8.5 Performance testing and optimization for staging environment
    - [ ] 9.8.6 User acceptance testing with real users in staging
    - [x] 9.8.7 Security audit and vulnerability assessment (comprehensive audit complete, fixes in progress - see `ai-dev-tasks/docs/SECURITY_AUDIT.md`)
    - [ ] 9.8.8 Load testing and scalability validation
    - [x] 9.8.9 Documentation and runbooks for staging operations

- [ ] 10.0 **Post-MVP Improvements and Cleanup**
  - [ ] 10.1 Remove frontend cache monitoring (move to admin page with OAuth)
  - [ ] 10.2 Fix card sorting issues (especially most & least expensive)
  - [ ] 10.3 Document current caching behavior and state flow
  - [ ] 10.4 Plan handling of complex interactions between card list and package state
  - [ ] 10.5 Address corner cases in state synchronization
  - [ ] 10.6 Design robust error handling for state conflicts
  - [ ] 10.7 Create comprehensive testing strategy for state management
  - [ ] 10.8 **Data Structure Standardization Analysis**
    - [ ] 10.8.1 Analyze MTGJSON data structures and compare with current MTGV data models
    - [ ] 10.8.2 Examine Magic: The Gathering JS SDK patterns and conventions
    - [ ] 10.8.3 Identify opportunities to align with community standards
    - [ ] 10.8.4 Evaluate potential benefits of adopting standardized field names and structures
    - [ ] 10.8.5 Assess compatibility with popular MTG tools and libraries
    - [ ] 10.8.6 Create migration plan for any structural changes
    - [ ] 10.8.7 Document findings and recommendations for future development
  - [ ] 10.9 **Random Card Functionality Evaluation**
    - [ ] 10.9.1 Evaluate user demand for random card generation feature
    - [ ] 10.9.2 Research potential use cases (deck building inspiration, testing, etc.)
    - [ ] 10.9.3 Assess technical complexity and maintenance burden
    - [ ] 10.9.4 Consider alternative approaches (curated lists, themed packages, etc.)
    - [ ] 10.9.5 Decide whether to expand into full feature or remove backend code
    - [ ] 10.9.6 If removing: create separate branch to preserve functionality
    - [ ] 10.9.7 If expanding: design comprehensive random card generation system
    - [ ] 10.9.8 Document decision and rationale for future reference

- [ ] 10.10 **Post-MVP Testing and Documentation**
  - [ ] 10.10.0 complete & refactor testing from - mtgv-web/scripts/test-responsive.js
  - [ ] 10.10.1 **Manual Responsive Testing**: Complete comprehensive responsive design testing across devices and browsers using RESPONSIVE_TESTING.md guide
  - [ ] 10.10.2 **Mobile Device Testing**: Test on actual mobile devices and tablets for touch interactions and layout
  - [ ] 10.10.3 **Documentation Consolidation**: Consolidate all documentation into a single, organized reference guide
    - [ ] 10.10.3.1 frontend documentation consolidated
    - [ ] 10.10.3.2 backend documentation consolidated

- [ ] 10.11 **Environment Infrastructure Upgrade (Staging & Production)**
  > **Note**: See `ai-dev-tasks/docs/HOSTING_MONITORING_PLAN.md` for detailed cost analysis and recommendations

  - [ ] 10.11.1 **Hosting & Monitoring Analysis**
    - [ ] 10.11.1.1 Review hosting options (Render, Vercel, Fly.io, Railway, DigitalOcean) - see HOSTING_MONITORING_PLAN.md
    - [ ] 10.11.1.2 Analyze monitoring/observability tools (Sentry, UptimeRobot, New Relic, Datadog, Better Stack) - see HOSTING_MONITORING_PLAN.md
    - [ ] 10.11.1.3 **Select identical products for both staging and production** (same hosting, monitoring, database providers)
    - [ ] 10.11.1.4 Document final selection with cost breakdown for staging tier vs production tier
    - [ ] 10.11.1.5 Create deployment checklist and migration plan

  - [ ] 10.11.2 **Free Monitoring Setup (Staging & Production)**
    - [ ] 10.11.2.1 Set up Sentry error tracking (FREE tier - 5k errors/month) for both environments
    - [ ] 10.11.2.2 Configure UptimeRobot health monitoring (FREE tier - 50 monitors) for both environments
    - [ ] 10.11.2.3 Add Cloudflare CDN/DDoS protection (FREE tier) in front of both environments
    - [ ] 10.11.2.4 Implement structured logging with correlation IDs (backend already has this)
    - [ ] 10.11.2.5 Add Next.js Image onError monitoring for Scryfall load failures
    - [ ] 10.11.2.6 Document cache hit rates and performance metrics

  - [ ] 10.11.3 **Staging Environment Upgrade**
    - [ ] 10.11.3.1 Back up current Render staging configuration and environment variables
    - [ ] 10.11.3.2 Migrate frontend to selected hosting provider (hobby/starter tier ~$7-20/month)
    - [ ] 10.11.3.3 Migrate backend to selected hosting provider (hobby/starter tier ~$7-25/month)
    - [ ] 10.11.3.4 Set up persistent Redis cache (if moving off Render - Upstash hobby ~$10/month)
    - [ ] 10.11.3.5 Configure MongoDB connection (keep M0 free or upgrade to M2/M5 ~$9-25/month)
    - [ ] 10.11.3.6 Update environment variables for new hosting provider
    - [ ] 10.11.3.7 Configure custom domain for staging (optional - e.g., staging.mtgv.app)
    - [ ] 10.11.3.8 Set up CI/CD auto-deployment from GitHub (staging branch)
    - [ ] 10.11.3.9 Test all functionality on upgraded staging environment

  - [ ] 10.11.4 **Production Environment Creation**
    - [ ] 10.11.4.1 Create production instances on same hosting provider as staging (production tier)
    - [ ] 10.11.4.2 Configure frontend production deployment (production tier ~$20-50/month)
    - [ ] 10.11.4.3 Configure backend production deployment (production tier ~$25-100/month)
    - [ ] 10.11.4.4 Set up production MongoDB instance with backups (M10+ tier ~$57/month minimum)
    - [ ] 10.11.4.5 Set up production Redis cache with persistence (production tier ~$10-15/month)
    - [ ] 10.11.4.6 Configure production environment variables (separate from staging)
    - [ ] 10.11.4.7 Set up custom domain for production (e.g., mtgv.app or <www.mtgv.app>)
    - [ ] 10.11.4.8 Configure SSL certificates (should be automatic with hosting provider)
    - [ ] 10.11.4.9 Set up CI/CD auto-deployment from GitHub (main/production branch)

  - [ ] 10.11.5 **Production Monitoring & Observability**
    - [ ] 10.11.5.1 Upgrade Sentry to paid tier if needed (Professional $26/month for 50k errors)
    - [ ] 10.11.5.2 Add log aggregation service (Better Stack $10-20/month or equivalent)
    - [ ] 10.11.5.3 Set up APM if budget allows (New Relic/Datadog ~$15-99/month)
    - [ ] 10.11.5.4 Configure alerting rules for critical errors, downtime, slow response times
    - [ ] 10.11.5.5 Set up PagerDuty or equivalent for on-call alerts (optional - $19/month)
    - [ ] 10.11.5.6 Create monitoring dashboard for key metrics (request rates, error rates, response times)
    - [ ] 10.11.5.7 Document monitoring runbook and alert response procedures

  - [ ] 10.11.6 **Production Launch & Validation**
    - [ ] 10.11.6.1 Deploy application code to production environment
    - [ ] 10.11.6.2 Run smoke tests on production (health checks, API endpoints, image loading)
    - [ ] 10.11.6.3 Verify monitoring and error tracking are working
    - [ ] 10.11.6.4 Test with sample Commander deck (80+ cards) for performance validation
    - [ ] 10.11.6.5 Share with friends/family for initial user feedback
    - [ ] 10.11.6.6 Monitor logs and metrics for first 48-72 hours
    - [ ] 10.11.6.7 Address any critical issues discovered during initial usage
    - [ ] 10.11.6.8 Document final production architecture and costs

- [ ] 10.12 **Comprehensive Logging, Monitoring, and Metrics Collection**
  - [ ] 10.12.1 **Real Memory Usage Monitoring**: Implement browser performance.memory API integration for accurate heap usage tracking
  - [ ] 10.12.2 **Performance Metrics Collection**: Add render performance, component lifecycle, and API response time tracking
  - [ ] 10.12.3 **Error Tracking and Reporting**: Implement centralized error logging with crash reporting and error aggregation
  - [ ] 10.12.4 **User Analytics**: Track usage patterns, performance bottlenecks, and user experience metrics
  - [ ] 10.12.5 **Application Performance Monitoring (APM)**: Integrate with tools like Sentry, LogRocket, or custom APM solution
  - [ ] 10.12.6 **Real-time Dashboard**: Create admin dashboard for monitoring application health, performance, and user metrics
  - [ ] 10.12.7 **Alerting System**: Set up automated alerts for performance degradation, error spikes, and memory issues
  - [ ] 10.12.8 **Data Retention and Privacy**: Implement GDPR-compliant data retention policies and user privacy controls
  - [ ] 10.12.9 **Performance Budgets**: Establish and enforce performance budgets for core web vitals and user experience metrics

- [ ] 10.13 **Database Update Script Optimization**
  - [ ] 10.13.1 **Memory Usage Analysis**: Profile pullBulkData script to identify memory bottlenecks and excessive allocations
  - [ ] 10.13.2 **Streaming Implementation**: Refactor to use streaming for large data downloads instead of loading entire datasets into memory
  - [ ] 10.13.3 **Batch Processing**: Implement chunked processing of card data to limit memory usage during updates
  - [ ] 10.13.4 **Memory Monitoring**: Add real-time memory usage tracking and automatic cleanup during bulk operations
  - [ ] 10.13.5 **Resource Management**: Implement proper cleanup of temporary objects and database connections
  - [ ] 10.13.6 **Startup Integration**: Re-enable database updates during startup once memory usage is optimized
  - [ ] 10.13.7 **Fallback Strategy**: Create graceful fallback when memory constraints are reached
  - [ ] 10.13.8 **Performance Testing**: Benchmark memory usage and optimize for Render's free tier constraints
  - [ ] 10.13.9 **Documentation**: Document memory optimization techniques and best practices for future development
- [ ] 10.14 **Production Security Hardening & Maintenance**

  > **Note**: See `ai-dev-tasks/docs/SECURITY_AUDIT.md` for comprehensive security audit findings and recommendations
  >
  > **Authentication Scope**: App designed for anonymous usage with ephemeral card list caching only. No user accounts, authentication, or persistent user data storage required.

  - [ ] 10.14.1 **Immediate Security Fixes (Critical - Week 1)**
    - [ ] 10.14.1.1 Deploy CORS whitelist configuration (already implemented in middleware/security.js)
    - [ ] 10.14.1.2 Fix npm vulnerabilities - run `npm audit fix` on both frontend and backend
    - [ ] 10.14.1.3 Add request size limits to body-parser (prevent DoS via large payloads)
    - [ ] 10.14.1.4 Add WebSocket message validation and rate limiting

  - [ ] 10.14.2 **Short-Term Security Enhancements (High Priority - Weeks 2-3)**
    - [ ] 10.14.2.1 Implement distributed rate limiting with Redis (multi-instance support)
    - [ ] 10.14.2.2 Add WebSocket authentication (token-based connection validation)
    - [ ] 10.14.2.3 Set up Sentry for comprehensive error tracking and security monitoring
    - [ ] 10.14.2.4 Implement API versioning (e.g., /v1/card_package)
    - [ ] 10.14.2.5 Add security event logging (rate limit violations, suspicious patterns)

  - [ ] 10.14.3 **Production Database Security (Before Real Launch)**
    - [ ] 10.14.3.1 Upgrade MongoDB from M0 to M10+ for encryption at rest and automated backups
    - [ ] 10.14.3.2 Configure MongoDB IP whitelist for additional network security
    - [ ] 10.14.3.3 Enable MongoDB audit logging for security event tracking
    - [ ] 10.14.3.4 Create separate database users for different environments
    - [ ] 10.14.3.5 Implement and test backup/restore procedures
    - [ ] 10.14.3.6 Set up MongoDB performance monitoring

  - [ ] 10.14.4 **Ongoing Security Maintenance (Monthly/Quarterly)**
    - [ ] 10.14.4.1 Regular npm dependency updates and vulnerability scanning
    - [ ] 10.14.4.2 Monthly security log review and incident analysis
    - [ ] 10.14.4.3 Quarterly security audit and penetration testing
    - [ ] 10.14.4.4 Review and rotate secrets/credentials (MongoDB passwords, API keys)
    - [ ] 10.14.4.5 Monitor SSL/TLS certificate expirations and renewals
    - [ ] 10.14.4.6 Update security headers and CSP policies as needed

- [ ] 10.15 **Frontend Error Handling Enhancement** (After React/Next.js Learning)

  > **Note**: See `ai-dev-tasks/docs/FRONTEND_ERROR_HANDLING_ESSENTIAL.md` for implementation details. To be completed after gaining more experience with React/Next.js patterns.

  - [ ] 10.15.1 **Standardized Error Classes**: Create MTGVAPIError class with user-friendly message mapping
  - [ ] 10.15.2 **Retry Mechanism**: Add automatic retry utility for transient failures (network errors, 5xx responses)
  - [ ] 10.15.3 **Error Boundary**: Create React ErrorBoundary component to prevent full app crashes
  - [ ] 10.15.4 **Enhanced ErrorDisplay**: Update ErrorDisplay component with user-friendly messages and retry button
  - [ ] 10.15.5 **Testing**: Test error scenarios (network failure, server errors, validation errors, component crashes)

- [ ] 11.0 Enhanced Card List Generation and Import Features
  - [ ] 11.2 **Import URL Generation for Popular Deckbuilding Websites**
    - [ ] 11.2.1 Research and implement TCGPlayer mass entry URL generation
    - [ ] 11.2.2 Add Moxfield deck import URL generation
    - [ ] 11.2.3 Add Archidekt deck import URL generation
    - [ ] 11.2.4 Add Deckstats.net import URL generation
    - [ ] 11.2.5 Add MTGGoldfish deck import URL generation
    - [ ] 11.2.6 Create unified import URL service with extensible architecture
    - [ ] 11.2.7 Add "Generate Import Links" button with dropdown menu
    - [ ] 11.2.8 Handle different URL formats and API requirements for each site
    - [ ] 11.2.9 Create unit tests for import URL generation
  - [ ] 11.3 **Random EDH Deck Populator**
    - [ ] 11.3.1 Research EDHrec API for popular deck lists
    - [ ] 11.3.2 Implement EDHrec deck fetching service
    - [ ] 11.3.3 Add "Random EDH Deck" button to CardInput component
    - [ ] 11.3.4 Support filtering by commander, theme, or popularity
    - [ ] 11.3.5 Add deck preview with commander and deck name
    - [ ] 11.3.6 Implement fallback to other EDH deck repositories
    - [ ] 11.3.7 Add deck metadata display (commander, theme, budget)
    - [ ] 11.3.8 Handle rate limiting and API quotas for external services
    - [ ] 11.3.9 Create unit tests for EDH deck fetching
  - [ ] 11.4 **Enhanced User Experience Features**
    - [ ] 11.4.1 Add deck list templates (Standard, Modern, Pioneer, etc.)
    - [ ] 11.4.2 Implement "Save Deck List" functionality with localStorage
    - [ ] 11.4.3 Add "Load Saved Deck" dropdown with deck names
    - [ ] 11.4.4 Create deck list sharing via URL parameters
    - [ ] 11.4.5 Add deck list export in multiple formats (JSON, CSV, plain text)
    - [ ] 11.4.6 Implement deck list validation and error correction
    - [ ] 11.4.7 Add bulk card search and add functionality
    - [ ] 11.4.8 Create comprehensive error handling for all import/export features
  - [ ] 11.5 **API and Backend Enhancements**
    - [ ] 11.5.1 Extend backend API to support deck list import/export
    - [ ] 11.5.2 Add deck list caching and optimization
    - [ ] 11.5.3 Implement rate limiting for external API calls
    - [ ] 11.5.4 Add deck list validation middleware
    - [ ] 11.5.5 Create deck list transformation services
    - [ ] 11.5.6 Add comprehensive logging for import/export operations
    - [ ] 11.5.7 Create unit tests for new backend functionality
  - [ ] 11.6 **Documentation and Testing**
    - [ ] 11.6.1 Document supported deck list formats and import/export features
    - [ ] 11.6.2 Create user guide for copy/paste functionality
    - [ ] 11.6.3 Document API endpoints for deck list operations
    - [ ] 11.6.4 Create integration tests for end-to-end deck list workflows
    - [ ] 11.6.5 Add performance testing for large deck lists
    - [ ] 11.6.6 Create accessibility testing for new UI components
    - [ ] 11.6.7 Document error handling and troubleshooting guides

- [ ] 12.0 React/Next.js Frontend Deep Dive (Post-MVP Learning)
  - [ ] 12.1 **Core React Patterns**
    - [ ] 12.1.1 Study useState vs useRef and stale closures (document useCardPackage lessons learned)
    - [ ] 12.1.2 Master useEffect dependencies and cleanup patterns
    - [ ] 12.1.3 Understand component lifecycle: mounting vs re-rendering vs remounting
    - [ ] 12.1.4 Learn useCallback/useMemo for performance optimization
  - [ ] 12.2 **WebSocket Integration in React**
    - [ ] 12.2.1 WebSocket lifecycle: connection management, event handlers, avoiding stale closures
    - [ ] 12.2.2 State synchronization patterns: keeping UI in sync with real-time messages
    - [ ] 12.2.3 Race condition handling and debouncing strategies
  - [ ] 12.3 **Next.js App Router Essentials**
    - [ ] 12.3.1 Server vs Client Components and data fetching patterns (SSR, SSG, ISR)
    - [ ] 12.3.2 Caching strategies and route handlers
    - [ ] 12.3.3 Image optimization and middleware patterns
  - [ ] 12.4 **Custom Hooks & TypeScript**
    - [ ] 12.4.1 Hook composition and return patterns
    - [ ] 12.4.2 Proper typing for hooks: useState, useRef, generic types
    - [ ] 12.4.3 Hook testing strategies and best practices
  - [ ] 12.5 **Performance & Common Pitfalls**
    - [ ] 12.5.1 React.memo, code splitting, bundle optimization
    - [ ] 12.5.2 Memory leaks: event listeners, subscriptions, cleanup
    - [ ] 12.5.3 Infinite loops and state batching patterns
  - [ ] 12.6 **Documentation & Application**
    - [ ] 12.6.1 Create internal pattern library with examples from this project
    - [ ] 12.6.2 Establish team coding guidelines and code review checklist
