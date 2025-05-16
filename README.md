# How to Get ChatGPT API and Use It in Your Custom Web App

---

## Step 1: Create an OpenAI Account

1. Visit [https://platform.openai.com/signup](https://platform.openai.com/signup)
2. Sign up with your email, Google, or Microsoft account
3. Verify your email and phone number

---

## Step 2: Generate Your API Key

1. Log in to [https://platform.openai.com/](https://platform.openai.com/)
2. Go to the API Keys page: [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys)
3. Click “Create new secret key”
4. Copy and store it safely (you won’t be able to view it again)

---

## Step 3: Set Up Billing

1. Visit [https://platform.openai.com/account/billing](https://platform.openai.com/account/billing)
2. Add a payment method
3. Monitor your usage at [https://platform.openai.com/account/usage](https://platform.openai.com/account/usage)

---

## Step 4: Choose the Right Model

Available models include:

* `gpt-3.5-turbo` – affordable and fast
* `gpt-4` – more accurate, higher cost
* `gpt-4o` – newer, faster, better at handling text, vision, and speech

Refer to [OpenAI pricing](https://openai.com/pricing) to decide based on cost and performance.

---

## Step 5: Build a Simple Web App (Client-Side Only Example)

### Project Structure:

```
my-chat-app/
├── index.html
├── script.js
```

### index.html

```html
<!DOCTYPE html>
<html>
<head>
  <title>Chat with GPT</title>
</head>
<body>
  <h1>ChatGPT Web App</h1>
  <textarea id="prompt" rows="4" cols="50"></textarea><br>
  <button onclick="sendMessage()">Send</button>
  <pre id="response"></pre>

  <script src="script.js"></script>
</body>
</html>
```

### script.js

```javascript
async function sendMessage() {
  const prompt = document.getElementById('prompt').value;
  const responseBox = document.getElementById('response');

  const result = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer YOUR_API_KEY_HERE'
    },
    body: JSON.stringify({
      model: 'gpt-3.5-turbo',
      messages: [{ role: 'user', content: prompt }]
    })
  });

  const data = await result.json();
  const reply = data.choices[0].message.content;
  responseBox.textContent = reply;
}
```

Note: Do **not** expose your API key on the client side. The above is only for testing or learning purposes.

---

## Step 6: Secure Your API Key (Use a Backend)

To protect your API key, handle requests on a server instead of in the browser.

### Example Using Node.js with Express

Install dependencies:

```bash
npm init -y
npm install express dotenv node-fetch
```

### server.js

```javascript
const express = require('express');
const fetch = require('node-fetch');
require('dotenv').config();

const app = express();
app.use(express.json());

app.post('/chat', async (req, res) => {
  const { message } = req.body;

  const response = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${process.env.OPENAI_API_KEY}`
    },
    body: JSON.stringify({
      model: 'gpt-3.5-turbo',
      messages: [{ role: 'user', content: message }]
    })
  });

  const data = await response.json();
  res.json({ reply: data.choices[0].message.content });
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

Create a `.env` file:

```
OPENAI_API_KEY=your-secret-key-here
```

Now your frontend can call your own backend (at `/chat`), which keeps the API key safe.

---

## Step 7: Test and Deploy

* Test locally on your machine
* Deploy frontend on platforms like Vercel or Netlify
* Deploy backend on platforms like Render, Railway, or Fly.io

---

## Step 8: Monitor and Optimize

* Watch usage at [OpenAI Dashboard](https://platform.openai.com/account/usage)
* Use parameters like `max_tokens`, `temperature`, and `top_p` for better control
* Cache results or re-use answers where possible to reduce cost
