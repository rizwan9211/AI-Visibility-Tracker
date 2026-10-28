# AIVT - AI Visibility Tracker

**Does AI recommend your brand?**

Ask "best financial service in Noida" to Gemini/ChatGPT/Perplexity - Is your brand in answer? AIVT checks.

🔗 Live Demo: https://rizwan9211.github.io/AI-Visibility-Tracker/
📹 Video: https://youtube.com/shorts/4YxKg0Wgf8c?feature=shared

### Problem
Local brands spend on Google SEO but 60% customers now ask AI. If AI doesn't mention you, you don't exist.

### Solution - Built with Appwrite
- **TablesDB**: Stores every visibility check (brand, niche, scores, competitors)
- **Functions**: Node function that calls Gemini API + OpenAI + Perplexity, does NER to detect brand mention
- **VectorsDB**: Embeds AI responses to find why competitors rank higher
- **Auth**: User login to track history
- **Sites**: Hosted

### How it works
User enters brand + niche → Function runs 3 prompts → Scores 0-100 → Shows competitors

### Tech Stack
Appwrite, Gemini API, JS, GitHub Pages

### Setup
Just open index.html - no backend needed for demo. For production, replace APPWRITE_CONFIG with your project ID.
