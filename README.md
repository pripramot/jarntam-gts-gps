# jarntam-gts-gps

# GTS Alpha: Vehicle Tracking & Forensics System
 
![Classification](https://img.shields.io/badge/Classification-CONFIDENTIAL-red)
![Status](https://img.shields.io/badge/Status-Development-yellow)
![TypeScript](https://img.shields.io/badge/TypeScript-5.3-blue)
 
**Codename:** GTS-Alpha
**Purpose:** High-speed vehicle tracking with AI-powered prediction and digital forensics capabilities
 
## 🎯 Core Features
 
### 1. Real-Time Vehicle Tracking
- **Kalman Filter Prediction**: Predict vehicle positions when GPS signal is lost/delayed
- **Multi-Source Tracking**: GPS, GSM, WiFi triangulation, and camera detection
- **Sub-second Latency**: Firebase real-time sync for instant location updates
- **Velocity Vector Analysis**: Speed and heading calculations with 99% accuracy
 
### 2. Predictive Analytics
- **Mathematical Model**: Implements state-space estimation using Kalman Filter
  ```
  State Estimate: x̂(k|k) = x̂(k|k-1) + K(z(k) - Hx̂(k|k-1))
  Kalman Gain: K = P H^T (H P H^T + R)^-1
  ```
- **Position Prediction**: Forecast coordinates up to 5 minutes ahead
- **Confidence Scoring**: 0-100% confidence level for each prediction
 
### 3. Geofencing & Alerts
- **Circle & Polygon Geofences**: Define restricted or monitored areas
- **Real-time Breach Detection**: Instant alerts on entry/exit/dwell
- **Multi-level Severity**: Info, Warning, Critical alert classification
- **Smart Notifications**: Reduce false positives with pattern recognition
 
### 4. Digital Forensics
- **File Analysis**: Scan for dangerous patterns, hardcoded secrets
- **Evidence Management**: Secure storage in Supabase with encryption
- **Audit Trail**: Complete tracking history with timestamps
 
## 🏗️ Architecture
 
```
┌─────────────────────────────────────────────────────────────┐
│                     GTS Alpha System                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │   GPS/GSM    │───▶│ Kalman Filter│───▶│  Prediction  │  │
│  │   Tracker    │    │   Engine     │    │   Results    │  │
│  └──────────────┘    └──────────────┘    └──────────────┘  │
│         │                     │                    │         │
│         ▼                     ▼                    ▼         │
│  ┌──────────────────────────────────────────────────────┐  │
│  │          Firebase Realtime Database                   │  │
│  │          (Sub-second latency sync)                    │  │
│  └──────────────────────────────────────────────────────┘  │
│         │                                                    │
│         ▼                                                    │
│  ┌──────────────────────────────────────────────────────┐  │
│  │           Supabase PostgreSQL + PostGIS               │  │
│  │    (Vehicle data, locations, alerts, sessions)        │  │
│  └──────────────────────────────────────────────────────┘  │
│         │                                                    │
│         ▼                                                    │
│  ┌──────────────────────────────────────────────────────┐  │
│  │         Google Maps Platform (Visualization)          │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```
 
## 📦 Tech Stack
 
| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Runtime** | Node.js + TypeScript | Type-safe backend |
| **Database** | Supabase (PostgreSQL + PostGIS) | Geospatial data storage |
| **Real-time** | Firebase Realtime Database | Sub-second location sync |
| **Compute** | Google Cloud Run | Scalable serverless |
| **Maps** | Google Maps Platform | Satellite/traffic visualization |
| **AI/ML** | Vertex AI | Model training (optional) |
 
## 🚀 Quick Start
 
### Prerequisites
- Node.js 18+ and npm
- Supabase account (Pro tier recommended)
- Firebase project
- Google Cloud account (optional, for Maps API)
 
### Installation
 
```bash
# 1. Clone the repository
git clone <repository-url>
cd gts-forensics
 
# 2. Install dependencies
npm install
 
# 3. Configure environment variables
cp .env.example .env
# Edit .env with your Supabase and Firebase credentials
 
# 4. Set up database
# Run database/schema.sql in your Supabase SQL editor
 
# 5. Build the project
npm run build
 
# 6. Start the system
npm start
```
 
## 📖 Usage
 
### CLI Commands
 
```bash
# Analyze a file for forensic artifacts
gts-forensics analyze <filepath> --verbose --output report.json
 
# Scan directory for suspicious patterns
gts-forensics scan <directory> --recursive --extensions js ts
 
# Track a vehicle (real-time)
npm run track -- --vehicle ABC-1234
 
# Start API server
npm run server
```
 
### API Endpoints (Coming Soon)
 
```bash
POST   /api/vehicles           # Register new vehicle
GET    /api/vehicles/:id       # Get vehicle details
POST   /api/locations          # Submit location update
GET    /api/predict/:id        # Get predicted position
POST   /api/geofences          # Create geofence
GET    /api/alerts             # Get unacknowledged alerts
```
 
## 🧮 Kalman Filter Implementation
 
### State Vector
```
x = [lat, lng, vx, vy]
```
- `lat`, `lng`: Current position (degrees)
- `vx`, `vy`: Velocity components (degrees/second)
 
### Prediction Step
```typescript
// Predict next position when GPS signal is lost
const prediction = kalmanFilter.predict(timeStep);
 
console.log(`Predicted: ${prediction.lat}, ${prediction.lng}`);
console.log(`Confidence: ${prediction.confidence}%`);
console.log(`Speed: ${prediction.velocity.speed} km/h`);
console.log(`Heading: ${prediction.velocity.heading}°`);
```
 
### Update Step
```typescript
// Update filter with new GPS measurement
const corrected = kalmanFilter.update({
  lat: 13.7563,
  lng: 100.5018,
  timestamp: Date.now()
});
```
 
## 🗄️ Database Schema
 
### Key Tables
- **vehicles**: Vehicle registry with current location and status
- **vehicle_locations**: Historical location points (time-series)
- **tracking_sessions**: Track start/end with statistics
- **geofences**: Circular or polygon restricted zones
- **vehicle_alerts**: Security/operational alerts
- **prediction_results**: Kalman filter predictions cache
 
### Spatial Queries
```sql
-- Find all vehicles within 5km radius
SELECT * FROM vehicles
WHERE ST_DWithin(
  last_location,
  ST_SetSRID(ST_MakePoint(100.5018, 13.7563), 4326)::geography,
  5000
);
```
 
## ⚡ Performance Optimization
 
### Budget Management
- **Remaining Credits**: 17,048 THB (Google Cloud)
- **Strategy**: Use cached maps, local processing where possible
- **Async Operations**: All Python scripts use FastAPI/AsyncIO
 
### Speed Targets
- **Unit Tests**: < 50ms per test
- **Integration Tests**: < 500ms per test
- **Location Update**: < 100ms end-to-end
- **Prediction**: < 50ms compute time
 
## 🔒 Security & Compliance
 
### Data Protection
- ✅ Row Level Security (RLS) on Supabase
- ✅ Encrypted credentials in environment variables
- ✅ Audit logging for all operations
- ✅ GDPR-compliant data retention policies
 
### Access Control
- **Authorized Personnel Only**: Law enforcement use only
- **API Authentication**: JWT tokens required
- **Role-Based Access**: Admin, Operator, Viewer roles
 
## 📊 Monitoring & Maintenance
 
### Regular Tasks
- **Weekly**: Review flaky tests, update dependencies
- **Monthly**: Analyze coverage trends, optimize queries
- **Quarterly**: Audit database performance, clean old data
 
### Metrics to Track
- GPS signal quality (accuracy < 10m target)
- Prediction accuracy (RMSE < 50m target)
- System uptime (99.9% SLA)
- Alert response time (< 5 seconds)
 
## 🛠️ Development
 
### Project Structure
```
gts-forensics/
├── src/
│   ├── tracking/          # Kalman Filter & tracking logic
│   ├── database/          # Supabase client
│   ├── analyzers/         # File forensics
│   ├── scanners/          # Directory scanning
│   ├── reporters/         # Report generation
│   ├── types/             # TypeScript interfaces
│   └── index.ts           # CLI entry point
├── database/
│   └── schema.sql         # Supabase schema
├── tests/                 # Unit & integration tests
├── dist/                  # Compiled JavaScript
└── README.md
```
 
### Build & Test
```bash
npm run build          # Compile TypeScript
npm run watch          # Watch mode for development
npm run clean          # Remove dist folder
npm run dev            # Run in development mode
```
 
## 🤝 Contributing
 
This is a **CONFIDENTIAL** law enforcement project. External contributions are not accepted.
 
### Internal Team Guidelines
1. Follow TypeScript strict mode
2. Write unit tests for all new features
3. Document complex algorithms with comments
4. Use conventional commits (feat, fix, docs, etc.)
 
## 📄 License
 
MIT License (for authorized organizations only)
 
---
 
**Developed by**: GTS Alpha Team
**Last Updated**: January 2026
**Status**: Active Development
**Contact**: [Classified]
