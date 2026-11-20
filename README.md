# 🚲 Cykelpendlarguiden — Bike Commuter's Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla%20ES6+-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Leaflet](https://img.shields.io/badge/Leaflet-199900?logo=leaflet&logoColor=white)](https://leafletjs.com/)
[![SMHI Open Data](https://img.shields.io/badge/Weather-SMHI%20Open%20Data-005697)](https://opendata.smhi.se/)

An intelligent, lightweight web application for bike commuters in Sweden. Check exact headwind and crosswinds along your commute, road surface conditions and black ice (*svartis*) warnings, sunrise/sunset bike light requirements, and customized clothing & tire recommendations — before you pedal out.

---

## ✨ Features

- **💨 Headwind & Tailwind Physics Engine**  
  Calculates the bearing angle between your start point (A) and destination (B) and contrasts it with the wind direction vector. Displays exactly how many m/s are helping or slowing you down on both outbound (*ditresa*) and return (*hemresa*) trips.

- **🗺️ Interactive Route Selection**  
  - Click on the interactive Leaflet map to set Start (A) and End (B) locations.
  - Search any address, station, or landmark in Sweden via OpenStreetMap Nominatim.
  - Use current device location (Geolocation API) with one click.
  - Instant route direction swap button (A ⇄ B).

- **❄️ Black Ice (*Svartis*) & Road Condition Warning**  
  Monitors temperature thresholds, air humidity (>80–88%), and precipitation to alert you to slippery road conditions and recommend studded winter tires (*dubbdäck*).

- **🔦 Astronomical Solar Elevation Engine**  
  Uses standard solar coordinate calculations (accounting for latitude, longitude, and day of year) to determine whether the sun will be below the horizon during your selected commute hours. Recommends bike lights strictly when it is dark.

- **🧥 Adaptive Clothing & Gear Recommender**  
  Provides advice for outer layers, windbreakers, rain gear, gloves, thermal insulation, and helmet caps based on temperature, wind chill, and precipitation.

- **📅 5-Day Forecast & Hourly Trip Sliders**  
  Select which day to ride and adjust your morning and afternoon departure times (00:00–23:00) with real-time recalculations.

- **🌐 Swedish & English Support**  
  Full bilingual interface with a one-click language switcher (`SV` / `EN`).

- **⚡ Zero Build Step / Completely Client-Side**  
  Self-contained single-page application built with HTML5, vanilla JavaScript, Tailwind CSS, Leaflet, and Lucide Icons. No `npm install`, compiler, or backend server required.

---

## 🧠 How It Works

### 1. Route Compass Bearing & Wind Component
The compass bearing $\theta_{\text{commute}}$ from point $A (\phi_1, \lambda_1)$ to point $B (\phi_2, \lambda_2)$ is calculated using the spherical forward azimuth:
$$\theta = \text{atan2}(\sin(\Delta \lambda) \cdot \cos(\phi_2), \cos(\phi_1)\sin(\phi_2) - \sin(\phi_1)\cos(\phi_2)\cos(\Delta \lambda))$$

Given wind speed $V_{\text{wind}}$ and wind origin angle $\theta_{\text{wind}}$:
- **Headwind component:** $V_{\text{head}} = V_{\text{wind}} \cdot \cos(\theta_{\text{wind}} - \theta_{\text{commute}})$
  - A positive value indicates opposing headwind.
  - A negative value indicates assisting tailwind.
- **Crosswind component:** $V_{\text{cross}} = |V_{\text{wind}} \cdot \sin(\theta_{\text{wind}} - \theta_{\text{commute}})|$

### 2. Weather Data Source
Weather data is queried directly from [SMHI Öppna Data](https://opendata.smhi.se/) via their point forecast API (`snow1g` v1) at the midpoint coordinate between A and B:
```
https://opendata-download-metfcst.smhi.se/api/category/snow1g/version/1/geotype/point/lon/{midLon}/lat/{midLat}/data.json
```
If the SMHI API is temporarily unreachable or coordinates are outside Sweden, the app automatically switches to an offline fallback simulation mode to keep the user interface responsive.

### 3. Solar Elevation & Daylight Calculation
Rather than relying on generic weather condition codes, the app computes solar elevation angle using the astronomical equation of time and solar declination:
- $\text{Elevation} < -0.833^\circ$: The sun is below the horizon (atmospheric refraction accounted for) $\rightarrow$ Bike lights required.
- $\text{Elevation} \ge -0.833^\circ$: Daylight $\rightarrow$ No bike lights required.

---

## 🛠️ Tech Stack

- **Markup & Styling**: HTML5, [Tailwind CSS](https://tailwindcss.com/) (CDN)
- **Scripting**: Pure Vanilla JavaScript (ES6+)
- **Mapping**: [Leaflet.js](https://leafletjs.com/) with OpenStreetMap & Esri World Topo tiles
- **Geocoding**: [Nominatim OpenStreetMap API](https://nominatim.openstreetmap.org/)
- **Icons**: [Lucide Icons](https://lucide.dev/)
- **Fonts**: [Inter](https://fonts.google.com/specimen/Inter) & [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono)

---

## 📁 Project Structure

```text
Bike-Commuters-Guide/
├── index.html     # Complete application (UI, styles, physics engine & logic)
└── README.md      # Documentation and deployment guide
```

---

## 👤 Author

Created with care by **Alfred Stengard**.

## 📄 License

This project is open source and available under the [MIT License](https://opensource.org/licenses/MIT).
