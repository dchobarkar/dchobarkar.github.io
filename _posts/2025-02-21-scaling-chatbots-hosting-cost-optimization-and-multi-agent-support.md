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

## 🌐 Hosting on Railway with Environment Isolation

Hosting your chatbot on **Railway** is a developer-friendly way to manage scalable infrastructure with built-in environment management, observability, and CI/CD. In this section, we'll deploy the chatbot we built in the previous section to Railway, configure isolated environments, manage secrets, and enable auto-deploy from GitHub.

### 🚉 Why Railway?

- **Simplicity**: Deploy full-stack apps without managing Docker or cloud config.
- **CI/CD Integration**: Auto-deploy on push to GitHub.
- **Environment Isolation**: Separate Dev, Staging, and Prod setups.
- **Secrets Management**: Encrypted environment variable support.
- **Scalability**: Railway autoscales based on usage.

### 🧪 Step-by-Step Deployment Guide

#### 1. 🛠 Prepare Your Repo

Create a GitHub repo named `chatbot-scalable-infra` and push your code:

```bash
git init
git remote add origin https://github.com/your-username/chatbot-scalable-infra.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

Ensure your `package.json` has:

```json
"scripts": {
  "start": "node dist/server.js",
  "build": "tsc"
}
```

#### 2. 🚀 Connect to Railway

1. Go to [https://railway.app](https://railway.app)
2. Click `New Project → Deploy from GitHub Repo`
3. Select your `chatbot-scalable-infra` repo
4. Set Build Command: `npm run build`
5. Set Start Command: `npm start`
6. Railway auto-detects the `server.ts` output after build

#### 3. 🔐 Add Environment Variables

Navigate to the **Variables** tab:

```env
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx
PORT=3000
```

You can define **Environment Groups** to manage shared secrets across projects.

#### 4. 🧪 Enable Multiple Environments

1. Go to project settings → Environments
2. Add new environments like `staging` and `prod`
3. Set unique env vars for each environment
4. Railway separates deployments and variables per environment

You can create environment-specific `.env.staging` or `.env.prod` locally for testing.

#### 5. ✅ Auto Deploy with GitHub Actions

Railway sets this up automatically, but you can customize it:

```yml
# .github/workflows/railway-deploy.yml
name: Deploy to Railway

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: railwayapp/railway-deploy-action@v1.2.0
        with:
          railwayToken: ${{ secrets.RAILWAY_TOKEN }}
          projectId: your_project_id_here
```

Generate the Railway token from your user settings and add it to GitHub Actions secrets.

### 🔍 Debugging Logs and Deployment Status

Railway gives you a real-time terminal view:

- Navigate to **Deployments** tab
- Click on any build to view logs
- Errors are shown clearly (build, runtime, port issues)

You can restart or redeploy from the dashboard at any time.

### 🧼 Production Hardening Tips

- **Set NODE_ENV to `production`** in env vars
- Use Railway's built-in metrics dashboard to monitor usage
- Set up custom domains under Railway’s domain tab
- Enable alerts or email notifications for failures

```bash
NODE_ENV=production
```

### 🔁 Redeploying After Changes

Every time you push to `main`, Railway will:

1. Pull changes from GitHub
2. Run `npm run build`
3. Start the app using `node dist/server.js`
4. Apply environment variables

Use `railway up` via CLI for local testing with your Railway project's environment:

```bash
npm i -g railway
railway login
railway link
railway run npm run dev
```

🎉 With this, your chatbot is now production-ready and hosted on a scalable Railway infrastructure. You can now build on this by connecting databases, Redis, or third-party APIs from Railway’s plugin marketplace!
