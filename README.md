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




















































































































































































































