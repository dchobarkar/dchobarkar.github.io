# Conversational AI: Building Intelligent Chatbots Across Platforms - 06: Instagram DM Chatbot for Lead Capture

## Introduction: Why Instagram DMs are a Goldmine for Lead Capture

Instagram is no longer just a photo-sharing app; it's a full-fledged engagement platform where brands, creators, and customers interact in real time. With over 2 billion monthly active users and strong adoption among Gen Z and millennials, Instagram has quietly become a business-critical messaging channel. More importantly, **Instagram Direct Messages (DMs)** have emerged as a prime touchpoint for lead capture.

### 🏛️ The Case for Lead Generation on Instagram DMs

Traditional lead forms on landing pages are becoming stale. Users prefer conversational, instant interactions—especially on platforms where they already spend time. Instagram DMs allow businesses to:

- Respond instantly to product inquiries
- Collect user details in a conversational format
- Qualify leads through back-and-forth Q\&A
- Trigger automation workflows based on user responses

This makes DMs a great fit for:

- Influencers promoting paid offers
- E-commerce brands capturing product interest
- Service businesses booking appointments
- Coaches and consultants qualifying prospects

### ✨ What Makes Instagram Bots Unique?

Unlike a website or even WhatsApp chatbot, an Instagram DM bot has:

- **Social Context:** Users often message after seeing a post, reel, or story.
- **User Handles:** Instagram usernames are attached to every DM.
- **Visual-first Messaging:** Emojis, media, and quick replies make the UX rich.
- **API Nuances:** Meta tightly controls access; bots must follow exact webhook flows and permission rules.

### 🚀 The Engagement Opportunity

The average Instagram user is far more likely to open and respond to a DM than an email or push notification. In fact:

> 🔹 _Instagram messages have an 80-90% open rate and 30-40% reply rate — significantly higher than traditional email._

This makes Instagram bots especially effective for top-of-funnel engagement and initial lead qualification. Once you capture the lead's name, email, and interest, you can pipe that into your CRM or sales system and follow up outside Instagram.

### 🤔 Why Automate Instead of Replying Manually?

If you're a small business or creator, replying manually is manageable — until it isn't. As DMs scale, manual responses:

- Become inconsistent and error-prone
- Delay response times (missed leads!)
- Can’t scale beyond a single person

A bot ensures:

- **24/7 availability**
- **Structured data collection** (name, email, interest)
- **Lead routing** to your CRM or Google Sheets
- **Personalized replies** at scale

### 🚧 What We'll Build

In this hands-on guide, we'll build an Instagram DM bot that:

- Replies to incoming messages automatically
- Walks users through a mini lead capture funnel
- Validates inputs (e.g., checks if email is valid)
- Stores leads in a Supabase table

By the end, you'll have:

- A Node.js + Express server running your DM bot
- Webhook integration with Meta's Instagram API
- A working conversation flow that captures leads

Let's dive in — starting with how Meta's API works under the hood ✨

## Meta’s Messaging API: Understanding the Tech Behind Instagram DM Bots

Before we write a single line of code, it's crucial to understand how Meta's ecosystem works when it comes to enabling Instagram messaging for bots. Unlike most messaging platforms, Meta enforces strict permissions, account configurations, and API contracts. Let's break it all down 🤷‍♂️

### 📂 Account Requirements: What You Need to Start

To interact with Instagram DMs via the API, Meta requires a few things to be properly set up:

#### 1. Facebook Developer Account

- Go to [developers.facebook.com](https://developers.facebook.com/) and create a developer account.

#### 2. Facebook App

- Create a new app of type **Business**. This gives access to Instagram Graph and Messaging APIs.

#### 3. Instagram Business Account

- Your Instagram account must be a **Professional (Business)** account.
- It must be connected to a Facebook Page.

#### 4. Facebook Page

- The connected Page acts as the container for the Instagram account.

#### 5. App Permissions

- Request and enable:

  - `pages_show_list`
  - `instagram_basic`
  - `instagram_manage_messages`
  - `pages_messaging`

#### 6. Webhook Configuration

- Set up a webhook to receive message events.
- Subscribe to the `messages` and `messaging_postbacks` fields for both Page and Instagram.

### 📊 API Flow: How the Instagram Messaging Lifecycle Works

Understanding the message lifecycle helps you design proper flows:

1. **User Sends Message**

   - A user DMs your business on Instagram.

2. **Meta Webhook Triggers**

   - Your webhook receives an HTTP POST event.

3. **Server Processes It**

   - Your code parses the message and checks if it’s a new lead.

4. **Bot Sends a Reply**

   - You call the Instagram Messaging API to reply using the Page access token.

### 🔑 Access Tokens and Authentication

You’ll use a **Page Access Token** to authenticate API calls. During development:

- Generate a **User Access Token** with `pages_show_list` permission
- Use Graph API Explorer to request a **long-lived Page Token**

Keep these tokens in `.env` files:

```env
PAGE_ACCESS_TOKEN=your_page_access_token_here
VERIFY_TOKEN=your_custom_verify_token
```

Use the Page Token when calling the Messaging API:

```bash
curl -X POST "https://graph.facebook.com/v19.0/me/messages" \
  -H "Content-Type: application/json" \
  -d '{
    "recipient": {"id": "<PSID>"},
    "message": {"text": "Hello from your bot!"},
    "messaging_type": "RESPONSE",
    "access_token": "<PAGE_ACCESS_TOKEN>"
  }'
```

### 🛠️ Webhook Event Format (Instagram Message)

Here’s what a typical webhook payload looks like when someone DMs your Instagram:

```json
{
  "object": "instagram",
  "entry": [
    {
      "id": "<page-id>",
      "time": 1698888888,
      "messaging": [
        {
          "sender": { "id": "<user-psid>" },
          "recipient": { "id": "<page-id>" },
          "timestamp": 1698888888,
          "message": {
            "mid": "<message-id>",
            "text": "Hi, I’m interested!"
          }
        }
      ]
    }
  ]
}
```

Your server needs to:

- Verify the webhook signature
- Parse this payload
- Decide how to respond

### 📖 API Docs to Bookmark

Keep these Meta docs handy as we build:

- [Instagram Messaging API Overview](https://developers.facebook.com/docs/instagram-api/guides/messaging/)
- [Webhook Reference](https://developers.facebook.com/docs/graph-api/webhooks/)
- [Graph API Explorer](https://developers.facebook.com/tools/explorer/)

Now that we understand how Meta's Instagram Messaging API works, we're ready to start building our backend!

Next up: **setting up the Node.js + Express bot server** ✨

## Project Setup: Building the Bot Server with Node.js + Express

With Meta's platform basics in place, it’s time to get our hands dirty and start building the actual Instagram DM bot backend ✨

We’ll use **Node.js + Express** as the foundation for our server. This will handle:

- Verifying Meta’s webhook
- Receiving and responding to messages
- Managing lead state and flow

### 🔄 Step 1: Initialize the Project

Create a new project folder:

```bash
mkdir insta-lead-bot && cd insta-lead-bot
npm init -y
```

### 📁 Step 2: Install Dependencies

We’ll need the following packages:

```bash
npm install express axios dotenv body-parser
```

**Breakdown:**

- `express`: Core web framework
- `axios`: For outbound API calls to Meta
- `dotenv`: Loads env vars from `.env`
- `body-parser`: Parses incoming webhook JSON

### 🔧 Step 3: Project Structure

Let’s organize our files for maintainability:

```structure
insta-lead-bot/
├── controllers/
│   └── messageController.js
├── routes/
│   └── webhook.js
├── services/
│   └── instagramService.js
├── .env
├── app.js
└── server.js
```

This will help separate logic by responsibility.

### 👁‍🗨️ Step 4: Create the Entry Point

`server.js` will launch the app:

```js
// server.js
const app = require("./app");
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Bot server running on port ${PORT}`);
});
```

### ⚖️ Step 5: Setup Express in `app.js`

```js
// app.js
const express = require("express");
const bodyParser = require("body-parser");
const dotenv = require("dotenv");
const webhookRoutes = require("./routes/webhook");

dotenv.config();

const app = express();
app.use(bodyParser.json());
app.use("/webhook", webhookRoutes);

module.exports = app;
```

### 🔒 Step 6: Create `.env` File

```env
PAGE_ACCESS_TOKEN=your_meta_page_token
VERIFY_TOKEN=your_custom_verify_token
```

This keeps your secrets out of version control.

### 🔍 Step 7: Webhook Route

Create `routes/webhook.js`:

```js
const express = require("express");
const router = express.Router();
const messageController = require("../controllers/messageController");

router.get("/", messageController.verifyWebhook);
router.post("/", messageController.handleMessage);

module.exports = router;
```

### 🤖 Step 8: Webhook Controller Stub

Create `controllers/messageController.js`:

```js
exports.verifyWebhook = (req, res) => {
  const VERIFY_TOKEN = process.env.VERIFY_TOKEN;
  const mode = req.query["hub.mode"];
  const token = req.query["hub.verify_token"];
  const challenge = req.query["hub.challenge"];

  if (mode && token === VERIFY_TOKEN) {
    console.log("Webhook verified");
    return res.status(200).send(challenge);
  } else {
    return res.sendStatus(403);
  }
};

exports.handleMessage = (req, res) => {
  console.log("Incoming webhook:", JSON.stringify(req.body, null, 2));
  res.sendStatus(200); // Acknowledge receipt to Meta
};
```

This code handles webhook verification and logs incoming payloads.

With this setup, your server is ready to receive Instagram DMs and respond to Meta’s webhook challenge.

Next up: **Handling Webhook Verification + Message Reception** ✅

## Webhook Verification + Receiving Instagram Messages

Now that our server is running and the routes are in place, the next step is to handle **Meta's webhook verification process** and start parsing real-time Instagram messages ✅

### 📃 Step 1: Understanding Meta's Webhook Verification

When you register your webhook URL in the Meta Developer Console, Meta sends a `GET` request with three query parameters:

- `hub.mode`
- `hub.verify_token`
- `hub.challenge`

You need to:

- Check if the `verify_token` matches what you set
- Respond with the `challenge` string if it does

This verifies that you control the endpoint — a required step before Meta starts sending real message events.

We already wrote the verification logic in our controller:

```js
exports.verifyWebhook = (req, res) => {
  const VERIFY_TOKEN = process.env.VERIFY_TOKEN;
  const mode = req.query["hub.mode"];
  const token = req.query["hub.verify_token"];
  const challenge = req.query["hub.challenge"];

  if (mode && token === VERIFY_TOKEN) {
    console.log("Webhook verified");
    return res.status(200).send(challenge);
  } else {
    return res.sendStatus(403);
  }
};
```

Test this with:

```bash
curl -X GET 'http://localhost:3000/webhook?hub.mode=subscribe&hub.verify_token=your_custom_verify_token&hub.challenge=1234'
```

### 📢 Step 2: Registering the Webhook with Meta

In the [Meta Developer Console](https://developers.facebook.com/apps/):

1. Go to your app > **Webhooks** section
2. Choose **Instagram** or **Page** (based on setup)
3. Enter your webhook URL, e.g. `https://yourdomain.com/webhook`
4. Enter your `VERIFY_TOKEN`
5. Subscribe to `messages`, `messaging_postbacks`

You’ll see a success message if verification worked!

### 📈 Step 3: Receiving Instagram DMs

When a user sends a DM to your Instagram account, Meta sends a `POST` request to your webhook endpoint:

Here's how we already log that:

```js
exports.handleMessage = (req, res) => {
  console.log("Incoming webhook:", JSON.stringify(req.body, null, 2));
  res.sendStatus(200);
};
```

Example webhook payload:

```json
{
  "object": "instagram",
  "entry": [
    {
      "id": "<page-id>",
      "time": 1698888888,
      "messaging": [
        {
          "sender": { "id": "<user-id>" },
          "recipient": { "id": "<page-id>" },
          "timestamp": 1698888888,
          "message": {
            "mid": "<msg-id>",
            "text": "Hi! I'm interested."
          }
        }
      ]
    }
  ]
}
```

You can extract the relevant info like this:

```js
exports.handleMessage = (req, res) => {
  const body = req.body;

  if (body.object === "instagram") {
    body.entry.forEach((entry) => {
      const event = entry.messaging[0];
      const senderId = event.sender.id;
      const message = event.message.text;

      console.log(`Message from ${senderId}: ${message}`);
      // Later: Call your response function here
    });
  }
  res.sendStatus(200);
};
```

### ✉️ Testing Message Events

Use a real Instagram account (with test user access) to DM the connected business profile.

You can also simulate via `curl` or Postman (for raw webhook testing).

At this point:

- ✅ Your webhook URL is verified
- 🔎 You can inspect incoming DMs

Next up: **Sending Replies to Instagram Messages using Meta’s API** 🚀

## Sending Messages Back: The DM Reply Handler

Receiving DMs is only half the job. To complete the loop, your bot needs to **reply to messages using the Instagram Messaging API.** Let's now implement the response logic so our bot can interact with users in real time 🚀

### 🔄 The Send Message Flow

When a DM arrives:

1. Your webhook receives and parses the message
2. Your server extracts the `sender.id`
3. You make a `POST` request to Meta's Graph API
4. Meta delivers your message to the user

### 📝 Meta Messaging API Endpoint

```url
POST https://graph.facebook.com/v19.0/me/messages?access_token=<PAGE_ACCESS_TOKEN>
```

This API requires:

- `recipient.id` (sender PSID from webhook)
- `message.text` (or structured message payload)

### 🛠️ Create Instagram Service

In `services/instagramService.js`, add this:

```js
const axios = require("axios");

const PAGE_ACCESS_TOKEN = process.env.PAGE_ACCESS_TOKEN;

async function sendTextMessage(recipientId, text) {
  const url = `https://graph.facebook.com/v19.0/me/messages?access_token=${PAGE_ACCESS_TOKEN}`;

  const payload = {
    recipient: { id: recipientId },
    message: { text: text },
    messaging_type: "RESPONSE",
  };

  try {
    const res = await axios.post(url, payload);
    console.log("Message sent:", res.data);
  } catch (err) {
    console.error("Error sending message:", err.response?.data || err.message);
  }
}

module.exports = {
  sendTextMessage,
};
```

This is your core DM response function.

### 🤖 Use It in the Controller

Update `controllers/messageController.js` to use the reply logic:

```js
const { sendTextMessage } = require("../services/instagramService");

exports.handleMessage = async (req, res) => {
  const body = req.body;

  if (body.object === "instagram") {
    for (const entry of body.entry) {
      const event = entry.messaging[0];
      const senderId = event.sender.id;
      const message = event.message?.text;

      if (message) {
        console.log(`User says: ${message}`);
        await sendTextMessage(
          senderId,
          "Hey! Thanks for messaging us. How can I help you today? 😊"
        );
      }
    }
  }
  res.sendStatus(200);
};
```

This sends a simple text reply for any incoming message.

### 📞 Optional: Typing Indicators + Seen Status

Instagram also allows:

- `sender_action: typing_on`
- `sender_action: mark_seen`

Add these to show user feedback:

```js
async function markSeen(recipientId) {
  const url = `https://graph.facebook.com/v19.0/me/messages?access_token=${PAGE_ACCESS_TOKEN}`;

  await axios.post(url, {
    recipient: { id: recipientId },
    sender_action: "mark_seen",
  });
}

async function showTyping(recipientId) {
  const url = `https://graph.facebook.com/v19.0/me/messages?access_token=${PAGE_ACCESS_TOKEN}`;

  await axios.post(url, {
    recipient: { id: recipientId },
    sender_action: "typing_on",
  });
}

module.exports = {
  sendTextMessage,
  markSeen,
  showTyping,
};
```

Then call these before sending a reply for a more human-like UX.

### 🚧 Debugging Tips

- Check Meta logs for failed API requests
- Make sure `PAGE_ACCESS_TOKEN` is current and has permissions
- Log full error responses from Axios

At this point, your bot can receive and **reply to Instagram DMs** automatically ✨

Next: Let’s build a **guided lead capture flow** that collects the user's name, email, and interest 📊

## Building the Lead Capture Flow: Name, Email, and Interest

With message handling and replies in place, it’s time to build the heart of our bot: a **structured, conversational lead capture flow**. Our goal is to collect three pieces of information:

- User's **Name**
- **Email Address** (validated)
- Area of **Interest** (optional free text)

We’ll maintain conversational context using a simple session-based memory and validate inputs step-by-step ✏️

### 🧠 State Management: In-Memory Session Store

To manage conversation state, we’ll track user progress in a simple object (or Redis in production).

Create `sessionStore.js`:

```js
const sessions = {};

function getSession(userId) {
  if (!sessions[userId]) {
    sessions[userId] = { step: 0, data: {} };
  }
  return sessions[userId];
}

function clearSession(userId) {
  delete sessions[userId];
}

module.exports = { getSession, clearSession };
```

This will store:

- `step`: Current conversation step
- `data`: Collected name/email/interest

### 🌐 Update Controller Logic

Edit `controllers/messageController.js` to support flow logic:

```js
const { sendTextMessage } = require("../services/instagramService");
const { getSession, clearSession } = require("../sessionStore");
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

exports.handleMessage = async (req, res) => {
  const body = req.body;

  if (body.object === "instagram") {
    for (const entry of body.entry) {
      const event = entry.messaging[0];
      const senderId = event.sender.id;
      const message = event.message?.text;

      if (message) {
        const session = getSession(senderId);

        switch (session.step) {
          case 0:
            await sendTextMessage(senderId, "Hey! 👋 What's your name?");
            session.step = 1;
            break;

          case 1:
            session.data.name = message;
            await sendTextMessage(
              senderId,
              `Nice to meet you, ${message}! What's your email? ✉️`
            );
            session.step = 2;
            break;

          case 2:
            if (!emailRegex.test(message)) {
              await sendTextMessage(
                senderId,
                "Hmm, that doesn't look like a valid email. Try again ✍️"
              );
            } else {
              session.data.email = message;
              await sendTextMessage(
                senderId,
                "Got it! Lastly, what are you interested in? 💡"
              );
              session.step = 3;
            }
            break;

          case 3:
            session.data.interest = message;
            await sendTextMessage(
              senderId,
              "Awesome! Thanks for sharing. We'll be in touch soon. 🚀"
            );
            console.log("Captured lead:", session.data);
            clearSession(senderId);
            break;
        }
      }
    }
  }
  res.sendStatus(200);
};
```

### 🔎 What This Flow Does

- Initiates only when a user sends a message
- Asks for each field sequentially
- Validates email using regex
- Logs full lead data once captured

You can extend this to:

- Send data to Supabase (next section)
- Add buttons or quick replies for predefined interest topics
- Handle restarts and `/reset` commands

### ℹ️ UX Notes

Keep your messages friendly and conversational. Use emojis and confirmations to show progress.

E.g., instead of:

> "Email?"

Say:

> "Cool! Now I just need your email so we can reach you later. ✉️"

Next up: We’ll **store the collected lead data in Supabase** so it doesn’t vanish when the server restarts 📂

## Storing Leads: Connecting to Supabase (or Firebase)

Capturing user data in-memory is fine for local testing, but in production we need to **persist lead data** to a proper database. For this, we’ll use **Supabase** — a serverless PostgreSQL platform with a slick API and dashboard that integrates perfectly with JavaScript.

### 📂 Why Supabase?

- Free and generous tier for small projects
- RESTful + real-time + SQL access
- Easy setup and JavaScript SDK
- Auth, storage, and other tools if needed later

### 📦 Step 1: Create a Supabase Project

1. Go to [https://supabase.com](https://supabase.com)
2. Sign in with GitHub and create a new project
3. Name it `instagram-lead-bot`
4. Set a strong password and choose a region

Once provisioned:

- Copy the **project URL** and **anon API key** from Project Settings > API

### 🔍 Step 2: Define the Leads Table

In the **Table Editor**, create a new table called `leads`:

| Column     | Type        | Notes                      |
| ---------- | ----------- | -------------------------- |
| id         | uuid        | Primary key (default UUID) |
| name       | text        | Name from the DM flow      |
| email      | text        | Validated email address    |
| interest   | text        | Freeform user input        |
| source     | text        | Set to 'instagram'         |
| created_at | timestamptz | Default: now()             |

Enable **Row Level Security**, then add a policy:

```sql
CREATE POLICY "Allow all insert" ON public.leads
FOR INSERT WITH CHECK (true);
```

### 🎓 Step 3: Install Supabase JS SDK

```bash
npm install @supabase/supabase-js
```

### 🔧 Step 4: Create Supabase Service

Create `services/supabaseService.js`:

```js
const { createClient } = require("@supabase/supabase-js");

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
);

async function storeLead({ name, email, interest }) {
  const { data, error } = await supabase
    .from("leads")
    .insert([{ name, email, interest, source: "instagram" }]);

  if (error) {
    console.error("Error inserting lead:", error);
  } else {
    console.log("Lead stored successfully:", data);
  }
}

module.exports = { storeLead };
```

### 🔐 Step 5: Add to `.env`

```env
SUPABASE_URL=https://xyzcompany.supabase.co
SUPABASE_ANON_KEY=your_anon_key_here
```

### 📝 Step 6: Plug Into Flow

Update `messageController.js` to import and call `storeLead`:

```js
const { storeLead } = require('../services/supabaseService');

...
case 3:
  session.data.interest = message;
  await sendTextMessage(senderId, "Awesome! Thanks for sharing. We'll be in touch soon. 🚀");
  await storeLead(session.data);
  clearSession(senderId);
  break;
```

### 🧰 Production Tips

- Sanitize input if handling raw text
- Use Supabase Auth for restricted access later
- Consider rate limiting to avoid spam/flooding

Now your bot persists leads to a real-time PostgreSQL database via Supabase.

Next: let’s **deploy the bot server and connect it with Meta’s live webhook config** ✨

## Deploying the Bot: Secure Hosting + Webhook Setup

Once our bot is functional locally, it’s time to take it live — so Meta can send real webhook events and users can DM our Instagram account to trigger the flow.

We’ll cover:

- Hosting the Node.js server
- Securing environment variables
- Exposing a public URL
- Configuring the webhook in Meta Developer Console

### 🏢 Step 1: Choose a Hosting Provider

Some great options for quick Node.js deployments:

#### ✅ Railway (Recommended)

- Free tier, instant deploy from GitHub
- Built-in environment variable management
- Good logs and CI/CD support

Other options:

- **Render** (also solid)
- **Vercel + Serverless Functions** (for smaller projects)

### ✈️ Step 2: Deploy to Railway

1. Go to [https://railway.app](https://railway.app)
2. Click **"Start a New Project" > Deploy from GitHub Repo**
3. Connect your GitHub and pick the `insta-lead-bot` repo

Set environment variables:

```env
PAGE_ACCESS_TOKEN=your_meta_token
VERIFY_TOKEN=your_custom_token
SUPABASE_URL=...
SUPABASE_ANON_KEY=...
```

Railway will auto-detect Node.js and start your server 🚀

### 📲 Step 3: Local Testing via Ngrok (Optional but Useful)

To test locally before deployment:

```bash
npm install -g ngrok
ngrok http 3000
```

Ngrok will output a public HTTPS URL like:

```link
https://a1b2c3.ngrok.io
```

Use this in Meta console temporarily for webhook testing.

### 🚧 Step 4: Configure Webhook in Meta Console

Go to [https://developers.facebook.com](https://developers.facebook.com):

1. Go to your App > **Webhooks** section
2. Choose **Instagram** (or **Page**, based on setup)
3. Click **Subscribe to this object**
4. Enter your webhook URL: `https://your-railway-url/webhook`
5. Add your **VERIFY_TOKEN**
6. Subscribe to these fields:

   - `messages`
   - `messaging_postbacks`

Meta will call your `GET /webhook` endpoint to verify. If success, you’re live ✅

### 🚀 Step 5: Go Live With Instagram Users

- Add test users to your Instagram App in the **Roles** tab
- Use those accounts to DM your connected IG business profile
- Watch the logs to confirm the flow is working

When ready to go public:

- Submit for **App Review** to access production users

### 🔒 Bonus: Securing Your Endpoint

- Only accept webhook POSTs where `req.body.object === 'instagram'`
- (Advanced) Use Meta X-Hub-Signature headers to verify source authenticity

Your bot is now deployed, reachable by Meta, and actively processing Instagram messages ✨

Next: we'll explore **testing workflows and debug techniques** to ensure the bot behaves correctly across scenarios.

## Testing Your Bot: DM Yourself, Use Test Users, and Monitor Logs

Now that the bot is deployed and connected to Meta’s webhook system, it's time to ensure everything works as expected — by **testing real message flows, validating data collection**, and **debugging edge cases**.

A well-tested chatbot can gracefully handle:

- Unexpected input
- Incomplete sessions
- API errors or outages

Let’s walk through a thorough testing process 🔎

### 🙋 Step 1: Add Instagram Test Users

In Meta Developer Console:

1. Go to **Roles > Instagram Testers**
2. Add your personal Instagram handle
3. On Instagram, accept the tester invite (check your app settings)

This gives you access to test the bot before public launch.

### 💬 Step 2: Send a DM to Your Business Profile

Open Instagram and DM something simple like:

```text
Hi there
```

You should see the bot reply:

> "Hey! 👋 What's your name?"

Walk through the full lead capture:

- Name
- Email (try invalid + valid)
- Interest

You should see the console log:

```bash
Captured lead: { name: ..., email: ..., interest: ... }
```

And in Supabase, a new row inserted.

### 🔍 Step 3: Test for Edge Cases

Try these inputs to test resilience:

- Empty messages (`""`)
- Emojis-only inputs
- Invalid email formats
- Starting mid-flow (simulate session loss)
- Multiple users messaging at once

Ensure your bot doesn’t crash and replies appropriately.

### 📊 Step 4: Monitor Logs (in Railway)

On Railway:

1. Go to your project
2. Click **Logs** tab
3. Watch live webhook events and debug output

This is crucial for:

- Tracking what payloads Meta sends
- Debugging why replies failed
- Logging Supabase insertions

### 🤖 Step 5: Use Postman or Curl for Manual Webhook Testing

Simulate incoming DMs using raw HTTP requests:

```bash
curl -X POST https://your-server.com/webhook \
  -H "Content-Type: application/json" \
  -d '{
    "object": "instagram",
    "entry": [{
      "id": "<page-id>",
      "messaging": [{
        "sender": { "id": "123456789" },
        "message": { "text": "Hello" }
      }]
    }]
  }'
```

This lets you:

- Bypass Instagram UI
- Automate QA scenarios

### 🤔 Step 6: Common Issues and Fixes

| Problem              | Likely Cause       | Fix                            |
| -------------------- | ------------------ | ------------------------------ |
| Bot not replying     | Wrong page token   | Regenerate via Graph Explorer  |
| 403 error on webhook | Token mismatch     | Check `VERIFY_TOKEN` in `.env` |
| No DM received       | Role not accepted  | Accept Instagram tester role   |
| Data not saving      | Supabase RLS block | Confirm insert policy enabled  |

With these tests, you can confidently validate your Instagram bot in multiple scenarios and ensure a smooth user experience 🚀

Next: we’ll look at **how to submit your bot for Meta review and launch it live**!

## Going Live: Submitting for Review and Launching

Once your Instagram bot is tested and production-ready, it’s time to **submit it for Meta review** so you can serve real users beyond just testers. Meta uses a human-in-the-loop process to verify bots comply with their policies.

Let’s walk through the final launch process step-by-step ✨

### 🚀 Step 1: Prepare for App Review

Before applying:

- Test thoroughly with your test users
- Clean up any debug logs or test flow artifacts
- Ensure responses are clear, compliant, and valuable to users

Meta reviewers will:

- Test your Instagram bot flow manually
- Verify responses make sense and don’t spam or mislead
- Confirm data handling (especially around emails or PII)

### 🔐 Step 2: Enable Required Permissions

Go to your app in the [Meta Developer Console](https://developers.facebook.com/):

1. Under **App Review > Permissions and Features**
2. Request the following:

   - `pages_messaging`
   - `instagram_basic`
   - `instagram_manage_messages`

Each permission needs justification. For example:

```text
We use `instagram_manage_messages` to respond to DMs from users with lead generation questions. We guide them through a brief Q&A and store their contact info securely.
```

### 📃 Step 3: Add Screencast and Instructions

Meta requires a **video walkthrough** of your bot in action. Include:

- Starting the flow from Instagram DMs
- Bot asking for name, email, interest
- User completing flow and receiving confirmation

Upload the video to Loom, YouTube (unlisted), or Google Drive (shared).

Also add **written steps** like:

```text
1. Go to our Instagram profile: @mybusinesshandle
2. Send "hi" to trigger the bot
3. The bot will guide you through a 3-step form
4. You’ll receive a thank-you message once complete
```

### 🔧 Step 4: Submit for Review

Once your permissions and screencast are ready:

1. Go to **App Review > Requests > Add items**
2. Add the permissions mentioned above
3. Fill in usage details + instructions
4. Click **Submit for Review**

Reviews typically take 3–7 days. If rejected:

- You’ll get a reason and can fix + resubmit
- Be clear, concise, and compliant with Meta's Messaging Policy

### 🚫 Step 5: Remove Development Mode

After approval:

1. Go to **App Settings > Basic**
2. Switch app mode from **Development** to **Live**
3. All users can now DM your Instagram and trigger the bot

Be sure to:

- Monitor logs in production
- Store leads securely
- Respect data privacy (e.g., show opt-in messages if needed)

### 📈 Final Checklist

| Item                        | Status |
| --------------------------- | ------ |
| Webhook deployed + verified | ✅     |
| Supabase storing leads      | ✅     |
| Bot handles edge cases      | ✅     |
| Permissions requested       | ✅     |
| Screencast prepared         | ✅     |
| Review instructions written | ✅     |
| App switched to Live        | ✅     |

Once your review is approved, you’re officially live — with a real Instagram DM bot generating real leads in real time 🚀

Congrats on shipping your bot into production!

Stay tuned for the next article where we plug this bot into **Shopify, WooCommerce, or a website** for seamless multi-platform lead generation 🛍️

---

**Hey, I’m Darshan Jitendra Chobarkar** — a freelance full-stack web developer surviving the caffeinated chaos of coding from Pune ☕💻 If you enjoyed this article (or even skimmed through while silently judging my code), you might like the rest of my tech adventures.

🔗 Explore more writeups, walkthroughs, and side projects at [dchobarkar.github.io](https://dchobarkar.github.io/)  
🔍 Curious where the debugging magic happens? Check out my commits at [github.com/dchobarkar](https://github.com/dchobarkar)  
👔 Let’s connect professionally on [LinkedIn](https://www.linkedin.com/in/dchobarkar/)

Thanks for reading — and if you’ve got thoughts, questions, or feedback, I’d genuinely love to hear from you. This blog’s not just a portfolio — it’s a conversation. Let’s keep it going 👋
