# PetConnect: Location-Based Pet Social Network

A mobile-first social platform for pet owners to arrange playdates, share advice, and locate pet-friendly venues nearby.

## Tech Stack

- **Frontend**: React Native (Expo) + React Navigation
- **Backend**: Node.js (Express) + GraphQL (Apollo Server)
- **Database**: PostgreSQL + PostGIS (geospatial extension)
- **Auth**: JWT (JSON Web Tokens) with bcrypt password hashing
- **Deployment**: Docker + Docker Compose (for development), AWS (for production)

## Key Features

- Geofenced discovery of nearby pets and events using PostGIS
- Real-time chat with media sharing (images/videos)
- Pet profiles with vaccination records and breed info
- Community forum with topic tagging and upvoting
- Integrated mapping with pet-friendly venues (APIs)
- Push notifications for meetup reminders and alerts
- Moderation system with reporting and content filtering

## Getting Started

### Prerequisites

- Node.js (v16 or later)
- npm or yarn
- Expo CLI (for frontend)
- Docker and Docker Compose
- PostgreSQL with PostGIS extension

### Installation

1. Clone the repository
   ```bash
   git clone <repository-url>
   cd petconnect-location-based-pet-social-network
   ```

2. Install backend dependencies
   ```bash
   cd backend
   npm install
   ```

3. Install frontend dependencies
   ```bash
   cd ../frontend
   npm install
   ```

4. Set up environment variables
   - Create a `.env` file in the backend directory based on `.env.example`
   - Create an `.env` file in the frontend directory for Expo (if needed)

5. Start the services
   ```bash
   # Start PostgreSQL with PostGIS via Docker Compose
   docker-compose up -d

   # Start the backend
   cd backend
   npm run dev

   # Start the frontend (Expo)
   cd ../frontend
   npm start
   ```

### Environment Variables

#### Backend (`.env` in backend directory)
```
PORT=4000
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_db_password
DB_NAME=petconnect
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=7d
UPLOAD_DIR=./uploads
MAX_FILE_SIZE=5242880
EMAIL_SERVICE=gmail
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
MAPBOX_ACCESS_TOKEN=your_mapbox_access_token
FRONTEND_URL=http://localhost:3000
```

#### Frontend (`.env` in frontend directory, if needed)
```
EXPO_PUBLIC_API_URL=http://localhost:4000/graphql
```

### Project Structure

```
petconnect-location-based-pet-social-network/
├── backend/
│   ├── src/
│   │   ├── models/        # Database models
│   │   ├── resolvers/     # GraphQL resolvers
│   │   ├── schema/        # GraphQL schema
│   │   ├── server.js      # Entry point
│   │   └── db.js          # Database connection
│   ├── .env               # Environment variables
│   ├── .env.example       # Example environment variables
│   ├── package.json
│   └── docker-compose.yml
├── frontend/
│   ├── src/
│   │   ├── components/    # Reusable components
│   │   ├── navigation/    # Navigation configuration
│   │   ├── screens/       # Screen components
│   │   ├── services/      # API services
│   │   └── App.js         # Root component
│   ├── .env               # Environment variables (optional)
│   ├── package.json
│   └── ...
└── README.md
```

### API Documentation

The GraphQL API is available at `http://localhost:4000/graphql` when the server is running.
You can use GraphQL Playground to explore and test the API.

### Docker Compose

The `docker-compose.yml` file in the backend directory defines the following services:
- `postgres`: PostgreSQL database with PostGIS extension
- `redis`: Redis for caching (if implemented)
- `backend`: Node.js application

### Deployment

The application can be deployed to AWS using:
- **Backend**: Docker containers on ECS or AWS Lambda (via Serverless Framework)
- **Database**: Amazon RDS PostgreSQL with PostGIS extension
- **Storage**: Amazon S3 for media files
- **Frontend**: Expo's web hosting or standalone iOS/Android apps built with EAS Build

### Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### License

This project is licensed under the MIT License.

## Status (checkup 2026-08-18)
> Revisado na campanha de repo-checkup. Relatorio completo: `~/repo-checkup/reports/petconnect.md` (local do mantenedor, nao no repo).
- **Build/Install**: `npm ci` (backend, apos `backend/package-lock.json` adicionado no checkup) RC=0 — 96 pacotes, 0 vulnerabilidades; backend Node sem build step. Frontend React Native NAO instalavel (so `frontend/App.js`, sem `package.json`); Docker NAO testado (ausente no ambiente).
- **Smoke test**: `node src/server.js` sobe e `GET /` -> HTTP 200 ("PetConnect Location-Based Pet Social Network API"); `node --check src/server.js` OK.
- **Para rodar de ponta-a-ponta precisa de**: PostgreSQL + PostGIS, Redis e Docker Compose (segundo o README; o relatorio nao testou Docker por estar ausente neste ambiente); frontend Expo incompleto.
- **Inconsistencias conhecidas (README vs codigo)**: README referencia `.env` a partir de `.env.example`, mas `backend/.env.example` nao existia (criado no checkup); `docs/ci-guide.md` afirma que o CI roda `npm run lint && npm run build`, mas `package.json` nao define `lint` nem `build`; frontend incompativel com o README (cita Expo + `package.json` + `src/` completo, mas so ha `App.js`); nenhum workflow `.github/workflows/ci.yml` real.
- **Seguranca**: 0 vulnerabilidades (`npm audit`); nenhum segredo hardcoded (secret scan: api_key/secret/token/password/sk-/ghp_/AIza); `.env` gitignored.
- **Estado resumido**: backend verde (install + smoke `/`->200, 0 vulns); frontend RN incompleto e Docker nao validado neste ambiente — precisa de acao humana para completar o frontend e validar runtime com Postgres/Redis/Docker.

