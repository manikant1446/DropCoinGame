# 🚀 Running CoinDrop - Quick Guide

## Current Status

✅ **Frontend Server**: Already running on http://localhost:8080  
⚠️ **Backend Server**: Needs to be started

## Option 1: Run Everything (Recommended)

Double-click this file:
```
start-project.bat
```

This will:
1. Setup backend environment
2. Install dependencies
3. Start backend server (port 3000)
4. Start frontend server (port 8080)
5. Open game in browser

## Option 2: Manual Start

### Start Backend Only

```bash
cd backend
npm install
node server.js
```

Backend will run on http://localhost:3000

### Frontend is Already Running!

Your frontend is already accessible at:
**http://localhost:8080**

## Access Points

- **Game**: http://localhost:8080
- **Backend API**: http://localhost:3000
- **Health Check**: http://localhost:3000/health
- **Leaderboard**: http://localhost:3000/api/leaderboard

## Troubleshooting

### PowerShell Script Errors

If you get "running scripts is disabled" error:

1. Open PowerShell as Administrator
2. Run: `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`
3. Type `Y` to confirm
4. Try again

### Backend Won't Start

Make sure you have Node.js installed:
- Download from https://nodejs.org/
- Install and restart terminal
- Try again

### Port Already in Use

If port 3000 is busy, edit `backend/.env`:
```
PORT=3001
```

## What's Running

Check your terminal - you should see:
```
Server is running. Serving files from: D:\New folder2
```

This means frontend is active!

## Next Steps

1. Open http://localhost:8080 in your browser
2. See the beautiful CoinDrop game UI
3. Start backend to enable leaderboard/stats
4. Deploy smart contract to enable blockchain features

Enjoy! 🎮
