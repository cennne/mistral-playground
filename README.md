# mistral-playground

## Get started

Install bun:

> https://bun.sh/docs/installation

And run the playground:

```bash
bun install
bun run dev --open
```

## Features

- Chat interface with the Mistral API
- All API options supported
- Customizable API endpoint to use your own
- History of your previous prompts
- Create Embeddings
- Can be used offline with a local endpoint
- Share generated chats

## Local endpoint

You can follow the official Mistral guide to have a local endpoint: [https://docs.mistral.ai/deployment/self-deployment/vllm/](https://docs.mistral.ai/deployment/self-deployment/vllm/).
Once your server is ready, you can set the url in the settings (e.g `http://localhost:8000` if you followed the example).

## Deploy to Vercel

To deploy this project to Vercel:

1.  **Install Vercel CLI (if you haven't already):**
   ```bash
   npm install -g vercel
   ```
   Or using bun:
   ```bash
   bun install -g vercel
   ```
2.  **Login to Vercel:**
   ```bash
   vercel login
   ```
3.  **Deploy:**
   Navigate to your project's root directory in your terminal and run:
   ```bash
   vercel
   ```
   Follow the prompts. Vercel will automatically detect it as a SvelteKit project and use the `vercel.json` configuration.

Alternatively, you can connect your Git repository (GitHub, GitLab, Bitbucket) to Vercel for automatic deployments on push.

## Resources

- [Mistral API documentation](https://docs.mistral.ai/api/)
