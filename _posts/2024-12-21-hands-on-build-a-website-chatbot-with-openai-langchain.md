# Conversational AI: Building Intelligent Chatbots Across Platforms - 03: Hands-On: Build a Website Chatbot with OpenAI + LangChain

## 🤖 Project Overview: What We’re Building

In this tutorial, we're diving into the practical side of conversational AI by building a **full-stack website chatbot** using **LangChain** and **OpenAI**, entirely on the **free tier**. This project is designed to help you understand how to integrate large language models into real-world applications without relying on paid tools. By the end, you'll have a fully functional chatbot that can hold multi-turn conversations directly on your website.

### 🎯 Chatbot Use Case: Conversational Website Assistant

The bot we'll build acts like a customer-facing assistant capable of:

- Holding a conversation (with memory)
- Answering questions based on previous inputs
- Responding naturally using an LLM like GPT-3.5 or open-source alternatives
- Running entirely within a free deployment setup (Railway + Vercel)

Think of it as a **starter template** for:

- Support bots for SaaS
- Product FAQs bots
- Personal portfolio site assistants
- E-learning bots for documentation

### 🧱 Project Stack Breakdown

Here’s the tech stack we’ll use:

#### Backend

- **Node.js + Express** — lightweight API server
- **LangChain (JS)** — orchestrates the LLM + memory + chains
- **OpenAI / Hugging Face** — for the LLM interface (we'll support both)
- **BufferMemory** — to track past user-bot interactions in memory

#### Frontend

- **React + TailwindCSS** — clean, minimal UI for the chatbot
- Fetch-based API calls to our backend

#### Deployment

- **Vercel** (Frontend)
- **Railway** (Backend)
- GitHub for version control and CI/CD

### 💸 Free-Tier Strategy: Cost-Conscious from the Ground Up

This project is built for developers who want to:

- Avoid vendor lock-in
- Experiment without needing a credit card
- Prototype with generous free usage limits

Here’s how we handle common cost-related blockers:

| Concern    | Free-Tier Solution                                                   |
| ---------- | -------------------------------------------------------------------- |
| LLM Usage  | OpenAI free-tier key or Hugging Face API (text-generation-inference) |
| Vector DB  | `Chroma` - local, embedded vector store                              |
| Deployment | Vercel (free hobby plan) + Railway (free backend instance)           |

You’ll be able to run this bot **end-to-end for \$0/month**.

### 🧩 Architecture Preview

The high-level flow looks like this:

```plaintext
User Input -> React Frontend -> Express API -> LangChain LLM Chain -> LLM Response -> Back to User
```

- Memory is preserved in the backend per session (in-process for now)
- Optional file upload uses LangChain's RAG tools with Chroma (all local)

### 📁 Folder Structure (Planned)

We’ll structure our project clearly for modularity:

```plaintext
/chatbot-openai-langchain-starter
|-- client/            # React frontend
|-- server/            # Express + LangChain backend
|   |-- routes/
|   |-- llm/
|   |-- memory/
|-- .env               # For API keys
|-- README.md          # Setup instructions
```

### 🚀 Next Steps

Now that we know what we’re building and how everything fits together, it’s time to get our hands dirty. In the next section, we’ll:

- Scaffold the backend
- Set up LangChain and connect to an LLM
- Write our first API route

Let’s go build a chatbot, shall we? 🛠️

## ⚙️ Setting Up the Backend: LangChain + Express API

Let’s kick things off by building the backend for our chatbot. We’ll use **Node.js + Express** as our server and plug in **LangChain** to handle language model interactions. This section walks you through the exact steps — code-first, no assumptions. Ready to build? Let’s go 🛠️

### 📁 Project Structure

Before diving into code, here’s how we’ll organize our project. Keeping a clean folder structure helps with scalability and debugging.

```plaintext
chatbot-openai-langchain-starter/
├── server/                 # Backend API
│   ├── index.js            # Main entry point
│   ├── routes/             # API routes
│   │   └── chat.js         # Chat endpoint logic
│   ├── llm/                # LangChain-related logic
│   │   └── langchain.js    # Chain + memory config
│   └── .env                # Environment variables
├── client/                 # Frontend (will be added later)
└── README.md               # Documentation
```

### 🧩 Step 1: Initialize the Project

Start by setting up your project folder and backend workspace:

```bash
mkdir chatbot-openai-langchain-starter && cd chatbot-openai-langchain-starter
mkdir server && cd server
npm init -y
```

This sets up a new Node.js project inside the `server` directory.

### 📦 Step 2: Install Dependencies

We need these packages:

- `express`: Web server
- `dotenv`: Manage environment variables
- `cors`: Allow frontend to call backend
- `langchain`: Power LLM chains and memory
- `openai`: SDK for calling OpenAI models

Install everything in one go:

```bash
npm install express dotenv cors langchain openai
```

Enable ES module support by editing `package.json`:

```json
{
  "type": "module"
}
```

This allows us to use `import`/`export` instead of `require()`.

### 🛠️ Step 3: Basic Express Server

Now we’ll set up our basic API server that will act as a middleman between the frontend and the LLM.

Create `server/index.js`:

```js
import express from "express";
import dotenv from "dotenv";
import cors from "cors";
import chatRoute from "./routes/chat.js";

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3001;

app.use(cors());
app.use(express.json());

app.use("/chat", chatRoute);

app.get("/", (req, res) => {
  res.send("Chatbot backend is running");
});

app.listen(PORT, () => {
  console.log(`Server listening on http://localhost:${PORT}`);
});
```

This initializes the server and mounts our chatbot API at `/chat`.

### 🧠 Step 4: Create the Chat Route

We’ll now define the POST route where incoming user messages are received and passed to the LLM.

Create `server/routes/chat.js`:

```js
import express from "express";
import { runChat } from "../llm/langchain.js";

const router = express.Router();

router.post("/", async (req, res) => {
  const { message } = req.body;
  if (!message) {
    return res.status(400).json({ error: "Message is required" });
  }
  try {
    const response = await runChat(message);
    res.json({ response });
  } catch (err) {
    console.error("Chat error:", err);
    res.status(500).json({ error: "Something went wrong" });
  }
});

export default router;
```

This is our `/chat` endpoint — it accepts a user message, passes it to LangChain, and returns the response.

### 🤖 Step 5: LangChain Integration

This is where we bring in the power of LLMs.

Create `server/llm/langchain.js`:

```js
import { ChatOpenAI } from "langchain/chat_models/openai";
import { ConversationChain } from "langchain/chains";
import { BufferMemory } from "langchain/memory";

const model = new ChatOpenAI({
  temperature: 0.7,
  openAIApiKey: process.env.OPENAI_API_KEY,
});

const memory = new BufferMemory();
const chain = new ConversationChain({ llm: model, memory });

export async function runChat(input) {
  const result = await chain.call({ input });
  return result.response;
}
```

Here’s what this setup does:

- Connects to OpenAI using `ChatOpenAI`
- Adds short-term memory via `BufferMemory`
- Wraps both in a `ConversationChain`, which is a dialogue loop
- Exposes `runChat()` that accepts user input and returns a response

### 🔐 Step 6: Setup Environment Variables

Inside your `server/` folder, create a `.env` file:

```env
OPENAI_API_KEY=your_openai_key_here
```

This keeps sensitive credentials out of your source code.

### ✅ Step 7: Run and Test Your Backend

Start your server:

```bash
node index.js
```

Use Postman or curl to test the chat route:

```bash
curl -X POST http://localhost:3001/chat \
     -H "Content-Type: application/json" \
     -d '{"message": "Hello, who are you?"}'
```

You should receive a response from your bot 🧠💬

With this, your chatbot backend is fully functional. You now have:

- An Express API that routes messages
- LangChain hooked to OpenAI with memory
- A clean project foundation to expand on

Next, we’ll look into improving context awareness by expanding how memory works in LangChain 🧠
