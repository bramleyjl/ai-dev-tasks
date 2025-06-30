# Product Requirements Document: Web-First MTGV Frontend

## Introduction/Overview

This document outlines the requirements for a web-first frontend application that will consume the MTGV API routes. The app is designed for Magic: The Gathering deck builders who need to choose among different versions of cards based on appearance and price. The primary problem this solves is the complexity of managing multiple card versions when building EDH decks, where players often want to customize their decks with specific card versions that can have significantly different monetary values. The application will be built as a React SPA with Next.js, designed to work seamlessly on both desktop and mobile browsers with a single codebase.

## Goals

1. **Primary Goal**: Enable deck builders to input a list of cards and easily select preferred versions based on appearance and price through a web interface
2. **Secondary Goal**: Provide export functionality for selected card versions in TCGPlayer and text formats
3. **Tertiary Goal**: Offer random package generation as a secondary feature
4. **Technical Goal**: Implement efficient card image caching to reduce load on both the client app and Scryfall
5. **Architecture Goal**: Maintain a single codebase that can be extended for mobile functionality post-MVP

## User Stories

1. **As a deck builder**, I want to input my card list manually through a web interface so that I can create a card package from my existing deck
2. **As a deck builder**, I want to see autocomplete suggestions for card names so that I can quickly and accurately input my deck list
3. **As a deck builder**, I want to see all available versions of each card with images and prices so that I can make informed decisions about which versions to choose
4. **As a deck builder**, I want to select specific card versions based on appearance and price so that I can customize my deck to my preferences
5. **As a deck builder**, I want to export my selected card versions to TCGPlayer so that I can easily purchase the cards
6. **As a deck builder**, I want to export my selected card versions as formatted text so that I can share my deck list with others
7. **As a deck builder**, I want to generate random card packages so that I can explore new deck ideas
8. **As a mobile user**, I want the web app to work seamlessly on my mobile browser so that I can use the app anywhere

## Functional Requirements

### 1. Card Input Interface
- The system must provide a text input interface for users to manually enter card names
- The system must implement autocomplete functionality using the MTGV API's MongoDB card database
- The system must support input of card quantities (default: 1)
- The system must validate card names against the available database
- The system must provide a clear way to add/remove cards from the input list
- The system must enforce a 100-card limit for performance reasons
- The system must provide real-time validation and feedback

### 2. Card Package Creation
- The system must call the `/card_package` POST endpoint with validated card list data
- The system must support game type selection (paper, mtgo, arena) with paper as default
- The system must support default selection preferences (oldest, newest, most_expensive, least_expensive) with newest as default
- The system must handle API responses and display card package data
- The system must provide loading states during API calls

### 3. Card Display Interface
- The system must display all available versions of each card with images from Scryfall
- The system must show card prices and relevant metadata for each version
- The system must allow users to select preferred versions of cards
- The system must handle both black-bordered and white-bordered card images appropriately
- The system must implement efficient image caching to reduce load on Scryfall
- The system must provide responsive design for mobile and desktop viewing
- The system must support smooth scrolling and interaction with large card lists

### 4. Export Functionality
- The system must support TCGPlayer export format with direct URL forwarding
- The system must support text export format with copy-to-clipboard functionality
- The system must call the `/card_package/export` POST endpoint with selected prints data
- The system must handle both export types appropriately in the UI
- The system must provide export success/error feedback to users

### 5. Random Package Generation
- The system must provide a simple interface for generating random card packages
- The system must call the `/card_package/random` GET endpoint with count parameter
- The system must handle random package responses identically to created packages
- The system must provide loading states for random package generation

### 6. Responsive Design and Mobile Optimization
- The system must provide a fully responsive design that works on desktop, tablet, and mobile
- The system must optimize layout and interactions for touch devices
- The system must maintain usability across different screen sizes
- The system must provide appropriate loading states and error handling
- The system must be mobile-functional from MVP launch (not post-MVP)

### 7. Performance Optimization
- The system must load quickly (< 3 seconds initial load)
- The system must handle 100-card lists efficiently
- The system must implement robust image caching strategies
- The system must optimize for mobile performance
- The system must minimize API calls through intelligent caching

### 8. Error Handling
- The system must handle API errors gracefully with user-friendly messages
- The system must validate user input before making API calls
- The system must provide appropriate loading states during API operations
- The system must handle network connectivity issues

## Non-Goals (Out of Scope)

1. **Offline Functionality**: The app assumes constant internet connectivity to MTGV API and Scryfall
2. **Advanced State Management**: No complex state management solutions required for MVP
3. **Deck Import from External Sources**: Import functionality will be stubbed, not implemented
4. **User Authentication**: No user accounts or authentication required
5. **Deck Saving**: No persistent storage of user decks beyond the current session
6. **Advanced Filtering**: No complex filtering or sorting beyond basic version selection
7. **Social Features**: No sharing, commenting, or social functionality
8. **Real-time Updates**: No real-time functionality required
9. **Native Mobile App**: No separate native mobile app development (mobile functionality is included in the web app)
10. **Bulk Image Download**: Image bulk download feature is post-MVP

## Design Considerations

### UI/UX Requirements
- Follow modern web design conventions and accessibility standards
- Keep the interface simple and intuitive for backend engineers
- Ensure the app works well with both black-bordered and white-bordered card images
- Implement responsive design for different screen sizes and devices
- Use clear visual hierarchy and consistent spacing
- Provide clear visual feedback for user actions and system states

### Image Handling
- Implement efficient image caching to reduce load on Scryfall
- Handle both black and white bordered card images appropriately
- Provide fallback images for failed loads
- Optimize image loading for performance across devices
- Consider implementing lazy loading for card images

### Navigation Design
- Simple, intuitive navigation between card input and card display
- Consider combining views into a single screen for simplicity
- Clear visual feedback for user actions and system states
- Mobile-friendly navigation patterns

## Technical Considerations

### Technology Stack
- **Framework**: React with Next.js for SPA functionality
- **Deployment**: Render (to match backend hosting)
- **Mobile Strategy**: Single codebase approach for future mobile functionality
- **API Integration**: Direct API calls with simple proxy if needed
- **Image Handling**: Direct Scryfall links with robust caching

### API Integration
- Integrate with existing MTGV API endpoints
- Handle API validation errors appropriately
- Implement proper error handling for network failures
- Use the MongoDB card database for autocomplete functionality
- Consider simple proxy implementation if needed for security/rate limiting

### Performance Requirements
- Implement efficient card image caching
- Optimize for handling large numbers of card images (100 cards)
- Minimize API calls through intelligent caching
- Ensure smooth scrolling and interaction with card lists
- Fast loading times (< 3 seconds)

### Mobile Considerations
- Fully responsive design from the start
- Touch-friendly interactions
- Optimized performance for mobile devices
- Single codebase approach for future mobile functionality

## Success Metrics

1. **Functional Success**: All three API endpoints (createCardPackage, randomPackage, export) are successfully integrated and functional
2. **Performance Success**: Card images load efficiently with minimal impact on Scryfall
3. **User Experience Success**: Users can successfully input card lists, view versions, and export results
4. **Responsive Success**: App works consistently across desktop, tablet, and mobile devices
5. **Error Handling Success**: App gracefully handles API errors and network issues
6. **Performance Success**: App loads quickly and handles 100-card lists efficiently

## Open Questions

1. **Proxy Implementation**: Should the proxy be implemented in the frontend app or as a separate backend service?
2. **Image Caching Strategy**: What specific caching approach should be used for card images?
3. **Card Database Integration**: How should the autocomplete functionality integrate with the MTGV API's MongoDB?
4. **Export URL Handling**: How should TCGPlayer URLs be handled in the web app context?
5. **Performance Testing**: What are the acceptable performance benchmarks for image loading and API response times?
6. **Error Message Localization**: Should error messages be localized or kept in English for MVP?
7. **Mobile Functionality Timeline**: When should mobile-specific features be prioritized post-MVP?

## Implementation Priority

1. **Phase 1**: Basic web app setup with Next.js and React
2. **Phase 2**: Card input interface with autocomplete
3. **Phase 3**: Card package creation and display functionality
4. **Phase 4**: Card version selection and export functionality
5. **Phase 5**: Random package generation
6. **Phase 6**: Performance optimization and image caching
7. **Phase 7**: Responsive design and mobile optimization
8. **Phase 8**: Deployment and testing 