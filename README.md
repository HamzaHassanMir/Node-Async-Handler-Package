# Async Handler

![npm version](https://img.shields.io/npm/v/your-package-name)
![license](https://img.shields.io/npm/l/your-package-name)

A lightweight utility for handling async errors in Express.js.

## Async Handler

A lightweight utility for handling async/await errors in Express.js route handlers without using repetitive `try/catch` blocks.

It automatically catches rejected promises and forwards errors to Express error middleware.

## Why Use This?

In Express applications, async routes often require repetitive error handling like this:

app.get("/users", async (req, res, next) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    next(error);
  }
})

With async-handler, you can write cleaner code:

app.get(
  "/users",
  asyncHandler(async (req, res) => {
    const users = await User.find();
    res.json(users);
  })
)

## Installation

```
npm install express-async-handler
```

## Usage

Import the handler and wrap your async route functions.

const asyncHandler = require("express-async-handler");

## Example

const express = require("express");
const asyncHandler = require("express-async-handler");

const app = express();

app.get(
  "/users",
  asyncHandler(async (req, res) => {
    const users = await User.find();
    res.json(users);
  })
)

If an error occurs, it will automatically be passed to Express error middleware.

## How It Works

The utility wraps an async function and ensures that rejected promises are passed to Express's next() function.

Implementation:

const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

module.exports = asyncHandler;

## Error Middleware Example

Make sure your Express app has an error handler.

app.use((err, req, res, next) => {
  res.status(500).json({
    message: err.message,
  })
})

## Features

1. Removes repetitive try/catch
2. Clean and readable async routes
3. Tiny and lightweight
4. Works with all Express versions supporting async functions

## License

MIT

