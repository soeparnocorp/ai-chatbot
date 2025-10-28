# AI Chatbot Template (Astro × Prompt‑kit)

This project ships a drop-in UI for AI chat experiences using Astro, React, Tailwind v4, and prompt-kit components. It is designed to be open-source friendly: no API keys are bundled, and the default assistant response is mocked until you wire up your own backend.

## Getting Started

```bash
pnpm install
pnpm dev
```

Copy the environment template and fill in the keys you plan to use (OpenAI, Anthropic, Gemini, etc.):

```bash
cp .env.example .env.local
```

Once you have a server route (for example `/api/chat`) using the [Vercel AI SDK](https://sdk.vercel.ai/), replace the placeholder logic in `src/components/Chatbot.tsx` to POST to that endpoint and stream tokens into the message list.

## Environment Variables

The template lists the most common providers supported by the Vercel AI SDK. Only populate the ones you really need. See `.env.example` for an annotated mockup covering:

- default model configuration (`AI_PROVIDER`, `AI_MODEL`, etc.)
- provider specific keys (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `GOOGLE_GENERATIVE_AI_API_KEY`, …)
- optional self-hosted gateway overrides (`AI_BASE_URL`, `AI_API_KEY`, `AI_PROJECT_ID`)

Keep the `.env.local` file out of source control (`.gitignore` already handles this).

## Structure

```
src/
 ├─ components/
 │   ├─ Chatbot.tsx          # main UI + state
 │   ├─ ThemeToggle.tsx      # light/dark switch
 │   └─ prompt-kit/*         # prompt-kit primitives
 ├─ pages/
 │   └─ index.astro          # mounts the chat experience
 └─ styles/global.css        # Tailwind v4 setup + theme tokens
```

## Tailwind v4 + prompt-kit

Tailwind v4 is imported via the `@import "tailwindcss";` entry in `src/styles/global.css`, along with `@tailwindcss/typography` and `tw-animate-css`. The prompt-kit primitives are vendored into `src/components/prompt-kit` so that the template works without hitting the network at build time.

## Production Build

```bash
pnpm build
pnpm preview
```

Astro builds to `dist/` in static output mode. If you deploy to Vercel, drop your API route files under `src/pages/api` or `src/pages/api/chat.ts` (using `export const config = { runtime: "edge" }` for edge streaming) and Vercel will handle the rest.

## Next Steps

1. Implement an API handler using the Vercel AI SDK (see [`ai` docs](https://sdk.vercel.ai/docs/getting-started)).  
2. Replace the mocked assistant response in `Chatbot.tsx` with a fetch to that handler and stream the response into `setMessages`.  
3. Persist chat history (Supabase, KV, SQLite, etc.) so the sidebar items load real transcripts.
