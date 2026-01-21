# API Overview

SignalRoot provides a comprehensive REST API for alert enrichment, incident management, and system configuration.

## Base URL

```
Production: https://api.signalroot.com
Development: http://localhost:8080
```

## Authentication

SignalRoot uses API key authentication for secure access.

### Getting Your API Key

1. Navigate to **Settings** → **API Keys** in the dashboard
2. Click **Generate New Key**
3. Copy the key and store it securely

### Using the API Key

Include the API key in the `Authorization` header:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.signalroot.com/api/teams
```

## Core Endpoints

### Health Check

```http
GET /actuator/health
```

Check the health status of the SignalRoot service.

**Response:**
```json
{
  "status": "UP",
  "components": {
    "db": {
      "status": "UP",
      "details": {
        "database": "PostgreSQL",
        "validationQuery": "isValid()"
      }
    }
  }
}
```

### Teams Management

#### Get All Teams

```http
GET /api/fake-customers/teams
Authorization: Bearer YOUR_API_KEY
```

**Response:**
```json
[
  {
    "id": "team-auth",
    "name": "Auth Team",
    "description": "Manages authentication and authorization",
    "alertFrequency": "MEDIUM",
    "services": [
      {
        "name": "auth-service",
        "description": "User authentication service"
      }
    ]
  }
]
```

#### Get Team Details

```http
GET /api/fake-customers/teams/{teamId}
Authorization: Bearer YOUR_API_KEY
```

#### Get Team Statistics

```http
GET /api/fake-customers/teams/{teamId}/stats
Authorization: Bearer YOUR_API_KEY
```

**Response:**
```json
{
  "teamId": "team-payments",
  "teamName": "Payments Team",
  "serviceCount": 3,
  "alertFrequency": "HIGH",
  "lastAlertTime": "2024-01-20T16:57:59.917157"
}
```

### Alert Processing

#### Process PagerDuty Alert

```http
POST /webhooks/alerts/pagerduty
Content-Type: application/json
```

**Request Body:**
```json
{
  "messages": [{
    "type": "incident",
    "incident": {
      "id": "INC123",
      "status": "triggered",
      "title": "API Gateway High Latency",
      "service": "payment-service",
      "severity": "high",
      "details": {
        "impacted_services": ["payment-api", "checkout-flow"],
        "error_rate": "12%",
        "response_time": "2.3s"
      }
    }
  }]
}
```

#### Process CloudWatch Alert

```http
POST /webhooks/alerts/cloudwatch/{orgKey}
Content-Type: application/json
```

#### Process GitHub Deployment

```http
POST /webhooks/deploy/github
Content-Type: application/json
```

### Dogfooding Scenarios

#### Get All Scenarios

```http
GET /api/dogfooding/scenarios
Authorization: Bearer YOUR_API_KEY
```

**Response:**
```json
[
  {
    "id": "payment-latency",
    "name": "Payment Service High Latency",
    "description": "API Gateway showing 2+ second response times",
    "teamId": "team-payments",
    "service": "payment-service",
    "severity": "HIGH",
    "steps": [
      "1. Recent GitHub deploy detected (15 mins ago)",
      "2. High latency alert triggered",
      "3. Correlation with deploy established",
      "4. Enriched notification sent to Slack",
      "5. Incident created with suggested checks"
    ]
  }
]
```

#### Run All Scenarios

```http
POST /api/dogfooding/run-all
Authorization: Bearer YOUR_API_KEY
```

#### Run Specific Scenario

```http
POST /api/dogfooding/scenarios/{scenarioId}/run
Authorization: Bearer YOUR_API_KEY
```

### Version Management

#### Get Current Version

```http
GET /api/versions/version/current
Authorization: Bearer YOUR_API_KEY
```

**Response:**
```json
{
  "version": "v1.1.0",
  "releasedAt": "2024-01-20T00:00:00Z",
  "isCurrent": true,
  "description": "v1.1.0 - Added interactive API explorer and changelog component"
}
```

#### Get Changelog

```http
GET /api/versions/changelog/{version}
Authorization: Bearer YOUR_API_KEY
```

## SDKs and Libraries

### JavaScript/TypeScript

```bash
npm install @signalroot/sdk
```

```javascript
import { SignalRootClient } from '@signalroot/sdk';

const client = new SignalRootClient({
  apiKey: 'YOUR_API_KEY',
  baseUrl: 'https://api.signalroot.com'
});

// Get teams
const teams = await client.getTeams();

// Process alert
const result = await client.processAlert({
  type: 'incident',
  service: 'payment-service',
  severity: 'high',
  title: 'API latency spike'
});
```

### Python

```bash
pip install signalroot-sdk
```

```python
from signalroot import SignalRootClient

client = SignalRootClient(
    api_key='YOUR_API_KEY',
    base_url='https://api.signalroot.com'
)

# Get teams
teams = client.get_teams()

# Process alert
result = client.process_alert({
    'type': 'incident',
    'service': 'payment-service',
    'severity': 'high',
    'title': 'API latency spike'
})
```

### Go

```bash
go get github.com/signalroot/go-sdk
```

```go
package main

import (
    "github.com/signalroot/go-sdk"
)

func main() {
    client := signalroot.NewClient("YOUR_API_KEY", "https://api.signalroot.com")
    
    // Get teams
    teams, err := client.GetTeams()
    if err != nil {
        log.Fatal(err)
    }
    
    // Process alert
    result, err := client.ProcessAlert(signalroot.Alert{
        Type: "incident",
        Service: "payment-service",
        Severity: "high",
        Title: "API latency spike",
    })
}
```

## Webhooks

### Setting Up Webhooks

SignalRoot can send enriched alerts to your endpoints:

#### Configure Webhook URL

```http
POST /api/webhooks/configure
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

**Request Body:**
```json
{
  "url": "https://your-app.com/webhooks/signalroot",
  "events": ["alert.enriched", "incident.created"],
  "secret": "your-webhook-secret",
  "active": true
}
```

#### Webhook Payload

SignalRoot sends POST requests with the following structure:

```json
{
  "event": "alert.enriched",
  "timestamp": "2024-01-20T16:51:04.533015Z",
  "data": {
    "originalAlert": {
      "id": "INC123",
      "title": "API Gateway High Latency",
      "severity": "high"
    },
    "enrichedData": {
      "deploymentContext": {
        "deployedAt": "2024-01-20T16:46:04.532734Z",
        "deployedBy": "john.doe",
        "commit": "abc123"
      },
      "similarIncidents": [
        {
          "id": "INC456",
          "resolvedAt": "2024-01-18T10:30:00Z",
          "resolution": "Restarted payment service"
        }
      ],
      "suggestedActions": [
        "Check recent deployments",
        "Review payment service logs",
        "Consider rollback if needed"
      ]
    }
  },
  "signature": "sha256=5d41402abc4b2a76b9719d911017c592..."
}
```

#### Verifying Webhooks

Verify webhook signatures using your secret:

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');
  
  return `sha256=${expectedSignature}` === signature;
}
```

## Rate Limits

SignalRoot API implements rate limiting to ensure fair usage:

| Plan | Requests per Minute | Requests per Hour | Requests per Day |
|------|-------------------|------------------|------------------|
| Free | 60 | 1,000 | 10,000 |
| Pro | 600 | 10,000 | 100,000 |
| Enterprise | 6,000 | 100,000 | 1,000,000 |

Rate limit headers are included in responses:

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1642694400
```

## Error Handling

### Standard Error Response

```json
{
  "error": {
    "code": "INVALID_API_KEY",
    "message": "The provided API key is invalid or expired",
    "details": {
      "timestamp": "2024-01-20T16:51:04.533015Z",
      "requestId": "req_abc123"
    }
  }
}
```

### Common Error Codes

| Code | Description | Solution |
|------|-------------|-----------|
| `INVALID_API_KEY` | API key is invalid or expired | Generate a new API key |
| `RATE_LIMIT_EXCEEDED` | Too many requests | Wait and retry |
| `TEAM_NOT_FOUND` | Team ID doesn't exist | Check team ID |
| `INVALID_WEBHOOK` | Webhook URL invalid | Verify webhook format |
| `INSUFFICIENT_PERMISSIONS` | API key lacks permissions | Update key permissions |

## Best Practices

### Security

1. **Never expose API keys** in client-side code
2. **Use HTTPS** for all API requests
3. **Rotate API keys** regularly
4. **Validate webhook signatures** for incoming webhooks
5. **Use least privilege** principle for API permissions

### Performance

1. **Batch requests** when possible
2. **Use appropriate pagination** for large datasets
3. **Cache frequently accessed data**
4. **Implement exponential backoff** for rate limits
5. **Monitor API usage** and optimize accordingly

### Error Handling

1. **Implement retry logic** with exponential backoff
2. **Handle rate limits** gracefully
3. **Log errors** for debugging
4. **Provide user feedback** for API failures
5. **Monitor API status** for downtime

## Support

- **API Documentation**: Complete endpoint reference
- **SDK Documentation**: Library-specific guides
- **Status Page**: api.signalroot.com/status
- **Support**: api-support@signalroot.com
- **Community**: Discord API channel
