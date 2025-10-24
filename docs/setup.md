# Setup Guide

## Prerequisites
- Node.js (v18+ recommended)
- npm
- Python 3.9+

## Installation
1. Clone the repo
2. Install frontend dependencies:
   ```bash
   npm install
   ```
3. Install backend dependencies:
   ```bash
   cd backend
   pip install -r requirements.txt
   ```
4. Copy `.env.example` to `.env` and set API keys
5. Start backend:
   ```bash
   uvicorn app.main:app --reload
   ```
6. Start frontend:
   ```bash
   npm run dev
   ```

## API Keys
- VITE_API_BASE: Backend API URL
- VITE_VIRUSTOTAL_API_KEY: VirusTotal API key
- VITE_ABUSEIPDB_API_KEY: AbuseIPDB key (optional)

See `docs/api.md` for details.