# Conversational AI: Building Intelligent Chatbots Across Platforms - 04: Multi-Turn Conversations and Context Memory

## 🧠 Why Context Matters in Conversational AI

In the previous article, we built a basic website chatbot using OpenAI and LangChain. It could generate responses based on the user's latest input, but every message was treated in isolation. Ask it your name in one message, then refer to “me” in the next — it wouldn't remember a thing. That’s because it lacked **memory**.

This limitation becomes immediately obvious to users. In fact, it’s one of the first signs of whether a chatbot is truly intelligent or just a glorified autocomplete.

### 🤯 The Illusion of Intelligence

Modern LLMs (like GPT-4) are trained on billions of tokens. They’re surprisingly good at pattern recognition and natural language generation. But when deployed naively, they don’t _actually_ retain anything between messages. Each API call is stateless unless we explicitly build context into the request.

Without context:

```txt
User: My name is Darshan.
Bot: Nice to meet you, Darshan!
User: What's my name?
Bot: I'm not sure who you are.
```

With memory-enabled context:

```txt
User: My name is Darshan.
Bot: Nice to meet you, Darshan!
User: What's my name?
Bot: You said your name is Darshan.
```

See the difference? The second experience feels human. It feels _alive_. That's the power of memory.

### 🧩 Single-Turn vs Multi-Turn Bots

Let’s draw a clear distinction:

- **Single-turn chatbot**: Handles one message at a time with no awareness of prior interactions.
- **Multi-turn chatbot**: Maintains some form of memory or history across the session.

A multi-turn bot enables:

- Following up with “what do you mean by that?”
- Remembering and referencing names, preferences, prior answers
- Complex tasks split across multiple steps
- Personalized and emotionally coherent conversations

And these are not just UX perks — they’re essential in domains like:

- 🛒 E-commerce (“Show me what I looked at yesterday”)
- 🧾 Customer support (“My last ticket was about my order #1234”)
- 🧳 Travel and booking (“I want to go to Paris again next summer”)

### 💡 What Context _Actually_ Means for LLMs

Now let’s get technical. When we say “context” in LLM-land, we’re talking about the **input prompt**. LLMs like GPT-3.5/4 don’t have memory across API calls. If you want them to _remember_, you need to include that memory in the next prompt.

That means **you are responsible for feeding prior messages** into each interaction. LangChain and similar libraries automate this for us by managing message history, summarizing it, or injecting structured memory.

There are three main ways to provide context:

1. **Full chat history** (e.g. using `ConversationBufferMemory`)
2. **Summarized history** (e.g. using `ConversationSummaryMemory`)
3. **Structured memory** (e.g. using `EntityMemory` or vector stores)

We’ll be exploring all three in this article.

But before that, here’s a visual breakdown of how stateless vs stateful bots behave:

```plaintext
User        Stateless Bot         Stateful Bot
------------------------------------------------
Hi          → Hi!                 → Hi, welcome back!
My name is D→ Hello D!            → Hello Darshan!
Who am I?   → I don't know        → You're Darshan
```

### 🚀 What's Next

In the upcoming sections, we’ll plug memory into our existing chatbot project. No more dumb replies. We’ll teach it to remember user names, summarize prior chats, and even recall vectorized memories from Supabase — all with LangChain’s free tooling.

Let’s start by exploring the **different types of memory** we can integrate — and when to use which.

## 🧩 Types of Memory in LangChain (Free & Practical)

LangChain offers several built-in memory modules — each designed for a different flavor of "context". The best part? They're **free to use**, built into the open-source LangChain core, and work seamlessly with OpenAI’s free-tier API access (as long as you're within token limits).

Let’s go through them one by one.

### 1. 🧠 `ConversationBufferMemory`

This is the simplest memory type: it stores the **entire conversation history** as a plain buffer and injects it into the prompt with every turn.

#### ✅ Use When

- Your conversations are short
- You want full transparency into the chat history

#### ❌ Avoid When

- The conversation gets long (token limit issues)

#### Code Example (Node.js + LangChain)

```ts
import { ChatOpenAI } from "langchain/chat_models/openai";
import { ConversationChain } from "langchain/chains";
import { ConversationBufferMemory } from "langchain/memory";

const model = new ChatOpenAI({
  temperature: 0.7,
  openAIApiKey: process.env.OPENAI_API_KEY,
});

const memory = new ConversationBufferMemory();

const chain = new ConversationChain({
  llm: model,
  memory,
});

const res1 = await chain.call({ input: "Hi, I'm Darshan." });
console.log(res1);

const res2 = await chain.call({ input: "What did I just say?" });
console.log(res2);
```

👉 This will result in a conversation where the model knows the entire prior history, just like a real human would.

### 2. 🧾 `ConversationSummaryMemory`

Instead of passing the entire history, this memory keeps a **running summary**. It summarizes the conversation every few messages and passes only that to the LLM.

#### ✅ Use When

- Conversations are long or detailed
- You need to stay under token limits

#### Code Example

```ts
import { ConversationSummaryMemory } from "langchain/memory";

const memory = new ConversationSummaryMemory({
  llm: model,
});

const chain = new ConversationChain({
  llm: model,
  memory,
});
```

You can inspect the summary like this:

```ts
console.log(await memory.loadMemoryVariables());
```

This will show the auto-generated summary being passed as context.

### 3. 🧍‍♂️ `EntityMemory`

This type of memory tracks **specific entities** mentioned in the conversation — like names, places, dates, and custom terms.

#### ✅ Use When

- You need to remember structured data from the user
- Building something like a booking or assistant bot

#### Code Example

```ts
import { ConversationEntityMemory } from "langchain/memory";

const memory = new ConversationEntityMemory({
  llm: model,
  entityTypes: ["name", "location"], // optional
});

const chain = new ConversationChain({
  llm: model,
  memory,
});
```

This memory will automatically track entity mentions and reuse them when needed.

### 4. 📦 Vector Store Memory (Long-Term)

This is where things get powerful. Instead of relying on short-term memory, you can store past interactions as **embeddings** and perform semantic searches over them. Great for FAQs, past sessions, or support transcripts.

We’ll cover the full implementation in Topic 6, but here’s a preview using `Chroma` (free + local):

#### Code Preview

```ts
import { MemoryVectorStore } from "langchain/memory";
import { Chroma } from "langchain/vectorstores/chroma";

const vectorStore = await Chroma.fromTexts(
  ["User likes dark mode", "User bought sneakers last time"],
  embeddings
);

const memory = new MemoryVectorStore({
  vectorStore,
  memoryKey: "long_term_memory",
});
```

This lets you retrieve semantically similar conversations or preferences without explicitly recalling them.

### 🧠 Quick Comparison Table

| Memory Type           | Strengths                     | Weaknesses               |
| --------------------- | ----------------------------- | ------------------------ |
| `BufferMemory`        | Full history, easy to debug   | Token bloat              |
| `SummaryMemory`       | Token-efficient, readable     | Can lose nuance          |
| `EntityMemory`        | Great for structured info     | Needs prompt tuning      |
| `Vector Store Memory` | Scalable, long-term, semantic | Requires embedding setup |

### 🤔 When to Use What

```ts
if (session.isShort) use(ConversationBufferMemory);
else if (session.isLong && compressible) use(ConversationSummaryMemory);
else if (needsStructuredData) use(EntityMemory);
else if (needsPersistentRecall) use(VectorStoreMemory);
```

Up next, we’ll modify our existing website chatbot to use `ConversationBufferMemory` and see it handle back-and-forth context with ease 💬

## 💬 Integrating `ConversationBufferMemory` into Our Web Chatbot

In this section, we’ll enhance the chatbot we built in Article 3 by giving it memory — specifically, `ConversationBufferMemory` from LangChain. This will allow our chatbot to remember past messages during a session and respond with continuity. 🧠

We’ll update both the **backend** (LangChain logic) and the **frontend** (Next.js chat UI).

### 🛠 Backend: Enhancing the Chat API Route

Let’s say we already have an API route at `pages/api/chat.ts` or in an Express backend.

#### ✅ Updated Code for Chat API with Memory

```ts
// lib/chatWithMemory.ts
import { ChatOpenAI } from "langchain/chat_models/openai";
import { ConversationChain } from "langchain/chains";
import { ConversationBufferMemory } from "langchain/memory";

const memoryMap = new Map<string, ConversationChain>();

export const getChainForSession = (sessionId: string) => {
  if (memoryMap.has(sessionId)) return memoryMap.get(sessionId)!;

  const memory = new ConversationBufferMemory();

  const model = new ChatOpenAI({
    openAIApiKey: process.env.OPENAI_API_KEY!,
    temperature: 0.7,
  });

  const chain = new ConversationChain({ llm: model, memory });
  memoryMap.set(sessionId, chain);

  return chain;
};
```

Now let’s use it in the API route:

```ts
// pages/api/chat.ts
import { NextApiRequest, NextApiResponse } from "next";
import { getChainForSession } from "../../lib/chatWithMemory";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const { input, sessionId } = req.body;

  if (!input || !sessionId)
    return res.status(400).json({ error: "Missing input or sessionId" });

  try {
    const chain = getChainForSession(sessionId);
    const response = await chain.call({ input });
    res.status(200).json({ response: response.response });
  } catch (error) {
    console.error("Chat error:", error);
    res.status(500).json({ error: "Something went wrong" });
  }
}
```

🔁 Here, we’re storing one `ConversationChain` per `sessionId` in a memory map (works well for dev). For production, this can be stored in Redis, a database, or a serverless memory layer.

### 🧑‍🎤 Frontend: Keeping Track of Session ID and Chat History

On the client-side, we need to persist a `sessionId` across the chat session.

```ts
// utils/session.ts
export const getSessionId = () => {
  let id = localStorage.getItem("chat_session_id");
  if (!id) {
    id = crypto.randomUUID();
    localStorage.setItem("chat_session_id", id);
  }
  return id;
};
```

Then use this in your frontend chat component:

```ts
// components/ChatBox.tsx
import { useState, useEffect } from "react";
import { getSessionId } from "../utils/session";

const ChatBox = () => {
  const [messages, setMessages] = useState<string[]>([]);
  const [input, setInput] = useState("");
  const sessionId = getSessionId();

  const sendMessage = async () => {
    const res = await fetch("/api/chat", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ input, sessionId }),
    });

    const data = await res.json();
    setMessages((msgs) => [...msgs, `You: ${input}`, `Bot: ${data.response}`]);
    setInput("");
  };

  return (
    <div>
      <div className="chat-window">
        {messages.map((msg, idx) => (
          <p key={idx}>{msg}</p>
        ))}
      </div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={sendMessage}>Send</button>
    </div>
  );
};

export default ChatBox;
```

### 🎯 What You Achieve

With this setup:

- Each user has a unique session that remembers their past inputs
- Your backend maintains context automatically with `ConversationBufferMemory`
- The bot will now respond like:

```txt
User: Hi, I’m Darshan.
Bot: Hi Darshan! How can I help you today?
User: What’s my name?
Bot: You said your name is Darshan.
```

### 🚨 Limitations of `ConversationBufferMemory`

- The full chat history gets appended to every prompt — token limits become a concern
- No summarization or pruning built-in
- Great for short-lived sessions (like live chat), not for persistent memory

Up next, we’ll explore `SummaryMemory` — a smarter way to compress context and scale conversations without losing track 💡

## 🧾 Upgrading to `SummaryMemory`: Scalable Context Compression

As your chatbot’s conversations grow, `ConversationBufferMemory` quickly hits limitations. Every message is included in the prompt, bloating token usage and slowing down responses. That's where `ConversationSummaryMemory` comes in — it compresses past exchanges into a concise summary 🧠📄.

This section walks you through swapping out `BufferMemory` with `SummaryMemory` in your LangChain chatbot.

### 🤖 What is `ConversationSummaryMemory`?

It’s a memory class that uses the LLM to summarize the chat history periodically. Instead of dumping all past messages, it keeps a **dynamic, evolving summary** that gets injected into each prompt.

You can think of it as giving your bot the ability to **“keep notes”** instead of remembering every word.

### 🔧 Backend: Refactor to Use Summary Memory

Let’s update our memory module in `lib/chatWithMemory.ts`.

#### Step 1: Import `ConversationSummaryMemory`

```ts
import { ConversationSummaryMemory } from "langchain/memory";
```

#### Step 2: Replace Memory Creation Logic

```ts
export const getChainForSession = (sessionId: string) => {
  if (memoryMap.has(sessionId)) return memoryMap.get(sessionId)!;

  const model = new ChatOpenAI({
    openAIApiKey: process.env.OPENAI_API_KEY!,
    temperature: 0.7,
    modelName: "gpt-3.5-turbo",
  });

  const memory = new ConversationSummaryMemory({
    llm: model,
    memoryKey: "chat_history",
    returnMessages: true,
  });

  const chain = new ConversationChain({ llm: model, memory });
  memoryMap.set(sessionId, chain);
  return chain;
};
```

📌 Note: We use the same model (`gpt-3.5-turbo`) to generate summaries, so it stays within the free tier as long as you control message volume.

### 🧪 Debug: View the Evolving Summary

To see what your bot “remembers”:

```ts
const variables = await memory.loadMemoryVariables();
console.log("Summary:", variables.chat_history);
```

You’ll get output like:

```txt
"The user introduced themselves as Darshan. They asked about chatbot memory."
```

This is what gets sent as context in future prompts!

### 🧠 Prompt Inspection Example

LangChain internally injects this summary as:

```txt
Previous conversation summary:
The user introduced themselves as Darshan. They asked about chatbot memory.
Current input:
What should I use for long-term recall?
```

This is lean, efficient, and ideal for longer conversations or slower clients (like WhatsApp bots).

### ✅ Benefits

- Reduces token usage dramatically
- Makes context scalable
- Provides human-readable memory

### ⚠️ Limitations

- The summarization is lossy — details might be omitted
- Requires good prompt tuning for accuracy
- Can be biased toward early conversation turns

### 🧭 When to Use

Use `SummaryMemory` when:

- Your bot needs to support long or meandering sessions
- You want efficient, compressible state
- Token costs or latency are becoming a concern

### 🔄 Optional: Fallback to Buffer for First Few Turns

To give summaries some initial substance, you can combine both:

```ts
const hybridMemory = new BufferWindowMemory({
  k: 5,
  returnMessages: true,
  memoryKey: "chat_history",
});
```

And wrap it with summary after a few turns. We'll cover hybrid models in further topic.

Next up: let’s teach our bot how to **remember names, places, and structured info** using `EntityMemory` 🏷️
