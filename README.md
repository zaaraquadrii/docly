# Docly

Docly is a Next.js application for chatting with a PDF document using Retrieval-Augmented Generation (RAG). The app indexes a PDF into Pinecone, retrieves the most relevant chunks for a user question, and answers using OpenAI models with chat history in context.

The project is configured to work with the sample paper "Attention Is All You Need" from the docs folder.

## Features

- PDF ingestion and chunking with LangChain
- Vector search using Pinecone
- OpenAI embeddings and GPT-powered responses
- Streaming chat UI built with Vercel AI SDK
- Theme toggle and polished UI using shadcn/ui and Tailwind CSS

## Tech Stack

- Next.js 14 (App Router)
- React 18 + TypeScript
- LangChain.js
- Vercel AI SDK
- OpenAI API
- Pinecone vector database
- Tailwind CSS
- shadcn/ui

## Project Structure

```bash
.
├── docs/
│   └── Attention_Is_All_You_Need.pdf
├── scripts/
│   └── seed.ts
├── src/
│   ├── app/
│   │   ├── api/chat/route.ts
│   │   ├── lib/
│   │   │   ├── langchain.ts
│   │   │   └── vector-store.ts
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   └── lib/
├── utils/
│   └── env-utils.ts
├── .env.example
├── package.json
├── next.config.mjs
├── tailwind.config.ts
├── tsconfig.json
└── README.md
```

## Prerequisites

Before running the app, make sure you have:

- Node.js 18+ and npm
- An OpenAI API key
- A Pinecone account and API key
- A Pinecone serverless index configured for the project

## Environment Setup

1. Copy the example environment file:

```bash
cp .env.example .env
```

2. Fill in the required values in `.env`:

```env
# OpenAI API key retrieved here: https://platform.openai.com/api-keys
OPENAI_API_KEY=

# Pinecone API key retrieved here: https://app.pinecone.io/organizations/keys
PINECONE_API_KEY=

# Pinecone index config
PINECONE_INDEX=index_name
PINECONE_CLOUD=aws
PINECONE_REGION=us-west-2

# Enables LangSmith (this exact names are required)
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=
```

## Pinecone Index Setup

The project seeds a Pinecone index automatically via the script in `scripts/seed.ts`.

You must create or choose an index name before running the seeding step. The script expects:

- `PINECONE_INDEX`
- `PINECONE_CLOUD`
- `PINECONE_REGION`

The index is created with dimension `256` and cosine similarity.

## Seed the Database

Install dependencies first:

```bash
npm install
```

Then run the seeding script:

```bash
npm run seed
```

This script:

- validates required env vars
- loads the PDF from `docs/Attention_Is_All_You_Need.pdf`
- splits the document into chunks
- creates an index in Pinecone if needed
- embeds the document chunks and stores them in Pinecone

## Run the Application

Start the local development server:

```bash
npm run dev
```

Then open:

```text
http://localhost:3000
```

## Notes

- The app expects the PDF file to exist in `docs/Attention_Is_All_You_Need.pdf`.
- If the Pinecone index already exists, the seeding script aborts to avoid overwriting it.
- The chat route uses a streaming LangChain chain and returns a `StreamingTextResponse`.
- The UI title is set to "Chat with: Attention is All you Need".

## Useful Commands

```bash
npm install
npm run seed
npm run dev
npm run build
npm run lint
```
