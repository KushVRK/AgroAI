# Software Requirements Specification (SRS)
# AgroAI Platform
**Version:** 1.0  
**Date:** June 2026  
**Author:** Kushagra (KushVRK)  
**Status:** In Progress

---

## 1. Introduction

### 1.1 Purpose
This document describes the complete software requirements for AgroAI — an AI-powered 
agriculture platform designed to help farmers make smarter crop and business decisions.

### 1.2 Problem Statement
Farmers in India, especially in Karnataka, face three major problems:
- They don't know which crop will be most profitable this season
- They can't detect crop diseases early enough
- They have no access to market price intelligence

### 1.3 Proposed Solution
AgroAI is a web and mobile platform that uses machine learning to give farmers:
- Crop recommendations based on their location, soil, and season
- Disease detection from leaf photos
- Market price trends and selling advice
- An AI assistant for common farming questions

### 1.4 Target Users
- Small and medium farmers in Karnataka (primary)
- Agricultural extension workers
- Farmers across India (future)

---

## 2. Scope

### In Scope (Phase 1 - MVP)
- Crop recommendation based on location, soil type, season, land area
- Top 3 crop suggestions with reasons
- Expected growing duration
- Basic cultivation tips
- Weather data integration (auto-fetched by location)

### Out of Scope (Phase 1)
- Market price prediction
- Disease detection
- AI chatbot
- Mobile app
- Payment features

---

## 3. Functional Requirements

### FR-01: Location Input
- User can enter their district or city name
- System fetches current weather data automatically
- System identifies the agricultural zone

### FR-02: Soil Input
- User selects soil type from dropdown:
  - Black soil
  - Red soil
  - Loamy soil
  - Sandy soil
  - Clay soil
- User selects season:
  - Kharif (June – October)
  - Rabi (November – March)
  - Zaid (April – June)

### FR-03: Crop Recommendation
- System returns top 3 recommended crops
- Each crop shows:
  - Crop name
  - Suitability score (percentage)
  - Why this crop is recommended (reason)
  - Expected growing duration (in days)
  - Basic cultivation tips (3–5 points)

### FR-04: Weather Integration
- System automatically fetches:
  - Temperature
  - Humidity
  - Rainfall data
- Source: OpenWeatherMap free API

### FR-05: Results Display
- Results shown on same page (no page reload)
- Mobile-friendly layout
- Results can be printed or saved as PDF (future)

---

## 4. Non-Functional Requirements

### NFR-01: Performance
- Recommendation result must load within 3 seconds

### NFR-02: Availability
- System should be available 24/7
- Hosted on free cloud platform (Render or Railway)

### NFR-03: Usability
- Interface must work on mobile phones (farmers use phones, not laptops)
- Minimum text — use icons and visuals where possible
- Future: Kannada language support

### NFR-04: Cost
- Phase 1 development cost: ₹0
- All APIs used must have free tiers
- Hosting must be free

---

## 5. System Architecture (Phase 1)
[Farmer's Browser]

↓

[Frontend: HTML + CSS + JS]

↓ (API call)

[Backend: FastAPI (Python)]

↓              ↓

[ML Model]    [Weather API]

(scikit-learn) (OpenWeatherMap)

↓

[SQLite Database]

## 6. Tech Stack

| Layer       | Technology         | Reason                        |
|-------------|-------------------|-------------------------------|
| Frontend    | HTML, CSS, JS      | Simple, no framework needed   |
| Backend     | FastAPI (Python)   | Fast, beginner-friendly       |
| ML Model    | scikit-learn       | Free, works offline           |
| Database    | SQLite             | Zero setup, file-based        |
| Weather API | OpenWeatherMap     | Free tier available           |
| Hosting     | Render.com         | Free tier available           |
| Version Control | Git + GitHub   | Industry standard             |

---

## 7. Database Design (Phase 1)

### Table: recommendations
| Column        | Type     | Description                    |
|---------------|----------|--------------------------------|
| id            | INTEGER  | Primary key                    |
| location      | TEXT     | Farmer's location              |
| soil_type     | TEXT     | Selected soil type             |
| season        | TEXT     | Selected season                |
| land_area     | REAL     | Land area in acres             |
| top_crop_1    | TEXT     | First recommended crop         |
| top_crop_2    | TEXT     | Second recommended crop        |
| top_crop_3    | TEXT     | Third recommended crop         |
| created_at    | DATETIME | Timestamp of recommendation    |

---

## 8. API Design (Phase 1)

### POST /recommend
**Input:**
```json
{
  "location": "Mandya",
  "soil_type": "black",
  "season": "kharif",
  "land_area": 2.5
}
```
**Output:**
```json
{
  "crops": [
    {
      "name": "Rice",
      "score": 92,
      "reason": "Black soil and Kharif season are ideal for rice",
      "duration_days": 120,
      "tips": ["Maintain water level", "Use paddy seeds", "..."]
    }
  ],
  "weather": {
    "temperature": 28,
    "humidity": 75,
    "rainfall": "moderate"
  }
}
```

### GET /health
Returns API status. Used to check if server is running.

---

## 9. Development Phases

| Phase | Feature                  | Timeline  | Cost |
|-------|--------------------------|-----------|------|
| 1     | Crop Recommendation MVP  | 2–3 weeks | ₹0   |
| 2     | Market Price Intelligence| 2 weeks   | ₹0   |
| 3     | Disease Detection        | 2 weeks   | ₹0   |
| 4     | Rule-based Assistant     | 1 week    | ₹0   |
| 5     | AI Chatbot               | After users| TBD |

---

## 10. Future Roadmap

- Kannada language support
- Voice input for farmers
- Android app (Flutter)
- Government scheme alerts
- Mandi price integration (Agmarknet API)
- WhatsApp bot integration

---

*This document will be updated as the project evolves.*