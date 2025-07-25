# 🧠 OpenOpenAI

**Run your own version of OpenAI's Assistants API — locally and fully customizable!**

<p align="center">
  <img alt="Example usage" src="/media/screenshot.jpg" width="600">
</p>

[![Build Status](https://github.com/transitive-bullshit/OpenOpenAI/actions/workflows/test.yml/badge.svg)](https://github.com/transitive-bullshit/OpenOpenAI/actions/workflows/test.yml)
[![MIT License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/transitive-bullshit/OpenOpenAI/blob/main/license)
[![Prettier](https://img.shields.io/badge/code_style-prettier-brightgreen.svg)](https://prettier.io)

---

## 🔍 What is OpenOpenAI?

**OpenOpenAI lets you self-host a version of OpenAI’s new Assistants API.** It works just like the official API but runs on your own server.

* Uses the **same data formats and types** as OpenAI
* Built from OpenAI’s official OpenAPI spec — so it's always up to date
* You only need to change the `baseURL` to switch between OpenAI and your local version!

✅ Works with the official OpenAI SDKs in Node.js and Python.

---

### 🧪 Quick Example (Node.js)

```ts
import OpenAI from 'openai'

const openai = new OpenAI({
  baseURL: 'http://localhost:3000' // 👈 Your local server
})

const assistant = await openai.beta.assistants.create({
  model: 'gpt-4-1106-preview',
  instructions: 'You are a helpful assistant.'
})
```

<details>
<summary>Python Example</summary>

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:3000")

assistant = client.beta.assistants.create(
    model="gpt-4-1106-preview",
    description="You are a helpful assistant."
)
```

</details>

---

## 🚀 Why Use It?

Running your own Assistants API unlocks powerful use cases:

* ✅ Use **custom models** (open source or local)
* 📂 Customizable **RAG** (Retrieval-Augmented Generation)
* 🧮 Plug in your own **code interpreter**
* 🏠 Full **self-hosting and on-premise** support
* 🧪 Safely **test GPTs and Actions** in a sandboxed environment
* 🛍️ Get ready for a potential decentralized "GPT Store"

---

## 🧱 Tech Stack

| Feature       | Tool                                                                                                  |
| ------------- | ----------------------------------------------------------------------------------------------------- |
| Database      | [Postgres](https://www.postgresql.org) via [Prisma](https://www.prisma.io)                            |
| Caching/Queue | [Redis](https://redis.io) with [BullMQ](https://bullmq.io)                                            |
| File Storage  | Any S3-compatible storage (e.g. Cloudflare R2)                                                        |
| API Server    | [Hono](https://hono.dev) + [Zod](https://github.com/honojs/middleware/tree/main/packages/zod-openapi) |
| RAG Tool      | [Dexter](https://github.com/dexaai/dexter)                                                            |
| Language      | [TypeScript](https://www.typescriptlang.org) ❤️                                                       |

---

## 🛠️ Getting Started

### 1. Prerequisites

* [Node.js](https://nodejs.org/en) `>= 18`
* [pnpm](https://pnpm.io) `>= 8`

### 2. Install Dependencies

```bash
pnpm install
```

### 3. Generate Prisma Types

```bash
pnpm generate
```

### 4. Set Up Environment Variables

```bash
cp .env.example .env
```

Update your `.env` with:

* **Postgres**: Set `DATABASE_URL` and run:

  ```bash
  npx prisma db push
  ```
* **OpenAI API key**: Required to use models
* **Redis**: Defaults work for local installs
* **S3**: Set up for file uploads (Cloudflare R2 recommended)

More detailed `.env` instructions are available in the full README above.

---

## 📦 Running the App Locally

There are two main services:

* 🖥️ API server
* 🧵 Async task runner

### Development mode:

```bash
# Terminal 1: Start API server
npx tsx src/server

# Terminal 2: Start task runner
npx tsx src/runner
```

### Production mode:

```bash
pnpm build

# Terminal 1
npx tsx dist/server

# Terminal 2
npx tsx dist/runner
```

---

## 🧪 Examples

### 🔧 Custom Function

Run an Assistant that calls a custom function (`get_weather`):

```bash
npx tsx e2e
```

Or with your local API:

```bash
OPENAI_API_BASE_URL='http://127.0.0.1:3000' npx tsx e2e
```

### 📚 Retrieval Tool

Run an Assistant that uses file-based retrieval (like RAG):

```bash
npx tsx e2e/retrieval.ts
```

Or locally:

```bash
OPENAI_API_BASE_URL='http://127.0.0.1:3000' npx tsx e2e/retrieval.ts
```

> Note: current retrieval is basic (returns whole file). Smarter chunking is coming soon!

---

## 📡 API Routes

All routes mirror OpenAI’s official ones. Here are a few examples:

```
POST   /assistants
GET    /assistants/:id
POST   /threads
GET    /threads/:id
POST   /threads/:id/runs
...
```

🔗 Visit `http://localhost:3000/openapi` to see the auto-generated OpenAPI docs.

---

## 📝 To-Do List

* [ ] Hosted demo
* [ ] Better thread/message locking
* [ ] `code_interpreter` support
* [ ] Smarter retrieval for non-text files
* [ ] Support for OpenAI-style prefix IDs
* [ ] Handle large context (truncation, etc.)

---

## 📄 License

MIT © [Travis Fischer](https://transitivebullsh.it)
If you find this project helpful, [consider sponsoring](https://github.com/sponsors/transitive-bullshit) or [follow on Twitter](https://twitter.com/transitive_bs).
