# Product — Recipe Management & Discovery App

A full-stack web application for sharing, managing, and discovering recipes. Users can register, log in, create their own recipes with images, browse recipes posted by others, search by name, and maintain a personal list of liked recipes.

---

## Tech Stack

| Layer | Technology | Version |
|---|---|---|
| Runtime | **Node.js** | — |
| Web framework | **Express.js** | 4.19.2 |
| Templating | **EJS** | 3.1.10 |
| CSS framework | **Bootstrap** | 5.3.3 (CDN) |
| DOM utilities | **jQuery** | 3.6.0 (CDN) |
| Database | **MySQL** | 2.18.1 (driver) |
| Session management | **express-session** | 1.18.0 |
| File uploads | **multer** | 1.4.5-lts.1 |
| Body parsing | **body-parser** | 1.20.2 |

> Dev-only: `@types/express` and `@types/body-parser` (TypeScript type definitions used for editor support).

---

## Project Structure

```
product/
├── app.js                     # Express server entry point
├── models/
│   ├── db.js                  # MySQL connection
│   └── authMiddleware.js      # Session-based auth guard
├── routes/
│   ├── authRoutes.js          # Login, register, logout
│   ├── productRoutes.js       # Recipe CRUD + search
│   └── userRoutes.js          # Like / unlike recipes
├── views/
│   ├── partials/navbar.ejs    # Shared navigation bar
│   ├── login.ejs
│   ├── register.ejs
│   ├── index.ejs              # User's own recipes
│   ├── homepage.ejs           # All users' recipes
│   ├── ProductInfo.ejs        # Recipe detail page
│   ├── addProduct.ejs
│   ├── updateProduct.ejs
│   ├── likes.ejs              # Liked recipes
│   ├── searchFound.ejs
│   └── searchNotFound.ejs
└── public/
    ├── images/                # Uploaded recipe images
    └── stylesheet/            # Per-page CSS files
```

---

## Database Schema

The application expects a MySQL database named `recipe` with the following tables:

**`account`** — user credentials  
`username` (PK), `password`

**`recipes`** — recipe data  
`recipeID` (PK), `recipeName`, `recipeImage` (binary), `totalTimeEstimated`, `servingSize`, `ingredients`, `preparationSteps`, `username` (FK → account)

**`liked_recipes`** — many-to-many likes  
`username` (FK → account), `recipeID` (FK → recipes)

---

## Getting Started

### Prerequisites
- Node.js (any recent LTS)
- MySQL server running locally

### Install dependencies
```bash
npm install
```

### Configure the database
Edit `models/db.js` and set your MySQL credentials:
```js
const db = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: '',
  database: 'recipe'
});
```

### Start the server
```bash
node app.js
```

The app is served at **http://localhost:3000**.

---

## Key Routes

| Method | Path | Description |
|---|---|---|
| GET | `/` | Login page |
| POST | `/loginAccount` | Authenticate user |
| GET | `/register` | Registration page |
| POST | `/registerAccount` | Create new account |
| GET | `/logout` | End session |
| GET | `/home` | Browse all recipes |
| GET | `/products` | Current user's recipes |
| GET | `/addProductForm` | Add recipe form |
| POST | `/addProduct` | Save new recipe (with image) |
| GET | `/products/:id` | Recipe detail |
| GET/POST | `/products/:id/update` | Edit recipe |
| POST | `/products/:id/delete` | Delete recipe |
| POST | `/search` | Search recipes by name |
| POST | `/addIntoLike` | Like a recipe |
| POST | `/unlike` | Unlike a recipe |
| GET | `/likes` | View liked recipes |
