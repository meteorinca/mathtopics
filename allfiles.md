## Tasklist.md

### Phase 1: Firebase Setup (Priority: High, Est. 2 hours) COMPLETED. Web api key is in secret.txt
- Create Firebase project at console.firebase.google.com
- Enable Firestore Database in test mode initially
- Enable Anonymous Authentication
- Copy web app config (apiKey, projectId, etc.)
- Set Firestore security rules:
  ```
  rules_version = '2';
  service cloud.firestore {
    match /databases/{database}/documents {
      match /tracks/{uuid}/{doc=**} {
        allow read: if false;  // Admin only
        allow write: if request.auth != null && request.auth.uid == uuid;
      }
    }
  }
  ```


### Phase 2: Main Tracker HTML (Priority: High, Est. 1 hour)
- Create `index.html` with Firebase SDK imports (v10 compat)
- Add buttons for each external link (percent, ratios, addfractions, etc.)
- Implement anonymous auth to generate/capture user UUID
- Display UUID on page for students to reference
- Create `trackAndGo(url)` function that:
  - Logs to `/tracks/{uuid}/actions/{autoId}` with timestamp, URL, userAgent
  - Opens external link in new tab after logging
- Style minimally with CSS grid/flexbox for button layout

### Phase 3: External Site Integration (Priority: Medium, Est. 1 hour per site)
- Add `?uuid=xxx` query param support to each GitHub.io site
- Extract UUID from URL params in each sub-site
- Add Firebase logging script to each sub-site:
  ```js
  // Log page load/completion
  firebase.firestore().collection('tracks').doc(uuid).collection('actions').add({
    url: window.location.href,
    action: 'page_load',
    timestamp: firebase.firestore.FieldValue.serverTimestamp()
  });
  ```
- Add completion button that logs `action: 'completed'` before next navigation

### Phase 4: Admin Dashboard (Priority: Medium, Est: 2 hours)
- Create `admin.html` with Google sign-in (your account only)
- Query all `tracks/{uuid}/actions` documents
- Display table: UUID | Actions Count | First Visit | Last Action | Links Completed
- Add search/filter by UUID
- Export button to download CSV of all data

### Phase 5: Deployment & Testing (Priority: High, Est: 1 hour)
- Host main tracker on GitHub Pages/Netlify
- Test anonymous UUID generation and logging
- Test cross-site UUID tracking
- Verify admin read access
- Test on mobile browsers

## PRD.md

# Student Progress Tracker PRD

## Overview
Single-page web app that tracks student interactions with external math exercise sites using Firebase UUIDs. Students have to enter a 6 digit code to that I will have pre-provided to them. Use that to create a unique tracking ID for each student. All clicks and page loads logged to Firestore for teacher analysis. I'll provide the 6 digit code for you to attach for tracking and creating a fetchable metadata that I already have a seperate webapp for teachers to create a profile on each student and this should be able to send the appropriate data to my web app based on just the 6 digit code. 

## Goals
- Track which students complete which exercises
- Minimal friction for students (no accounts/passwords)
- Real-time data collection viewable in Firebase Console
- Exportable data for progress reports
- Works on desktop/mobile browsers

## User Stories

### Students
```
As a student, I want to:
- See my unique tracking ID immediately
- Click familiar buttons to access math exercises  
- Continue working even if internet drops temporarily
- Work across multiple devices (UUID persists in Firebase)
```

### Teacher (You)
```
As a teacher, I want to:
- See real-time list of all student UUIDs and their progress
- Filter students by completion status per exercise
- Export data to CSV for gradebook/parent reports
- Block access to admin data from students
```

## Success Metrics
- 95%+ link click logging success rate
- <2 second delay between click and external site load
- 100% data privacy (UUIDs anonymous, no PII collected)
- Mobile responsive across iOS/Android Chrome/Safari

## Non-Goals
- Student accounts/login
- Multi-teacher access
- Payment/student management
- Complex analytics dashboard
- Real-time notifications

## Tech Stack
| Component | Technology |
|-----------|------------|
| Frontend | Vanilla HTML/CSS/JS |
| Backend | Firebase Firestore + Auth |
| Hosting | GitHub Pages/Netlify |
| Tracking | Firebase UUID (anonymous auth) |

## Data Model
```
tracks/
  {student_uuid}/
    actions/{auto_id}:
      url: "https://..."
      action: "click" | "page_load" | "completed"
      timestamp: firestore_timestamp
      userAgent: "..."
      referrer: "..."
```

## PRD.md (continued)

## MVP Launch Criteria
- [ ] Main tracker page logs button clicks to Firestore
- [ ] UUID displays and persists across sessions
- [ ] External sites receive UUID via query param
- [ ] Admin can view all student progress in Firebase Console
- [ ] Works on Chrome/Firefox/Safari mobile/desktop
- [ ] Security rules prevent student data access

## Future Enhancements
- Timer tracking per exercise
- Quiz score logging
- Progress badges/visualization
- Email reports
- Multi-language support

## BasicPlan.md

# Implementation Roadmap

## Week 1: Core Tracking (MVP)
**Day 1-2**: Firebase setup + main tracker page
```
1. Firebase project + Firestore/Auth enabled
2. index.html with 3 exercise buttons
3. Anonymous auth → UUID display
4. Click logging → external redirect
5. Deploy to GitHub Pages
```
**Day 3**: Test + external site UUID param support
```
1. Add ?uuid=xxx parsing to 3 exercise sites
2. Log page loads from external sites
3. Verify end-to-end tracking
```

## Week 2: Admin + Polish
**Day 1-2**: Admin dashboard
```
1. admin.html with Google auth
2. Query/display all student progress
3. CSV export functionality
```
**Day 3**: Mobile testing + documentation
```
1. Test on phone/tablet
2. Write student instructions
3. Deploy final version
```

## Cost: $0/month
- Firebase Spark Plan (free): 1GB Firestore, 10GB transfer
- GitHub Pages: Free hosting
- Expected usage: ~100 students × 20 clicks = 2K writes/month

## Rollout Plan
```
1. Test with 2-3 students
2. Deploy to class of 20 students
3. Monitor Firebase usage/costs
4. Scale to full classes
```


later we'll add random exam after every 5 exercises. (exam will be a random selection of 5 questions from the database)

We'll work on an AMAZING VERRY VERRY HIGH TECHNICAL HIGH QUALITY celebration (explained a bit later) using parrallax effect and crazy fireworks high end animation like all of knowledge of technical capability you can come up with using any shortcuts. If you can pull off using manim library to create this effect wow that would be dope. You ask what this effect is for? Bro. I'm glad you asked. It's for the if you get every question right the website will explode with fireworks and parallax effect and the students' mind will be completely blown and also high end programmers and animators (even on pixar level) will be impressed if they were to see this effect.