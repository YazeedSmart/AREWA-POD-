const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
app.use(express.json());
app.use(cors());

// Connect to MongoDB
mongoose.connect("mongodb://localhost:27017/ecommerce", { useNewUrlParser: true, useUnifiedTopology: true });

// Product Schema
const Product = mongoose.model("Product", new mongoose.Schema({
    name: String,
    price: Number,
    description: String,
    image: String
}));

// Routes
app.get("/products", async (req, res) => {
    const products = await Product.find();
    res.json(products);
});

app.post("/products", async (req, res) => {
    const newProduct = new Product(req.body);
    await newProduct.save();
    res.json({ message: "Product added!" });
});

// Start Server
app.listen(5000, () => console.log("Server running on port 5000"));
