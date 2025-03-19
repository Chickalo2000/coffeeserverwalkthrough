# Coffee Shop Location API Setup Guide

## Initial Setup Checklist
- [x] Create a new directory for your project
- [x] Initialize npm project: `npm init -y`
- [?] Install required dependencies: `npm install express mongoose dotenv` 

## MongoDB Setup
- [x] Sign up for a free MongoDB Atlas account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
- [ ] Create a new cluster (free tier is fine)
- [ ] Set up database access:
  - Create a database user with password
  - Save these credentials securely
- [ ] Set up network access: this is done inside of the browser after sign up
  - Add your IP address
  - Or set to allow access from anywhere (0.0.0.0/0) for development
- [ ] Get your connection string from MongoDB Atlas:
  - Click "Connect"
  - Choose "Connect your application"
  - Copy the connection string

## Project Structure Setup
- [ ] Create the following files in your project root:
  ```
  /your-project
    ├── server.js
    ├── .env
    ├── models/
    │   └── CoffeeRecipe.js
    └── package.json
  ```

## Environment Setup
- [ ] Create `.env` file with:
  ```
  MONGODB_URI=your_mongodb_connection_string
  PORT=3000
  ```

## Model Setup
- [ ] In `models/CoffeeRecipe.js`, add:
  ```javascript
  const mongoose = require('mongoose');

  const coffeeRecipeSchema = new mongoose.Schema({
    name: {
      type: String,
      required: true
    },
    latitude: {
      type: Number,
      required: true
    },
    longitude: {
      type: Number,
      required: true
    },
    // Optional additional fields you might want:
    description: String,
    ingredients: [String],
    createdAt: {
      type: Date,
      default: Date.now
    }
  });

  module.exports = mongoose.model('CoffeeRecipe', coffeeRecipeSchema);
  ```

## Server Setup
- [ ] In `server.js`, add:
  ```javascript
  const express = require('express');
  const mongoose = require('mongoose');
  require('dotenv').config();
  const CoffeeRecipe = require('./models/CoffeeRecipe');

  const app = express();
  app.use(express.json());

  // Connect to MongoDB
  mongoose.connect(process.env.MONGODB_URI)
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Could not connect to MongoDB:', err));

  // GET route to fetch all recipes
  app.get('/api/recipes', async (req, res) => {
    // TODO: Implement fetching all recipes
  });

  // POST route to create a new recipe
  app.post('/api/recipes', async (req, res) => {
    // TODO: Implement creating a new recipe
  });

  const PORT = process.env.PORT || 3000;
  app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
  ```

## Route Implementation Guide
Your task is to implement the following routes:

### GET /api/recipes
This route should:
- Fetch all coffee recipes from the database
- Return them as JSON
- Handle potential errors appropriately

### POST /api/recipes
This route should:
- Accept recipe data in the request body
- Validate the required fields (name, latitude, longitude)
- Create a new recipe in the database
- Return the created recipe
- Handle validation and database errors

## Testing Your Setup
- [ ] Start your server: `node server.js`
- [ ] Test POST route using Postman or curl:
  ```bash
  curl -X POST http://localhost:3000/api/recipes \
    -H "Content-Type: application/json" \
    -d '{
      "name": "Classic Espresso",
      "latitude": 40.7128,
      "longitude": -74.0060,
      "description": "Traditional Italian espresso",
      "ingredients": ["Coffee beans", "Water"]
    }'
  ```
- [ ] Test GET route: Visit `http://localhost:3000/api/recipes` in your browser

## Next Steps
- [ ] Add error handling
- [ ] Implement input validation
- [ ] Add authentication
- [ ] Create additional routes (UPDATE, DELETE)
- [ ] Add more fields to your model as needed
- [ ] Implement searching and filtering

## Common Issues & Solutions
- If you can't connect to MongoDB:
  - Check if your IP is whitelisted
  - Verify your connection string
  - Make sure your username/password are correct
- If routes aren't working:
  - Check if server is running
  - Verify the correct port
  - Ensure request body matches model schema

Remember to never commit your `.env` file to version control!
