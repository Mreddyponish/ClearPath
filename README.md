# ClearPath - Emergency Route Optimization System

ClearPath is a MERN-stack web application designed to demonstrate emergency route planning using road network data and graph algorithms. It helps users identify suitable routes for ambulance, fire service, and police vehicles based on configured routing priorities.

> **Safety Notice:** ClearPath is a hackathon prototype that uses sample road data. It is not a live emergency navigation service and does not provide verified real-time traffic information. In a real emergency, contact your official emergency number.

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [System Architecture](#system-architecture)
- [How It Works](#how-it-works)
- [Routing Algorithm](#routing-algorithm)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
- [API Overview](#api-overview)
- [Future Enhancements](#future-enhancements)
- [Limitations](#limitations)
- [Contributing](#contributing)
- [License](#license)

## Overview

During emergencies, choosing a suitable route can be difficult when roads have different distances, traffic conditions, or blockages.

ClearPath provides a web interface where users can select an emergency type, a starting location, and a destination. The backend processes the request, evaluates the available road network, and calculates a route using a graph algorithm.

The application demonstrates how frontend development, backend APIs, database management, and route optimization can work together in a single system.

## Problem Statement

Emergency vehicles may need to travel through road networks containing traffic delays or blocked roads. Selecting a route based only on distance may not always produce the most suitable result.

ClearPath addresses this problem by demonstrating a route-planning approach that considers road information and configurable priorities for different emergency services.

## Objectives

- Develop a web-based emergency route-planning application.
- Build a responsive user interface using React.
- Create backend REST APIs using Node.js and Express.js.
- Store and manage application data using MongoDB.
- Apply graph algorithms to calculate suitable routes.
- Demonstrate how blocked roads can be excluded from route calculations.
- Provide a foundation for future integration with real-time traffic and location services.

## Features

The project is designed around the following capabilities. Availability depends on the features implemented and configured in the current version.

- **Emergency route planning:** Select a starting location, destination, and emergency type.
- **Emergency-specific routing:** Configure routing priorities for ambulance, fire service, and police vehicles.
- **Road network management:** Represent locations as nodes and roads as edges.
- **Blocked-road handling:** Exclude roads marked as blocked from route calculations.
- **Map visualization:** Display locations and calculated routes through a map interface.
- **User authentication:** Support registration, login, and protected access where configured.
- **Route history:** Maintain records of route requests where implemented.
- **Administrative tools:** Manage road network information where implemented.

## Technology Stack

| Technology | Purpose |
|---|---|
| React.js | Builds the frontend user interface |
| Vite | Runs and builds the frontend application |
| Node.js | Executes backend JavaScript |
| Express.js | Creates REST API endpoints |
| MongoDB | Stores application and road network data |
| Mongoose | Models and manages MongoDB documents |
| Dijkstra's Algorithm | Calculates a route with the lowest configured cost |
| React-Leaflet / Leaflet | Displays map information |
| Axios | Sends HTTP requests between frontend and backend |
| JWT | Supports user authentication |
| bcryptjs | Hashes user passwords |

## System Architecture

The application follows a client-server architecture.

```text
User
  |
  v
React Frontend
  |
  | HTTP / REST API
  v
Node.js + Express Backend
  |
  +---- MongoDB Database
  |     Locations, roads, users,
  |     and route records
  |
  +---- Routing Algorithm
        Dijkstra's Algorithm
  |
  v
Route Result
  |
  v
Frontend Map and Route Details
```

### Main Components

**Frontend:** Provides the screens and forms that users interact with.

**Backend:** Receives requests, validates inputs, accesses stored information, and executes route calculations.

**Database:** Stores road network information and other application records.

**Routing algorithm:** Evaluates the available roads and determines a route according to the configured cost model.

**Map interface:** Presents the calculated route visually.

## How It Works

1. The user opens the route planner.
2. The user selects the emergency type, starting location, and destination.
3. The frontend sends the request to the backend API.
4. The backend retrieves the required road network information from MongoDB.
5. The routing algorithm evaluates available roads and excludes blocked roads.
6. The backend returns the route result to the frontend.
7. The frontend displays the route and available journey information.

## Routing Algorithm

ClearPath uses graph-based route planning.

A road network can be represented as a graph:

- **Nodes:** Locations such as junctions, hospitals, fire stations, and police stations.
- **Edges:** Roads connecting locations.
- **Weights:** Numerical costs assigned to roads based on the selected routing criteria.

Dijkstra's algorithm repeatedly explores the reachable location with the lowest known accumulated cost until it determines the lowest-cost route to the destination.

The configured routing priorities are intended to support different emergency requirements:

- **Ambulance:** Prioritize estimated travel time and apply a stronger penalty for heavy traffic.
- **Fire service:** Prioritize distance with a moderate traffic penalty.
- **Police:** Balance distance and estimated travel time.

Roads marked as blocked are excluded from the route calculation.

The algorithm's output depends on the stored road data and the cost rules configured in the application. The calculated route is not a guarantee of the fastest real-world journey.

## Project Structure

The project uses a separate frontend and backend.

```text
ClearPath/
├── client/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── context/
│   │   ├── styles.css
│   │   └── ...
│   ├── package.json
│   └── ...
├── server/
│   ├── config/
│   ├── models/
│   ├── controllers/
│   ├── routes/
│   ├── middleware/
│   ├── utils/
│   ├── .env.example
│   ├── server.js
│   └── package.json
├── package.json
└── README.md
```

Some folders may differ depending on the current project version.

## Prerequisites

Install the following before running the application:

- [Node.js](https://nodejs.org/) and npm
- [MongoDB Community Server](https://www.mongodb.com/try/download/community) running locally, or a MongoDB Atlas database
- [Git](https://git-scm.com/) for version control

## Installation and Setup

### 1. Clone the repository

Replace `YOUR_USERNAME` with your GitHub username.

```bash
git clone https://github.com/YOUR_USERNAME/ClearPath.git
cd ClearPath
```

If you have not uploaded the project to GitHub yet, open the extracted project folder directly in your terminal.

### 2. Install root dependencies

```bash
npm install
```

### 3. Install frontend dependencies

```bash
cd client
npm install
cd ..
```

### 4. Install backend dependencies

```bash
cd server
npm install
cd ..
```

### 5. Configure environment variables

Create a `.env` file inside the `server` directory.

Example for a local MongoDB database:

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/clearpath
JWT_SECRET=replace_with_a_long_random_secret
CLIENT_ORIGIN=http://localhost:5173
CONTACT_PHONE=
CONTACT_EMAIL=
```

Use the actual environment variable names expected by your backend code. If your project uses different names, follow the server configuration.

For MongoDB Atlas, replace `MONGO_URI` with your database connection string.

**Important:** Never commit your real `.env` file, database credentials, or JWT secret to GitHub.

### 6. Start MongoDB

If using local MongoDB, make sure the MongoDB service is running before starting the backend.

If using MongoDB Atlas, verify that the database user and network access settings permit the connection.

## Running the Application

Open a terminal in the project root and run the available development script:

```bash
npm run dev
```

If the root script does not start both applications, use two terminals.

**Terminal 1: Backend**

```bash
cd server
npm run dev
```

**Terminal 2: Frontend**

```bash
cd client
npm run dev
```

The usual development addresses are:

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:5000`

The actual addresses depend on your project configuration.

If you encounter errors, check that dependencies are installed, MongoDB is running, the environment variables are configured, and the scripts exist in the corresponding `package.json` files.

## API Overview

The following endpoints describe the intended API design. Confirm the actual routes in your backend source code before relying on them.

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/auth/register` | Register a user |
| POST | `/api/auth/login` | Authenticate a user |
| GET | `/api/auth/me` | Retrieve the current user's information |
| GET | `/api/locations` | Retrieve available locations |
| POST | `/api/locations` | Create a location, if authorized |
| GET | `/api/roads` | Retrieve road information |
| POST | `/api/roads` | Create a road, if authorized |
| PATCH | `/api/roads/:id/block` | Update a road's blocked status, if implemented |
| POST | `/api/routes/find` | Calculate a route |
| GET | `/api/routes/history` | Retrieve route history, if implemented |
| DELETE | `/api/routes/history/:id` | Delete a route history record, if implemented |
| GET | `/api/analytics/summary` | Retrieve analytics, if implemented |

Protected endpoints may require a valid authentication token and the appropriate user role.

## Future Enhancements

Potential improvements include:

- Integration with verified live traffic data.
- Real-time road closure updates.
- GPS-based current location selection.
- Alternative route recommendations.
- Improved travel-time estimation.
- Integration with authorized emergency service systems.
- Additional automated tests for the route algorithm and APIs.
- Deployment to a secure hosting environment.

## Limitations

- The current project is a demonstration and uses sample road network data.
- Live traffic and road closure information are not available unless separately integrated.
- Estimated travel times depend on configured data and rules.
- A calculated route does not guarantee real-world safety or the fastest arrival.
- The project is not a replacement for official emergency dispatch or navigation systems.

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Make and test your changes.
4. Commit your changes with a descriptive message.
5. Open a pull request describing the changes.

## License

No license has been specified yet. Add a `LICENSE` file before distributing the project for reuse. If you choose the MIT License, include the appropriate copyright notice and license text.

---

**ClearPath**  
Emergency Route Optimization System  
Developed as a hackathon project to demonstrate full-stack development and graph-based route planning.
