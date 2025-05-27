# Conversational AI: Building Intelligent Chatbots Across Platforms - 09: Scaling Chatbots: Hosting, Cost Optimization, and Multi-Agent Support

## 🚀 Project Setup for Scalability

Before scaling a chatbot to production-grade levels, you need a solid foundation. This starts with a modular, maintainable codebase and a structure that supports growth without collapsing under complexity. In this section, we'll scaffold a scalable chatbot server using **TypeScript + Node.js**, designed to be hosted on platforms like Railway, Vercel, or even Docker containers.

We'll use a clean monorepo-style layout (not necessarily with Nx or Turborepo unless complexity demands it), with focus on modular folders like `agents/`, `routes/`, `services/`, and `utils/`.

### 📁 Folder Structure

Here's the recommended structure for a multi-agent chatbot setup:

```structure
chatbot-scalable-infra/
├── agents/
│   ├── supportAgent.ts
│   └── salesAgent.ts
├── routes/
│   └── chatbot.route.ts
├── services/
│   ├── openai.service.ts
│   ├── memory.service.ts
│   └── agentManager.service.ts
├── utils/
│   └── logger.ts
├── app.ts
├── server.ts
├── types/
│   └── index.d.ts
├── .env
├── tsconfig.json
├── package.json
└── README.md
```

Each part of the system will be encapsulated so you can plug in or replace components (like agents or memory logic) independently.

### 🧱 Base Setup – `package.json`

Create a new Node.js + TypeScript project:

```bash
npm init -y
npm install express dotenv openai
npm install -D typescript ts-node-dev @types/node @types/express
```

Then, set up `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist",
    "baseUrl": ".",
    "paths": {
      "@agents/*": ["agents/*"],
      "@services/*": ["services/*"],
      "@utils/*": ["utils/*"]
    }
  },
  "include": ["**/*.ts"],
  "exclude": ["node_modules"]
}
```

### 🌐 Entry Point – `server.ts`

This file boots up the Express server and loads routes.

```ts
// server.ts
import express from "express";
import dotenv from "dotenv";
import chatbotRoutes from "./routes/chatbot.route";
import { logger } from "./utils/logger";

dotenv.config();

const app = express();
const port = process.env.PORT || 3000;

app.use(express.json());
app.use("/api/chat", chatbotRoutes);

app.listen(port, () => {
  logger.info(`Chatbot server running at http://localhost:${port}`);
});
```

### 🤖 Agent Example – `agents/supportAgent.ts`

Each agent handles its own prompts and logic:

```ts
// agents/supportAgent.ts
import { ChatCompletionRequestMessage } from "openai";

export const supportAgentPrompt: ChatCompletionRequestMessage[] = [
  {
    role: "system",
    content:
      "You are a friendly customer support agent. Answer questions politely and clearly.",
  },
];
```

### 📡 Route Layer – `routes/chatbot.route.ts`

```ts
// routes/chatbot.route.ts
import { Router } from "express";
import { handleChatRequest } from "../services/agentManager.service";

const router = Router();

router.post("/:agentId", handleChatRequest);

export default router;
```

### 🧠 Agent Manager – `services/agentManager.service.ts`

```ts
// services/agentManager.service.ts
import { Request, Response } from "express";
import { supportAgentPrompt } from "../agents/supportAgent";
import { askOpenAI } from "./openai.service";

const agentMap: Record<string, any> = {
  support: supportAgentPrompt,
};

export const handleChatRequest = async (req: Request, res: Response) => {
  const agentId = req.params.agentId;
  const userMessage = req.body.message;

  if (!agentMap[agentId]) {
    return res.status(404).json({ error: "Agent not found" });
  }

  const prompt = [...agentMap[agentId], { role: "user", content: userMessage }];

  try {
    const reply = await askOpenAI(prompt);
    res.json({ reply });
  } catch (err) {
    res.status(500).json({ error: "Failed to process message" });
  }
};
```

### ⚙️ OpenAI Wrapper – `services/openai.service.ts`

```ts
// services/openai.service.ts
import { Configuration, OpenAIApi, ChatCompletionRequestMessage } from "openai";

const config = new Configuration({ apiKey: process.env.OPENAI_API_KEY });
const openai = new OpenAIApi(config);

export const askOpenAI = async (messages: ChatCompletionRequestMessage[]) => {
  const response = await openai.createChatCompletion({
    model: "gpt-3.5-turbo",
    messages,
  });
  return response.data.choices[0].message?.content;
};
```

### 🛠️ Logger Utility – `utils/logger.ts`

```ts
// utils/logger.ts
export const logger = {
  info: (...args: any[]) => console.log("[INFO]", ...args),
  error: (...args: any[]) => console.error("[ERROR]", ...args),
};
```

### ✅ Running the Bot

Add the following to your `package.json`:

```json
"scripts": {
  "dev": "ts-node-dev server.ts"
}
```

Create a `.env` file:

```env
OPENAI_API_KEY=your-api-key
```

Run the dev server:

```bash
npm run dev
```

Make a POST request to `http://localhost:3000/api/chat/support` with `{ "message": "Hello!" }` and you'll get a response from the support agent 🚀
