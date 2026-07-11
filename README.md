# UTS HUNTERS - Render Deployment Guide

## Required Files for GitHub Repo

Upload these files to your GitHub repo:

```
your-repo/
├── uts_hunters_pro.py      # Main app code
├── requirements.txt         # Python dependencies
├── Procfile                 # Render start command
├── render.yaml              # Render service config (optional)
├── Numbers_Export.csv       # Team data file (YOUR file)
├── .streamlit/
│   ├── config.toml          # Streamlit server config
│   └── secrets.toml         # Optional secrets (can be empty)
└── README.md                # This file
```

## Step-by-Step Render Deployment

### 1. Push to GitHub
Create a new GitHub repo and upload ALL files above.

### 2. Create Render Account
- Go to https://render.com
- Sign up / Log in with GitHub

### 3. Create New Web Service
- Click "New +" → "Web Service"
- Connect your GitHub repo
- Select the repo with these files

### 4. Configure Settings
If you uploaded `render.yaml`, Render will auto-detect settings.
Otherwise set manually:

| Setting | Value |
|---------|-------|
| **Name** | `uts-hunters` |
| **Runtime** | Python 3 |
| **Build Command** | `pip install -r requirements.txt` |
| **Start Command** | `streamlit run uts_hunters_pro.py --server.port=$PORT --server.address=0.0.0.0 --server.headless=true` |
| **Plan** | Free |

### 5. Environment Variables
No environment variables needed - all config is in the code.

### 6. Deploy
Click "Create Web Service" → Render will build and deploy.

Your app will be live at: `https://uts-hunters.onrender.com`

## Important Notes

- **Numbers_Export.csv**: You MUST upload this file to the repo root.
  Without it, team data won't load (app still works, just no team info).

- **Free Plan Limitations**: Render free tier sleeps after 15 min inactivity.
  First request after sleep takes ~30 seconds to wake up.

- **Auto-Deploy**: If you enabled autoDeploy, every push to GitHub
  triggers a new deployment automatically.

- **Port**: Render sets the port via `$PORT` env var. The Procfile
  and render.yaml handle this automatically.

## Troubleshooting

### App won't start?
1. Check Render logs (Deploy tab → Logs)
2. Verify all files are in the repo
3. Make sure `requirements.txt` has all packages

### Data server unreachable?
- The app connects to `http://51.77.216.195` - if that server is down,
  the app will show an error message but won't crash

### Memory issues?
- Free plan has 512MB RAM limit
- The app uses ~80-100MB normally
