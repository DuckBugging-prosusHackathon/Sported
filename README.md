# Sported

**Sported** is an AI-powered sports matchmaking platform that automatically organizes sports events by finding compatible players, creating balanced teams, and scheduling games. The platform consists of three main components working together to provide a seamless experience for sports enthusiasts to connect and play.

## Overview

Sported uses artificial intelligence to solve the common problem of organizing sports matches. Instead of manually coordinating with friends, the AI agent automatically:

- Finds players in your area using geolocation clustering
- Matches compatible players based on skill level, preferences, and availability
- Creates balanced teams
- Schedules matches at convenient times and locations
- Sends notifications and questions to participants

## Architecture

The application is built as a monorepo with three main repositories:

```
Sported/
├── agent/      # AI matchmaking agent (Python)
├── client/     # Mobile application (React + Capacitor)
└── server/     # Backend API (Node.js + Express)
```

---

## 📱 Client (`/client`)

**A cross-platform mobile application for iOS and Android**

The client is a React-based mobile app built with Capacitor that provides the user interface for the Sported platform.

### Key Features

- **Authentication**: Firebase-based login and user registration
- **Onboarding**: User profile setup with skill levels, sport preferences, and calendar integration
- **Home Screen**: View upcoming matches, activity streaks, and friend activity
- **Match Organization**: Manually create matches by selecting friends, sport type, and time
- **Timeline**: Social feed with posts automatically generated from completed matches
- **Profile Management**: Update user profile, skills, preferences, and sport settings
- **Push Notifications**: Receive match invitations and question prompts from the AI agent
- **Native Features**: 
  - Geolocation for finding nearby players
  - Contacts integration for inviting friends
  - Haptic feedback for better UX

### Tech Stack

- **React 19** with TypeScript
- **Vite** for build tooling
- **Capacitor** for native iOS/Android support
- **Tailwind CSS** for styling
- **Zustand** for state management
- **Firebase** for authentication
- **Capacitor Plugins**: Push notifications, geolocation, contacts, haptics

### Getting Started

```bash
cd client
npm install
npm run dev          # Development mode (web)
npm run build        # Build for production
npm run cap:sync     # Sync with native projects
npx cap open ios     # Open in Xcode
npx cap open android # Open in Android Studio
```

---

## 🤖 Agent (`/agent`)

**An AI-powered matchmaking system that automatically organizes sports events**

The agent is a Python-based service that runs continuously, processing user clusters and organizing matches using AI agents.

### How It Works

1. **Clusters Users by Location**: Groups users geographically using K-means clustering on latitude/longitude coordinates to find players in the same area

2. **Coordinates Match Organization**: Uses an AI coordinator agent to:
   - Select compatible players from each cluster
   - Ask users about their availability and preferences via push notifications
   - Create balanced teams based on skill level, position, and compatibility
   - Schedule matches at suitable times and locations

3. **Creates Matches**: Automatically creates match records in the database with all participants, teams, field location, and scheduled time

### Components

- **Coordinator Agent**: The main AI agent (using LangChain) that orchestrates the matchmaking process. It uses LLM tools to ask questions, validate match plans, and organize teams.

- **User Agents**: Individual AI agents that represent each user. They answer questions about availability and preferences on behalf of the user.

- **Clustering Algorithm**: Uses scikit-learn's K-means to group users by location (latitude, longitude) and age to find players in nearby areas.

- **Database Integration**: Fetches user profiles and creates matches via the server API endpoints.

### Tech Stack

- **Python 3.x**
- **LangChain** for AI agent framework
- **OpenAI** for LLM capabilities
- **scikit-learn** for clustering algorithms
- **requests** for API calls to the server

### Getting Started

```bash
cd agent
pip install -r requirements.txt
python main.py
```

The agent runs continuously, processing clusters and organizing matches in cycles.

### Environment Variables

- `OPENAI_KEY`: Your OpenAI API key

---

## 🖥️ Server (`/server`)

**A REST API backend that powers the sports matchmaking platform**

The server is a Node.js/Express backend that handles all data persistence, authentication, and business logic for the platform.

### API Modules

- **`/api/auth`** - User authentication (signup, login, token verification)
- **`/api/profile`** - User profile management (create, update, retrieve profiles with location data)
- **`/api/calendar`** - Google Calendar integration (connect calendars, fetch schedules, manage availability)
- **`/api/matches`** - Match scheduling and management (create, retrieve, update, delete matches)
- **`/api/fields`** - Sports field/venue information management
- **`/api/posts`** - Social feed posts (auto-generated from completed matches, likes)
- **`/api/push`** - Push notification services (device registration, sending notifications, question prompts)

### Key Features

- **Authentication**: JWT-based authentication with bcrypt password hashing
- **Database**: MongoDB with Mongoose ODM
- **Push Notifications**: Firebase Admin SDK for sending push notifications to iOS and Android devices
- **Calendar Integration**: Google Calendar API integration for checking user availability
- **Background Services**: Auto-generates social posts from completed matches

### Tech Stack

- **Node.js** with **Express**
- **TypeScript** for type safety
- **MongoDB** with **Mongoose**
- **Firebase Admin SDK** for push notifications
- **Google APIs** for calendar integration
- **JWT** for authentication

### Getting Started

```bash
cd server
npm install
npm run dev    # Development mode with hot reload
npm run build  # Build TypeScript
npm start      # Production mode
```

The server runs on port 3000 by default (configurable via `PORT` environment variable).

### Environment Variables

- `MONGODB_URI`: MongoDB connection string
- `PORT`: Server port (default: 3000)
- `JWT_SECRET`: Secret key for JWT tokens
- `FIREBASE_PROJECT_ID`: Firebase project ID
- `GOOGLE_CLIENT_ID`: Google OAuth client ID
- `GOOGLE_CLIENT_SECRET`: Google OAuth client secret

---

## How It All Works Together

1. **Users** sign up and create profiles in the mobile app (client), providing their location, skill levels, and preferences.

2. **The Agent** continuously:
   - Fetches users from the server API
   - Clusters them by geographic location
   - For each cluster, uses AI agents to find compatible players
   - Asks users questions via push notifications (sent through the server)
   - Creates balanced teams and schedules matches
   - Creates match records via the server API

3. **The Server**:
   - Stores all user data, matches, and posts in MongoDB
   - Handles authentication and authorization
   - Sends push notifications to users when the agent has questions
   - Integrates with Google Calendar to check availability
   - Auto-generates social posts from completed matches

4. **Users** receive notifications, answer questions, and view their matches in the mobile app.

---

## Getting Started (Full Stack)

### Prerequisites

- Node.js (v18+)
- Python 3.x
- MongoDB database
- Firebase project (for authentication and push notifications)
- Google OAuth credentials (for calendar integration)
- OpenAI API key (for the agent)

### Setup

1. **Server Setup**:
   ```bash
   cd server
   npm install
   # Configure .env with MongoDB URI, JWT secret, Firebase credentials, etc.
   npm run dev
   ```

2. **Client Setup**:
   ```bash
   cd client
   npm install
   # Configure Firebase in src/config/firebase.ts
   npm run dev
   ```

3. **Agent Setup**:
   ```bash
   cd agent
   pip install -r requirements.txt
   # Configure .env with OPENAI_KEY
   python main.py
   ```

---

## Project Structure

```
Sported/
├── agent/                    # AI matchmaking agent
│   ├── main.py              # Main entry point
│   ├── src/
│   │   ├── coordinator_agent.py  # Main AI coordinator
│   │   ├── user_agent.py         # Individual user agents
│   │   └── database.py           # API client for server
│   ├── data_processing/     # Clustering algorithms
│   └── requirements.txt
│
├── client/                   # Mobile application
│   ├── src/
│   │   ├── screens/         # Main app screens
│   │   ├── components/      # Reusable components
│   │   ├── api/            # API client functions
│   │   ├── store/          # State management
│   │   └── config/         # Firebase config
│   ├── android/            # Android native project
│   ├── ios/               # iOS native project
│   └── package.json
│
└── server/                  # Backend API
    ├── src/
    │   ├── modules/        # Feature modules (auth, matches, etc.)
    │   ├── config/         # Database, Firebase config
    │   └── index.ts        # Main entry point
    └── package.json
```

---

## License

ISC

