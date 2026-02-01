## 🔗 Repositories

- **Backend** [smart-fleet-insight-backend](https://github.com/massaka99/smart-fleet-insight-backend)
- **Simulator** [smart-fleet-insight-simulator](https://github.com/massaka99/smart-fleet-insight-simulator)
- **Frontend** [smart-fleet-insight-frontend](https://github.com/massaka99/smart-fleet-insight-frontend)

# Sådan kører du hele Smart Fleet Insight-stakken

Kør alle kommandoer fra roden `C:\path\to\root`.

Alle nødvendige compose-filer og OSRM-data er allerede i zip-filen – ingen ekstra downloads eller migrationer er påkrævet (databasen er hosted).

## Krav

- Docker Desktop med Compose v2.
- .NET 8 SDK (for at køre API'en lokalt).
- Node.js 18+ (anbefalet 20) og npm.

## Hurtigt overblik over mapper

- **smart-fleet-insight-simulator/** – Docker-stack med Mosquitto (MQTT), OSRM og telemetri-simulator.
- **smart-fleet-insight-backend/** – .NET 8 API på port 8080.
- **smart-fleet-insight-frontend/** – Vite/React-frontend på port 5173.

## 0) Opret det delte Docker-netværk (første gang)

Begge compose-filer forventer netværket `smartfleet_shared` som eksternt. Opret det én gang:

```powershell
cd C:\path\to\root
docker network create smartfleet_shared
```

## 1) Start simulatoren (MQTT + OSRM + telemetri)

Kør fra `smart-fleet-insight-simulator/`:

```bash
cd smart-fleet-insight-simulator
docker compose up --build
```

- Mosquitto: localhost:1883 (TCP) og localhost:9001 (WebSocket)
- OSRM: http://localhost:5001
- Telemetri: publicerer på fleet/telemetry

## 2) Start backend via Docker

Kør fra `smart-fleet-insight-backend/`:

```bash
cd smart-fleet-insight-backend
docker compose up --build
```

API'et kører på http://localhost:8080 og er allerede konfigureret til hosted database og simulatorens MQTT.

## 3) Start frontend (Vite/React)

Kør fra `smart-fleet-insight-frontend/app/`:

```bash
cd smart-fleet-insight-frontend/app
npm install
npm run dev # http://localhost:5173
```

`.env` ligger allerede i mappen og peger mod backend.

## 4) Login oplysninger (Admin)

admin@smartfleet.local  
Admin123!
