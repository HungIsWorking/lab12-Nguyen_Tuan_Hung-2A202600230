# Deployment Information

## Public URL
https://lab12-production-e0585.up.railway.app

## Platform
Railway

## Test Commands

### Health Check
```bash
curl https://lab12-production-e0585.up.railway.app/health
# Expected: {"status": "ok"}
```

### API Test (with authentication)
```bash
curl -X POST https://lab12-production-e0585.up.railway.app/ask \
  -H "X-API-Key: YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"user_id": "test", "question": "Hello"}'
```

## Environment Variables Set
- PORT
- REDIS_URL
- AGENT_API_KEY
- JWT_SECRET
- OPENAI_API_KEY
- ENVIRONMENT
- APP_NAME
- APP_VERSION
- RATE_LIMIT_PER_MINUTE
- DAILY_BUDGET_USD
- LOG_LEVEL

## Screenshots
- [Railway dashboard](screenshots/railway-dashboard.png)
- [Railway health check](screenshots/railway-health.png)
- [API gateway auth test](screenshots/api-gateway-production.png)
- [Production readiness check](screenshots/lab-complete-3.png)
- - [Test results](screenshots/testing.png)