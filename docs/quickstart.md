# Quick Start Guide

Get SignalRoot up and running in minutes with this comprehensive quick start guide.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (version 16 or higher)
- **Java** (version 17 or higher)
- **Maven** (version 3.6 or higher)
- **Docker** (recommended for database)
- **Git** for version control

## Option 1: Docker Quick Start (Recommended)

The fastest way to get SignalRoot running is with Docker:

```bash
# Clone the repository
git clone https://github.com/signalroot/signalroot.git
cd signalroot

# Start all services
docker-compose up -d

# Wait for services to be ready (about 2 minutes)
docker-compose logs -f
```

This will start:
- **Backend API** on http://localhost:8080
- **Frontend Dashboard** on http://localhost:3000
- **PostgreSQL Database** on port 5432
- **Redis Cache** on port 6379

## Option 2: Manual Installation

### 1. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies and build
mvn clean install

# Start the application
mvn spring-boot:run
```

The backend will start on `http://localhost:8080` with:
- API endpoints at `/api/*`
- Swagger documentation at `/swagger-ui.html`
- Health checks at `/actuator/health`

### 2. Database Setup

#### Using Docker (Recommended)
```bash
# Start PostgreSQL
docker run -d \
  --name signalroot-db \
  -e POSTGRES_DB=signalroot \
  -e POSTGRES_USER=your_db_username \
  -e POSTGRES_PASSWORD=your_secure_db_password \
  -p 5432:5432 \
  postgres:15
```

#### Manual Installation
```bash
# Install PostgreSQL 15+
# Create database and user
createdb signalroot
createuser your_db_username
psql -c "ALTER USER your_db_username PASSWORD 'your_secure_db_password';"
psql -c "GRANT ALL PRIVILEGES ON DATABASE signalroot TO your_db_username;"
```

### 3. Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

The frontend will start on `http://localhost:3000`.

## Configuration

### Environment Variables

Create a `.env` file in the backend root:

```env
# Database Configuration
DATABASE_URL=jdbc:postgresql://localhost:5432/signalroot
DATABASE_USERNAME=your_db_username
DATABASE_PASSWORD=your_secure_db_password

# Application Configuration
SERVER_PORT=8080
SERVICE_MODE=production

# Webhook Configuration
WEBHOOK_SECRET=your-secure-webhook-secret-generate-with-openssl-rand-hex-32
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK

# Notification Configuration
EMAIL_HOST=your-smtp-server.com
EMAIL_PORT=587
EMAIL_USERNAME=your-email@domain.com
EMAIL_PASSWORD=your-app-specific-password

# External Services
GITHUB_TOKEN=your-github-personal-access-token
PAGERDUTY_TOKEN=your-pagerduty-api-token
```

### Application Properties

Update `backend/src/main/resources/application.properties`:

```properties
# Server Configuration
server.port=8080

# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/signalroot
spring.datasource.username=your_db_username
spring.datasource.password=your_secure_db_password

# Service Mode
signalroot.service.mode=production

# Frontend URL
signalroot.frontend.url=http://localhost:3000
```

## Verify Installation

### 1. Check Backend Health

```bash
curl http://localhost:8080/actuator/health
```

Expected response:
```json
{"status":"UP"}
```

### 2. Test API Endpoints

```bash
# Test dogfooding scenarios
curl -X POST http://localhost:8080/api/dogfooding/run-all

# Test fake customer teams
curl http://localhost:8080/api/fake-customers/teams

# Test webhook processing
curl -X POST http://localhost:8080/webhooks/alerts/pagerduty \
  -H "Content-Type: application/json" \
  -d '{"messages":[{"type":"incident","incident":{"id":"INC123","status":"triggered","title":"Test Alert"}}]}'
```

### 3. Access Frontend

Open http://localhost:3000 in your browser. You should see:
- ✅ SignalRoot dashboard
- ✅ Incident management interface
- ✅ Service configuration options
- ✅ Webhook integration guides

## First Steps

### 1. Configure Your Organization

1. Navigate to **Services** page
2. Add your team information
3. Configure service mappings
4. Set up alert frequencies

### 2. Set Up Webhooks

1. Go to your monitoring tool (PagerDuty, CloudWatch, etc.)
2. Create webhooks pointing to:
   - PagerDuty: `http://localhost:8080/webhooks/alerts/pagerduty`
   - CloudWatch: `http://localhost:8080/webhooks/alerts/cloudwatch`
   - GitHub: `http://localhost:8080/webhooks/deploy/github`

### 3. Configure Notifications

1. Navigate to **Settings** → **Notifications**
2. Add Slack webhook URL
3. Configure email settings
4. Test notification delivery

### 4. Test Alert Processing

```bash
# Send a test alert
curl -X POST http://localhost:8080/webhooks/alerts/pagerduty \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [{
      "type": "incident",
      "incident": {
        "id": "TEST-001",
        "status": "triggered",
        "title": "Test Alert from Quick Start",
        "service": "test-service",
        "severity": "high"
      }
    }]
  }'
```

## Production Deployment

### Environment Setup

For production deployment:

1. **Database**: Use managed PostgreSQL (AWS RDS, Google Cloud SQL, etc.)
2. **Application**: Deploy to container orchestration (Kubernetes, ECS, etc.)
3. **Load Balancer**: Configure SSL and domain routing
4. **Monitoring**: Set up application monitoring and logging

### Security Configuration

```bash
# Generate secure webhook secret
openssl rand -hex 32

# Update production environment variables
export WEBHOOK_SECRET=your-generated-secret
export DATABASE_PASSWORD=your-secure-db-password
```

### Performance Optimization

```properties
# Production application.properties
spring.datasource.hikari.maximum-pool-size=20
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=30000
server.tomcat.max-threads=200
```

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Find and kill process on port 8080
lsof -ti:8080 | xargs kill -9

# Or change port in application.properties
server.port=8081
```

#### Database Connection Failed
```bash
# Check PostgreSQL status
docker ps | grep postgres

# Test database connection
psql -h localhost -p 5432 -U signalroot -d signalroot
```

#### Frontend Build Errors
```bash
# Clear node modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Getting Help

- **Documentation**: Complete [documentation overview](./overview)
- **API Reference**: [API endpoints](./api-overview)
- **Architecture**: [System architecture](./architecture)
- **Community**: [Discord server](https://discord.gg/signalroot)
- **Support**: support@signalroot.com

## Next Steps

Once you have SignalRoot running:

1. **Explore the Dashboard**: Navigate through different sections
2. **Configure Integrations**: Set up your monitoring tools
3. **Test Alert Flow**: Send test alerts to verify enrichment
4. **Review Documentation**: Check [API reference](./api-overview) for advanced usage

Congratulations! 🎉 You now have SignalRoot running and ready to enrich your alerts!
