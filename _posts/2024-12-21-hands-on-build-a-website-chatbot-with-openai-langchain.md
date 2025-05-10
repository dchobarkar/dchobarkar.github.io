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

## 🧠 Adding Memory with LangChain: Context Awareness

Most real conversations rely on memory. Without it, every new message would feel like talking to a goldfish 🐟. In this section, we’ll make our chatbot context-aware using **LangChain’s memory modules**.

By the end of this section, your bot will:

- Understand follow-up questions
- Refer back to previous user messages
- Feel more natural and intelligent 🤖

### 🔍 What is BufferMemory?

`BufferMemory` is one of LangChain’s simplest memory classes. It keeps a running history of the chat in memory. This is suitable for most short-lived chatbot sessions.

It stores conversation as a list of alternating messages (user → bot → user → bot...) and passes the entire history into the prompt every time. No database needed.

### 🧩 Step 1: Refactor the LangChain Setup

Let’s update `server/llm/langchain.js` to make memory a bit more dynamic.

```js
// server/llm/langchain.js
import { ChatOpenAI } from "langchain/chat_models/openai";
import { ConversationChain } from "langchain/chains";
import { BufferMemory } from "langchain/memory";

// 🧠 Create memory per session/user
const memoryStore = new Map();

function getChain(sessionId) {
  if (!memoryStore.has(sessionId)) {
    const memory = new BufferMemory();
    const model = new ChatOpenAI({
      temperature: 0.7,
      openAIApiKey: process.env.OPENAI_API_KEY,
    });
    const chain = new ConversationChain({ llm: model, memory });
    memoryStore.set(sessionId, chain);
  }
  return memoryStore.get(sessionId);
}

export async function runChat(input, sessionId = "default") {
  const chain = getChain(sessionId);
  const result = await chain.call({ input });
  return result.response;
}
```

**🧠 Explanation:**

- We use a simple `Map` object to track memory by `sessionId`
- Each session gets its own `ConversationChain` and `BufferMemory`
- You can eventually replace this with Redis or DB-backed memory for production

### 🛠️ Step 2: Update the Chat Route to Use Sessions

Update `server/routes/chat.js` to accept a session ID:

```js
// server/routes/chat.js
import express from "express";
import { runChat } from "../llm/langchain.js";

const router = express.Router();

router.post("/", async (req, res) => {
  const { message, sessionId } = req.body;
  if (!message) {
    return res.status(400).json({ error: "Message is required" });
  }
  try {
    const response = await runChat(message, sessionId);
    res.json({ response });
  } catch (err) {
    console.error("Chat error:", err);
    res.status(500).json({ error: "Something went wrong" });
  }
});

export default router;
```

Now, the API can track multiple conversations at once by passing different `sessionId`s.

You can test it like this:

```bash
curl -X POST http://localhost:3001/chat \
     -H "Content-Type: application/json" \
     -d '{"message": "Hello, who are you?", "sessionId": "abc123"}'
```

### 🔍 Debugging Memory State

Want to peek under the hood? Log the memory content by modifying `runChat()`:

```js
console.log("\n--- Chat History ---\n", memory.chatHistory);
```

This will help you debug and understand how memory evolves.

### ✅ Result: Multi-Turn Chatbot

You’ve now built a chatbot that:

- Tracks conversation per session
- Supports multiple concurrent users
- Handles context like a pro

In the next section, we’ll switch gears and build a beautiful **frontend interface in React** to talk to our bot 💬

## 💬 Building the React Frontend Chat Interface

With the backend running smoothly, it’s time to give our chatbot a face! In this section, we’ll build a sleek and responsive **React + TailwindCSS** frontend that connects to our Express + LangChain backend.

By the end, you’ll have a fully functional chat UI with:

- Chat history
- Message input box
- Bot typing indicator
- API integration

### 🧩 Step 1: Create React App with Vite

Navigate to the project root and scaffold your frontend:

```bash
cd ..
npm create vite@latest client -- --template react
cd client
npm install
```

Then install Tailwind CSS:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

Update `tailwind.config.js`:

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

In `index.css`, replace with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 🛠️ Step 2: Build the Chat UI

Create a clean layout in `src/App.jsx`:

```jsx
import { useState } from "react";

function App() {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);
  const sessionId = "demo-session";

  const sendMessage = async () => {
    if (!input.trim()) return;
    const userMessage = { sender: "user", text: input };
    setMessages([...messages, userMessage]);
    setInput("");
    setLoading(true);

    try {
      const res = await fetch("http://localhost:3001/chat", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ message: input, sessionId }),
      });
      const data = await res.json();
      const botMessage = { sender: "bot", text: data.response };
      setMessages((prev) => [...prev, botMessage]);
    } catch (err) {
      alert("Something went wrong");
    } finally {
      setLoading(false);
    }
  };

  const handleKeyPress = (e) => {
    if (e.key === "Enter") sendMessage();
  };

  return (
    <div className="min-h-screen bg-gray-100 flex flex-col items-center justify-center p-4">
      <div className="w-full max-w-xl bg-white rounded-xl shadow p-4 space-y-4">
        <div className="overflow-y-auto h-96 border p-3 rounded">
          {messages.map((msg, i) => (
            <div
              key={i}
              className={`mb-2 ${
                msg.sender === "user" ? "text-right" : "text-left"
              }`}
            >
              <span
                className={`inline-block px-3 py-2 rounded-lg ${
                  msg.sender === "user" ? "bg-blue-200" : "bg-gray-200"
                }`}
              >
                {msg.text}
              </span>
            </div>
          ))}
          {loading && (
            <div className="text-sm italic text-gray-500">Bot is typing...</div>
          )}
        </div>
        <div className="flex items-center gap-2">
          <input
            className="flex-1 border rounded px-3 py-2"
            placeholder="Type a message..."
            value={input}
            onChange={(e) => setInput(e.target.value)}
            onKeyDown={handleKeyPress}
          />
          <button
            onClick={sendMessage}
            className="bg-blue-500 text-white px-4 py-2 rounded"
          >
            Send
          </button>
        </div>
      </div>
    </div>
  );
}

export default App;
```

### 🔌 Step 3: Connect to Backend API

Make sure your backend (`localhost:3001`) is running when you test this. We use the `fetch()` API to send the message and receive a response.

**💡 Dev tip**: You can use Vite proxy for CORS-free development by updating `vite.config.js` later.

### ✅ Result: Interactive Chat UI

You now have a frontend that:

- Tracks messages in React state
- Sends input to your chatbot API
- Displays both user and bot replies
- Shows loading feedback

In the next part, we’ll deploy this using Vercel and Railway so others can use your chatbot online 🚀

## 🚀 Free Deployment: Vercel (Frontend) + Railway (Backend)

Now that your chatbot works locally, let’s take it live — for free! We’ll use **Vercel** to host the frontend and **Railway** to host the backend. Both have generous free tiers and integrate smoothly with GitHub.

You’ll end up with:

- A live website for your chatbot UI (Vercel)
- A live API server that handles LangChain + OpenAI logic (Railway)

### 🧩 Step 1: Push Your Code to GitHub

First, commit and push your project to a GitHub repo:

```bash
git init
git add .
git commit -m "Initial chatbot app"
git remote add origin https://github.com/yourusername/chatbot-openai-langchain-starter.git
git push -u origin main
```

### 🛠️ Step 2: Deploy Backend to Railway

1. Go to [https://railway.app](https://railway.app)
2. Click **New Project → Deploy from GitHub Repo**
3. Select your repo and choose the `server/` directory as root

#### 📁 Railway Settings

- **Root Directory**: `server`
- **Start Command**: `node index.js`

#### 🔐 Add Environment Variables

- Key: `OPENAI_API_KEY`
- Value: your OpenAI key

Railway will detect Express and auto-deploy it. After deployment, you’ll get a public URL like:

```plaintext
https://chatbot-api-production.up.railway.app
```

### 🎨 Step 3: Deploy Frontend to Vercel

1. Go to [https://vercel.com](https://vercel.com)
2. Click **Add New Project** and select your GitHub repo
3. Choose the `client/` folder as the project root

#### 🛠 Vercel Build Settings

- **Framework Preset**: Vite
- **Output Directory**: `dist`

#### 🔌 Add Environment Variable (Optional)

If you’re proxying API URLs, you can set:

- `VITE_API_URL=https://your-backend-url/chat`

### ⚙️ Optional: Fix CORS on Backend

If your frontend can’t reach your backend, you might need to tweak CORS:

```js
// server/index.js
app.use(cors({ origin: "https://your-frontend.vercel.app" }));
```

Or allow all (for testing only):

```js
app.use(cors());
```

### 🌐 Result: Public Chatbot App

You now have:

- Live frontend at `https://your-chatbot.vercel.app`
- Live backend API at `https://your-chatbot-api.railway.app`

Test it in the browser, share with friends, or embed it in your portfolio!

Next up: we’ll add optional file upload + document Q&A using Retrieval-Augmented Generation 📄🧠

## 📄 Optional: Add File Upload + RAG (Retrieval-Augmented Generation)

Want your chatbot to answer questions based on a **user-uploaded PDF**? This is where **RAG (Retrieval-Augmented Generation)** comes in.

We’ll enable:

- Uploading a PDF
- Parsing its text
- Embedding with OpenAI or Hugging Face
- Storing + searching in a local Chroma vector store
- Querying the doc contextually

All of this works on a **local, free setup** — no paid vector DBs needed! 🧠

---

### 📦 Step 1: Install Additional Dependencies

In your `server/` folder:

```bash
npm install multer pdf-parse
```

These packages handle file uploads and PDF parsing.

---

### 📁 Step 2: Add Upload Endpoint

Create `server/routes/upload.js`:

```js
import express from "express";
import multer from "multer";
import pdf from "pdf-parse";
import fs from "fs";
import path from "path";

const upload = multer({ dest: "uploads/" });
const router = express.Router();

router.post("/", upload.single("file"), async (req, res) => {
  const filePath = req.file.path;
  try {
    const dataBuffer = fs.readFileSync(filePath);
    const parsed = await pdf(dataBuffer);
    fs.unlinkSync(filePath); // delete uploaded file

    res.json({ text: parsed.text });
  } catch (err) {
    console.error("PDF parse error:", err);
    res.status(500).json({ error: "Failed to parse PDF" });
  }
});

export default router;
```

In `index.js`:

```js
import uploadRoute from "./routes/upload.js";
app.use("/upload", uploadRoute);
```

---

### 🧠 Step 3: Create Vector Store + QA Chain

Create `server/llm/retriever.js`:

```js
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { Chroma } from "langchain/vectorstores/chroma";
import { RetrievalQAChain } from "langchain/chains";
import { ChatOpenAI } from "langchain/chat_models/openai";
import { TextLoader } from "langchain/document_loaders/fs/text";
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import fs from "fs";

let vectorStore;

export async function createKnowledgeBase(text) {
  fs.writeFileSync("uploads/docs.txt", text);

  const loader = new TextLoader("uploads/docs.txt");
  const docs = await loader.load();

  const splitter = new RecursiveCharacterTextSplitter({
    chunkSize: 1000,
    chunkOverlap: 200,
  });
  const chunks = await splitter.splitDocuments(docs);

  const embeddings = new OpenAIEmbeddings();
  vectorStore = await Chroma.fromDocuments(chunks, embeddings);
}

export async function askDocQuestion(query) {
  if (!vectorStore) throw new Error("No knowledge base loaded");
  const model = new ChatOpenAI();
  const chain = RetrievalQAChain.fromLLM(model, vectorStore.asRetriever());
  const result = await chain.call({ query });
  return result.text;
}
```

---

### 🔌 Step 4: Create Chat Route for RAG

In `server/routes/chat-doc.js`:

```js
import express from "express";
import { askDocQuestion } from "../llm/retriever.js";

const router = express.Router();

router.post("/", async (req, res) => {
  const { message } = req.body;
  if (!message) return res.status(400).json({ error: "Message required" });
  try {
    const response = await askDocQuestion(message);
    res.json({ response });
  } catch (err) {
    console.error("RAG chat error:", err);
    res.status(500).json({ error: "RAG failed" });
  }
});

export default router;
```

Add this to `index.js`:

```js
import chatDocRoute from "./routes/chat-doc.js";
app.use("/chat-doc", chatDocRoute);
```

---

### ✅ Final Flow

1. Upload PDF → Extract + embed text
2. Store in Chroma (in-memory vector DB)
3. Ask questions → Retrieve relevant chunks → LLM answers based on them

Next, we’ll polish UX with reset, typing indicators, and error boundaries 💬✨
