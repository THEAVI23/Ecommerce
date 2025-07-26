# REEBOK - An AI-Powered E-commerce Platform with Negotiation Bot

## Overview
REEBOK is a full-fledged e-commerce platform built with Flask, MySQL, and integrated with an AI-powered chatbot for price negotiation. This platform aims to provide a dynamic shopping experience where customers can haggle for product prices, enhancing engagement and flexibility, alongside offering product recommendations.

## Features

### E-commerce Platform
* **Product Catalog:** Browse products across various categories (Men, Women, Kids).
* **Product Details:** View detailed information for each product, including descriptions, prices, and available sizes.
* **Shopping Cart:** Add products to a persistent shopping cart, manage quantities, and remove items.
* **Checkout Flow:** A simplified checkout process.
* **User Authentication:** Secure registration and login for both customers and administrators.
* **Product Recommendation System:** Get personalized product recommendations based on item similarity (TF-IDF and Cosine Similarity).

### AI-Powered Negotiation Chatbot
* **Interactive Negotiation:** Users can initiate price negotiations for products directly on the product page.
* **Intent Recognition:** The bot understands various user intents related to negotiation (e.g., "offer price," "ask for fair offer," "accept deal").
* **Dynamic Offer Generation:** The bot calculates and proposes counter-offers based on the product's original price, a predefined minimum acceptable price, and the history of negotiations.
* **Offer Evaluation:** It validates user offers against business rules (e.g., too high, too low, acceptable).
* **Context Management:** Maintains conversational context to provide relevant responses.
* **Seamless Integration:** The negotiation outcome (discounted price) is reflected when adding the item to the cart.

### Admin Panel
* **Dashboard:** Overview of key e-commerce metrics (e.g., product count, admin count).
* **User Management:** View, create, and delete admin accounts. View customer accounts.
* **Product Management:** Add new products with details like article number, name, category, type, description, original price, minimum acceptable price, and size quantities.
* **Order Management:** (Placeholder for future implementation, as `admin/orders` route exists but no detailed logic is shown in the provided code snippet).
* **Secure File Uploads:** Handles image uploads for products and admin profiles.

## Technologies Used

### Backend
* **Python 3:** Core programming language.
* **Flask:** Web framework for building the application.
* **MySQL:** Relational database for storing user, product, and order data.
* **Flask-MySQLdb:** Flask extension for MySQL integration.
* **WTForms:** For creating and validating web forms.
* **Passlib (sha256_crypt):** For secure password hashing.
* **Werkzeug:** Utilities for WSGI applications (used for `secure_filename`).
* **`os`, `pickle`, `json`, `random`:** Standard Python libraries for various functionalities.

### AI/ML
* **NLTK (Natural Language Toolkit):** For tokenization and stemming in the chatbot.
* **TensorFlow / Keras:** For loading and running the pre-trained deep learning model (`chatmodel.h5`) for intent classification.
* **Pandas:** For data manipulation in the recommendation system.
* **Scikit-learn (TfidfVectorizer, linear_kernel):** For building the content-based recommendation system.

### Frontend
* **HTML5, CSS3, JavaScript:** For the overall website structure, styling, and interactivity.
* **JQuery:** (Likely used for AJAX calls, especially for the chatbot and add-to-cart functionality, though not explicitly imported in the provided `app.py`).

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

* Python 3.x
* MySQL Server
* `pip` (Python package installer)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/your-repository-name.git](https://github.com/your-username/your-repository-name.git)
    cd your-repository-name
    ```
2.  **Set up the MySQL database:**
    * Create a database named `indus`.
    * You'll need to create the necessary tables (`users`, `products`, `product_sizes`, `orders`, `order_items`). Here's a basic schema (you might need to adjust column types/constraints based on your full application logic):

        ```sql
        -- Users Table
        CREATE TABLE users (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(100),
            email VARCHAR(50) UNIQUE,
            password VARCHAR(255),
            user_type VARCHAR(20),
            image_path VARCHAR(255) DEFAULT ''
        );

        -- Products Table
        CREATE TABLE products (
            id INT AUTO_INCREMENT PRIMARY KEY,
            article_no VARCHAR(100) UNIQUE,
            prd_name VARCHAR(100),
            prd_description TEXT,
            prd_type VARCHAR(100),
            prd_category VARCHAR(100),
            prd_price INT,
            prd_min_price INT,
            image_path VARCHAR(255)
        );

        -- Product Sizes Table (for product quantities per size)
        CREATE TABLE product_sizes (
            id INT AUTO_INCREMENT PRIMARY KEY,
            article_no VARCHAR(100),
            size_s INT DEFAULT 0,
            size_m INT DEFAULT 0,
            size_l INT DEFAULT 0,
            size_xl INT DEFAULT 0,
            FOREIGN KEY (article_no) REFERENCES products(article_no) ON DELETE CASCADE
        );

        -- Orders Table
        CREATE TABLE orders (
            order_id INT AUTO_INCREMENT PRIMARY KEY,
            order_status VARCHAR(50),
            order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            -- Add user_id if you want to link orders to users
        );

        -- Order Items Table
        CREATE TABLE order_items (
            item_id INT AUTO_INCREMENT PRIMARY KEY,
            order_id INT,
            item VARCHAR(100), -- article_no of the product
            quantity INT,
            price INT, -- price at the time of purchase
            FOREIGN KEY (order_id) REFERENCES orders(order_id) ON DELETE CASCADE
        );
        ```

    * Update `app.config['MYSQL_USER']` and `app.config['MYSQL_PASSWORD']` in `app.py` if your MySQL credentials are different from `root`/`root`.

3.  **Install Python dependencies:**
    Create a `requirements.txt` file in your project root with the following content:
    ```
    Flask
    Flask-MySQLdb
    WTForms
    Werkzeug
    passlib
    numpy
    pandas
    scikit-learn
    nltk
    tensorflow
    keras
    ```
    Then install them:
    ```bash
    pip install -r requirements.txt
    ```

4.  **Download NLTK data:**
    In your Python environment, run:
    ```python
    import nltk
    nltk.download('punkt')
    ```

5.  **Place AI/ML models and data:**
    * Ensure your `chatbot/training_data` (a pickled file) and `chatbot/chatmodel.h5` are in the correct paths as specified in `app.py`:
        `E:\Avishkar imp\modified Ecommerce Chatbot\Ecommerce v1.0\chatbot\`
    * Ensure your `recommendation/WebProducts.csv` is in the correct path as specified in `app.py`:
        `E:\Avishkar imp\modified Ecommerce Chatbot\Ecommerce v1.0\recommendation\`
    * Make sure `intent.json` is also in the `chatbot` directory.

6.  **Run the application:**
    ```bash
    python app.py
    ```
    The application will run on `http://127.0.0.1:5050`.

