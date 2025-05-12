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
