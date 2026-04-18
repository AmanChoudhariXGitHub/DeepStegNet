# ⚡ Quick Deployment Checklist

## Pre-Deployment Checklist (5 minutes)

- [ ] Model file exists: `saved_models/best_model.pth`
- [ ] All Python files are error-free
- [ ] Requirements.txt has all dependencies
- [ ] GitHub repository is up to date
- [ ] `.gitignore` is properly configured

## Quick Deploy Steps (3 minutes)

### 1. Push Code to GitHub
```bash
git add .
git commit -m "Ready for Render deployment"
git push origin main
```

### 2. Go to Render Dashboard
Visit: https://render.com/dashboard

### 3. Create New Web Service
```
Click: New + → Web Service
```

### 4. Connect GitHub
- Select your **DeepStegNet** repository
- Authorize if prompted
- Click **Connect**

### 5. Configure Service

| Field | Value |
|-------|-------|
| Name | deepstegnet |
| Environment | Python 3 |
| Region | Oregon |
| Build Command | `pip install -r requirements.txt && python setup.py` |
| Start Command | `streamlit run app.py --server.port=$PORT --server.address=0.0.0.0` |

### 6. Add Environment Variables
```
PYTHONUNBUFFERED = true
STREAMLIT_SERVER_HEADLESS = true
```

### 7. Add Persistent Disk
```
Mount Path: /var/data
Size: 1 GB
```

### 8. Deploy!
Click **Create Web Service** and wait ~5-10 minutes

---

## After Deployment

✅ Visit your live URL
✅ Upload test images
✅ Check logs for any errors
✅ Share the URL with others!

---

## If Something Goes Wrong

**Check logs:**
1. Go to **Logs** tab in Render dashboard
2. Look for error messages
3. Common issues: Missing model file, wrong Python version

**See RENDER_DEPLOYMENT.md for detailed troubleshooting**
