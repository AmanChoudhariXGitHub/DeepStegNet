# 🚀 DeepStegNet - Render Deployment Guide

## Prerequisites
- GitHub account with this repository
- Render account (free tier available at https://render.com)
- Your model file (`best_model.pth` or similar)

---

## Step 1: Prepare Your Repository

### 1.1 Check Git Status
```bash
cd /path/to/DeepStegNet
git status
```

### 1.2 Add Model File to Git LFS (Recommended for large files)
If your model file is >50MB, use Git LFS:

```bash
# Install Git LFS
git lfs install

# Track .pth files
git lfs track "*.pth"

# Add to git
git add .gitattributes
git add saved_models/best_model.pth
git commit -m "Add model file with Git LFS"
```

**OR** keep model file in repository:
```bash
git add saved_models/best_model.pth
git commit -m "Add model files"
```

### 1.3 Commit All Changes
```bash
git add .
git commit -m "Prepare for Render deployment"
git push origin main
```

---

## Step 2: Create Render Service

### 2.1 Go to Render Dashboard
1. Log in to https://render.com
2. Click **New +** → **Web Service**

### 2.2 Connect GitHub Repository
1. Select **GitHub** as the repository source
2. Authorize Render to access your GitHub account
3. Find and select your `DeepStegNet` repository
4. Click **Connect**

### 2.3 Configure Web Service

| Setting | Value |
|---------|-------|
| **Name** | deepstegnet |
| **Environment** | Python 3 |
| **Region** | Oregon (or closest to you) |
| **Branch** | main |
| **Build Command** | `pip install -r requirements.txt && python setup.py` |
| **Start Command** | `streamlit run app.py --server.port=$PORT --server.address=0.0.0.0` |
| **Plan** | Free (or Pro for better performance) |

### 2.4 Add Environment Variables
Click **Add Environment Variable**:

| Key | Value |
|-----|-------|
| PYTHONUNBUFFERED | true |
| STREAMLIT_SERVER_HEADLESS | true |

### 2.5 Add Persistent Disk (Important!)
1. Click **Add Disk**
2. Set:
   - **Mount Path**: `/var/data`
   - **Size**: 1 GB
3. This preserves uploaded images between restarts

### 2.6 Review and Deploy
- Verify all settings
- Click **Create Web Service**
- Render will start building and deploying

---

## Step 3: Monitor Deployment

### 3.1 Check Build Logs
1. Go to your service page
2. Click **Logs** tab
3. Watch for build progress

### 3.2 Common Issues

| Error | Solution |
|-------|----------|
| **Model file not found** | Ensure `saved_models/best_model.pth` is committed to Git |
| **Out of memory** | Upgrade to Pro plan or optimize model |
| **Port already in use** | Use `--server.port=$PORT` in start command |
| **Module not found** | Check `requirements.txt` has all dependencies |

---

## Step 4: Update After Deployment

### 4.1 Push Code Changes
```bash
git add .
git commit -m "Update feature"
git push origin main
```

Render will automatically redeploy!

### 4.2 Update Model File
```bash
# If using Git LFS
git lfs pull
# Replace model file
cp /path/to/new_model.pth saved_models/best_model.pth
git add saved_models/best_model.pth
git commit -m "Update model"
git push origin main
```

### 4.3 Clear Persistent Disk
1. Go to Service Settings
2. Click **Disks** → Delete disk
3. Render will recreate on next deploy

---

## Step 5: Optimization Tips

### 5.1 Reduce Model Size
```python
# In app.py, optimize model loading
import torch.quantization as quant

# Use INT8 quantization for faster inference
quantized_model = quant.quantize_dynamic(model, {torch.nn.Linear}, dtype=torch.qint8)
```

### 5.2 Enable GPU (Pro Plan)
```yaml
# In render.yaml, add:
gpu: true
gpuCount: 1
```

### 5.3 Use Redis for Caching
```python
import streamlit_cache as cache
cache.use_redis()
```

---

## Step 6: Monitor Performance

### 6.1 Check Service Health
- Visit your app URL
- Check **Logs** for errors
- Monitor **CPU & Memory** usage

### 6.2 View Real-time Logs
```bash
# You can also tail logs from command line if SSH is enabled
# Check Render dashboard for latest updates
```

---

## Troubleshooting

### Issue: App shows "Model file not found"
**Solution:**
```bash
# Verify model is tracked in git
git ls-files | grep best_model.pth

# If not shown, add it
git add -f saved_models/best_model.pth
git commit -m "Add model file"
git push origin main
```

### Issue: Slow uploads on free tier
**Solution:**
- Upgrade to Pro plan for better specs
- Use smaller images for testing
- Compress images before uploading

### Issue: Persistent disk not working
**Solution:**
1. Redeploy service: **Logs** → **Manual Deploy**
2. Or clear disk in **Settings** → **Disks**

### Issue: Out of memory errors
**Solution:**
- Process images in smaller batches
- Reduce model size with quantization
- Upgrade to Pro plan

---

## Cost Considerations

### Free Tier
- ✅ 750 compute hours/month (330 hours continuous)
- ✅ 1 GB persistent disk
- ✅ Auto-deploy on push
- ❌ Spins down after 15 min inactivity
- Cost: **$0/month**

### Pro Tier
- ✅ Always running
- ✅ 10 GB persistent disk
- ✅ Better CPU/Memory
- ✅ Priority support
- Cost: **~$7-12/month per service**

---

## Useful Resources

- [Render Documentation](https://render.com/docs)
- [Streamlit Deployment](https://docs.streamlit.io/deploy/streamlit-cloud)
- [PyTorch Model Saving](https://pytorch.org/tutorials/beginner/saving_loading_models.html)
- [Git LFS Guide](https://git-lfs.github.com/)

---

## Success! 🎉

Your DeepStegNet app is now live! Share the Render URL with others.

**Questions?** Check Render logs or visit the [Render Community](https://community.render.com)
