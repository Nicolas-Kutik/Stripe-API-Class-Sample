# Stripe-API-Class-Sample
Javascript Framework In-class sample for Group Deliverables. This will contain step by step instructions for basic Stripe API Checkout.

NPM Packages:
- dotenv
- express
- stripe

Steps:

Step 1: Create a new initial project

Step 2b: Create a Stripe Account and get API Key

Step 2a: Create a "Server.js" or "App.js"

Step 2b: Initialize through "npm init -y"

Step 2c: Install NPM packages "npm install express dotenv stripe"

Step 2d: Create a .env file and replace api key with your own

Step 3a: Setup "Server.js" or "App.js" through following commands

Step 3b: Require Express, the Dotenv package for the API Key, and the Stripe package:
    require('dotenv').config();
    const express = require('express');
    const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

Step 3c: Use Express through:
    const app = express();
    app.use(express.static('public'));
    app.use(express.json());

Step 3d: Create Checkout Process within the server.js and Listen to Port 4242:
    app.post('/create-checkout-session', async (req, res) => {
    try {
        const session = await stripe.checkout.sessions.create({
        payment_method_types: ['card'],
        mode: 'payment',
        line_items: [
            {
                price_data: {
                currency: 'usd',
                product_data: {
                    name: 'T-shirt',
            },
            unit_amount: 2000, // $20.00 USD
          },
          quantity: 1,
        }
      ],
      success_url: 'http://localhost:4242/success.html',
      cancel_url: 'http://localhost:4242/cancel.html',
    });

    res.json({ url: session.url });
    } catch (err) {
    res.status(500).json({ error: err.message });
    }
    });

    app.listen(4242, () => console.log('Node server listening on port 4242!'));

Step 4a: Create a folder called "public" and route within called "index.html"

Step 4b: Use the "!" function to automatically populate the HTML

Step 4c: Add basic html for an example product like "T-shirt" when creating checkout process and add header label:
    <h1>Buy White T-shirt - $20</h1>
    <button id="checkout-button">Checkout</button>

Step 4d: Create a script section to listen when checkout is clicked and call the create checkout session function:
    <script>
        const checkoutButton = document.getElementById('checkout-button');

        checkoutButton.addEventListener('click', () => {
            fetch('/create-checkout-session', {
                method: 'POST',
            })
            .then(res => res.json())
            .then(data => {
                window.location.href = data.url;
            });
        });
    </script>

Step 5: Run server.js and vist the port used to test your basic stripe checkout. Use any test card listed here: https://docs.stripe.com/testing