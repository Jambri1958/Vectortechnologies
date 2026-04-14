# Email Service Integration with SendGrid or Resend

## Overview
This document outlines the integration of an email notification system for the Media Restore AI project in the vectortechnologies repository. 

## 1. Email Service Integration
Choose between **SendGrid** or **Resend** for our email service. Here’s how to set it up:

### Using SendGrid:
- Sign up for a free account on [SendGrid](https://sendgrid.com/).
- Obtain your API key from the SendGrid dashboard.

### Using Resend:
- Sign up for a free account on [Resend](https://resend.com/).
- Obtain your API key from the Resend dashboard.

## 2. API Endpoint for Sending Notifications
Create an endpoint in your application to send notifications when items are ready. 
Example (Node.js):
```javascript
const express = require('express');
const sendEmail = require('./sendEmail'); // Function to send email using the selected service

const app = express();
app.use(express.json());

app.post('/api/notify', async (req, res) => {
    const { email, items } = req.body;
    const message = `Your items are ready: ${items.join(', ')}`;
    try {
        await sendEmail(email, message);
        res.status(200).send('Notification sent');
    } catch (err) {
        res.status(500).send('Error sending notification');
    }
});
```

## 3. Database Schema for Tracking Order Status and Email Preferences
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    status VARCHAR(20) NOT NULL,
    email_preference BOOLEAN NOT NULL DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

## 4. Email Template for 'Your Items Are Ready' Message
Create a template for the email notification:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Items Are Ready</title>
</head>
<body>
    <h1>Your Items Are Ready!</h1>
    <p>Your following items are ready for you:</p>
    <ul>
        {{items}}
    </ul>
    <p>Thank you for using Media Restore AI!</p>
</body>
</html>
```

## 5. Order Status Dashboard
Create a dashboard showing processing, ready, and delivered states. Utilize a front-end library (like React) to display order status cards.

## 6. Webhook Integration
Set up a webhook to trigger emails automatically when the order status changes. Using Express.js:
```javascript
app.post('/api/webhook', (req, res) => {
    const { orderId, status } = req.body;
    if (status === 'ready') {
        // Retrieve user email and send notification
    }
    res.sendStatus(200);
});
```

## 7. Setup Instructions for .env.local.example
Create a `.env.local.example` file with the following:
```env
SENDGRID_API_KEY=your_sendgrid_api_key
RESEND_API_KEY=your_resend_api_key
EMAIL_FROM=your_email@example.com
```
