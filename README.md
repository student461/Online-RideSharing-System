# Online Ride-Sharing — Full System

This repository contains the **full Online Ride-Sharing system**, composed of:

1. **Rider App** (`/OnlineRIdesharing_Rider`) — Rider-facing frontend built with Next.js.  
2. **Driver App** (`/OnlineRIdesharing_Driver`) — Driver-facing frontend with vehicle verification.  
3. **Server** (`/OnlineRIdesharing_Server`) — Core backend, with:
   - **STS4 Spring Boot server** for WebSocket connections, persistence, live updates.
   - **Matching & Processing TCP server** for route generation, pricing, and driver allocation algorithms.

---

## 🚀 Features

- **Rider side**:
  - Book rides, track vehicles on map (Leaflet).
  - Address search & geocoding with **Photon API**.
  - Route optimization & alternate routes with **OSRM**.
  - Traffic simulation for realistic ride visuals.
  - Payment with **Razorpay test keys** + in-app wallet.
  - Vehicle classification (bike, sedan, SUV, etc.).

- **Driver side**:
  - Sign in with Clerk.
  - **Vehicle verification** before ride acceptance.
  - Dashboard for ride requests, accept/decline, history.
  - Wallet balance & earnings tracking.

- **Server side**:
  - **STS4 Spring Boot server**:
    - WebSocket session persistence & live updates.
    - Handles rider requests, driver notifications, cancellations.
  - **Matching TCP server**:
    - Route & ETA calculation (OSRM).
    - Multi-factor driver scoring:
      - Loyalty points
      - Acceptance ratio
      - Driver rating
      - Distance/ETA
    - Pricing strategies:
      - Surge pricing
      - Rating-based pricing
      - Vehicle-based pricing
    - Returns ranked driver list to STS4 → notifies drivers.
  - MongoDB persistence for users, vehicles, rides, wallets.

---

## 🏗️ Architecture

```

[Rider Frontend (Next.js)]         [Driver Frontend (Next.js)]
|                                 |
| WebSocket (via Clerk auth)      |
v                                 v
[STS4 Spring Boot Server]  <---->  [MongoDB]
|
| TCP
v
[Matching & Processing Server]
(pricing + driver ranking)
|
[OSRM]   [Photon]

```

---

## 📂 Repository Structure

```

root/
├── OnlineRIdesharing_Rider/      # Rider Next.js app
├── OnlineRIdesharing_Driver/     # Driver Next.js app
└── OnlineRIdesharing_Server/     # STS4 Spring Boot + Matching TCP server

````

Each subfolder contains its own `README.md` with setup instructions.

---

## ⚙️ Getting Started

### 1. Clone the repo
```bash
git clone https://github.com/your-org/online-ridesharing.git
cd online-ridesharing
````

### 2. Setup Rider app

See [`OnlineRIdesharing_Rider/README.md`](./OnlineRIdesharing_Rider/README.md)

### 3. Setup Driver app

See [`OnlineRIdesharing_Driver/README.md`](./OnlineRIdesharing_Driver/README.md)

### 4. Setup Server

See [`OnlineRIdesharing_Server/README.md`](./OnlineRIdesharing_Server/README.md)

---

## 🔑 Environment Variables

Each module has its own `.env.local` or `application.properties`. Common variables:

* **Frontend (Rider/Driver)**:

  ```
  NEXT_PUBLIC_API_URL=http://localhost:8080/api
  NEXT_PUBLIC_OSRM_URL=http://localhost:5000
  NEXT_PUBLIC_PHOTON_URL=https://photon.komoot.io
  NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=clerk_pk_...
  NEXT_PUBLIC_RAZORPAY_KEY_ID=rzp_test_...
  ```

* **Server (STS4 + TCP)**:

  ```
  MONGODB_URI=mongodb://localhost:27017/onlineridesharing
  CLERK_API_KEY=clerk_sk_...
  RAZORPAY_KEY_ID=rzp_test_...
  RAZORPAY_KEY_SECRET=rzp_test_secret
  OSRM_BASE_URL=http://localhost:5000
  PHOTON_BASE_URL=https://photon.komoot.io
  TCP_SERVER_PORT=6000
  ```

---

## 🧪 Testing Flow

1. Rider requests ride (frontend → STS4 server).
2. STS4 forwards to Matching TCP server.
3. TCP server ranks drivers & returns results.
4. STS4 notifies drivers → driver accepts.
5. Ride is confirmed → Rider sees live updates.
6. Payment simulation via Razorpay test wallet.
7. Cancel/decline flows update driver loyalty & acceptance ratios.

---

## 🤝 Contributing

1. Fork this repo.
2. Create a feature branch: `git checkout -b feat/new-feature`.
3. Commit changes and push.
4. Open a Pull Request.

---

## 📄 License

MIT License. See [LICENSE](./LICENSE) for details.

---

## 🙌 Acknowledgements

* **Next.js** for frontend
* **Leaflet** for maps
* **OSRM** for routing
* **Photon** for geocoding
* **Clerk** for authentication
* **Razorpay** for payments (test mode)
* **MongoDB** for persistence
* **Spring Boot (STS4)** for backend

