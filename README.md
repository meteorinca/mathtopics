
# Student Math Tracker

## What It Does
Tracks student progress through math exercises using anonymous Firebase UUIDs. Students click buttons → progress auto-logs → you see completion data.


## Live Demo
**[Your GitHub Pages URL here]**

## Quick Start

1. **Firebase Setup** (5 min)
   ```bash
   console.firebase.google.com → New Project
   Enable Firestore + Anonymous Auth
   Copy web config → paste in index.html
   ```

2. **Deploy** (2 min)
   ```bash
   # Option A: GitHub Pages
   Create repo → enable Pages → done
   
   # Option B: Netlify
   Drag/drop folder to netlify.com
   ```

3. **Share with Students**
   ```
   "Go to [your-site.com]
    Copy your ID at top
    Click exercises - progress auto-saves!"
   ```

## View Student Progress
1. Firebase Console → Firestore → `tracks` collection
2. See each student's UUID folder + click history
3. Or use `admin.html` for formatted view/CSV export

## Data Structure
```
Student clicks "Ratios" button →
POST /tracks/abc123/actions/def456:
{
  "url": "https://meteorinca.github.io/ratios/",
  "action": "click",
  "timestamp": "2026-02-21T18:33:00Z"
}
```

## Security
- Students can only write their own data
- Admin dashboard requires your Google login
- No student names/emails collected
- Firebase Spark Plan = $0/month

## Supported
- Chrome, Firefox, Safari (desktop/mobile)
- Works offline (queues logs)
- UUID persists across devices/browsers

## Customization
```html
<!-- Add new exercise -->
<button onclick="trackAndGo('https://your-site.com/new/')">
  New Exercise Name
</button>
```

## Troubleshooting
| Issue | Fix |
|-------|-----|
| "Loading..." forever | Check Firebase config |
| No data in console | Verify security rules |
| Buttons don't work | Browser adblocker? |

## Files
```
├── index.html      # Student dashboard
├── admin.html      # Teacher progress view
├── README.md       # You're reading it
└── firebase.json   # Your config (gitignore this!)
```

**Built with ❤️ for teachers** - Deploy in 7 minutes, track forever.

no copy no license no responsible for any damage
or loss of data etc.
