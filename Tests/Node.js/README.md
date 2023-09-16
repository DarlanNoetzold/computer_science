# Node.js tests

## Unit Tests
Unit tests in Node.js focus on testing individual functions or components. In this example, we have a simple function called add that we want to test.

```
// math.js
function add(a, b) {
    return a + b;
}

module.exports = { add };
```

Now, let's write a unit test for the add function using Mocha and Chai:

```
// test/math.test.js
const { expect } = require('chai');
const { add } = require('../math');

describe('Math Library', () => {
    it('should add two numbers correctly', () => {
        const result = add(2, 3);
        expect(result).to.equal(5);
    });

    it('should handle negative numbers', () => {
        const result = add(-2, 3);
        expect(result).to.equal(1);
    });

    it('should handle zero', () => {
        const result = add(0, 5);
        expect(result).to.equal(5);
    });
});
```

In this unit test, we describe the "Math Library," write test cases using it(), and use Chai's expect() assertions to verify the behavior of the add function.

## Integration Tests
Integration tests in Node.js verify the interactions between different parts of your application. In this example, let's assume we have a simple Express.js API endpoint.

```
// app.js
const express = require('express');
const app = express();

app.get('/api/greetings', (req, res) => {
    res.status(200).json({ message: 'Hello, world!' });
});

module.exports = app;
```

Here's an integration test for the /api/greetings endpoint using Mocha, Chai, and Supertest:

```
// test/app.test.js
const { expect } = require('chai');
const request = require('supertest');
const app = require('../app');

describe('API Endpoints', () => {
    it('should return a greeting message', (done) => {
        request(app)
            .get('/api/greetings')
            .expect(200)
            .end((err, res) => {
                if (err) return done(err);
                expect(res.body.message).to.equal('Hello, world!');
                done();
            });
    });
});
```

In this integration test, we use supertest to make an HTTP request to the /api/greetings endpoint and validate the response using Chai's assertions.

These examples demonstrate how to write unit tests and integration tests in a Node.js project using Mocha, Chai, and Supertest. Proper testing ensures the reliability and functionality of your Node.js applications in different scenarios, from individual functions to interactions with HTTP endpoints.


⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
