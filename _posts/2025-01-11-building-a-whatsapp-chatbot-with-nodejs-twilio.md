# Conversational AI: Building Intelligent Chatbots Across Platforms - 05: Building a WhatsApp Chatbot with Node.js + Twilio

## Why WhatsApp? Business Impact + Technical Considerations

WhatsApp isn’t just a messaging app anymore — it’s rapidly becoming a key part of customer communication strategies across industries. With over **2.7 billion monthly active users** and message open rates exceeding **98%**, businesses can no longer afford to ignore its potential. Whether it's for lead generation, order support, reminders, or conversational commerce, WhatsApp bots are now core touchpoints in the customer journey.

But building for WhatsApp is different from building for the web. You can’t just spin up a chat widget and start messaging users. WhatsApp has **tight API restrictions**, enforced by **Meta**, and the easiest way to get started is by using a **business messaging platform like Twilio**.

### 🔹 WhatsApp as a Conversational Channel

- **Ubiquity:** WhatsApp is installed on nearly every smartphone in regions like India, Brazil, and large parts of Europe and Africa.
- **Engagement:** The average user opens the app multiple times per day.
- **Frictionless experience:** No logins, apps, or new UX paradigms. Users chat the same way they chat with friends.

If you’re building anything B2C, WhatsApp bots let you meet users exactly where they are. Unlike email (cluttered inboxes) or apps (low install rate), WhatsApp offers near-instant feedback loops.

### 💼 Business Profile Types: Personal vs. Business vs. API

To build a WhatsApp chatbot, you need a **WhatsApp Business Account (WABA)**. Here’s a quick breakdown:

| Type                | Use Case                             | API Access | Messaging Limits             |
| ------------------- | ------------------------------------ | ---------- | ---------------------------- |
| Personal            | Regular chat app                     | No         | NA                           |
| Business App        | Small business replies               | No         | Manual messaging only        |
| Business API (WABA) | Automated chatbots, CRM integrations | Yes        | Tiered (1K → 10K → 100K/day) |

> Note: WhatsApp Business API cannot be used with the mobile app. It’s backend-driven and requires a third-party provider like Twilio or Vonage to interface with Meta’s systems.

### 🛍️ Enter Twilio: The Bridge to WhatsApp

Meta doesn’t give direct access to developers. Instead, they work with **Business Solution Providers (BSPs)** like Twilio, who manage message delivery, opt-in compliance, template submissions, and more.

Twilio makes WhatsApp development significantly easier by:

- Exposing a simple REST API
- Handling encryption, delivery, and compliance
- Offering a **sandbox environment** for testing
- Providing built-in support for **media messages, templates, and replies**

All we need to do is:

1. Connect our WhatsApp number to Twilio's sandbox
2. Set up a webhook to receive and respond to messages
3. Build our logic in Node.js

In this hands-on guide, we’ll do exactly that — starting from the sandbox and moving all the way to deploying a smart bot with OpenAI integration.

But first, let’s set up our project environment.

## Project Setup — Twilio Sandbox + Node.js Boilerplate

Before we can start handling real WhatsApp messages, we need a functional project environment. This section will guide you step-by-step through setting up Twilio's sandbox and initializing a Node.js server that can receive WhatsApp messages via webhooks.

### 🧰 Why Twilio?

Meta (WhatsApp's parent company) restricts direct API access — you can’t just hit a public WhatsApp endpoint. Instead, you need to go through approved **Business Solution Providers (BSPs)**. **Twilio** is one of the most popular BSPs and acts as the bridge between your server and WhatsApp's infrastructure.

Twilio offers:

- A free **WhatsApp Sandbox** for local testing
- A simplified webhook integration model
- Helpful developer tooling and libraries

Let's get started. 🔧

### 1️⃣ Twilio Account Setup and Sandbox Activation

1. Sign up or log into your [Twilio Console](https://www.twilio.com/console)
2. Navigate to **Messaging → Try it Out → Send a WhatsApp Message**
3. Follow the on-screen instructions to join the sandbox using your WhatsApp number

Once activated, Twilio provides a sandbox number (typically `+14155238886`) and a code to message from your personal WhatsApp number to join the test environment.

### 2️⃣ Create the Node.js Project

We’ll use a modular folder structure with Express.js for the server. Here’s how to initialize the boilerplate:

```bash
mkdir whatsapp-chatbot
cd whatsapp-chatbot
npm init -y
npm install express body-parser dotenv twilio
```

### 📁 Suggested Project Structure

```plaintext
whatsapp-chatbot/
├── node_modules/
├── .env
├── index.js
├── routes/
│   └── whatsapp.js
└── utils/
    └── twilioClient.js
```

### 3️⃣ Environment Variables (.env)

This file securely stores your Twilio credentials:

```env
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=whatsapp:+14155238886
```

> Replace `your_account_sid` and `your_auth_token` with values from your Twilio console.

### 4️⃣ Main Server Entry File (`index.js`)

```js
const express = require("express");
const bodyParser = require("body-parser");
const dotenv = require("dotenv");
const whatsappRoutes = require("./routes/whatsapp");

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3000;

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.use("/whatsapp", whatsappRoutes);

app.get("/", (req, res) => res.send("WhatsApp Chatbot Server Running"));

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

This sets up our Express server and mounts the `/whatsapp` route where Twilio will POST incoming messages.

### 5️⃣ Webhook Handler (`routes/whatsapp.js`)

```js
const express = require("express");
const router = express.Router();
const MessagingResponse = require("twilio").twiml.MessagingResponse;

router.post("/", (req, res) => {
  const twiml = new MessagingResponse();
  const incomingMsg = req.body.Body;
  console.log("Incoming Message:", incomingMsg);

  twiml.message("Hello from your WhatsApp bot!");
  res.writeHead(200, { "Content-Type": "text/xml" });
  res.end(twiml.toString());
});

module.exports = router;
```

This is the webhook endpoint that Twilio will call whenever a message is received. For now, it simply replies with a static greeting.

### 6️⃣ Twilio Client Helper (`utils/twilioClient.js`)

```js
const twilio = require("twilio");
const dotenv = require("dotenv");
dotenv.config();

const client = twilio(
  process.env.TWILIO_ACCOUNT_SID,
  process.env.TWILIO_AUTH_TOKEN
);
module.exports = client;
```

This wrapper gives us easy access to Twilio's messaging API anywhere in our project.

### 🔌 Local Testing with Ngrok

Since Twilio’s webhook needs to call your local server, we’ll expose it using [ngrok](https://ngrok.com/):

```bash
npm install -g ngrok
ngrok http 3000
```

Take the HTTPS forwarding URL from ngrok (e.g. `https://abcd1234.ngrok.io`) and set it in your Twilio sandbox settings under **Webhook URL** as:

```code
https://<your-ngrok-subdomain>.ngrok.io/whatsapp
```

🎉 Congrats! Your basic server is ready and live. Next, we’ll build a more intelligent reply system, first using rule-based routing and then integrating OpenAI for dynamic responses.

## Handling Incoming WhatsApp Messages with Webhooks

Now that our server is running and Twilio is configured to forward WhatsApp messages to it, it’s time to build our webhook logic.

In this section, we’ll:

- Understand how Twilio sends data to our webhook
- Parse and log the incoming message
- Send back a dynamic reply
- Add security with Twilio signature validation (optional but production-friendly)

### 🧾 What Happens Under the Hood?

When a user sends a message to your Twilio sandbox number, Twilio issues an **HTTP POST request** to your webhook endpoint. This request includes:

- `Body`: the actual text message
- `From`: the user's WhatsApp number
- `ProfileName`: sender's name
- Other metadata (timestamps, message SIDs, etc.)

Here's a sample payload from Twilio:

```json
{
  "SmsMessageSid": "SM1234567890",
  "NumMedia": "0",
  "SmsSid": "SM1234567890",
  "SmsStatus": "received",
  "Body": "Hello bot",
  "To": "whatsapp:+14155238886",
  "NumSegments": "1",
  "MessageSid": "SM1234567890",
  "AccountSid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "From": "whatsapp:+919876543210",
  "ApiVersion": "2010-04-01",
  "ProfileName": "Darshan"
}
```

### 🛠 Enhancing Our Webhook Logic

Let’s update our webhook route to:

- Extract message fields
- Reply based on the message content
- Send a fallback if the message is unknown

#### 📄 `routes/whatsapp.js`

```js
const express = require("express");
const router = express.Router();
const MessagingResponse = require("twilio").twiml.MessagingResponse;

router.post("/", (req, res) => {
  const { Body, From, ProfileName } = req.body;
  console.log(
    `[Incoming] From: ${From}, Name: ${ProfileName}, Message: ${Body}`
  );

  const twiml = new MessagingResponse();
  const msg = twiml.message();

  const normalizedText = Body.trim().toLowerCase();

  switch (normalizedText) {
    case "hi":
    case "hello":
      msg.body(
        `Hi ${
          ProfileName || "there"
        } 👋\nHow can I help you today? Type 'menu' to see options.`
      );
      break;
    case "menu":
      msg.body(
        `Here’s what I can do:\n- 'help': Show usage info\n- 'pricing': View our plans\n- 'agent': Talk to a human`
      );
      break;
    case "pricing":
      msg.body(
        "Our pricing starts at $9.99/month. Visit example.com/pricing for details."
      );
      break;
    case "agent":
      msg.body("Sure! I'll connect you to a support agent shortly. 🧑‍💼");
      break;
    default:
      msg.body(
        "Sorry, I didn’t understand that. 🤖\nType 'menu' to see what I can do."
      );
  }

  res.writeHead(200, { "Content-Type": "text/xml" });
  res.end(twiml.toString());
});

module.exports = router;
```

### 🔐 [Optional] Securing Your Webhook (Signature Validation)

Twilio includes an `X-Twilio-Signature` header with each request. You can validate it to ensure the request genuinely came from Twilio.

#### Install the validator

```bash
npm install twilio
```

#### Add validation middleware (before route)

```js
const twilio = require("twilio");
const twilioSignature = require("twilio").validateRequest;

function validateTwilioRequest(req, res, next) {
  const signature = req.headers["x-twilio-signature"];
  const url = `https://${req.headers.host}${req.originalUrl}`;
  const params = req.body;

  const isValid = twilio.validateRequest(
    process.env.TWILIO_AUTH_TOKEN,
    signature,
    url,
    params
  );

  if (!isValid) {
    return res.status(403).send("Invalid Twilio request");
  }

  next();
}
```

Then add this to your route:

```js
router.post('/', validateTwilioRequest, (req, res) => { ... });
```

> 🔐 This is especially important in production to prevent spoofed messages.

With this, we’ve built a more interactive and slightly secure chatbot. In the next section, we’ll push this further with OpenAI-powered smart responses!

## Creating a Basic Chatbot with Rule-Based Responses

At this point, your bot can receive messages and respond with hardcoded replies. But what happens when your logic grows? You don’t want to stuff your entire routing logic inside a single `switch` block. 🧱

Let’s now refactor our code to:

- Build a clean command router structure
- Organize logic per intent (e.g., `menu`, `help`, `pricing`)
- Make it easy to extend and test

### 📁 Updated Project Structure

```structure
whatsapp-chatbot/
├── handlers/
│   ├── agent.js
│   ├── help.js
│   ├── menu.js
│   ├── pricing.js
│   └── fallback.js
├── routes/
│   └── whatsapp.js
└── utils/
    └── commandRouter.js
```

### 1️⃣ Intent Handlers (e.g., `handlers/menu.js`)

Each intent has its own module that returns a response string:

#### `handlers/menu.js`

```js
module.exports = (name) =>
  `Hi ${
    name || "there"
  }! Here’s what I can help with:\n\n- 'help': Show usage info\n- 'pricing': View our plans\n- 'agent': Talk to a human`;
```

#### `handlers/help.js`

```js
module.exports = () =>
  `Type any of the following commands:\n- 'menu'\n- 'pricing'\n- 'agent'`;
```

#### `handlers/pricing.js`

```js
module.exports = () =>
  `Our pricing plans start at $9.99/month. Learn more at https://example.com/pricing.`;
```

#### `handlers/agent.js`

```js
module.exports = () => `Connecting you to a human agent now. 👩‍💼 Please wait...`;
```

#### `handlers/fallback.js`

```js
module.exports = () =>
  `I didn’t get that. 🤔 Type 'menu' to see available commands.`;
```

### 2️⃣ Command Router (`utils/commandRouter.js`)

```js
const menu = require("../handlers/menu");
const help = require("../handlers/help");
const pricing = require("../handlers/pricing");
const agent = require("../handlers/agent");
const fallback = require("../handlers/fallback");

const commandRouter = (input, profileName) => {
  const text = input.trim().toLowerCase();

  if (["menu"].includes(text)) return menu(profileName);
  if (["help"].includes(text)) return help();
  if (["pricing"].includes(text)) return pricing();
  if (["agent"].includes(text)) return agent();

  return fallback();
};

module.exports = commandRouter;
```

### 3️⃣ Updating Webhook to Use the Router (`routes/whatsapp.js`)

```js
const express = require("express");
const router = express.Router();
const MessagingResponse = require("twilio").twiml.MessagingResponse;
const commandRouter = require("../utils/commandRouter");

router.post("/", (req, res) => {
  const { Body, ProfileName } = req.body;

  const replyText = commandRouter(Body, ProfileName);

  const twiml = new MessagingResponse();
  twiml.message(replyText);

  res.writeHead(200, { "Content-Type": "text/xml" });
  res.end(twiml.toString());
});

module.exports = router;
```

### 🤖 Why This Approach?

- **Separation of concerns:** Each command is self-contained.
- **Scalability:** New features = new file.
- **Testing:** Each handler can be unit-tested in isolation.
- **Readability:** Your webhook logic stays clean and simple.

With this refactor, your bot now has a **modular, extensible rule-based brain** — a perfect base to plug in OpenAI logic next. Let’s do that in the next section!

## Integrating OpenAI for Dynamic Conversations

Rule-based bots are simple but limited. They can’t handle unexpected input, slang, or natural conversation flows. This is where OpenAI comes in 🧠.

In this section, we’ll:

- Integrate the OpenAI API into our WhatsApp chatbot
- Handle stateless queries (since WhatsApp doesn’t maintain session context)
- Craft a good prompt strategy for short-form messaging
- Ensure fallbacks if the AI fails

### 🔑 Step 1: Add OpenAI SDK and API Key

Install the SDK:

```bash
npm install openai
```

Create or update your `.env`:

```env
OPENAI_API_KEY=your_openai_api_key
```

### 📦 Step 2: OpenAI Client Utility (`utils/openaiClient.js`)

```js
const { OpenAI } = require("openai");
require("dotenv").config();

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

async function getOpenAIReply(message, name = "") {
  try {
    const prompt = `You are a helpful WhatsApp bot for a small business. Keep responses under 100 words.\n\nUser (${name}): ${message}\nBot:`;

    const completion = await openai.chat.completions.create({
      messages: [
        {
          role: "system",
          content: "You are a concise customer service bot for WhatsApp.",
        },
        { role: "user", content: prompt },
      ],
      model: "gpt-3.5-turbo",
      max_tokens: 100,
    });

    return completion.choices[0].message.content.trim();
  } catch (err) {
    console.error("OpenAI Error:", err);
    return "Sorry, I'm having trouble understanding that. Please try again later.";
  }
}

module.exports = getOpenAIReply;
```

This function builds a prompt, sends it to OpenAI, and returns the response.

### 🧠 Step 3: Add AI as a Fallback in Router (`utils/commandRouter.js`)

Modify your command router to use OpenAI when no rule matches:

```js
const menu = require("../handlers/menu");
const help = require("../handlers/help");
const pricing = require("../handlers/pricing");
const agent = require("../handlers/agent");
const getOpenAIReply = require("./openaiClient");

const commandRouter = async (input, profileName) => {
  const text = input.trim().toLowerCase();

  if (["menu"].includes(text)) return menu(profileName);
  if (["help"].includes(text)) return help();
  if (["pricing"].includes(text)) return pricing();
  if (["agent"].includes(text)) return agent();

  return await getOpenAIReply(input, profileName);
};

module.exports = commandRouter;
```

> 📌 The function is now async! Be sure to reflect this in your webhook.

### ✨ Step 4: Update Webhook to Handle Promises (`routes/whatsapp.js`)

Update your handler to use `await`:

```js
const express = require("express");
const router = express.Router();
const MessagingResponse = require("twilio").twiml.MessagingResponse;
const commandRouter = require("../utils/commandRouter");

router.post("/", async (req, res) => {
  const { Body, ProfileName } = req.body;
  const replyText = await commandRouter(Body, ProfileName);

  const twiml = new MessagingResponse();
  twiml.message(replyText);

  res.writeHead(200, { "Content-Type": "text/xml" });
  res.end(twiml.toString());
});

module.exports = router;
```

### 🤖 Smart, Flexible Bot Ready

Your bot can now:

- Handle known commands like `menu`, `pricing`, etc.
- Fallback to OpenAI for everything else
- Offer flexible, natural replies tailored to WhatsApp’s short-form style

In the next section, we’ll **deploy this bot and connect it with Twilio’s webhook console for live testing!**

## Deploying the Bot (Vercel/Railway + Twilio Webhook Setup)

With our bot logic in place, it’s time to take it live. In this section, we'll:

- Deploy the Node.js server to Railway (you can also use Vercel or Render)
- Update Twilio’s webhook to point to the live URL
- Test the bot in real-time via WhatsApp

### 🚀 Step 1: Choose a Hosting Platform

While Vercel excels for frontend (Next.js) projects, **Railway** and **Render** are easier for backend Express apps with file structure flexibility.

For this guide, we'll use **Railway**:

### 🧪 Step 2: Initialize Git and Push to GitHub

```bash
git init
git add .
git commit -m "Initial WhatsApp bot commit"
git remote add origin https://github.com/your-username/whatsapp-chatbot.git
git push -u origin main
```

### 🛠 Step 3: Deploy to Railway

1. Go to [https://railway.app](https://railway.app)
2. Click **New Project → Deploy from GitHub Repo**
3. Select your `whatsapp-chatbot` repo
4. Set environment variables:
   - `TWILIO_ACCOUNT_SID`
   - `TWILIO_AUTH_TOKEN`
   - `TWILIO_PHONE_NUMBER`
   - `OPENAI_API_KEY`
5. Railway auto-detects Node and installs dependencies
6. Set `Start Command` as:

```bash
node index.js
```

Once deployed, Railway provides a live HTTPS URL like:

```link
https://whatsapp-bot.up.railway.app
```

### 🔗 Step 4: Update Twilio Webhook URL

Go to your [Twilio Console > Messaging > Sandbox](https://www.twilio.com/console/sms/whatsapp/sandbox) and set:

```text
WHEN A MESSAGE COMES IN:
https://whatsapp-bot.up.railway.app/whatsapp
```

Click **Save**.

> ✅ Tip: You can test it by simply messaging your Twilio sandbox number again from WhatsApp.

### ✅ Deployment Checklist

- [x] Code pushed to GitHub
- [x] Deployed to Railway with correct env vars
- [x] Twilio webhook updated to live URL
- [x] Testing works on WhatsApp via sandbox

### 🧪 Local vs Production Differences

| Feature          | Local (ngrok)        | Production (Railway)          |
| ---------------- | -------------------- | ----------------------------- |
| Webhook URL      | Random subdomain     | Fixed, custom domain possible |
| Speed            | Slightly slower      | Faster, scalable              |
| Restart Required | Yes, after code edit | Automatic via Railway builds  |

Now that our bot is live and reachable via WhatsApp, we’re ready to scale it — next stop: business verification and message templates!

## From Demo to Real Bot — Approval, Templates, and Rollout

Now that our chatbot works in Twilio’s sandbox and is deployed live, let’s make it production-ready. WhatsApp requires **business verification, template approvals**, and user **opt-ins** before you can use the API outside the sandbox.

This section walks through:

- Business verification with Meta
- Requesting message templates in Twilio
- Handling opt-in flows
- Moving to a live WhatsApp Business number

### 🏢 Step 1: Verify Your Business on Facebook

You must have a **Facebook Business Manager** account linked to your Twilio project.

1. Visit [https://business.facebook.com/settings](https://business.facebook.com/settings)
2. Complete **Business Verification**: upload docs, verify domain, etc.
3. Link your WhatsApp Business Account (WABA) to Twilio by following their guided onboarding in Twilio Console under **Messaging > Senders > WhatsApp Senders**

Once verified, you’ll be allowed to request higher messaging tiers and use your own phone number.

### ☎️ Step 2: Add a Real WhatsApp Number

In the Twilio Console:

1. Go to **Messaging > Senders > WhatsApp Senders**
2. Click **Add a Sender**
3. Follow the steps to register your real business number (it must be disconnected from the WhatsApp app)

You’ll receive a 6-digit verification code on WhatsApp which you’ll enter during setup.

### ✍️ Step 3: Submit Message Templates

WhatsApp does not allow businesses to message users freely unless the user initiates the session. To send messages outside of the 24-hour window, you need **pre-approved message templates**.

Examples:

- Appointment reminders
- Delivery updates
- Promo alerts

Submit templates in Twilio Console:

1. Go to **Messaging > Content > WhatsApp Templates**
2. Click **Create Template**
3. Fill details: name, category, language, and content with placeholders

Example template:

```json
{
  "name": "appointment_reminder",
  "language": "en",
  "category": "TRANSACTIONAL",
  "components": [
    {
      "type": "BODY",
      "text": "Hi {{1}}, your appointment is scheduled for {{2}}."
    }
  ]
}
```

Once approved (usually within minutes or hours), you can send this using Twilio’s API.

### ✅ Step 4: Ensure User Opt-In

WhatsApp’s policy requires **explicit user opt-in** before you message them outside the session window.

Recommended opt-in channels:

- Webforms with checkboxes
- SMS/Email confirmation
- In-app toggle or chatbot prompt

Store their opt-in timestamp + context in your DB.

### 🧪 Step 5: Test with Real Users

Now that you’re out of sandbox:

- Use your live number and approved templates to send messages
- Make sure opt-in is captured and respected
- Keep logs for compliance

### 🚀 You’re Production Ready

You now have:

- A verified WhatsApp Business Account
- A working bot on a real number
- Template-based messaging support
- Compliance-ready opt-in tracking

Next, let’s enhance the experience with **rich media, quick replies, and multi-language support**!

## Bonus — Rich Media, Quick Replies, and Language Support

Once your WhatsApp bot is live, it's time to make it feel truly engaging. In this final section, we’ll enhance your bot with:

- Rich media like images and PDFs
- Quick reply buttons (via templates)
- Multi-language support (i18n)

### 🖼 Sending Rich Media (Images, Documents)

Twilio lets you send media by attaching a `MediaUrl` to the response. You can also send media using Twilio’s REST API.

#### Example: Sending an Image in Your Webhook

```js
const twiml = new MessagingResponse();
const msg = twiml.message();
msg.body("Here is our catalog 📚");
msg.media("https://example.com/catalog.jpg");
```

#### Example: Sending a PDF

```js
msg.body("Download our brochure");
msg.media("https://example.com/brochure.pdf");
```

Make sure your URLs are HTTPS and publicly accessible.

### 🧮 Sending Interactive Buttons (via Message Templates)

WhatsApp doesn’t support dynamic buttons in freeform messages, but you can create **interactive templates** with buttons.

These must be pre-approved like any template.

#### Template JSON (with buttons)

```json
{
  "name": "order_options",
  "language": "en",
  "category": "UTILITY",
  "components": [
    {
      "type": "BODY",
      "text": "Hi {{1}}, how can we help you today?"
    },
    {
      "type": "BUTTONS",
      "buttons": [
        {
          "type": "QUICK_REPLY",
          "text": "Track Order"
        },
        {
          "type": "QUICK_REPLY",
          "text": "Contact Support"
        }
      ]
    }
  ]
}
```

Use Twilio’s API to send this template by name once approved.

### 🌍 Multi-Language Support (i18n)

To make your bot multilingual:

- Maintain translation files per language
- Detect user language via metadata or explicit input
- Swap response templates accordingly

#### Example: Basic i18n with JSON

`/locales/en.json`

```json
{
  "menu": "Here’s what I can help with:\n- 'help': Show usage info\n- 'pricing': View our plans"
}
```

`/locales/es.json`

```json
{
  "menu": "En qué puedo ayudarte:\n- 'ayuda': Información\n- 'precios': Ver planes"
}
```

#### Dynamic Locale Loader (`utils/i18n.js`)

```js
const fs = require("fs");

function getTranslation(lang, key) {
  try {
    const content = fs.readFileSync(`./locales/${lang}.json`, "utf8");
    const messages = JSON.parse(content);
    return messages[key] || key;
  } catch {
    return key;
  }
}

module.exports = getTranslation;
```

Now update handlers to use:

```js
const t = require("../utils/i18n");
const msg = t(lang, "menu");
```

> Detect `lang` from user input or WhatsApp’s `Language` field (if available).

### 🎉 Conclusion: A Feature-Rich WhatsApp Bot

With rich media, localized messages, and interactive buttons, your WhatsApp bot is now:

- Engaging
- Scalable
- User-friendly

This closes our hands-on guide to building an intelligent, multi-platform chatbot — ready for real-world users! 🌍🤖

---

**Hey, I’m Darshan Jitendra Chobarkar** — a freelance full-stack web developer surviving the caffeinated chaos of coding from Pune ☕💻 If you enjoyed this article (or even skimmed through while silently judging my code), you might like the rest of my tech adventures.

🔗 Explore more writeups, walkthroughs, and side projects at [dchobarkar.github.io](https://dchobarkar.github.io/)  
🔍 Curious where the debugging magic happens? Check out my commits at [github.com/dchobarkar](https://github.com/dchobarkar)  
👔 Let’s connect professionally on [LinkedIn](https://www.linkedin.com/in/dchobarkar/)

Thanks for reading — and if you’ve got thoughts, questions, or feedback, I’d genuinely love to hear from you. This blog’s not just a portfolio — it’s a conversation. Let’s keep it going 👋
