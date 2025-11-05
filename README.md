# 🥐 TAMAQ — Food Saver (React Native + Django)

![Python](https://img.shields.io/badge/Python-3.11-blue.svg)
![Django](https://img.shields.io/badge/Django-5.0-success.svg)
![React_Native](https://img.shields.io/badge/React%20Native-Expo-61A6FF.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-✔-3178C6)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13+-336791.svg)


---

## 🧩 Overview
**TAMAQ** is a TooGoodToGo-style marketplace where users reserve surplus food from local stores at a discount.  
It’s a full-stack app with:

- **Mobile (Expo / React Native)** app for browsing nearby deals, filtering by category & distance, and reserving items.
- **Django + DRF** backend with JWT auth (email/password, Google, Apple), password reset, reservations, cart, distance sorting, and category search.

---

## 🚀 Features

| Area | Highlights |
| --- | --- |
| 👤 **Auth** | Email/password, Google OAuth, Apple Sign-In, short-lived Access Token + Refresh Token rotation |
| 🗺️ **Discovery** | Search by text, filter by *grocery / fast food / pastry*, sort by nearest (Haversine), distance cap (`max_distance_km`) |
| 🛒 **Reserve** | Real-time quantity decrement on reservation; view active/past orders; simple cart endpoints |
| 🖼️ **Media** | Image upload fields for stores and items |
| ✉️ **Reset** | Email-based password reset with tokenized link |
| 📱 **Mobile UX** | Smooth tab transitions, modal product sheet, pull-to-refresh, toast feedback, skeleton loaders |
| 🔐 **Security** | Custom `JWTAuthentication` (DRF) + refresh token store (`UserToken`) with expiry |

---
## Images
<img width="921" height="421" alt="Screenshot 2025-07-25 at 1 56 16 PM" src="https://github.com/user-attachments/assets/fe8b5aa5-f181-4bcc-9db3-fdc87bf82858" />

---
## 🧱 Tech Stack

**Frontend (Mobile)**
- Expo / React Native
- TypeScript
- `expo-router`, Reanimated, Gesture Handler, Haptics
- AsyncStorage for token storage

**Backend**
- Django 5, Django REST Framework
- Custom `User` (email as username)
- JWT (PyJWT) + `UserToken` table
- PostgreSQL
- Email (SMTP) for reset links
- Google / Apple identity verification


---

## 📲 Mobile App (Expo) Highlights

- **Tabs:** Home, Browse, Cart (Reserves), Accaunts (profile/logout)
- **Home:** search + category chips + distance chips (Any/1/3/5/10 km)
- **Browse:** category galleries
- **Detail Modal:** pull down to dismiss; reserve button with toast
- **Location:** asks permission; falls back to newest items when denied
- **Transitions:** slide animations between tabs; skeleton screens for perceived speed

## Data Model
- **User** (custom, email login)
- **Store**: owner, name, logo, address, city, latitude, longitude, is_verified
- **FoodItem**: item_id(UUID), store, image, rating, address, pickup_date/start/end, available_quantity, price_before/price, category
- **Reservation**: user, food_item, quantity, is_collected, reserved_at
- **CartItem**: user, food_item, quantity
- **UserToken**: tracks refresh tokens (expiry)
- **Reset**: reset tokens for password reset

## 🔐 Authentication
- **Access** token (HS256, SECRET_KEY) — very short-lived ~20s
- **Refresh** token (HS256, REFRESH_SECRET) — 7 days, stored server-side in UserToken
- **Headers:** Authorization: Bearer <access_token>

## ⚙️ Backend — Environment & Run

Create backend/.env:
```env
# Django
SECRET_KEY=your_django_secret
REFRESH_SECRET=your_refresh_secret
DEBUG=true
ALLOWED_HOSTS=127.0.0.1,localhost,recall-op1f.onrender.com,bekarys2003.github.io

# DB
DB_NAME=your_db
DB_USER=your_user
DB_PASSWORD=your_pass
DB_HOST=127.0.0.1
DB_PORT=5432

# Email (for password reset)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_HOST_USER=your_email@gmail.com
EMAIL_HOST_PASSWORD=your_app_password

```

Install & run:
```env
cd backend
python -m venv .venv && source .venv/bin/activate 
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver 0.0.0.0:8000
```

## 📱 Frontend — Run (Expo)
```env
cd MyLoginApp
npm install
npx expo start
```
