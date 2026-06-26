# AgroAI Platform

An AI-powered agriculture platform to help farmers make smarter crop decisions.

## How It Works
1. Farmer selects their location, soil type, current month, and land area
2. System fetches live weather data for that location automatically
3. System maps the month → agricultural season internally (farmer never sees this)
4. ML model recommends the top 3 best crops with reasons and tips

## Month → Season Mapping (Internal)
| Month | Season |
|-------|--------|
| June – October | Kharif |
| November – March | Rabi |
| April – May | Zaid |

## Soil Types (Phase 1)
- Red Soil
- Black Soil
- Normal Soil

## Tech Stack
| Layer | Technology |
|-------|-----------|
| Frontend | HTML, CSS, JavaScript |
| Backend | Python (FastAPI) |
| ML | scikit-learn (Random Forest) |
| Database | SQLite |
| Weather | OpenWeatherMap API (free tier) |
| Hosting | Render.com (free tier) |

## Project Phases
| Phase | Feature | Status |
|-------|---------|--------|
| 1 | Crop Recommendation MVP | 🔨 In Progress |
| 2 | Market Price Intelligence | 📋 Planned |
| 3 | Disease Detection | 📋 Planned |
| 4 | Rule-based Assistant | 📋 Planned |
| 5 | AI Chatbot | 📋 Planned |

## Developer
Kushagra – GitHub: https://github.com/KushVRK