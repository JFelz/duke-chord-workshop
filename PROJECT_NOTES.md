# Document:

## What you learned about the codebase
+ It is a client-side JavaScript Single Page Application (SPA) designed to serve as a platform for musical instruments and educational resources.
+ It uses a local JSON server (`json-server`) running on Node.js to mock a backend API, managing data related to users, instruments, instrument types, and music classes.
+ The approach for the technology choice:
  - **Low Overhead and Simplicity:** Eliminating large framework dependencies results in a smaller bundle size and simpler tooling (only requiring Node.js and json-server).
  - **Direct Browser Interaction:** The architecture relies on direct DOM manipulation and native Custom Events (Observer Pattern) for state synchronization, promoting a deeper understanding of how modern browsers handle UI updates and decoupling.
  - **Fit for Purpose:** For an application with modest complexity and a mock API backend, the overhead of a full framework is unnecessary. The modular structure of the JS files provides sufficient organization without the need for framework-specific lifecycle management.
+ Main features are:
  - **User Authentication:** Handles user login, registration, and persistent session management.
  - **Instrument Marketplace:** Allows users to view, filter, and see detailed information about available instruments. Includes a form for users to submit instruments for sale.
  - **Educational Resources:** Provides listings and details for music classes, fetched from the API and associated with musicians/instructors.
  - **Interactive Controls:** Features include toggling sound effects on and off for a richer instrument interaction experience.
  - **Staff Directory:** Provides an "About" view that lists employees/staff members.
+ The code is heavily modularized using JavaScript ES6 modules
+ It has a centralized State Management pattern
+ The DukeChord.js module acts as the front controller, responsible for determining the view to render.
+ The project adheres to a clear, feature-based directory structure to separate responsibilities between the backend mock server, the client-side single-page application (SPA), and configuration files.
+ Data movement in Duke & Chord Music is unidirectional and follows a clear lifecycle: from persistence, through state management, and finally to the user interface, mediated by asynchronous operations and custom events.
+ Multiple design patterns: Observer pattern, Component Pattern, Controller Pattern
+ Core Component Relationships and how components relate with eachother and data pass through them
## How AI assistance changed your workflow
+ It changed it in many ways. It changed it by increasing development speed, understanding architecture design, codebases, and overview of programming concepts that were hard to grasp and see be used practically. My workflow has given me the edge I needed to succeed faster. I now have a more architect point-of-view when I code instead of how to write every little feature from scratch.
## Challenges you encountered
+ Challenges I encountered were times when I tried to use the LLM to re-iterate on a section of code or text it just generated. It either made it worse or changed it completely even when trying to be thorough with the context prompt. This created fear of writing vague prompts that generated more problems.
+ Another challenge was figuring out how to keep the memory bank up to date with progress. It was so easy to continue code implementation and miss out on documenting the progress you were making. I could easily write a prompt to add in the progress but it sacrificed details I would have otherwise had if I did it incrementally.
## Strategies that worked well
+ Strategies that worked well were utilizing the “quiz” method to help the developer grasp the concepts of what they were building.
+ Iterative development was extremely helpful. To use the engineering problem-solving method of taking a big idea and breaking it down into smaller tasks to tackle.
  - Start Small,
  - Test Each Step,
  - Ask Clarifying Questions,
  - Request Modifications,
  - Document Your Progress.
+ Utilizing Ask Mode and Architect Mode to help understand codebases and build documentation. It obliterated the obstacle in my path when trying to learn new codebases.
+ Utilizing memory banks and markdown files to reference prompts persistently across files, projects, and different chats.
+ Utilizing ai agents to switch between different modes to work for you on different tasks
+ Asking the LLM to provide an explanation at the end of its response explaining gaps of understanding and confusion so you can write better prompts.
+ Asking the LLM what kind of prompt it would write itself and explain how each section helps it understand context and fill any gaps.
## Areas where AI was most/least helpful
+ Least helpful in the vagueness of your prompts. It’s easy to want to accelerate the process to the end goal by quickly replying to it but it only leads to a destructive, confusing end.
+ Without memory banks, your messages are recorded by 200K tokens at a time until they disapear forever. The LLM will hallucinate and give you completely random direction after not recalling the context information.

# Key Questions to Answer:

## How did using an agentic AI tool compare to working alone?
+ It honestly made it less lonely as it felt more like peer programming. It also sped up my own development.
## What was surprising about how the AI understood the code?
+ I wasn’t necessarily surprised as I knew it was trained off of immense data and the prediction capabilities are impressive.
## Where did you need to correct or refine AI suggestions?
+ After realizing it was generating outputs based off assumptions, I realized the context wasn’t thorough. This caused hallucinations.
+ Code generations
+ Not letting it run off on building what it wants but keeping it controlled through iterative development
## How did you verify AI-generated code was correct?
+ By making it explain its conclusion and how it got there.
+ Already existing foundational knowledge.
