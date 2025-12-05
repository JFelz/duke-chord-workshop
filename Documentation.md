# Overview

## Project Description
The "Duke & Chord Music" application is a client-side JavaScript Single Page Application (SPA) designed to serve as a platform for musical instruments and educational resources. It uses a local JSON server (`json-server`) running on Node.js to mock a backend API, managing data related to users, instruments, instrument types, and music classes.

### Target Audience
The application targets customers interested in purchasing or selling musical instruments, as well as students looking to browse or register for music classes offered by the organization.
 
## Architecture Documentation
 
### 1. Single Page Application (SPA)
The application is structured as a Single Page Application, where the main HTML file ([`src/index.html`](src/index.html)) serves as a container. All primary content rendering and navigation are managed dynamically by JavaScript, utilizing the URL query parameters (e.g., `?view=store`) for basic client-side routing.
 
### 2. Modular and Component-Based Structure
The code is heavily modularized using JavaScript ES6 modules (`import`/`export`). Each major view, component ([`src/scripts/instruments/MiniInstrument.js`](src/scripts/instruments/MiniInstrument.js)), and functional area (e.g., `src/scripts/auth/`) resides in its own dedicated file or directory.
*   **Views/Components:** Functions that return HTML string representations for dynamic rendering.
*   **Logic:** Separate files for data fetching, authentication, and state management.
 
### 3. Centralized State Management (State + Event Broadcasting)
A custom, lightweight state management pattern is employed across the `src/scripts/data/` directory (e.g., [`src/scripts/data/UserStateManager.js`](src/scripts/data/UserStateManager.js), [`src/scripts/data/InstrumentsStateManager.js`](src/scripts/data/InstrumentsStateManager.js)).
*   **State Containers:** Dedicated modules hold arrays or objects representing the current application data.
*   **Communication:** When the state changes, the state manager dispatches a `stateChanged` Custom Event on the main content container (`#content`).
*   **Re-rendering:** Components and views listen for this global `stateChanged` event, prompting them to call getter functions from the state managers and re-render the relevant parts of the DOM.
 
### 4. Front Controller / View Switching
The [`src/scripts/DukeChord.js`](src/scripts/DukeChord.js) module acts as the front controller, responsible for determining the view to render. It reads the `view` parameter from the URL query string and uses a `switch` statement to call the corresponding view-rendering function (e.g., `Home()`, `InstrumentList()`), ensuring the application displays the correct page content dynamically.
## Design Pattern Implementation Details

The application leverages specific design patterns to organize the client-side code:

### 1. Front Controller Pattern (Routing)
The component [`src/scripts/DukeChord.js`](src/scripts/DukeChord.js) acts as the Front Controller. It is the single point responsible for receiving requests (via the main `stateChanged` event or initial load) and delegating the request to the appropriate view handler (e.g., `Home()`, `InstrumentList()`) based on the URL query string parameter `?view`. This centralizes control flow and routing logic.

### 2. Observer Pattern (State Management)
The core of state communication is handled using the **Observer Pattern** (also known as Publish/Subscribe), which provides decoupling between data changes and UI updates.
*   **Subject/Publisher:** The Data/State Manager modules (e.g., [`src/scripts/data/InstrumentsStateManager.js`](src/scripts/data/InstrumentsStateManager.js)) act as the Subject. When they modify internal data, they "publish" a notification by dispatching a `CustomEvent("stateChanged")` on the global `document.querySelector("#content")` element.
*   **Observers/Subscribers:** All view components (e.g., [`src/scripts/instruments/InstrumentList.js`](src/scripts/instruments/InstrumentList.js)) act as Observers. They subscribe to the `#content` element's `stateChanged` event and re-render themselves when the event is received, ensuring the UI reflects the current data state.

### 3. Component Pattern (Structure)
The UI adheres to the **Component Pattern**. The application is built by composing reusable, independent JavaScript functions that represent small, highly cohesive UI parts. These functions are responsible for generating self-contained HTML strings, promoting reusability and a modular UI structure.


## Core Component Relationships
 
The application follows a loosely coupled, component-driven architecture:
 
### 1. Entry Point ([`src/index.html`](src/index.html) & [`src/scripts/main.js`](src/scripts/main.js))
*   [`src/index.html`](src/index.html) defines the root `div` elements (`#container`, `#header`, `#content`).
*   [`src/scripts/main.js`](src/scripts/main.js) initializes the application by calling `DukeChord()` and sets up global event listeners, including the primary `stateChanged` listener on `#content` which triggers re-rendering.
 
### 2. Front Controller ([`src/scripts/DukeChord.js`](src/scripts/DukeChord.js))
*   Renders the main layout, including the persistent `NavBar` and the dynamic view (`buildView()`).
*   Determines the current view (e.g., `Home`, `InstrumentList`, `Login`) based on the URL's `?view` parameter and calls the corresponding view function.
 
### 3. Data/State Managers ([`src/scripts/data/*.js`](src/scripts/data/*.js))
*   Handles fetching data from the API (via [`server.js`](server.js)).
*   Maintains the central application state (e.g., `state.instruments`, `state.currentUser`).
*   Mutations (e.g., `setInstrument`, `login`) dispatch a `stateChanged` Custom Event to notify components of changes.
 
### 4. Views/Components ([`src/scripts/*/*.js`](src/scripts/index/DukeChord.js))
*   These are responsible for generating specific HTML structure (returning string templates).
*   They listen for the global `stateChanged` event and call state manager getters (e.g., `getInstruments()`) to retrieve the latest data and re-render themselves when notified, ensuring the UI reflects the current state.
 
### 5. API Mock Server ([`server.js`](server.js) / `json-server`)
*   A Node.js server that routes all API requests to the mock database ([`api/database.json`](api/database.json)), providing necessary CRUD functionality for all application data.

## User Authentication Flow

The application uses a stateless, client-side authentication mechanism suitable for a mock environment, relying on a JSON API mock server and browser storage.

### 1. Registration
The [`src/scripts/auth/Register.js`](src/scripts/auth/Register.js) component captures the user's name and email. On form submission, it performs a `POST` request to the `/api/users` endpoint to create a new user record in the mock database. Upon successful creation, the application calls the `login()` function to immediately establish a session.

### 2. Login
The [`src/scripts/auth/Login.js`](src/scripts/auth/Login.js) component captures the user's email. On form submission, it performs a `GET` request to the `/api/users?email=[email]` endpoint to search for the matching user record. If found, the session is initiated via the `login()` function.

### 3. Session Persistence
The [`src/scripts/data/UserStateManager.js`](src/scripts/data/UserStateManager.js) module manages persistence:
*   **Initialization:** The `login(user)` function Base64 encodes the entire user object (`btoa(JSON.stringify(user))`) and stores this string in the browser's `localStorage` under the key `"chord_user"`.
*   **Validation:** The `isAuthenticated()` function checks the in-memory state first, and then decodes the `localStorage` item (using `atob()`) to repopulate the application's state if the user object is present, allowing sessions to persist across page reloads.
*   **Security Note:** This mechanism is intended for development/mocking purposes and lacks production security features like password hashing or secure tokens.
 
## Data Flow and Communication

Data movement in Duke & Chord Music is unidirectional and follows a clear lifecycle: from persistence, through state management, and finally to the user interface, mediated by asynchronous operations and custom events.

### 1. Persistence Layer (Database)
*   **Location:** [`api/database.json`](api/database.json)
*   **Access:** Handled by the Node.js server ([`server.js`](server.js)) running `json-server`, which exposes RESTful API endpoints (e.g., `/api/instruments`).

### 2. Data Access and Synchronization (State Managers)
*   State Manager modules (e.g., [`src/scripts/data/InstrumentsStateManager.js`](src/scripts/data/InstrumentsStateManager.js)) are responsible for fetching data using standard `fetch` API calls to the mock server.
*   Once fetched, the raw data is stored in the module's private `state` object (the client-side cache).

### 3. Client-Side Mutation (Action)
*   When a user action occurs (e.g., clicking a button, submitting a form), the corresponding State Manager function is called.
*   The function typically performs two actions:
    1.  Either sends a `POST`/`PUT`/`DELETE` request to the API to persist the change.
    2.  Updates the private `state` object in memory.

### 4. Communication Channel (Event Broadcasting)
*   After any significant state change (including successful API operations), the State Manager dispatches a `stateChanged` Custom Event on the main content container (`#content`). This broadcast serves as the notification mechanism to the UI.

### 5. Presentation Layer (Views/Components)
*   Views and components subscribe to the `stateChanged` event.
*   When notified, they execute the following steps:
    1.  Call the State Manager's public *getter* functions (e.g., `getInstruments()`) to retrieve a copy of the latest data from the cached state.
    2.  Generate the updated HTML string based on the fresh data.
    3.  Replace the existing DOM element content, thus re-rendering the relevant part of the UI.

## Main Features
*   **User Authentication:** Handles user login, registration, and persistent session management.
*   **Instrument Marketplace:** Allows users to view, filter, and see detailed information about available instruments. Includes a form for users to submit instruments for sale.
*   **Educational Resources:** Provides listings and details for music classes, fetched from the API and associated with musicians/instructors.
*   **Interactive Controls:** Features include toggling sound effects on and off for a richer instrument interaction experience.
*   **Staff Directory:** Provides an "About" view that lists employees/staff members.
