# git-action-capstone-project-

## E-Commerce Platform CI/CD Pipeline 

### Overview:  This project demonstrates a full-stack e-commerce platform with an automated CI/CD pipeline using GitHub Actions, Docker, and cloud deployment.


## Features
- **Backend**: Node.js API with Express.
- **Frontend**: React app with Vite.
- **CI/CD**: Automated testing, building, and deployment using GitHub Actions.
- **Cloud Deployment**: Dockerized services deployed to AWS.

#### Create a GitHub repository named e-commerce-platform.
![image](https://github.com/user-attachments/assets/890590d5-d618-4931-b29c-212f6ced6ff8)


#### Navigate to the e-commerce-platform directory using cd and create two directories: api (for backend) and webapp (for frontend).
![image](https://github.com/user-attachments/assets/577c0ef5-3d80-4b8c-b9a4-5a306b3a1102)


#### Initialize Git for version control: - **git init** -  - **git branch -M main** -
![image](https://github.com/user-attachments/assets/03f8a41b-95ad-4ebf-89df-90ce7907b933)


#### Create a .gitignore file in the root directory to ignore unnecessary files: - **touch .gitignore** - - **echo "node_modules/" >> .gitignore** - - **echo ".env" >> .gitignore** -
![image](https://github.com/user-attachments/assets/92a97dc7-e24c-4d7a-a0ce-41138deb1e4f)

#### Navigate to the 'api' directory - **cd api** - and Initialize a Node.js project  - **npm init -y** -
![image](https://github.com/user-attachments/assets/5273fc4d-279d-481c-8277-23f2e6ac4f49)

#### Install the necessary dependencies: - **npm install express mongoose dotenv cors** - - **npm install --save-dev jest supertest nodemon** -
![image](https://github.com/user-attachments/assets/ecb17d54-f042-471b-8183-4411e52b5a48)

#### Create the backend folder structure: - **mkdir src** -  - **cd src** - - **mkdir controllers models routes** -
![image](https://github.com/user-attachments/assets/31606a35-e002-4d3c-9b84-e7ee989f14a0)

#### Add the main backend server file: **App.js**
![image](https://github.com/user-attachments/assets/2d89844d-bbf7-4f56-af00-0d8c60a24460)

##  Create the Backend Application

#### Set up app.js: **Add the following codes to the file**
const express = require("express");
const mongoose = require("mongoose");
const dotenv = require("dotenv");
const cors = require("cors");

dotenv.config();

const app = express();

// Middleware
app.use(cors());
app.use(express.json());

// Routes
app.use("/api/products", require("./routes/productRoutes"));

// Start the server
const PORT = process.env.PORT || 5000;
mongoose
  .connect(process.env.DB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
  })
  .catch((error) => console.log(error));

#### Create MongoDB models (models/productModel.js): Add the following code into it.
const mongoose = require("mongoose");

const productSchema = new mongoose.Schema({
  name: { type: String, required: true },
  price: { type: Number, required: true },
  description: { type: String },
  stock: { type: Number, default: 0 }
});

module.exports = mongoose.model("Product", productSchema);
![image](https://github.com/user-attachments/assets/ab2134b1-4592-4490-b478-003e584e6d03)


#### Add controllers (controllers/productController.js: Add the following codes into it.
const Product = require("../models/productModel");

exports.getAllProducts = async (req, res) => {
  try {
    const products = await Product.find();
    res.status(200).json({ success: true, data: products });
  } catch (error) {
    res.status(500).json({ success: false, message: error.message });
  }
};

exports.createProduct = async (req, res) => {
  try {
    const product = await Product.create(req.body);
    res.status(201).json({ success: true, data: product });
  } catch (error) {
    res.status(400).json({ success: false, message: error.message });
  }
};
![image](https://github.com/user-attachments/assets/c31ae633-6ec4-461a-a895-0b5096b74d3f)


#### Set up routes (routes/productRoutes.js): Add the following codes into it.
const express = require("express");
const router = express.Router();
const { getAllProducts, createProduct } = require("../controllers/productController");

router.route("/").get(getAllProducts).post(createProduct);

module.exports = router;
![image](https://github.com/user-attachments/assets/a3b330d9-d88b-4c22-99d7-e7d275d828dc)


#### Create .env for environment variables: **touch .env** and this this code into it. 
DB_URI=mongodb://username:password@localhost:27017/ecommerce-test
![image](https://github.com/user-attachments/assets/62baed1c-2c82-4faf-a8c6-ff4bf7d64c18)


#### Add the backend start script to package.json
"scripts": {
  "start": "node app.js",
  "dev": "nodemon app.js",
  "test": "jest"
}

#### Test the backend **npm run dev**
![image](https://github.com/user-attachments/assets/2c1a69a8-29db-4721-9eb2-6473efa09524)


#### Access your application on port 5000: localhost:5000
![image](https://github.com/user-attachments/assets/56f26622-fafb-43bf-a73e-388dd4470ff6)




## Create Frontend Application 

#### Navigate to the webapp directory and initialize a React project: - **cd ../webapp** -  - **npm create vite@latest . --template react** -  - **npm install** - 
![image](https://github.com/user-attachments/assets/44739b98-cedd-442e-939a-56d4658d7f95)


#### Install additional dependencies:   - **npm install axios react-router-dom** -  - **npm install --save-dev jest @testing-library/react** -
![image](https://github.com/user-attachments/assets/b2e3dc65-64f4-481f-ab76-a58953cc98c0)


#### Create the frontend folder structure: - **mkdir src/components src/pages** -
![image](https://github.com/user-attachments/assets/c93ee551-bdcf-42ab-bdf1-896f6508cc76)


#### Add the main frontend entry point: - **src/main.jsx** -
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.tsx'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>,
)

#### Add React Router setup: - **src/App.jsx** - 
import React from 'react'
import './App.css'

function App() {
  return (
    <div className="app">
      <header>
        <h1>Welcome to My E-Commerce Platform</h1>
        <nav>
          <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/products">Products</a></li>
            <li><a href="/cart">Cart</a></li>
          </ul>
        </nav>
      </header>
      <main>
        <p>Start building your e-commerce platform here!</p>
        {/* Add components like ProductList, Navbar, Footer, etc. */}
      </main>
      <footer>
        <p>&copy; 2024 My E-Commerce Platform</p>
      </footer>
    </div>
  )
}

export default App

#### Create a page component (src/pages/ProductListing.jsx)
import React, { useEffect, useState } from "react";
import axios from "axios";

const ProductListing = () => {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    axios.get("http://localhost:5000/api/products").then((res) => setProducts(res.data.data));
  }, []);

  return (
    <div>
      <h1>Product Listing</h1>
      <ul>
        {products.map((product) => (
          <li key={product._id}>{product.name} - ${product.price}</li>
        ))}
      </ul>
    </div>
  );
};

export default ProductListing;
![image](https://github.com/user-attachments/assets/7787a4c3-fadb-466f-a9ce-494053f32089)

#### Test the frontend locally: - **npm run dev** -
![image](https://github.com/user-attachments/assets/9b4e80e0-9f14-4408-b959-d311b1cd461c)

#### Access the application on localhost:5173

![image](https://github.com/user-attachments/assets/d8b61bbb-e0fe-49c3-adba-20a261970db8)


# Create Folder Structure for GitHub Workflows

## GitHub workflows are stored in the .github/workflows/ directory of your repository. Each workflow is defined in a YAML file


####  Write workflow for the Frontend Application: Create a directory - **.github/workflows/frontend.yml** -
name: Frontend CI/CD Workflow

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the repository
      - name: Checkout code
        uses: actions/checkout@v3

      # Step 2: Set up Node.js environment
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      # Step 3: Install dependencies
      - name: Install dependencies
        run: npm install
        working-directory: ./webapp

      # Step 4: Run tests
      - name: Run tests
        run: npm test
        working-directory: ./webapp

      # Step 5: Build the application
      - name: Build frontend
        run: npm run build
        working-directory: ./webapp

      # Step 6: Upload build artifacts (optional)
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: frontend-build
          path: ./webapp/dist



## Write Workflow for the Backend Application: - *.github/workflows/backend-node.yml** -
name: Backend CI/CD Workflow (Node.js)

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the repository
      - name: Checkout code
        uses: actions/checkout@v3

      # Step 2: Set up Node.js environment
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      # Step 3: Install dependencies
      - name: Install dependencies
        run: npm install
        working-directory: ./backend

      # Step 4: Run tests
      - name: Run tests
        run: npm test
        working-directory: ./backend

      # Step 5: Start the backend server (optional for deployment)
      - name: Start backend server
        run: npm start
        working-directory: ./backend

##  Test the Workflows by running these commands: -**git add .** -  - **git commit -m "Test workflows"** -**git push origin main** _
![image](https://github.com/user-attachments/assets/c1af907c-0f80-4d9b-89a6-040bc65d93f2)
![image](https://github.com/user-attachments/assets/99ae4750-da5c-4661-8f4c-0473321eed0e)




























        
          














































































































































































































































































































