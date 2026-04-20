# SmartVenue

SmartVenue is a production-grade intelligent crowd and event management platform designed for large-scale sporting venues.

## Features Built
- **Backend**: Express + TS + Prisma + Postgres + Redis. Real-time connections via `Socket.IO`. Background CRON worker for crowd simulation.
- **Frontend**: Vite + React 18 + TS + TailwindCSS. Separate dashboards for Attendee, Staff, Vendor, and Admin.
- **Data Layer**: Dockerized Postgres with robust relationship mapping (schema includes tickets, users, zones, events, SOS incidents).

## Running the Application

### 1. Prerequisites
- Docker & Docker Compose installed.

### 2. Setup
Check the `.env` file in the root directory. Ensure that Stripe keys or other credentials you intend to use are populated. Note: I've configured `.env.example` placeholder keys for STRIPE. 

> Note: To use your **specific SVG map**, replace `frontend/public/venue-map.svg` with your SVG file.

### 3. Launch with Docker
Run the unified multi-tenant architecture locally using Docker:

```bash
docker-compose up --build
```

### 4. Database Seeding (First-time only)
Once the containers are running and PostgreSQL is healthy, execute the Prisma migration and seed script to populate sample zones and users:

```bash
docker exec -it smartvenue-backend npx prisma migrate dev --name init
docker exec -it smartvenue-backend npm run seed
```

### 5. Access Dashboards
Open `http://localhost:3000` or `http://localhost` (Nginx proxy) to access the application.

Mock login emails for different roles:
- **Attendee**: `attendee1@smartvenue.local` (Password: `password`)
- **Vendor**: `vendor@smartvenue.local`
- **Staff**: `staff@smartvenue.local`
- **Admin**: `admin@smartvenue.local`

## Live Features to Test
- **Crowd Heatmap**: Check the Attendee "Map" tab or Staff "Heatmap" tab to see auto-refreshing zone occupancies calculated by the backend CRON job.
- **SOS Alerting**: The Attendee can trigger an SOS which is broadcasted live to the Staff Dashboard via WebSockets.
- **Vendor Kanban**: Move mock orders between "Placed" and "Ready".
- **Admin Metrics**: View real-time visual capacity charts on the Admin Dashboard interface.
