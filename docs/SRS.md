# Software Requirements Specification (SRS)
# AgroAI Platform
**Version:** 1.1  
**Date:** June 2026  
**Author:** Kushagra (KushVRK)  
**Status:** In Progress  
**Changelog:** v1.1 – Replaced season selection with month selection; simplified soil types

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
- Crop recommendations based on their location, soil, and month
- Disease detection from leaf photos (Phase 3)
- Market price trends and selling advice (Phase 2)
- An AI assistant for common farming questions (Phase 4–5)

### 1.4 Target Users
- Small and medium farmers in Karnataka (primary)
- Agricultural extension workers
- Farmers across India (future)

### 1.5 Design Philosophy
> Keep it as simple as possible for a farmer using a mobile phone.
- Never ask technical questions the farmer doesn't know the answer to
- Never use agricultural jargon in the UI
- Minimize the number of inputs required

---

## 2. Scope

### In Scope (Phase 1 – MVP)
- Crop recommendation based on location, soil type, month, and land area
- Automatic season detection from selected month (internal, not shown to user)
- Top 3 crop suggestions with reasons
- Expected growing duration and basic cultivation tips
- Live weather data integration (auto-fetched by location)

### Out of Scope (Phase 1)
- Market price prediction (Phase 2)
- Disease detection (Phase 3)
- AI chatbot (Phase 5)
- Mobile app
- Soil image upload (Future Enhancement – see Section 12)
- Payment features

---

## 3. User Inputs (Phase 1)

The application collects exactly four inputs from the farmer:

| Input | Type | Options / Format |
|-------|------|-----------------|
| Location | Text field | Any city or district name |
| Soil Type | Dropdown | Red Soil, Black Soil, Normal Soil |
| Current Month | Dropdown | January through December |
| Land Area | Number + unit | Acres or Hectares |

### What the farmer does NOT select
- Agricultural season — calculated internally from month
- NPK values — estimated internally from soil type
- Temperature / Humidity — fetched automatically from weather API

---

## 4. Month to Season Mapping (Internal Logic)

The backend converts the selected month into an agricultural season automatically.
This is never shown to the farmer.

| Month | Season |
|-------|--------|
| June | Kharif |
| July | Kharif |
| August | Kharif |
| September | Kharif |
| October | Kharif |
| November | Rabi |
| December | Rabi |
| January | Rabi |
| February | Rabi |
| March | Rabi |
| April | Zaid |
| May | Zaid |

---

## 5. Functional Requirements

### FR-01: Location Input
- User enters district or city name as free text
- System fetches weather data automatically via OpenWeatherMap API

### FR-02: Soil Type Selection
- User selects from three plain-language options:
  - Red Soil
  - Black Soil
  - Normal Soil

### FR-03: Month Selection
- User selects current month (January – December)
- Backend maps month → season internally
- Season is never displayed to the user

### FR-04: Land Area Input
- User enters a number and selects unit (acres or hectares)
- System converts to standard unit internally

### FR-05: Crop Recommendation Output
Each of the top 3 crops shows:
- Crop name
- Suitability score (percentage)
- Reason in plain language
- Expected growing duration (days)
- 3–5 cultivation tips

### FR-06: Weather Integration
Automatically fetches and displays:
- Current temperature
- Humidity
- Weather description

### FR-07: Results Display
- Results appear without full page reload
- Mobile-first card layout
- No agricultural jargon in any displayed text

---

## 6. Non-Functional Requirements

| ID | Requirement |
|----|------------|
| NFR-01 | Results load within 3 seconds |
| NFR-02 | System available 24/7 on free hosting |
| NFR-03 | Works on low-end Android mobile browsers |
| NFR-04 | Total development cost: ₹0 |
| NFR-05 | Minimum font size 16px on mobile |

---

## 7. System Architecture (Phase 1)

```
[Farmer's Phone / Browser]
          ↓
[Frontend: HTML + CSS + JavaScript]
          ↓  HTTP POST /recommend
[Backend: FastAPI (Python)]
     ↓               ↓
[Month → Season]  [OpenWeatherMap API]
     ↓
[ML Model: Random Forest (scikit-learn)]
     ↓
[SQLite Database – logs every request]
```

---

## 8. Tech Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| Frontend | HTML, CSS, JavaScript | Simple, no framework needed |
| Backend | FastAPI (Python) | Fast, beginner-friendly, auto docs |
| ML Model | scikit-learn Random Forest | Free, works offline |
| Database | SQLite | Zero setup, file-based |
| Weather API | OpenWeatherMap | Free tier, simple |
| Hosting | Render.com | Free tier available |
| Version Control | Git + GitHub | Industry standard |

---

## 9. Database Design

### Table: recommendations

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key, auto-increment |
| location | TEXT | Farmer's entered location |
| soil_type | TEXT | red / black / normal |
| month | TEXT | Selected month (e.g. "June") |
| season | TEXT | Auto-calculated (e.g. "Kharif") |
| land_area | REAL | Land area value |
| land_unit | TEXT | "acres" or "hectares" |
| top_crop_1 | TEXT | First recommended crop |
| top_crop_2 | TEXT | Second recommended crop |
| top_crop_3 | TEXT | Third recommended crop |
| temperature | REAL | From weather API |
| humidity | REAL | From weather API |
| created_at | DATETIME | Timestamp |

---

## 10. API Design

### POST /recommend

**Request:**
```json
{
  "location": "Mandya",
  "soil_type": "black",
  "month": "June",
  "land_area": 2.5,
  "land_unit": "acres"
}
```

**Response:**
```json
{
  "season": "Kharif",
  "weather": {
    "temperature": 28.4,
    "humidity": 74,
    "description": "Partly cloudy"
  },
  "crops": [
    {
      "rank": 1,
      "name": "Rice",
      "score": 92,
      "reason": "Black soil with high humidity and Kharif season is ideal for rice.",
      "duration_days": 120,
      "tips": [
        "Maintain 5cm water level in field",
        "Use certified paddy seeds",
        "Apply nitrogen fertilizer at tillering stage",
        "Monitor for blast disease"
      ]
    }
  ]
}
```

### GET /health
```json
{ "status": "ok", "version": "1.0.0" }
```

---

## 11. Development Phases

| Phase | Feature | Timeline | Cost |
|-------|---------|----------|------|
| 1 | Crop Recommendation MVP | 2–3 weeks | ₹0 |
| 2 | Market Price Intelligence | 2 weeks | ₹0 |
| 3 | Disease Detection | 2 weeks | ₹0 |
| 4 | Rule-based Assistant | 1 week | ₹0 |
| 5 | AI Chatbot | After users | TBD |

---

## 12. Future Enhancements

### Soil Image Upload (Planned – Phase 3 or later)
Many farmers don't know their soil type. A future feature will:
1. Let the farmer upload a photo of their soil
2. Use a CNN model to predict soil type from the image
3. Auto-fill the soil type field with the prediction
4. Let the farmer confirm or correct before submitting

This will completely remove the need for the farmer to know their soil type.

### Other Planned Features
- Kannada and regional language support
- Voice input
- Android app (Flutter)
- Government scheme alerts
- Mandi price integration (Agmarknet API)
- WhatsApp bot integration
- GPS-based automatic location detection

---