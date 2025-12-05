# Overview

## Project Description
The "Duke & Chord Music" application is a client-side JavaScript Single Page Application (SPA) designed to serve as a platform for musical instruments and educational resources. It uses a local JSON server (`json-server`) running on Node.js to mock a backend API, managing data related to users, instruments, instrument types, and music classes.

### Target Audience
The application targets customers interested in purchasing or selling musical instruments, as well as students looking to browse or register for music classes offered by the organization.
 
# Architecture Documentation
 
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

# Code Organization

The project adheres to a clear, feature-based directory structure to separate responsibilities between the backend mock server, the client-side single-page application (SPA), and configuration files.

### Root Directory

| File/Directory | Purpose |
| :--- | :--- |
| [`server.js`](server.js) | Node.js entry point. Sets up the static file server and mounts `json-server` to handle the mock API. |
| `package.json` | Project metadata and dependencies, primarily `json-server`. |
| [`api/`](api/) | Contains mock data files (e.g., [`api/database.json`](api/database.json)) used by `json-server`. |
| [`src/`](src/) | Root directory for all client-side application code and assets. |

### Client-Side Structure (`src/`)

| Directory | Purpose |
| :--- | :--- |
| `src/index.html` | The main HTML container file for the SPA. |
| [`src/scripts/`](src/scripts/) | Contains all JavaScript application logic and components. |
| [`src/styles/`](src/styles/) | Contains all CSS files for styling components and layouts. |
| [`src/audio/`](src/audio/) | Static sound files used for interactive features. |
| [`src/images/`](src/images/) | Static image assets (e.g., instrument pictures). |

### JavaScript Logic Structure (`src/scripts/`)

The JavaScript files are further organized by function:

| Directory | Purpose |
| :--- | :--- |
| `src/scripts/data/` | Centralized state management modules (e.g., `UserStateManager`, `InstrumentsStateManager`) and API settings. These modules handle all CRUD operations and state broadcasting. |
| `src/scripts/auth/` | Components responsible for user authentication views and logic (Login, Register). |
| `src/scripts/instruments/` | Views and components related to the instrument marketplace (List, Detail, Form). |
| `src/scripts/classes/` | Views and components related to music class listings and details. |
| `src/scripts/nav/` | Components for the navigation bar and header elements. |
| [`src/scripts/DukeChord.js`](src/scripts/DukeChord.js) | The main application component, acting as the Front Controller for view routing. |

## Core Component Relationships
 
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

# Frontend Technology Choice

The Duke & Chord Music application is intentionally built using **Vanilla JavaScript (ES6 modules)** and browser-native APIs. The application uses a stateless, client-side authentication mechanism suitable for a mock environment, relying on a Node.js-based JSON API mock server (`json-server`) and browser storage.

### Rationale for Vanilla JS
This approach offers several key benefits for this type of smaller, learning-focused application:
*   **Low Overhead and Simplicity:** Eliminating large framework dependencies results in a smaller bundle size and simpler tooling (only requiring Node.js and json-server).
*   **Direct Browser Interaction:** The architecture relies on direct DOM manipulation and native Custom Events (Observer Pattern) for state synchronization, promoting a deeper understanding of how modern browsers handle UI updates and decoupling.
*   **Fit for Purpose:** For an application with modest complexity and a mock API backend, the overhead of a full framework is unnecessary. The modular structure of the JS files provides sufficient organization without the need for framework-specific lifecycle management.

### Comparison to Frameworks
While frameworks offer advanced features (e.g., Virtual DOM for optimized updates, complex routing libraries, sophisticated component lifecycles), the Vanilla JS approach with custom state management was the preferred route here for maximum transparency and simplicity in the data flow.

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

# Development Setup

The Duke & Chord Music application requires Node.js and npm/yarn for running the mock API server and managing dependencies.

### Installation Instructions

1.  **Clone the Repository:**
    ```bash
    git clone [repository URL]
    cd duke-chord-workshop
    ```
2.  **Install Dependencies:**
    Use npm to install the required package (`json-server`):
    ```bash
    npm install
    ```

### Running the Application (Integrated Approach)

The standard way to run the application is via the integrated server script:
1.  **Start the Server:**
    Run the start script defined in package.json:
    ```bash
    npm start
    ```
    *This command executes `node server.js` which serves the static client files from the `src/` directory and mounts the JSON mock API on port 5002.*

2.  **Access the Application:**
    Access the application in your web browser: `http://localhost:5002`

### Running the API Server Directly (For Testing Only)

If you only need to run the mock REST API without serving the frontend HTML/JS files, you can execute `json-server` directly. Note that the client application will not fully function without the static file server:

```bash
# Assuming json-server is locally available via npx
npx json-server --watch api/database.json --port 5002
```
# Main Features
*   **User Authentication:** Handles user login, registration, and persistent session management.
*   **Instrument Marketplace:** Allows users to view, filter, and see detailed information about available instruments. Includes a form for users to submit instruments for sale.
*   **Educational Resources:** Provides listings and details for music classes, fetched from the API and associated with musicians/instructors.
*   **Interactive Controls:** Features include toggling sound effects on and off for a richer instrument interaction experience.
*   **Staff Directory:** Provides an "About" view that lists employees/staff members.
