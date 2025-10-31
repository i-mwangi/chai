# Complete Render Deployment Guide - Step by Step

## Step 1: Set Up Turso Database (5 minutes)

### 1.1 Create Turso Account
1. Go to https://turso.tech/
2. Click "Sign Up" (free, no credit card needed)
3. Sign up with GitHub (easiest option)

### 1.2 Install Turso CLI (Optional but helpful)
```bash
# Windows (PowerShell as Administrator)
irm get.turso.tech/install.ps1 | iex

# Or use npm
npm install -g @turso/cli
```

### 1.3 Create Your Database
**Option A: Using Turso Dashboard (Easiest)**
1. Go to https://app.turso.tech/
2. Click "Create Database"
3. Name it: `chai-platform` (or any name you like)
4. Select region: Choose closest to your users (e.g., US East)
5. Click "Create"

**Option B: Using CLI**
```bash
# Login to Turso
turso auth login

# Create database
turso db create chai-platform

# Get database URL
turso db show chai-platform --url
```

### 1.4 Get Your Credentials
1. In Turso Dashboard, click on your database name
2. Click "Generate Token" or "Create Token"
3. Copy these two values:
   - **Database URL**: `libsql://chai-platform-[your-name].turso.io`
   - **Auth Token**: `eyJhbGc...` (long string)

### 1.5 Test Connection (Optional)
```bash
# Test that your database works
turso db shell chai-platform

# You should see a SQL prompt
# Type .quit to exit
```

---

## Step 2: Update Your Configuration Files

### 2.1 Update render.yaml
Open `render.yaml` and uncomment the Turso lines:

```yaml
services:
  # Web Service - Mock API Server
  - type: web
    name: chai-platform-api
    env: node
    buildCommand: pnpm install
    startCommand: node frontend/api-server.js
    envVars:
      - key: NODE_VERSION
        value: 18
      - key: API_PORT
        value: 3005
      - key: NODE_ENV
        value: production
      # Add your Turso credentials here
      - key: TURSO_DATABASE_URL
        value: libsql://chai-platform-yourname.turso.io  # ← Replace with your URL
      - key: TURSO_AUTH_TOKEN
        value: eyJhbGc...your-token-here  # ← Replace with your token
    plan: free
    healthCheckPath: /health
```

### 2.2 Update .env for Local Development
Add to your `.env` file (create if it doesn't exist):

```bash
# Add these lines to .env
TURSO_DATABASE_URL=libsql://chai-platform-yourname.turso.io
TURSO_AUTH_TOKEN=eyJhbGc...your-token-here
```

---

## Step 3: Update Your API Server to Use Turso

### 3.1 Check if Your API Uses Database
Look at `frontend/api-server.js` - it currently uses in-memory mock data.

**For production, you have two options:**

**Option A: Keep Mock Data (Simplest)**
- Your current setup works fine
- Data resets on each deploy (acceptable for demo)
- No database changes needed
- **Skip to Step 4**

**Option B: Use Turso for Persistence (Recommended)**
- Data persists across deploys
- Real production setup
- Continue below

### 3.2 Install Turso Client (if using Option B)
```bash
pnpm add @libsql/client
```

### 3.3 Update API Server (if using Option B)
Add this to the top of `frontend/api-server.js`:

```javascript
import { createClient } from '@libsql/client';

// Initialize Turso client
const turso = process.env.TURSO_DATABASE_URL ? createClient({
    url: process.env.TURSO_DATABASE_URL,
    authToken: process.env.TURSO_AUTH_TOKEN
}) : null;

// Initialize database tables
async function initDatabase() {
    if (!turso) return;
    
    try {
        // Create tables if they don't exist
        await turso.execute(`
            CREATE TABLE IF NOT EXISTS groves (
                id TEXT PRIMARY KEY,
                groveName TEXT,
                location TEXT,
                farmerAddress TEXT,
                treeCount INTEGER,
                coffeeVariety TEXT,
                createdAt TEXT
            )
        `);
        
        await turso.execute(`
            CREATE TABLE IF NOT EXISTS harvests (
                id TEXT PRIMARY KEY,
                groveId TEXT,
                farmerAddress TEXT,
                harvestDate TEXT,
                yieldKg REAL,
                totalRevenue REAL,
                revenueDistributed INTEGER
            )
        `);
        
        console.log('✅ Turso database initialized');
    } catch (error) {
        console.error('⚠️ Database init error:', error.message);
    }
}

// Call on startup
initDatabase();
```

---

## Step 4: Push to GitHub

### 4.1 Check What You're Pushing
```bash
# See what files changed
git status

# Review changes
git diff
```

### 4.2 Commit and Push
```bash
# Add all files
git add .

# Commit with message
git commit -m "Configure for Render deployment with Turso"

# Push to GitHub
git push origin main
```

**Note:** Your `.env` file with secrets is NOT pushed (it's in `.gitignore`)

---

## Step 5: Deploy on Render

### 5.1 Connect GitHub to Render
1. Go to https://render.com
2. Click "Sign Up" or "Log In"
3. Sign up with GitHub (easiest)
4. Authorize Render to access your repositories

### 5.2 Create New Blueprint
1. Click "New +" button (top right)
2. Select "Blueprint"
3. Connect your repository:
   - Search for your repo name
   - Click "Connect"

### 5.3 Render Detects Configuration
- Render will automatically detect `render.yaml`
- You'll see two services:
  - `chai-platform-frontend` (Static Site)
  - `chai-platform-api` (Web Service)

### 5.4 Review and Deploy
1. Review the services
2. Click "Apply" or "Create Services"
3. Wait for deployment (5-10 minutes)

### 5.5 Monitor Deployment
- Watch the logs in real-time
- Frontend builds first (Vite build)
- API starts second (Node.js server)

---

## Step 6: Verify Deployment

### 6.1 Check Service URLs
After deployment, you'll get URLs like:
- Frontend: `https://chai-platform-frontend.onrender.com`
- API: `https://chai-platform-api.onrender.com`

### 6.2 Test API Health
Open in browser:
```
https://chai-platform-api.onrender.com/health
```

Should return:
```json
{
  "success": true,
  "message": "Coffee Tree Platform API is running",
  "timestamp": "2024-..."
}
```

### 6.3 Test Frontend
1. Open: `https://chai-platform-frontend.onrender.com`
2. You should see your landing page
3. Try connecting wallet
4. Navigate to different sections

### 6.4 Check Logs
If something doesn't work:
1. Go to Render Dashboard
2. Click on the service (frontend or api)
3. Click "Logs" tab
4. Look for errors

---

## Step 7: Update API URL (If Needed)

### 7.1 Check Actual URLs
If Render gave you different URLs than expected:

1. Go to Render Dashboard
2. Note the actual API URL
3. Update environment variable:
   - Click on `chai-platform-frontend`
   - Go to "Environment" tab
   - Edit `VITE_API_URL`
   - Change to your actual API URL
   - Save (triggers rebuild)

---

## Troubleshooting

### Issue: Build Fails
**Check:**
- Node version (should be 18)
- Run `pnpm install` locally to verify dependencies
- Check build logs in Render dashboard

**Fix:**
```bash
# Test build locally
pnpm run frontend:build
```

### Issue: API Returns 404
**Check:**
- API service is running (check Render dashboard)
- Health endpoint works: `/health`
- CORS is enabled (already configured)

**Fix:**
- Check API logs in Render
- Verify `API_PORT=3005` in environment variables

### Issue: Frontend Can't Reach API
**Check:**
- `VITE_API_URL` is set correctly
- API URL is accessible (try in browser)
- No CORS errors in browser console

**Fix:**
```bash
# Rebuild frontend with correct API URL
# Update VITE_API_URL in Render dashboard
```

### Issue: Database Connection Fails
**Check:**
- Turso credentials are correct
- Database exists in Turso dashboard
- Token hasn't expired

**Fix:**
1. Generate new token in Turso dashboard
2. Update `TURSO_AUTH_TOKEN` in Render
3. Redeploy

### Issue: Service Sleeps (Free Tier)
**Expected Behavior:**
- Free tier sleeps after 15 min inactivity
- First request takes ~30 seconds (cold start)
- Subsequent requests are fast

**Solutions:**
1. Accept it (most users won't notice)
2. Use UptimeRobot to ping every 14 min
3. Upgrade to paid plan ($7/month)

---

## Cost Summary

### Free Tier (What You Get)
- ✅ Frontend: Unlimited bandwidth, always online
- ✅ API: 750 hours/month (enough for 24/7)
- ✅ Turso: 9 GB storage, 1 billion row reads/month
- ⚠️ API sleeps after 15 min inactivity

### Paid Tier (Optional)
- Frontend: Still free
- API: $7/month (always on, no sleep)
- Turso: Still free (very generous limits)

**Total: $0-7/month**

---

## Next Steps After Deployment

### 1. Custom Domain (Optional)
1. Go to frontend service in Render
2. Click "Settings" → "Custom Domain"
3. Add your domain (e.g., `chai-platform.com`)
4. Update DNS records as instructed
5. SSL certificate auto-generated

### 2. Set Up Monitoring
1. Enable email notifications in Render
2. Set up UptimeRobot for uptime monitoring
3. Monitor Turso usage in dashboard

### 3. Database Backups
Turso automatically backs up your data, but you can also:
```bash
# Export database
turso db shell chai-platform .dump > backup.sql

# Import to new database
turso db shell new-database < backup.sql
```

### 4. Update Your App
```bash
# Make changes locally
git add .
git commit -m "Update feature"
git push

# Render auto-deploys on push to main branch
```

---

## Quick Reference

### Turso Commands
```bash
# List databases
turso db list

# Show database info
turso db show chai-platform

# Open SQL shell
turso db shell chai-platform

# Create token
turso db tokens create chai-platform
```

### Render Commands
- Deploy: Push to GitHub (auto-deploys)
- Logs: Dashboard → Service → Logs
- Restart: Dashboard → Service → Manual Deploy
- Environment: Dashboard → Service → Environment

### Local Testing
```bash
# Test API locally
node frontend/api-server.js

# Test frontend locally
pnpm run frontend:vite

# Build for production
pnpm run frontend:build
```

---

## Support

- **Render Docs**: https://render.com/docs
- **Turso Docs**: https://docs.turso.tech/
- **Render Community**: https://community.render.com/
- **Turso Discord**: https://discord.gg/turso

---

## Checklist

Before deploying, make sure:
- [ ] Turso database created
- [ ] Turso credentials added to `render.yaml`
- [ ] Changes committed to GitHub
- [ ] GitHub connected to Render
- [ ] Blueprint deployed on Render
- [ ] Frontend URL works
- [ ] API health check works
- [ ] Wallet connection works
- [ ] Data persists (if using Turso)

You're ready to deploy! 🚀
