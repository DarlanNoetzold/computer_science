# Security with Node

## 1. Install Dependencies:

First, set up your Node.js project and install the necessary dependencies, such as express for building the API and jsonwebtoken for handling JSON Web Tokens (JWT) for authentication.


```
npm install express jsonwebtoken
```

## 2. Implement Authentication:

Create a registration and login system for your users. When a user registers, store their credentials securely in your database (e.g., MongoDB, MySQL).
Implement a login route that verifies the user's credentials against the stored data and generates a JWT upon successful login.

## 3. Generate JWTs:

When a user logs in, generate a JWT containing user-specific information (e.g., user ID, roles) and a secret key. Sign the JWT using the secret key.

```
const jwt = require('jsonwebtoken');
const secretKey = 'your-secret-key';

// Create and sign a JWT
const token = jwt.sign({ userId: user.id, role: user.role }, secretKey, { expiresIn: '1h' });
```

## 4. Implement Middleware for Authentication:

Create a middleware function that verifies the JWT in the request headers for protected routes. You can use the jsonwebtoken library to verify and decode the JWT.
```
const jwt = require('jsonwebtoken');
const secretKey = 'your-secret-key';

const authenticateToken = (req, res, next) => {
    const token = req.header('Authorization');
    if (!token) return res.status(401).json({ message: 'Access denied' });

    jwt.verify(token, secretKey, (err, user) => {
        if (err) return res.status(403).json({ message: 'Invalid token' });
        req.user = user;
        next();
    });
};

module.exports = authenticateToken;
```

## 5. Protect Routes:

Apply the authenticateToken middleware to the routes that require authentication and authorization.
```
const express = require('express');
const app = express();
const authenticateToken = require('./middleware/authenticateToken');

// Protected route
app.get('/api/protected', authenticateToken, (req, res) => {
    // This route is protected, and req.user contains user information
    res.json({ message: 'Protected route accessed', user: req.user });
});

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
```
With these steps, you can secure your Node.js API by implementing custom authentication and authorization using JWTs.


⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
