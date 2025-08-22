# Windborne Application - 24-Hour Constellation + Open-Meteo (GFS)

**Live demo:** https://case-study-wind-borne-systems.vercel.app  
**Windborne proxy (hard-coded in the app):** `https://proxy-fmd5.vercel.app/api/proxy?url=`

This single-page app visualizes the last 24 hours of Windborne’s global balloon constellation on a Leaflet map and, on demand, compares each balloon’s **third JSON value** with pressure-level wind speed from **Open-Meteo GFS** (150 hPa by default).


## Features

- **Live 24h history from Windborne**
  - Pulls `https://a.windbornesystems.com/treasure/00.json … 23.json`
  - 00 = most recent hour; 01 = 1 hour ago; …; 23 = 23 hours ago

- **Robust parsing for “quirky” data**
  - Handles normal JSON, line-delimited JSON, trailing commas, and falls back to extracting numeric triplets `[lat, lon, value3]`

- **Smooth animation (no trajectories)**
  - Pre-builds 24 Leaflet `LayerGroup`s and swaps them frame-by-frame (loop **23 → 0**)
  - Uniform markers; label shows **third JSON value × 100** (truncated to integer)

- **On-demand model comparison**
  - Click a marker to fetch the nearest model hour at **150 hPa**
  - Shows: value3, model wind speed (from `u/v` with speed fallback), and **Δ = model − value3**


## Why Open-Meteo?

Open-Meteo’s GFS endpoint is **free, global, pressure-level capable**, and requires **no API key**. Pressure levels (e.g., 150 hPa) provide a reasonable reference plane to compare modeled upper-level winds with balloon behavior in real time.


## URLs & Proxies

- **Main app:** https://case-study-wind-borne-systems.vercel.app  
- **Windborne proxy (hard-coded):** `https://proxy-fmd5.vercel.app/api/proxy?url=`

Windborne endpoints are fetched **via the proxy** to avoid CORS issues.  
Open-Meteo requests are made **directly** (no proxy) to prevent secondary CORS failures.

> Supported proxy patterns in code:
>
> - `https://worker.example.com/?url=`
> - `https://proxy.example.com/` *(encoded URL appended)*
> - Any string containing `{url}` (placeholder replaced by encoded target)
