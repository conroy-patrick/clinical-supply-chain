# Arc | Clinical Supply Chain — Oak Avenue Pharma

## Project Overview
A static HTML prototype/dashboard for Arc's Clinical Supply Chain management system for Oak Avenue Pharma. It features a role-based UI for managing SARFs (Site Activation Request Forms), dose orders, batch planning, shipments, and quality control.

## Files
- `index.html` — Main application dashboard (Clinical Operations view)
- `arc-csc-sprint2.html` — Sprint 2 iteration of the dashboard

## Architecture
- Pure static HTML/CSS/JS — no build system, no package manager
- Served via Python's built-in HTTP server on port 5000

## Running
The workflow `Start application` runs:
```
python3 -m http.server 5000 --bind 0.0.0.0
```

## Deployment
Configured as a static site deployment with `publicDir: "."`.
