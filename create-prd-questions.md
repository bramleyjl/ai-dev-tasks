1. Target User & Problem
A) Who is the primary user of this React Native app? (e.g., Magic: The Gathering players, card collectors, deck builders)
  - The app is targeted for deck builders that already know what cards they want in their deck but havent chosen among the many different versions.
B) What specific problem does this app solve for them? (e.g., difficulty managing card versions, need for quick deck analysis, etc.)
  - Over the past years there are often many different versions of the same card, many players enjoy customizing their decks with specific versions, and different versions can have very different monetary prices. This app helps players input a list of 100 cards (a standard EDH deck size) and then easily choose which versions based on both appearance and price.
2. Core Functionality Priority
Which of these API functions should be prioritized for the MVP?
A) createCardPackage - Create card packages from a list of cards
B) randomPackage - Generate random card packages
C) export - Export card packages in different formats (TCGPlayer, text)
D) All three equally important
 - A & C are the main functions for the app to function, B is a secondary feature but has simple inputs and should have an identical response handling to A.
3. User Interface Design
A) Do you have any existing design preferences or brand guidelines?
  - No, I'm a backend engineer and I'm not familiar with design.
B) Should the app follow a specific design system (Material Design, iOS Human Interface Guidelines, custom)?
  - I'm not familiar with design, so I'm not sure. I'd like to follow standard conventions as much as possible and keep things simple.
C) Any specific color scheme or theme preferences?
  - The majority of card images have a black border, a minority have a white border. So it will need to work with both.
4. Navigation & User Flow
A) How should users navigate between the different features? (Tab navigation, stack navigation, drawer navigation?)
  - I'm not sure, there should only be two views, one for card input and one for the display of cards in the package. They could event be combined into one view.
B) What should be the main landing screen of the app?
  - A user input for cards, and a button to create a card package.
5. Data Input Methods
For the createCardPackage feature:
A) How should users input card lists? (Text input, file upload, manual entry, search/select from database?)
  - Text input to build a list manually
  - Ability to import from other MTG deckbuilding websites (stubbed, not part of MVP)
B) Should there be card search/autocomplete functionality?
  - Yes, the mtgv-api maintains a MongoDB of cards, their names should be used in the frontend app for autocompletion
6. Export Functionality
For the export feature:
A) How should exported data be handled? (Copy to clipboard, save to file, share via other apps?)
  - TCGPlayer: forwarded to generated TCGPlayer URL
  - Text: window with formatted text & button to copy to clipboard.
B) Which export formats are most important? (TCGPlayer, text, both?)
  - Both
7. Technical Constraints
A) Any specific React Native version requirements?
 - No, just modern versions
B) Should the app work on both iOS and Android, or focus on one platform first?
  - Both platforms.
C) Any specific state management preferences? (Redux, Context API, Zustand, etc.)
 - No.
8. API Integration
A) Should the app handle offline scenarios or require constant internet connection?
 - Client app assumes mtgv-API and scryfall are available.
B) Any specific error handling or loading state requirements?
  - App involves displaying a large number of images from Scryfall.com. Pay extra attention to card image caching to reduce load on both the MTGV client app and Scryfall.


  Clarifying Questions for Web-First MTGV Frontend
1. Technology Stack Preferences
A) What web framework would you prefer for the frontend?
React (with Create React App or Next.js)
Vue.js
Angular
Other (specify)

React with Next.js
B) Should the web app be a Single Page Application (SPA) or Multi-Page Application (MPA)?
SPA (faster navigation, better for complex interactions)
MPA (better SEO, simpler routing)

SPA
2. Mobile Strategy
A) For the post-MVP mobile functionality, what approach do you prefer?
Progressive Web App (PWA) - works on mobile browsers
React Native Web - same codebase for web and mobile
Separate mobile app (React Native) - different codebase
Hybrid approach (PWA + native app)

I only want to have to manage a single frontend codebase as I am a backend dev.
3. Deployment & Hosting
A) Where should the web app be deployed?
Vercel (easy deployment, good for React)
Netlify (static hosting)
AWS/GCP/Azure (cloud hosting)
Self-hosted

Is it possible to use Render since the backend is also hosted there?
4. API Integration
A) Should the web app communicate directly with the MTGV API, or do you need a proxy/backend?
Direct API calls from frontend
Backend proxy for security/rate limiting
API Gateway

simple proxy - is this managed in the backend or frontend app?
5. Image Handling for Web
A) How should card images be handled in the web version?
Direct links to Scryfall images
Image proxy/caching service
CDN for optimized delivery
Local image storage

Minimal load on scryfall - robust caching and potentially a post-mvp feature where all scryfall images are pulled in bulk.
6. User Experience
A) Should the web app be responsive (mobile-friendly) from the start?
Yes, fully responsive design
Desktop-first, mobile optimization later
Separate mobile and desktop layouts

fully responsive
7. Performance Requirements
A) What are the performance expectations for the web app?
Fast loading (< 3 seconds)
Optimized for large card lists (100+ images)
Offline capability
Real-time updates

100 images (standard Commander deck size) is mandatory, also at least at the start a 100 card limit should be in place for performance reasons
fast loading & responsive performance is also important
offline capability is not necessary
real-time updates are also not necessary, eventually we'll create a pullBulkData chron job