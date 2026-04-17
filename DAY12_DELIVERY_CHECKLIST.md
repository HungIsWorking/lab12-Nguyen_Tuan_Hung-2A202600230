#  Delivery Checklist — Day 12 Lab Submission

> **Student Name:** Nguyễn Tuấn Hưng  
> **Student ID:** 2A202600230  
> **Date:** 17/04/2026

---

##  Submission Requirements

Submit a **GitHub repository** containing:

### 1. Mission Answers (40 points)
`MISSION_ANSWERS.md` 

### 2. Full Source Code - Lab 06 Complete (60 points)

Your final production-ready agent with all files:

```
your-repo/
├── app/
│   ├── main.py              # Main application
│   ├── config.py            # Configuration
│   ├── auth.py              # Authentication
│   ├── rate_limiter.py      # Rate limiting
│   └── cost_guard.py        # Cost protection
├── utils/
│   └── mock_llm.py          # Mock LLM (provided)
├── Dockerfile               # Multi-stage build
├── docker-compose.yml       # Full stack
├── requirements.txt         # Dependencies
├── .env.example             # Environment template
├── .dockerignore            # Docker ignore
├── railway.toml             # Railway config (or render.yaml)
└── README.md                # Setup instructions
```

**Requirements:**
-  All code runs without errors
-  Multi-stage Dockerfile (image < 500 MB)
-  API key authentication
-  Rate limiting (10 req/min)
-  Cost guard ($10/month)
-  Health + readiness checks
-  Graceful shutdown
-  Stateless design (Redis)
-  No hardcoded secrets

---

### 3. Service Domain Link
 `DEPLOYMENT.md`

##  Pre-Submission Checklist

- [x] Repository is public (or instructor has access)
- [x] `MISSION_ANSWERS.md` completed with all exercises
- [x] `DEPLOYMENT.md` has working public URL
- [x] All source code in `app/` directory
- [x] `README.md` has clear setup instructions
- [x] No `.env` file committed (only `.env.example`)
- [x] No hardcoded secrets in code
- [x] Public URL is accessible and working
- [x] Screenshots included in `screenshots/` folder
- [x] Repository has clear commit history

##  Submission

**Submit your GitHub repository URL:**

```
https://github.com/HungIsWorking/lab12-Nguyen_Tuan_Hung-2A202600230
```