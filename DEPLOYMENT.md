# 🚀 Item API Deployment Guide

Your Spring Boot REST API is ready to deploy! Since platforms like Vercel/Netlify don't support Java, here are the best alternatives.

## ✅ Quick Deploy Options

### 1. **Railway.app** (Recommended - Easiest)
**Cost:** ~$5/month (free tier available)
**Setup Time:** ~5 minutes

**Steps:**
1. Go to [railway.app](https://railway.app)
2. Sign up with GitHub
3. Create new project → Deploy from GitHub
4. Select this repository
5. Railway auto-detects the Dockerfile
6. Your app deploys automatically!
7. Visit the Railway dashboard to get your public URL

**Command-line (optional):**
```bash
npm install -g @railway/cli
railway link
railway up
```

---

### 2. **Render.com** (Free tier available)
**Cost:** Free (with limitations) or pay-as-you-go
**Setup Time:** ~5 minutes

**Steps:**
1. Go to [render.com](https://render.com)
2. Sign up with GitHub
3. Create new Web Service → Deploy from repository
4. Select your repo
5. Runtime: Docker
6. Region: Choose closest to you
7. Click Deploy
8. Get your public URL from dashboard

---

### 3. **AWS** (Free tier - 1 year)
**Cost:** Free first year, then ~$3-10/month
**Setup Time:** ~10 minutes

**Using AWS App Runner:**
1. Go to [AWS Console](https://console.aws.amazon.com)
2. Search "App Runner"
3. Create service → Deploy with source code
4. Connect GitHub repository
5. Select Dockerfile configuration
6. Deploy!

**Or use Elastic Beanstalk (alternative):**
```bash
eb init
eb create item-api-env
eb deploy
```

---

### 4. **DigitalOcean** (Simple & Affordable)
**Cost:** $5/month
**Setup Time:** ~5 minutes

**Steps:**
1. Go to [digitalocean.com](https://digitalocean.com)
2. Create App → GitHub Deploy
3. Select your repo
4. Configure: choose Dockerfile build
5. Deploy!

---

## 🐳 Local Docker Test (Before Deploying)

Test the Docker image locally first:

```bash
# Build image
docker build -t item-api .

# Run container
docker run -p 8080:8080 item-api

# Test endpoint
curl -X POST http://localhost:8080/api/items \
  -H "Content-Type: application/json" \
  -d '{"name":"Test Item","description":"Test desc","price":99.99}'

curl http://localhost:8080/api/items/1
```

---

## 📋 Pre-Deployment Checklist

- ✅ `Dockerfile` — Created for containerization
- ✅ `.dockerignore` — Excludes unnecessary files
- ✅ `railway.toml` — Railway config
- ✅ `render.yaml` — Render config
- ✅ `pom.xml` — Maven build configured
- ✅ App runs locally on port 8080

---

## 🔗 API Endpoints

Once deployed, your API will have these endpoints:

**Create Item (POST)**
```
POST /api/items
Content-Type: application/json

{
  "name": "Laptop",
  "description": "Gaming Laptop",
  "price": 1299.99
}
```

**Get Item by ID (GET)**
```
GET /api/items/{id}
```

**Response:**
```json
{
  "id": 1,
  "name": "Laptop",
  "description": "Gaming Laptop",
  "price": 1299.99
}
```

---

## ✨ Next Steps

1. **Choose a platform** from above (I recommend Railway for speed)
2. **Push code to GitHub** if not already done
3. **Link your GitHub repo** on the platform
4. **Deploy** (usually automatic)
5. **Share the public URL** — your API is live!

---

## 🆘 Troubleshooting

**Port conflicts?**
```bash
mvn spring-boot:run -Dspring-boot.run.arguments="--server.port=8081"
```

**Docker build fails?**
```bash
mvn clean package -DskipTests
docker build -t item-api .
```

**Need to check logs?**
- Railway: Dashboard → Logs tab
- Render: Dashboard → Logs
- AWS: CloudWatch Logs

---

## 📞 Support

Need help deploying? Paste the error and I'll fix it!
