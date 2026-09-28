# AI-WEATHERWISE-API-PROJECT

# 🌦️ AI WeatherWise API

### Weather data, made human.

*A backend that turns raw weather numbers into friendly, personalized advice using Google Gemini AI.*



![Node.js](https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white)




![Express](https://img.shields.io/badge/Express.js-000000?logo=express&logoColor=white)




![MongoDB](https://img.shields.io/badge/MongoDB-47A248?logo=mongodb&logoColor=white)




![Gemini](https://img.shields.io/badge/Google%20Gemini-4285F4?logo=google&logoColor=white)




![JWT](https://img.shields.io/badge/Auth-JWT-orange)




![Postman](https://img.shields.io/badge/Tested%20with-Postman-FF6C37?logo=postman&logoColor=white)



</div>

---

## 💡 The Idea

Most weather apps show 34°C, 79% humidity, windy and leave you to work out what it means.

*AI WeatherWise does the thinking for you.* It fetches live weather, asks Google Gemini to interpret it, and returns advice like:

> "Stay hydrated, and wear light cotton clothes."

It is a backend-only API, so any website, mobile app, or bot can use it.

## 📑 Contents

[Problem and Solution](#-problem-and-solution) · [How It Works](#-how-it-works) · [Features](#-features) · [Tech Stack](#-tech-stack) · [Architecture](#-architecture) · [Getting Started](#-getting-started) · [API Reference](#-api-reference) · [Examples](#-examples) · [Database](#-database-design) · [Security](#-security) · [Demo](#-demo-and-screenshots) · [Roadmap](#-roadmap) · [Author](#-author)

## 🎯 Problem and Solution

*Problem.* Take Alex, who travels a lot:
- Tracking weather for many cities is hard.
- There is no personal advice on what to wear or do.
- Raw weather numbers are hard to understand.
- There is no secure way to save preferences across devices.

*Solution.* One backend that lets users sign up securely, save favorite cities, fetch live weather, and get AI-written summaries and recommendations.

## 🔄 How It Works

mermaid
flowchart LR
    A[Client App] -->|Register / Login| B[Auth API]
    B -->|JWT token| A
    A -->|Ask for weather| C[Weather API]
    C -->|Live data| D[(OpenWeatherMap)]
    C -->|Temperature, humidity, condition| A
    A -->|Send weather data| E[AI API]
    E -->|Prompt| F[(Google Gemini)]
    F -->|Summary / recommendation| E
    E --> A
    B <--> G[(MongoDB)]


*User flow:* register or log in → save a favorite city → fetch its weather → request an AI summary or recommendation. The JWT token is checked on protected routes.

## ✨ Features

| | Feature | Details |
|---|---|---|
| 🔐 | Secure login | JWT tokens and bcrypt password hashing |
| 📍 | Favorite locations | Create, read, update, delete cities |
| 🌦️ | Live weather | Real-time data from OpenWeatherMap |
| 🤖 | AI insights | Summaries and recommendations from Gemini |
| 🛡️ | Fallback mode | Uses mock data if weather or AI keys are missing |
| 🧹 | Input sanitization | Protects against XSS and NoSQL injection |
| 🌍 | Public endpoints | Some weather data needs no login |

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | JavaScript (Node.js) |
| Framework | Express.js |
| Database | MongoDB with Mongoose |
| AI | Google Gemini (@google/genai) |
| Weather data | OpenWeatherMap API |
| Authentication | JWT and bcryptjs |
| Other packages | cors, dotenv, multer, nodemon |
| API testing | Postman |

## 🏗️ Architecture

Built with the *MVC pattern* plus a service layer. Routes replace views because this is a backend-only API.


Backend/
 └── src/
      ├── config/db.js
      ├── controllers/   auth, location, weather, ai
      ├── middleware/    authMiddleware.js
      ├── models/        User.js, Location.js
      ├── routes/        auth, location, weather, ai
      ├── services/      weatherService.js, aiService.js
      ├── app.js
      └── server.js


| Layer | Job |
|---|---|
| Models | Define data and talk to MongoDB |
| Controllers | Handle requests and send responses |
| Routes | Map URLs to controllers |
| Middleware | Check JWT tokens |
| Services | Call OpenWeatherMap and Gemini |

## 🚀 Getting Started

*You need:* Node.js, a MongoDB database (local or Atlas), a Gemini API key, and an OpenWeatherMap API key.

bash
git clone https://github.com/amsa27/AI-WeatherWise-API-.git
cd AI-WeatherWise-API-/Backend
npm install


Create a .env file inside Backend:

| Variable | Purpose |
|---|---|
| PORT | Port the server runs on |
| MONGO_URI | MongoDB connection string |
| JWT_SECRET | Secret used to sign tokens |
| GEMINI_API_KEY | Google Gemini key |
| OPENWEATHER_API_KEY | OpenWeatherMap key |

> ⚠️ Never upload your real .env file to GitHub.

Start the server:

bash
npm start


Open http://localhost:5001 (or your chosen port). A welcome message means it works.

## 📡 API Reference

| Method | Endpoint | Description |
|---|---|---|
| POST | /api/auth/register | Create an account, returns a JWT |
| POST | /api/auth/login | Log in, returns a JWT |
| GET | /api/weather/:city | Current weather for a city |
| POST | /api/ai/weather-summary | AI-written weather summary |
| POST | /api/ai/weather-recommendation | AI-written advice |

Favorite-city routes live under /api/locations and need a token:


Authorization: Bearer <your_jwt_token>


## 🧪 Examples

*Register*

json
POST /api/auth/register
{ "name": "Alex", "email": "alex@example.com", "password": "yourpassword" }


*Weather*


GET /api/weather/chennai


json
{ "city": "Chennai", "temperature": 21, "humidity": 66, "condition": "Cloudy" }


*AI summary*

json
POST /api/ai/weather-summary
{ "city": "Chennai", "temperature": 21, "humidity": 66, "condition": "Cloudy" }


Result: "Today's weather in Chennai is pleasant and cloudy with moderate humidity."

*AI recommendation*

json
POST /api/ai/weather-recommendation
{ "city": "Chennai", "temperature": 21, "humidity": 66, "condition": "Cloudy" }


Result: "Enjoy the comfortable temperature, great day for outdoor plans, and a light jacket might be handy."

## 🗄️ Database Design

| Collection | Fields |
|---|---|
| *User* | name, email (unique), password (hashed), role (default: reader) |
| *Location* | user (reference), city, country, createdAt |

One user can have many saved locations.

## 🔒 Security

- 🔑 Passwords are hashed with bcrypt.
- 🎫 Protected routes need a valid JWT.
- 🧹 Inputs are sanitized against XSS and NoSQL injection.
- 🙈 Secrets stay in .env, out of GitHub.
- 🛡️ Fallback mode keeps the API running if an external key is missing.

## 🎬 Demo and Screenshots

📹 *API testing video:* add link
📹 *Full demo video:* add link
📄 *Project document:* add link

Add your Postman screenshots here, for example:

`

![Register API](screenshots/register.png)

`

## 🗺️ Roadmap

- [ ] Weather alerts
- [ ] Multi-day forecasts
- [ ] Admin dashboard for logs and usage
- [ ] Frontend app using this API

## 👩‍💻 Author

*Amsa* · [@amsa27](https://github.com/amsa27)

---

<div align="center">⭐ If you like this project, give it a star.</div>
