<div align="center">

# 🌦️ AI WeatherWise API

### Weather data, made human.

*A smart backend that turns raw weather numbers into friendly, personalized advice using Google Gemini AI.*



![Node.js](https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white)




![Express](https://img.shields.io/badge/Express.js-000000?logo=express&logoColor=white)




![MongoDB](https://img.shields.io/badge/MongoDB-47A248?logo=mongodb&logoColor=white)




![Gemini](https://img.shields.io/badge/Google%20Gemini-4285F4?logo=google&logoColor=white)




![JWT](https://img.shields.io/badge/Auth-JWT-orange)




![Thunder Client](https://img.shields.io/badge/Tested%20with-Thunder%20Client-5C2D91)



</div>

---

## ⚡ In 30 Seconds

*AI WeatherWise* is a backend API (the "engine" behind an app, with no screen of its own). It does four things:

1. Lets users *sign up and log in* securely.
2. Lets them *save favorite cities*.
3. Fetches *live weather* for any city.
4. Asks *Google Gemini AI* to explain that weather in plain, friendly language, for example:

> "Stay hydrated, and wear light cotton clothes."

Any website, mobile app, or chatbot can use it.

## 📑 Table of Contents

1. [About the Project](#1--about-the-project)
2. [How It Works](#2--how-it-works)
3. [Features Explained](#3--features-explained)
4. [Tech Stack and Why](#4--tech-stack-and-why)
5. [Architecture and File Guide](#5--architecture-and-file-guide)
6. [Database Design](#6--database-design)
7. [Authentication Explained](#7--authentication-explained)
8. [Fallback Mode](#8--fallback-mode)
9. [Getting Started](#9--getting-started)
10. [API Reference](#10--api-reference)
11. [Testing with Thunder Client](#11--testing-with-thunder-client)
12. [Security](#12--security)
13. [User Roles](#13--user-roles)
14. [Troubleshooting](#14--troubleshooting)
15. [Glossary](#15--glossary)
16. [Demo and Screenshots](#16--demo-and-screenshots)
17. [Roadmap](#17--roadmap)
18. [Author](#18--author)

## 1. 📖 About the Project

### The problem

Imagine a user named *Alex*, who travels a lot.

- Alex wants to track the weather in many favorite cities, but it is hard to keep up.
- Weather apps show numbers like 34°C, 79% humidity, but do not say what to *wear* or *do*.
- Alex's preferences are not saved securely across devices.

### The solution

AI WeatherWise is one central backend that:

- keeps Alex's account and favorite cities safe in a database,
- fetches live weather on demand,
- and uses AI to turn numbers into *advice a human can use*.

### Why I built it

Write 2 or 3 lines here in your own words: what inspired you, what you learned, what was hardest. This is the part that makes the README truly yours.

## 2. 🔄 How It Works

### The big picture

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


### What happens step by step

| Step | What the user does | What the system does |
|---|---|---|
| 1 | Registers or logs in | Checks the details, stores the user in MongoDB, returns a *JWT token* |
| 2 | Saves a favorite city | Checks the token, blocks duplicates, stores the city for that user |
| 3 | Asks for a city's weather | Calls OpenWeatherMap and returns temperature, humidity, and condition |
| 4 | Asks for an AI summary or advice | Sends the weather to Gemini and returns a friendly sentence |

## 3. ✨ Features Explained

| Feature | What it means in simple words |
|---|---|
| 🔐 *JWT authentication* | After login, the user gets a digital "pass" (token). The server checks it on protected routes. |
| 🔑 *bcrypt hashing* | Passwords are scrambled before saving, so even the database never holds a readable password. |
| 📍 *Favorite locations (CRUD)* | Users can *Create, **Read, **Update, and **D*elete their saved cities. Duplicates are blocked. |
| 🌦️ *Live weather* | Real-time data from OpenWeatherMap. |
| 🤖 *AI summary and recommendation* | Gemini writes a short summary or a tip based on the weather. |
| 🛡️ *Fallback mode* | If the weather or AI key is missing, the API still works with sample (mock) data instead of crashing. |
| 🧹 *Input sanitization* | Cleans user input to block XSS and NoSQL-injection attacks. |
| 🌍 *Public endpoints* | Some weather data can be used without logging in. |

## 4. 🛠️ Tech Stack and Why

| Technology | Used for | Why it was chosen |
|---|---|---|
| *Node.js* | Running JavaScript on the server | Fast, and one language for the whole backend |
| *Express.js* | Web framework | Makes routes and middleware simple |
| *MongoDB + Mongoose* | Database | Flexible data storage, and Mongoose gives clear data models |
| *Google Gemini* (@google/genai) | AI text generation | Turns weather data into natural language |
| *OpenWeatherMap API* | Live weather data | Reliable, real-time weather |
| *JWT + bcryptjs* | Login security | Standard, trusted way to protect users |
| *dotenv* | Secret settings | Keeps keys out of the code |
| *cors, multer, nodemon* | Cross-origin access, file handling, auto-restart in development | Support tools |
| *Thunder Client* | Testing the API | A VS Code extension for sending and checking API requests |

## 5. 🏗️ Architecture and File Guide

The project uses the *MVC pattern* (Model, View, Controller) plus a *service layer. This is a backend-only API, so **routes* take the place of views.


Backend/
 └── src/
      ├── config/
      │    └── db.js                   → connects to MongoDB
      ├── controllers/
      │    ├── authController.js       → register and login logic
      │    ├── locationController.js   → favorite cities logic
      │    ├── weatherController.js    → weather requests
      │    └── aiController.js         → AI summary and recommendation
      ├── middleware/
      │    └── authMiddleware.js       → checks the JWT token
      ├── models/
      │    ├── User.js                 → shape of a user in the database
      │    └── Location.js             → shape of a saved city
      ├── routes/
      │    ├── authRoutes.js           → /api/auth/...
      │    ├── locationRoutes.js       → /api/locations/...
      │    ├── weatherRoutes.js        → /api/weather/...
      │    └── aiRoutes.js             → /api/ai/...
      ├── services/
      │    ├── weatherService.js       → talks to OpenWeatherMap
      │    └── aiService.js            → talks to Google Gemini
      ├── app.js                       → sets up Express, middleware, routes
      └── server.js                    → starts the server


### Who does what?

| Layer | Its one job |
|---|---|
| *Models* | Describe the data and talk to MongoDB |
| *Controllers* | The "brain": read the request, apply logic, send the response |
| *Routes* | Map a URL (like /api/weather/chennai) to the right controller |
| *Middleware* | A checkpoint that runs before the controller (for example, checking the token) |
| *Services* | Keep outside-world work (weather and AI calls) separate from the controllers |

## 6. 🗄️ Database Design

### Core collections

*User*

| Field | Meaning |
|---|---|
| _id | Unique ID created by MongoDB |
| name | User's name |
| email | Unique, so two accounts cannot share one email |
| password | Stored *hashed*, never as plain text |
| role | Defaults to reader |

*Location*

| Field | Meaning |
|---|---|
| _id | Unique ID |
| user | Points to the User who saved the city |
| city, country | The saved place (both are required) |
| createdAt | When it was saved |

*Relationship:* one user can have many saved locations (one-to-many).

The ER design also includes Weather_Data, AI_Insights, Alerts, Subscriptions, and API_Usage_Logs, which are planned for weather history, AI insights, and system logs.

## 7. 🔐 Authentication Explained

1. *Register:* the user sends name, email, and password. The password is *hashed with bcrypt* and saved. The server returns a *JWT token*.
2. *Login:* the user sends email and password. If they match, the server returns a fresh JWT token.
3. *Using the token:* for protected routes, send the token in the request header:


Authorization: Bearer <your_jwt_token>


4. *Checking:* authMiddleware.js reads the token. If it is valid, the request continues. If not, the server refuses it.

## 8. 🛡️ Fallback Mode

External services can fail or be unconfigured. If the OpenWeatherMap or Gemini key is not set, the API returns *mock (sample) data* instead of crashing. This keeps the project runnable for demos and testing.

## 9. 🚀 Getting Started

### What you need

- Node.js installed
- A MongoDB database (local or MongoDB Atlas)
- A Google Gemini API key
- An OpenWeatherMap API key

### Steps

*1. Clone the repository*

bash
git clone https://github.com/amsa27/AI-WeatherWise-API-.git
cd AI-WeatherWise-API-/Backend


*2. Install dependencies*

bash
npm install


*3. Create a .env file* inside the Backend folder:

| Variable | What it is |
|---|---|
| PORT | Port the server runs on (for example 5001) |
| MONGO_URI | MongoDB connection string |
| JWT_SECRET | Secret used to sign tokens |
| GEMINI_API_KEY | Google Gemini key |
| OPENWEATHER_API_KEY | OpenWeatherMap key |


PORT=5001
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
GEMINI_API_KEY=your_gemini_api_key
OPENWEATHER_API_KEY=your_openweather_api_key


> ⚠️ Never upload your real .env file to GitHub.

*4. Start the server*

bash
npm start


*5. Check that it works.* Open http://localhost:5001 in your browser. You should see:

json
{
  "message": "Welcome to the AI WeatherWise API!",
  "documentation": "Use Postman collection or check README.md for endpoint information"
}


This also confirms that the server started and connected to MongoDB.

## 10. 📡 API Reference

Base URL: http://localhost:5001

| Method | Endpoint | What it does | Token needed |
|---|---|---|---|
| POST | /api/auth/register | Creates an account, returns a JWT | No |
| POST | /api/auth/login | Logs in, returns a JWT | No |
| GET | /api/weather/:city | Current weather for a city | No |
| POST | /api/ai/weather-summary | AI-written summary of the weather | See note |
| POST | /api/ai/weather-recommendation | AI-written advice | See note |
| — | /api/locations | Save, list, update, and delete favorite cities | Yes |

> *Note:* If your AI routes are protected in your setup, add the Authorization header shown in section 7.

### Register

json
POST /api/auth/register
{
  "name": "Alex",
  "email": "alex@example.com",
  "password": "yourpassword"
}


Returns *201 Created* with the user details and a JWT token.

### Login

json
POST /api/auth/login
{
  "email": "alex@example.com",
  "password": "yourpassword"
}


Returns *200 OK* with a JWT token.

### Get weather


GET /api/weather/chennai


json
{
  "city": "Chennai",
  "temperature": 21,
  "humidity": 66,
  "condition": "Cloudy"
}


### AI weather summary

json
POST /api/ai/weather-summary
{
  "city": "Chennai",
  "temperature": 21,
  "humidity": 66,
  "condition": "Cloudy"
}


Example result: "Today's weather in Chennai is pleasant and cloudy with moderate humidity."

### AI recommendation

json
POST /api/ai/weather-recommendation
{
  "city": "Chennai",
  "temperature": 21,
  "humidity": 66,
  "condition": "Cloudy"
}


Example result: "Enjoy the comfortable temperature, great day for outdoor plans, and a light jacket might be handy."

### Favorite locations

A saved location needs both city and country. If one is missing, the API replies with an error:

json
{ "success": false, "message": "Please provide both city and country" }


The API also checks whether the user already saved that city (the check ignores upper or lower case), so the same city is not saved twice.

## 11. 🧪 Testing with Thunder Client

All endpoints were tested with *Thunder Client*, a VS Code extension for sending API requests.

Test in this order:

1. *Register* a new user.
2. *Login* with the same email and password.
3. *Get weather* for a city, for example bangalore or chennai.
4. *AI weather summary* with the weather data.
5. *AI recommendation* with the weather data.

Every step above was tested and works.

## 12. 🔒 Security

- 🔑 Passwords are hashed with bcrypt.
- 🎫 Protected routes need a valid JWT.
- 🧹 Inputs are sanitized against XSS and NoSQL injection.
- 🙈 Secrets stay in .env, which is kept out of GitHub.
- 🛡️ Fallback mode prevents crashes when a key is missing.

## 13. 👥 User Roles

| Role | What they can do |
|---|---|
| *Admin* | Manages user accounts, monitors API health and logs, oversees database integrity |
| *Registered user* | Saves favorite cities, fetches weather, requests AI insights, manages their own profile |
| *Guest* | Uses the public weather endpoints without an account |

## 14. 🩺 Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| Cannot find module ... | Dependencies not installed, or wrong folder | Run npm install inside the Backend folder |
| Server stops right after starting | MongoDB connection failed | Check MONGO_URI and your internet or Atlas access |
| 401 or "invalid token" | Missing or wrong token | Log in again and send Authorization: Bearer <token> |
| Port already in use | Another app uses that port | Change PORT in .env |
| Weather or AI returns sample data | API key missing or wrong | Check the keys in .env (this is fallback mode) |

## 15. 📚 Glossary

| Word | Simple meaning |
|---|---|
| *API* | A way for one program to ask another program for data |
| *Backend* | The hidden part of an app that stores data and does the work |
| *REST* | A common style for building APIs with URLs and methods like GET and POST |
| *JWT* | A signed digital pass that proves who a user is |
| *Hashing* | Scrambling a password so it cannot be read back |
| *MVC* | Splitting code into Models, Views, and Controllers |
| *Middleware* | A checkpoint that runs before the main code |
| *ODM (Mongoose)* | A tool that helps JavaScript work with MongoDB |
| *Environment variable* | A secret setting kept outside the code |

## 16. 🎬 Demo and Screenshots

- 📹 *API testing video:* add link
- 📹 *Full demo video:* add link
- 📄 *Project document:* add link

*Screenshots:* add your Thunder Client screenshots here, for example:




![Register API](screenshots/register.png)




![Weather API](screenshots/weather.png)




![AI Recommendation](screenshots/recommendation.png)




## 17. 🗺️ Roadmap

- [ ] Weather alerts
- [ ] Multi-day forecasts
- [ ] Admin dashboard for logs and API usage
- [ ] Frontend app that uses this API

## 18. 👩‍💻 Author

*Amsa*
GitHub: [@amsa27](https://github.com/amsa27)

Add your college and course here if you want.

---

<div align="center">

⭐ If you like this project, give it a star.

</div>
