Stripe API Documentation

# Part 1: Overview

The Stripe API is a highly scalable, REST API that enables developers to integrate payment processing capabilities into their applications.

It is designed with a resource-oriented architecture, which allows businesses to securely accept payments, manage financial transactions, and automate billing workflows with minimal friction.

The API supports multiple payment methods, including credit cards, bank transfers, and digital wallets, allowing global commerce.

It has a standardized interface for handling complex financial operations, reducing the overhead of compliance and security concerns while maintaining high performance and reliability.

## Key Features

*   Adheres to REST principles with structured and intuitive endpoints.
*   Processes payments via cards, ACH transfers, Apple Pay, Google Pay, and local payment methods.
*   Automates invoice generation, proration, and customer lifecycle events.
*   API-driven refund processing and chargeback resolution tools.
*   Handles payments in 135+ currencies with automatic currency conversion.
*   PCI DSS Level 1 compliance, tokenization, encryption, and fraud prevention tools.
*   Real-time event notifications for payment statuses, disputes, and customer updates.
*   Official client libraries for Python, Node.js, Ruby, PHP, Java, and .NET for seamless integration.

Stripe’s API is built to support high-performance applications and enterprise-grade payment solutions, making it an ideal choice for developers looking to implement robust financial transaction capabilities at any scale.

## Use Cases

The Stripe API is designed to support a wide range of payment processing scenarios, making it a versatile solution for developers across different industries.

Below are key use cases illustrating how businesses can leverage the API to optimize their financial workflows.

1\. E-Commerce and Online Retail

2\. Subscription and Recurring Billing.

3\. On-Demand Services and Marketplaces

4\. SaaS (Software-as-a-Service) Platforms

5\. Mobile and In-App Payments

6\. International and Multi-Currency Transactions

7\. Custom Payment Workflows

By leveraging the Stripe API, developers can build secure, scalable, and feature-rich payment solutions tailored to their specific business needs.

# Part 2: Getting Started

## Stripe API Base URL

The base URL for Stripe’s API is:

https://api.stripe.com/v1/

This is the foundation for all API requests. Every request to Stripe’s API must be prefixed with this URL.

## How It Works

1.  **RESTful Requests:** Stripe follows RESTful principles, meaning API calls are made via standard HTTP methods such as GET, POST, DELETE, and PUT.
2.  **Versioning:** The /v1/ in the URL indicates the current version of the API. Stripe occasionally updates its API, but older versions remain available to maintain compatibility.
3.  **Endpoints:** Specific API resources (e.g., customers, charges, payments) are appended to the base URL. For example:

### Retrieve a customer:

GET https://api.stripe.com/v1/customers/{CUSTOMER\_ID}

### Create a charge:

POST https://api.stripe.com/v1/charges

1.  **Authentication:** Every request must include your Stripe **API key** in the headers for authentication.

(The -u flag is for authentication, where sk\_test\_4eC39HqLyjWDarjtT1zdp7dc is the secret key.)

## How to Make a Request: Basic Example

To interact with Stripe’s API, you need to send HTTP requests using tools like cURL, Postman, or programming languages such as Python, JavaScript, or PHP.

### 1\. Basic API Request Example (cURL)

To retrieve your Stripe account details, use the following cURL command:

curl https://api.stripe.com/v1/account -u sk\_test\_4eC39HqLyjWDarjtT1zdp7dc:

This command sends a GET request to Stripe’s API with authentication using your secret key.

**Example Response**

If the request is successful, Stripe returns a JSON response:

{

"id": "acct\_1234ABCDE",

"object": "account",

"business\_name": "My Online Store",

"country": "US",

"email": "owner@store.com",

"payouts\_enabled": true

}

### 2\. Making a Request with Python

Using Python’s requests library:

import requests

api\_key = "sk\_test\_4eC39HqLyjWDarjtT1zdp7dc"

url = "https://api.stripe.com/v1/account"

response = requests.get(url, auth=(api\_key, ""))

print(response.json())

### 3\. Making a Request with JavaScript (Node.js)

Using axios:

const axios = require('axios');

const apiKey = 'sk\_test\_4eC39HqLyjWDarjtT1zdp7dc';

const url = 'https://api.stripe.com/v1/account';

axios.get(url, {

auth: { username: apiKey, password: '' }

}).then(response => {

console.log(response.data);

}).catch(error => {

console.error(error.response.data);

});

All API requests must include your Stripe secret key for authentication. The API returns responses in JSON format. You can make requests using cURL, Python, JavaScript, Postman, or any HTTP client.

## Rate Limits: Restrictions on API Calls in Stripe API

Stripe enforces **rate limits** to ensure fair usage and maintain API stability. If you exceed these limits, Stripe will return a **429 Too Many Requests** error.

### 1\. How Stripe Handles Rate Limits

Stripe uses a **dynamic rate limiting system** based on:

*   The **type of request** (read vs. write operations).
*   The **frequency of requests** from a single API key.
*   The **impact of the request** on Stripe’s infrastructure.

### 2\. General Rate Limits

While Stripe does not publish fixed rate limits, general guidelines include:

**Read requests** (GET requests) are allowed at a higher rate.

**Write requests** (POST, DELETE, PUT) have stricter limits.

Burst requests may be temporarily allowed but throttled if sustained.

### 3\. What Happens When You Hit the Rate Limit?

If you exceed the allowed API request rate, Stripe will return a 429 Too Many Requests response:

{

"error": {

"type": "rate\_limit",

"message": "Too many requests made to the API too quickly."

}

}

### 4\. How to Handle Rate Limits

*   **Implement Retry Logic**: Use exponential backoff to retry requests after a short delay.
*   **Monitor API Usage**: Log request counts to detect potential throttling.
*   **Use Webhooks for Real-time Updates**: Instead of polling Stripe’s API, use webhooks to receive automatic updates.

#### Example of Handling Rate Limits in Python

import requests

import time

api\_key = "sk\_test\_4eC39HqLyjWDarjtT1zdp7dc"

url = "https://api.stripe.com/v1/charges"

def make\_request():

for attempt in range(5): # Retry up to 5 times

response = requests.get(url, auth=(api\_key, ""))

if response.status\_code == 429: # Rate limit error

retry\_after = int(response.headers.get("Retry-After", 2)) # Default to 2 seconds

time.sleep(retry\_after)

else:

return response.json()

result = make\_request()

print(result)

# Part 3: Authentication and Authorization

Stripe uses **API keys** to authenticate and authorize API requests. These keys act as credentials that allow applications to interact with Stripe’s services, such as processing payments, managing customers, and handling subscriptions.

## How to Obtain API Keys

You can find your API keys in your **Stripe Dashboard** by logging into Stripe and navigating to **Developers → API Keys** in the menu. Here, you will find different types of keys:

*   **Secret Key (sk\_...)** – Used for server-side API requests.
*   **Publishable Key (pk\_...)** – Used for client-side interactions.
*   **Restricted Keys (rk\_...)** – Optional keys with limited permissions.
*   **Webhook Signing Secret** – Used to verify webhook event authenticity.

Click **Reveal Key** to view your secret key. Never share it or expose it in frontend code.

## How to Use API Keys in API Requests

Every Stripe API request must include a **Secret Key** for authentication.

### Example: Making a Request with cURL

curl https://api.stripe.com/v1/customers -u sk\_test\_4eC39HqLyjWDarjtT1zdp7dc:

### Example: Using API Keys in Python

import requests

api\_key = "sk\_test\_4eC39HqLyjWDarjtT1zdp7dc"

url = "https://api.stripe.com/v1/customers"

response = requests.get(url, auth=(api\_key, ""))

print(response.json())

### Best Practices for Handling API Keys

Keep Secret Keys private and never expose them in frontend code. Store keys securely using environment variables instead of hardcoding them. Rotate keys regularly for security and use **Restricted Keys** to limit permissions.

### Testing vs. Live API Keys

Stripe provides two environments for API keys:

*   **Test Mode Keys (sk\_test\_..., pk\_test\_...)** – Used for development and testing.
*   **Live Mode Keys (sk\_live\_..., pk\_live\_...)** – Used for processing real payments.

To switch from test to live mode, replace test keys with live keys.

## OAuth 2.0 in Stripe API: How to Implement It

Stripe supports **OAuth 2.0** for authorizing third-party applications to access user accounts without exposing their API keys. This is useful when building **Stripe Connect** applications that allow multiple users to connect their Stripe accounts to your platform.

### Does Stripe Use OAuth 2.0 or JWT?

Stripe **does not use JWT (JSON Web Tokens)** for authentication. Instead, it follows the **OAuth 2.0 standard** to allow third-party applications to request access to a user’s Stripe account.

### How OAuth 2.0 Works in Stripe

The user is redirected to Stripe, where they approve the connection. After logging in, the user grants your application permission to access their account. Stripe then redirects the user back to your app with an authorization code. Your app sends this authorization code to Stripe’s API to get an **access token**, which allows you to perform actions on behalf of the user’s Stripe account.

### Step-by-Step Implementation

**1\. Create an OAuth Application in Stripe**

Go to your **Stripe Dashboard**, navigate to **Settings → Connect settings**, and configure your **Redirect URI**, which is where Stripe will send users after authorization.

**2\. Redirect Users to Stripe for Authorization**

To initiate OAuth, send users to the following URL:

[https://connect.stripe.com/oauth/authorize?response\_type=code&client\_id=YOUR\_CLIENT\_ID&scope=read\_write](https://connect.stripe.com/oauth/authorize?response_type=code&client_id=YOUR_CLIENT_ID&scope=read_write)

Example in Python (Flask)

from flask import redirect

client\_id = "ca\_1234567890abcdef"

redirect\_uri = "https://yourapp.com/callback"

def authorize():

auth\_url = f"https://connect.stripe.com/oauth/authorize?response\_type=code&client\_id={client\_id}&scope=read\_write&redirect\_uri={redirect\_uri}"

return redirect(auth\_url)

### 3\. Exchange Authorization Code for an Access Token

Once the user approves, Stripe redirects them to your **redirect URI** with an authorization code. You then exchange it for an access token.

#### Example in cURL

curl -X POST https://connect.stripe.com/oauth/token \\

\-d client\_secret=sk\_test\_4eC39HqLyjWDarjtT1zdp7dc \\

\-d code=AUTHORIZATION\_CODE \\

\-d grant\_type=authorization\_code

#### Example in Python

import requests

client\_secret = "sk\_test\_4eC39HqLyjWDarjtT1zdp7dc"

auth\_code = "AUTHORIZATION\_CODE"

response = requests.post(

"https://connect.stripe.com/oauth/token",

data={

"client\_secret": client\_secret,

"code": auth\_code,

"grant\_type": "authorization\_code"

}

)

print(response.json())

### 4\. Using the Access Token

Once you receive an access token, you can use it to make API calls on behalf of the connected account.

#### Example in cURL

curl https://api.stripe.com/v1/account \\

\-H "Authorization: Bearer ACCESS\_TOKEN"

#### Example in Python

headers = {"Authorization": "Bearer ACCESS\_TOKEN"}

response = requests.get("https://api.stripe.com/v1/account", headers=headers)

print(response.json())

### 5\. Revoking Access Tokens

If a user disconnects their Stripe account from your app, you must revoke their access token.

#### Example Request to Revoke a Token

curl -X POST https://connect.stripe.com/oauth/deauthorize \\

\-d client\_id=YOUR\_CLIENT\_ID \\

\-d client\_secret=YOUR\_SECRET\_KEY \\

\-d stripe\_user\_id=CONNECTED\_ACCOUNT\_ID

# Part 4: Endpoints & Methods

This section outlines the key Stripe API endpoints and the HTTP methods used to interact with them.

## 1\. Base URL

All Stripe API requests use the following base URL:

https://api.stripe.com/v1/

## 2\. Authentication

All requests must include your **Secret API Key** in the header:

Authorization: Bearer YOUR\_SECRET\_KEY

## 3\. Key API Endpoints & Methods

### **1\. Customers**

Manage customer information.

*   **Create a customer**

POST /v1/customers

*   **Retrieve a customer**

GET /v1/customers/{CUSTOMER\_ID}

*   **Update a customer**

POST /v1/customers/{CUSTOMER\_ID}

*   **Delete a customer**

DELETE /v1/customers/{CUSTOMER\_ID}

### **2\. Payments**

Process and manage payments.

*   **Create a payment intent**

POST /v1/payment\_intents

*   **Retrieve a payment intent**

GET /v1/payment\_intents/{PAYMENT\_INTENT\_ID}

*   **Confirm a payment intent**

POST /v1/payment\_intents/{PAYMENT\_INTENT\_ID}/confirm

*   **Cancel a payment intent**

POST /v1/payment\_intents/{PAYMENT\_INTENT\_ID}/cancel

### **3\. Charges**

Legacy method for handling payments (use payment\_intents instead).

*   **Create a charge**

POST /v1/charges

*   **Retrieve a charge**

GET /v1/charges/{CHARGE\_ID}

*   **Refund a charge**

POST /v1/refunds

### **4\. Subscriptions**

Manage recurring billing.

*   **Create a subscription**

POST /v1/subscriptions

*   **Retrieve a subscription**

GET /v1/subscriptions/{SUBSCRIPTION\_ID}

*   **Cancel a subscription**

DELETE /v1/subscriptions/{SUBSCRIPTION\_ID}

## 4\. Response Format

Stripe returns JSON responses for all API calls. Example response for creating a customer:

{

"id": "cus\_123456789",

"object": "customer",

"email": "customer@example.com",

"created": 1625256000

}

This concludes Part 4 of the Stripe API documentation. The next section covers Error Handling.

# Part 5: Error Handling

This section explains how Stripe handles errors and how to interpret API error responses.

## 1\. Standard Error Response Format

When an API request fails, Stripe returns an error response in JSON format:

{

"error": {

"type": "card\_error",

"code": "card\_declined",

"message": "Your card was declined.",

"param": "payment\_method"

}

}

### Fields in the Error Response:

*   **type** – The category of error (e.g., invalid\_request\_error, api\_error, authentication\_error, card\_error).
*   **code** – A specific error code describing the issue (e.g., expired\_card, insufficient\_funds).
*   **message** – A human-readable description of the error.
*   **param** (optional) – The parameter that caused the error, if applicable.

## 2\. Common Error Types

Stripe errors fall into the following categories:

### 1\. Invalid Request Errors **(invalid\_request\_error)**

Occurs when a request has missing or incorrect parameters.

**Example:**

{

"error": {

"type": "invalid\_request\_error",

"message": "Missing required param: currency."

}

}

### 2\. Authentication Errors **(authentication\_error)**

Happens when the API key is missing or incorrect.

**Example:**

{

"error": {

"type": "authentication\_error",

"message": "Invalid API Key provided."

}

}

### 3\. Permission Errors **(permission\_error)**

Occurs when the API key does not have the required permissions.

### 4\. Card Errors **(card\_error)**

Happens due to issues with card payments, such as insufficient funds or a declined card.

**Example:**

{

"error": {

"type": "card\_error",

"code": "insufficient\_funds",

"message": "Your card has insufficient funds."

}

}

### 5\. Rate Limit Errors **(rate\_limit\_error)**

Occurs when too many requests are sent in a short period.

**Example:**

{

"error": {

"type": "rate\_limit\_error",

"message": "Too many requests. Try again later."

}

}

### 6\. API Errors **(api\_error)**

Happens due to internal Stripe issues (e.g., server downtime).

## 3\. Handling Errors in Your Application

### **Best Practices:**

*   **Check HTTP status codes**:
    *   400 – Bad request (e.g., missing parameters)
    *   401 – Unauthorized (e.g., invalid API key)
    *   403 – Forbidden (e.g., permission issues)
    *   404 – Resource not found
    *   429 – Rate limit exceeded
    *   500+ – Server error
*   **Implement retry logic** for 500+ and rate\_limit\_error responses.
*   **Use error codes** to display user-friendly messages.
*   **Log errors** for debugging and monitoring.

## 4\. Reporting Issues

If you encounter unexpected errors, contact Stripe support with:

*   Request URL and method
*   Request payload
*   Response received
*   Timestamp of the error

This concludes Part 5 of the Stripe API documentation. The next section covers Webhooks.

# Part 6: Webhooks

Webhooks allow Stripe to send real-time notifications about events occurring in your account. This section covers how to configure, receive, and handle webhooks securely.

## 1\. What are Webhooks?

Webhooks enable your application to receive updates when specific events happen in Stripe, such as successful payments, failed transactions, or subscription renewals.

## 2\. Setting Up Webhooks

To configure webhooks in Stripe:

1.  Go to your [Stripe Dashboard](https://dashboard.stripe.com/)
2.  Navigate to **Developers > Webhooks**
3.  Click **Add endpoint** and enter your webhook URL (e.g., https://yourdomain.com/webhooks/stripe)
4.  Select events you want to listen for (e.g., payment\_intent.succeeded, invoice.payment\_failed)
5.  Click **Add endpoint**

## 3\. Handling Webhook Events

When Stripe sends an event, your webhook endpoint will receive a POST request with a JSON payload containing event details.

### Example Webhook Event Payload:

{

"id": "evt\_1ABCdEFGHIJKL",

"object": "event",

"type": "payment\_intent.succeeded",

"data": {

"object": {

"id": "pi\_123456789",

"amount": 2000,

"currency": "usd",

"status": "succeeded"

}

}

}

### Sample Code to Handle Webhooks (Node.js)

const express = require("express");

const stripe = require("stripe")(process.env.STRIPE\_SECRET\_KEY);

const app = express();

app.post("/webhooks/stripe", express.raw({ type: "application/json" }), (req, res) => {

const sig = req.headers\["stripe-signature"\];

const endpointSecret = process.env.STRIPE\_WEBHOOK\_SECRET;

let event;

try {

event = stripe.webhooks.constructEvent(req.body, sig, endpointSecret);

} catch (err) {

console.error("Webhook signature verification failed.", err.message);

return res.status(400).send(\`Webhook Error: ${err.message}\`);

}

if (event.type === "payment\_intent.succeeded") {

console.log("Payment successful!", event.data.object);

}

res.json({ received: true });

});

## 4\. Securing Webhooks

To ensure webhook security:

*   **Verify webhook signatures** using the stripe-signature header and your webhook secret.
*   **Respond with 2xx status codes** to acknowledge receipt; otherwise, Stripe retries the event.
*   **Use HTTPS** for secure communication.
*   **Limit event types** to only those your application needs.

## 5\. Retrying & Debugging

If your endpoint does not return a 2xx response, Stripe retries delivery using exponential backoff for up to **3 days**.

*   View logs in the **Stripe Dashboard > Developers > Webhooks**
*   Use the Stripe CLI for local testing:

stripe listen --forward-to localhost:3000/webhooks/stripe

This concludes Part 6 of the Stripe API documentation. The next section covers additional best practices and recommendations.

# Part 7: SDKs & Libraries

Stripe provides a variety of SDKs and libraries that facilitate seamless integration with its API across different programming languages and platforms.

These libraries simplify API interactions by handling authentication, request validation, and response parsing, allowing developers to focus on building their applications.

## Official Stripe SDKs

Stripe offers officially maintained SDKs for various programming languages, ensuring compatibility and ease of use.

### 1\. **Stripe Node.js SDK**

#### Install via npm:

npm install stripe

#### Example Usage:

const stripe = require('stripe')('your\_secret\_key');

stripe.customers.create({

email: 'customer@example.com',

}).then(customer => console.log(customer));

### 2\. **Stripe Python SDK**

#### Install via pip:

pip install stripe

#### Example Usage:

import stripe

stripe.api\_key = "your\_secret\_key"

customer = stripe.Customer.create(email="customer@example.com")

print(customer)

### 3\. **Stripe PHP SDK**

#### Install via Composer:

composer require stripe/stripe-php

#### Example Usage:

require 'vendor/autoload.php';

\\Stripe\\Stripe::setApiKey('your\_secret\_key');

$customer = \\Stripe\\Customer::create(\[

'email' => 'customer@example.com'

\]);

print\_r($customer);

### 4\. **Stripe Ruby SDK**

#### Install via gem:

gem install stripe

#### Example Usage:

require 'stripe'

Stripe.api\_key = 'your\_secret\_key'

customer = Stripe::Customer.create(email: 'customer@example.com')

puts customer

### 5\. **Stripe Java SDK**

#### Install via Maven:

<dependency>

<groupId>com.stripe</groupId>

<artifactId>stripe-java</artifactId>

<version>latest\_version</version>

</dependency>

#### Example Usage:

import com.stripe.Stripe;

import com.stripe.model.Customer;

import java.util.HashMap;

import java.util.Map;

public class StripeExample {

public static void main(String\[\] args) {

Stripe.apiKey = "your\_secret\_key";

Map<String, Object> params = new HashMap<>();

params.put("email", "customer@example.com");

Customer customer = Customer.create(params);

System.out.println(customer);

}

}

## Third-Party Libraries

While Stripe maintains official SDKs, there are also third-party libraries available for additional frameworks and languages. These may include integrations for:

*   **Go** (stripe-go)
*   **C#/.NET** (Stripe.net)
*   **Dart/Flutter** (stripe-dart)
*   **Mobile SDKs** (Stripe iOS and Android SDKs)

## Mobile SDKs

Stripe provides specialized SDKs for mobile app development, enabling seamless payment experiences on iOS and Android platforms.

### 1\. **Stripe iOS SDK**

#### Install via CocoaPods:

pod 'Stripe'

#### Example Usage:

import Stripe

### 2\. **Stripe Android SDK**

#### Install via Gradle:

dependencies {

implementation 'com.stripe:stripe-android:latest\_version'

}

#### Example Usage:

import com.stripe.android.PaymentConfiguration;

## Choosing the Right SDK

Selecting the appropriate SDK depends on your application’s technology stack. The official Stripe libraries ensure security, maintainability, and access to the latest API features..

# Part 8: FAQs & Troubleshooting

## Frequently Asked Questions (FAQs)

### 1\. **How do I get my Stripe API keys?**

*   You can find your API keys in the Stripe Dashboard under **Developers > API keys**. Use test keys for development and live keys for production.

### 2\. **Why am I getting an 'Invalid API Key' error?**

*   Ensure you are using the correct key (test vs. live)
*   Double-check for typos or missing characters
*   Ensure the key has not been revoked

### 3\. **How do I handle failed payments?**

*   Use Stripe’s built-in **webhooks** to listen for failed payments
*   Implement retry logic for declined transactions
*   Provide clear error messages to users

### 4\. **Can I use Stripe in my country?**

*   Check the list of supported countries on the Stripe website: [https://stripe.com/global](https://stripe.com/global)

### 5\. **What is the difference between one-time payments and subscriptions?**

*   One-time payments charge a customer once
*   Subscriptions allow for recurring billing (e.g., monthly, annually)

## Troubleshooting Common Issues

### 1\. **Payment Declined Errors**

*   Common reasons:
    *   Insufficient funds
    *   Expired card
    *   Bank restrictions
*   Solutions:
    *   Ask the user to try another card
    *   Contact the issuing bank for more details

### 2\. **Webhook Not Working**

*   Ensure the endpoint is correctly configured in the Stripe Dashboard
*   Verify that your server is accepting and processing requests properly
*   Use the **Stripe CLI** to test webhooks locally

### 3\. **3D Secure Authentication Issues**

*   Ensure the payment method supports 3D Secure
*   Implement Stripe’s **Payment Intents API** for handling authentication

### 4\. **Errors in SDK Implementation**

*   Check Stripe’s official SDK documentation for proper setup
*   Update to the latest version of the SDK
*   Verify API key and environment settings

### 5\. **Refund Not Processing**

*   Ensure the charge exists and is eligible for a refund
*   Check if the refund is being issued within Stripe’s processing window
*   Monitor the status in the Stripe Dashboard

# Part 9: Changelog & Versioning

Stripe continuously updates its API to improve functionality, security, and performance. This section provides guidance on understanding Stripe’s versioning system and keeping track of API changes.

## API Versioning

Stripe follows a date-based versioning system (YYYY-MM-DD). When Stripe releases a new version, it does not automatically update existing integrations. You must explicitly upgrade your API version in the Stripe Dashboard.

### 1\. **Checking Your Current API Version**

*   Go to **Stripe Dashboard > Developers > API Keys**
*   Your API version is displayed at the top

### 2\. **Upgrading to a New API Version**

*   Review the Stripe changelog before upgrading
*   Test the new version in a development environment
*   Update the API version in the Stripe Dashboard
*   Ensure all endpoints and parameters are compatible with the new version

### 3\. **Backward Compatibility**

*   Older API versions continue to function but may lack new features
*   Breaking changes are documented in the changelog
*   Webhooks should be tested to ensure continued functionality

## Changelog

Stripe maintains a public changelog documenting all updates. Changes typically fall into these categories:

*   **New Features**: Introduction of new endpoints, parameters, or enhancements
*   **Bug Fixes**: Resolution of known issues affecting functionality
*   **Breaking Changes**: Modifications that require updates to existing integrations

### Example Changelog Entries

*   **2024-02-15**: Added support for real-time payment confirmations
*   **2023-11-10**: Deprecated customer.sources in favor of paymentMethods API
*   **2023-07-22**: Improved error messages for declined payments

### Staying Updated

To ensure your integration remains up to date:

*   Subscribe to Stripe’s [developer updates](https://stripe.com/docs/upgrades)
*   Regularly check the [Stripe changelog](https://stripe.com/docs/changelog)
*   Monitor API deprecation notices in the dashboard

By following Stripe’s versioning best practices, you can maintain a stable and secure integration while leveraging new features as they become available.

# Part 10: Contact & Support

Stripe provides multiple support channels to assist developers and businesses with integration issues, billing inquiries, and troubleshooting.

### 1\. **Official Stripe Support Channels**

*   **Help Center**: Comprehensive documentation and FAQs - [https://support.stripe.com](https://support.stripe.com)
*   **Developer Docs**: In-depth API references and guides - [https://stripe.com/docs](https://stripe.com/docs)
*   **Community Forum**: Engage with other developers and Stripe engineers - [https://community.stripe.com](https://community.stripe.com)

### 2\. **Contacting Stripe Support**

*   **Live Chat**: Available via the Stripe Dashboard for logged-in users
*   **Email Support**: Submit queries through [https://support.stripe.com/contact](https://support.stripe.com/contact)
*   **Phone Support**: Available for eligible customers; access through the Stripe Dashboard

### 3\. **Developer Support & Technical Assistance**

*   **GitHub Issues**: Report bugs or suggest improvements in Stripe SDKs - [https://github.com/stripe](https://github.com/stripe)
*   **Stack Overflow**: Ask technical questions using the stripe-api tag - [https://stackoverflow.com/questions/tagged/stripe-api](https://stackoverflow.com/questions/tagged/stripe-api)

### 4\. **Emergency Support & Incident Updates**

*   **Status Page**: Monitor API uptime and incidents - [https://status.stripe.com](https://status.stripe.com)
*   **Twitter Updates**: Follow @StripeStatus for real-time announcements - [https://twitter.com/stripestatus](https://twitter.com/stripestatus)

For urgent issues impacting payments or customer experience, reach out via live chat or phone support through your Stripe Dashboard.