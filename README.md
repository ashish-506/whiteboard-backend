
# Collaborative Whiteboard Backend API

The backend infrastructure for a real-time collaborative whiteboard application. This service manages user authentication, persistent storage for canvas states, and handles high-frequency, low-latency WebSocket connections to synchronize drawing events seamlessly across multiple clients.

## Core Features
- **Real-Time Synchronization:** Utilizes Socket.io to broadcast drawing elements instantly to all connected users within isolated canvas rooms.
- **Secure Authentication:** Stateless JWT-based authentication pipeline with Bcrypt password hashing.
- **Canvas Access Control:** Strict permission modeling ensuring only canvas owners and explicitly whitelisted users can view or mutate a board.
- **Optimized Data Pipeline:** Decoupled architecture using Express REST APIs for heavy initial data loads/metadata operations, and WebSockets exclusively for transient, high-frequency stroke updates.

## Tech Stack
- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB Atlas (via Mongoose ODM)
- **WebSockets:** Socket.io
- **Security:** JSON Web Tokens (JWT) & CORS

## Local Setup & Installation

1. Clone the repository and navigate to the project root.
2. Install the required dependencies:
```bash
npm install

```


3. Boot up the server:
```bash
npm start

```



The server will initialize and listen for connections on `http://localhost:5000`.

## REST API Reference

### Authentication (`/users`)

* `POST /register` - Register a new user account.
* `POST /login` - Authenticate user credentials and return a signed JWT.
* `GET /me` - Retrieve the currently authenticated user's profile. *(Protected)*

### Canvas Management (`/canvas`)

*Note: All endpoints below require a valid JWT passed in the `Authorization: Bearer <token>` header.*

* `POST /create` - Instantiate a new blank canvas.
* `GET /list` - Fetch all canvases owned by or shared with the authenticated user.
* `GET /load/:id` - Retrieve the full vector state of a specific canvas.
* `PUT /update` - Persist the current canvas state to the database.
* `PUT /share/:id` - Grant canvas access to another user via their email address.
* `PUT /unshare/:id` - Revoke canvas access from a previously shared user.
* `DELETE /delete/:id` - Permanently delete a canvas (Owner only).

## WebSocket Event Architecture

* **`joinCanvas`**: Validates the user's JWT handshakes, verifies room permissions, and subscribes the client to the requested canvas channel.
* **`drawingUpdate`**: Ingests new vector element data from a client and immediately queues it for broadcasting.
* **`receiveDrawingUpdate`**: Emits real-time delta updates to all peers in the active room.
* **`loadCanvas`**: Transmits the base state of the canvas to a newly connected client upon a successful join.

## Deployment

This repository is configured for serverless deployment. A `vercel.json` file is included to map incoming API and WebSocket requests correctly through Vercel's Node.js runtime environment. Ensure that your cloud provider has the correct environment variables securely injected before deployment.

```

```