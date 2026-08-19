# 🔄 Retrospective Board

A Salesforce DX application for running team retrospectives, collecting
feedback, and voting on ideas in an organized and collaborative board.

Built with **Apex**, **Lightning Web Components (LWC)**, and
**Salesforce DX (SFDX)**.

------------------------------------------------------------------------

## ✨ Overview

The **Retrospective Board** helps teams conduct structured
retrospectives by organizing feedback into boards, sections, and
individual items.

Typical retrospective sections can include:

-   ✅ What went well
-   ⚠️ What could be improved
-   💡 Action items
-   🚀 Ideas for the next sprint

Team members can add feedback items and interact with them through
voting/like functionality.

------------------------------------------------------------------------

## 🏗️ Architecture

The application follows a simple Salesforce backend/frontend
architecture:

``` text
┌─────────────────────────────────────┐
│       Lightning Web Components      │
│                                     │
│  boards/       → Board list/create  │
│  boardDetail/  → Board details      │
└──────────────────┬──────────────────┘
                   │
                   │ @AuraEnabled
                   ▼
┌─────────────────────────────────────┐
│          Apex Backend               │
│                                     │
│       BoardController.cls           │
│                                     │
│  • getBoards()                      │
│  • saveBoard()                      │
│  • getBoardDetails()                │
└──────────────────┬──────────────────┘
                   │
                   │ SOQL / DML
                   ▼
┌─────────────────────────────────────┐
│        Salesforce Data Model        │
│                                     │
│  Board__c                            │
│      │                              │
│      └── Board_Section__c            │
│              │                      │
│              └── Board_Section_Item__c│
└─────────────────────────────────────┘
```

------------------------------------------------------------------------

## 🧰 Tech Stack

  Technology                           Purpose
  ------------------------------------ ------------------------------------------
  **Salesforce DX (SFDX)**             Project and development environment
  **Apex**                             Backend logic and Salesforce data access
  **Lightning Web Components (LWC)**   Frontend UI
  **JavaScript**                       LWC client-side logic
  **XML Metadata**                     Salesforce component configuration
  **SOQL**                             Querying Salesforce data
  **Jest**                             LWC unit testing
  **ESLint**                           JavaScript/LWC linting
  **Prettier**                         Code formatting
  **Husky**                            Git hooks
  **API v56.0**                        Salesforce metadata/API version

------------------------------------------------------------------------

## 📂 Project Structure

``` text
retrospective-board/
│
├── force-app/
│   └── main/
│       └── default/
│           │
│           ├── classes/
│           │   └── BoardController.cls
│           │
│           └── lwc/
│               ├── boards/
│               │   ├── boards.html
│               │   ├── boards.js
│               │   └── boards.js-meta.xml
│               │
│               └── boardDetail/
│                   ├── boardDetail.html
│                   ├── boardDetail.js
│                   └── boardDetail.js-meta.xml
│
├── config/
│   └── project-scratch-def.json
│
├── scripts/
│   ├── apex/
│   └── soql/
│
├── package.json
├── sfdx-project.json
└── README.md
```

------------------------------------------------------------------------

## 🗃️ Data Model

The application uses three custom Salesforce objects.

### `Board__c`

Represents a retrospective board.

Examples:

-   Sprint 12 Retrospective
-   Q3 Team Retrospective
-   Project Alpha Retrospective

### `Board_Section__c`

Represents a category/section within a board.

Examples:

-   What went well
-   What didn't go well
-   What should we improve?

### `Board_Section_Item__c`

Represents an individual feedback item submitted under a section.

Each item can also maintain a **like/vote count**.

### Relationship

``` text
Board__c
   │
   ├── Board_Section__c
   │       │
   │       ├── Board_Section_Item__c
   │       ├── Board_Section_Item__c
   │       └── Board_Section_Item__c
   │
   └── Board_Section__c
           │
           └── Board_Section_Item__c
```

------------------------------------------------------------------------

## ⚙️ Core Functionality

### 1. View Boards

The `boards` LWC retrieves available retrospective boards through:

``` text
BoardController.getBoards()
```

The component displays the boards and allows users to select a board.

### 2. Create a Board

Users can create a new retrospective board and define its sections.

The request is handled by:

``` text
BoardController.saveBoard()
```

The Apex controller persists the board and its associated sections.

### 3. View Board Details

Selecting a board opens the `boardDetail` component.

It retrieves the board's sections and nested feedback items using:

``` text
BoardController.getBoardDetails()
```

### 4. Feedback & Voting

Users can organize feedback into sections and interact with individual
feedback items using the item's like/vote count.

------------------------------------------------------------------------

## 🔌 Apex Controller

`BoardController.cls` acts as the main backend controller for the
application.

It exposes three methods using `@AuraEnabled`:

  -----------------------------------------------------------------------
  Method                              Purpose
  ----------------------------------- -----------------------------------
  `getBoards()`                       Retrieves retrospective boards

  `saveBoard()`                       Creates a board and its sections

  `getBoardDetails()`                 Retrieves a board with sections and
                                      nested items
  -----------------------------------------------------------------------

This allows the Lightning Web Components to communicate with Salesforce
Apex securely through the standard LWC-Apex integration model.

------------------------------------------------------------------------

## 🖥️ Lightning Web Components

### `boards`

Responsible for:

-   Displaying available boards
-   Creating new boards
-   Calling Apex methods
-   Navigating to board details

### `boardDetail`

Responsible for:

-   Displaying the selected board
-   Showing sections
-   Displaying feedback items
-   Presenting voting/like information

------------------------------------------------------------------------

## 🧪 Testing

The project uses **Jest** for Lightning Web Component unit testing.

Testing dependencies include:

``` text
@salesforce/sfdx-lwc-jest
```

Run the test suite with:

``` bash
npm test
```

For watch mode:

``` bash
npm run test:unit:watch
```

For code coverage:

``` bash
npm run test:unit:coverage
```

> The exact npm scripts depend on the project's `package.json`.

------------------------------------------------------------------------

## 🧹 Code Quality

The project uses several development tools to maintain code quality.

### ESLint

Used for identifying JavaScript and LWC coding issues.

### Prettier

Used to keep source code consistently formatted.

### Husky

Used to run automated checks through Git hooks before commits.

------------------------------------------------------------------------

## 🚀 Getting Started

### Prerequisites

Make sure you have:

-   Salesforce CLI
-   Node.js and npm
-   A Salesforce Developer org or scratch org
-   Git
-   VS Code
-   Salesforce Extension Pack for VS Code

### Clone the Repository

``` bash
git clone <repository-url>
cd retrospective-board
```

### Authenticate with Salesforce

For a scratch org:

``` bash
sf org login web --alias retrospective-org
```

Or authenticate with an existing org using the Salesforce CLI.

### Create a Scratch Org

``` bash
sf org create scratch \
  --definition-file config/project-scratch-def.json \
  --alias retrospective-org
```

### Deploy the Project

``` bash
sf project deploy start
```

### Open the Org

``` bash
sf org open --target-org retrospective-org
```

------------------------------------------------------------------------

## 🔄 Development Flow

``` text
Create / Select Board
        ↓
Add Retrospective Sections
        ↓
Open Board Details
        ↓
Add Feedback Items
        ↓
Vote / Like Items
        ↓
Review Team Feedback
```

------------------------------------------------------------------------

## 🎯 Use Cases

This application can be used for:

-   🏃 Sprint retrospectives
-   👥 Team feedback sessions
-   📊 Project reviews
-   🔄 Agile ceremonies
-   💡 Idea collection
-   📈 Continuous improvement discussions

------------------------------------------------------------------------

## 🌟 Key Salesforce Concepts Demonstrated

This project demonstrates practical Salesforce development concepts
including:

-   Lightning Web Components
-   Apex controllers
-   `@AuraEnabled` methods
-   SOQL queries
-   Salesforce custom objects
-   Parent-child data relationships
-   DML operations
-   Salesforce DX project structure
-   Scratch org development
-   LWC unit testing
-   Salesforce metadata
-   JavaScript-based frontend development

------------------------------------------------------------------------

## 🔮 Future Improvements

Potential enhancements include:

-   🔐 User authentication and role-based access
-   👤 Displaying feedback author information
-   🗳️ Multiple voting reactions
-   🔎 Search and filtering
-   📊 Retrospective analytics
-   📱 Improved mobile responsiveness
-   ✏️ Edit and delete feedback
-   📌 Action-item tracking
-   🔔 Notifications
-   📤 Export retrospective results

------------------------------------------------------------------------

## 📸 Suggested Demo Flow

For showcasing the project in a portfolio or interview:

``` text
1. Open the Retrospective Board
2. Create a new board
3. Add sections such as:
      • What went well?
      • What could improve?
      • Action items
4. Open the board
5. Add feedback items
6. Vote on important feedback
7. Review the final retrospective
```

------------------------------------------------------------------------

## 👨‍💻 Project Highlights

**Backend:** Apex + SOQL\
**Frontend:** Lightning Web Components + JavaScript\
**Platform:** Salesforce\
**Architecture:** LWC → Apex → Salesforce Custom Objects\
**Testing:** Jest\
**Code Quality:** ESLint + Prettier\
**Development:** Salesforce DX + Scratch Orgs

------------------------------------------------------------------------

## 📄 License

This project is intended for learning, portfolio, and demonstration
purposes.

------------------------------------------------------------------------

## ⭐ If You Like This Project

Feel free to ⭐ the repository and use the project as a starting point
for building Salesforce-based collaboration and productivity
applications.
