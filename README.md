# MelvisMaduagwu_TeamOwl_ITAI2376
# Blog-to-Podcast Agent

> An AI agent that converts any blog post or article into a podcast-ready mp3 audio file automatically.

---

## Team Members

| Name | Role |
|------|------|
| Mel | Developer — built the agent pipeline, tool integration, and notebook |
| Muna | Documentation & Testing — wrote project documentation and tested the agent across multiple inputs |

---

## The Problem

People today consume more content than ever, but reading long blog posts and articles requires dedicated screen time that many people simply do not have. Commuters, gym-goers, and busy professionals often want to stay informed but cannot always sit down to read. At the same time, content creators publish written material that never reaches the portion of their audience who prefer audio over text.

Converting a written article into a listenable podcast is not as simple as reading the text aloud. Blog posts are written for eyes, not ears; they contain bullet points, headers, hyperlinks, and visual references that make no sense in audio format. A direct text-to-speech conversion of raw blog text sounds robotic and hard to follow.

**Target Users:** Busy professionals, commuters, students, and content creators who want to consume or distribute written content in audio format without manual editing.

---

## Agent Option

**Single Agent** — two tools.

>Note: The original Midterm blueprint proposed a **Multi-Agent** system (Priority Briefing Agent using Gmail). We later switched to a Single Agent approach after encountering infrastructure blockers with Gmail OAuth, corporate email policy restrictions, and shared account limitations. The Single Agent with two tools helped us circumvent that.

---

## Architecture Overview

The agent uses a single GPT-4o-mini brain orchestrating two tools in sequence. First, the BeautifulSoup tool scrapes the raw text from a provided blog or article URL and strips all HTML noise, returning clean paragraph text. If scraping fails due to site restrictions, the agent falls back to a manual text paste. Second, GPT-4o-mini rewrites the cleaned text into a natural spoken script, removing all visual references, bullet points, hyperlinks, and written-only language that would sound awkward in audio. Finally, the OpenAI TTS tool converts the rewritten script into a downloadable mp3 audio file. A human checkpoint is built into the pipeline between the rewriting and audio steps so the user can review the script before committing to audio conversion.

### Architecture Diagram

![Architecture Diagram](blog_to_podcast_architecture.svg)

---

## Frameworks & Tools

| Component | Technology |
|-----------|-----------|
| Agent Framework | LangChain |
| LLM | GPT-4o-mini (OpenAI) |
| Rewriting Brain | GPT-4o-mini via OpenAI API |
| Text-to-Speech | OpenAI TTS (`tts-1` model) |
| Web Scraper | BeautifulSoup4 + Requests |
| Free Fallback LLM | Mistral-7B via HuggingFace Inference API |
| Free Fallback TTS | gTTS (Google Text-to-Speech) |
| Notebook Environment | Jupyter Notebook |
| Language | Python 3.11 |

---

## Installation

### 1. Prerequisites

- Python 3.11 installed on your machine
- Jupyter Notebook or JupyterLab installed
- An OpenAI account with API credits (minimum $5 recommended) **OR** a free HuggingFace account

### 2. Install Dependencies

Open your terminal and run:

```bash
pip install openai beautifulsoup4 requests langchain langchain-openai gtts huggingface_hub jupyter
```
Or we can install and imort the libraries within the notebook.

### 3. Set Up Environment Variables

**Option A — Enter key at runtime (recommended to us):**
The notebook will prompt you to enter your OpenAI API key when you run Cell 3. Nothing to set up in advance.

**Option B — Set it in your terminal before launching Jupyter:**
```bash
export OPENAI_API_KEY="your-api-key-here"        # Mac/Linux
set OPENAI_API_KEY="your-api-key-here"            # Windows
```

> We Learned to not hardcode our API key directly into the notebook file before submitting or sharing.

### 4. Get Your API Key

1. Go to [platform.openai.com](https://platform.openai.com) and create an account
2. Navigate to **API Keys** in the left sidebar
3. Click **Create new secret key**, name it, and copy it immediately
4. Add billing credits under **Settings → Billing** (about $5 should cover the entire project)

---

##  How to Run

### Launch the Notebook

```bash
jupyter notebook blog_to_podcast_agent.ipynb
```

### Run Order

Run the cells **top to bottom in order:**

| Cell | Action |
|------|--------|
| Cell 2 | Install all dependencies |
| Cell 3 | Select Mode A (OpenAI) or Mode B (Free) and enter API key |
| Cell 4 | Define the scraper tool |
| Cell 5 | Define the TTS tool |
| Cell 6 | Define the agent and rewriter |
| **Cell 7** | **Run the full agent pipeline** |
| Cell 8 | Inspect the full rewritten script |
| Cell 9 | Play the audio file directly in the notebook |

---

##  Example Usage

### Example 1 — Wikipedia Article (URL input)

**Input:**
```
Mode: A
Input method: 1 (URL)
URL: https://en.wikipedia.org/wiki/Artificial_intelligence
```

**Output:**
- A rewritten spoken script opening with something like *"Welcome. Today we're diving into one of the most talked-about topics of our time — artificial intelligence..."*
- All headers converted to spoken transitions, citation brackets removed, lists rewritten as flowing sentences
- A `podcast_output.mp3` file ready to listen to

---

### Example 2 — DEV.to Tech Article (URL input)

**Input:**
```
Mode: A
Input method: 1 (URL)
URL: https://dev.to/any-article-url
```

**Output:**
- Raw technical blog text rewritten into conversational audio by GPT 4-o-mini
- Code references and "click here" language removed
- Clean mp3 output with natural pacing

---

### Example 3 — Manual Paste (fallback mode)

**Input:**
```
Mode: B (Free)
Input method: 2 (Manual paste)
Text: [pasted article text from Medium or any paywalled site]
```

**Output:**
- Same rewriting pipeline runs using GPT 4-o-mini (free)
- Audio converted using OpenAI TTS
- Same voice quality compared tro the first, and fully functional
- `podcast_output.mp3` generated, able to be downloaded locally

---

## Known Limitations

**1. Some websites block scraping.**
Sites like Medium, Forbes, and most news paywalls actively block automated HTTP requests. BeautifulSoup cannot bypass login walls or JavaScript-rendered content. The manual paste fallback exists for exactly this reason; if a URL fails, the user can copy and paste the article text directly.

**2. OpenAI TTS has a 4,096 character limit per request.**
Very long articles get trimmed before audio conversion. This means the end of a long blog post may be cut off in the audio output. A future improvement would be splitting the script into chunks and stitching the audio files together.

**3. The agent does not retain memory between runs.**
Each run is completely stateless; the agent has no recollection of previously processed articles. This is intentional for this use case but means there is no history, favorites, or batch processing capability.

**4. No support for non-English content yet.**
The rewriting prompt and TTS voice are optimized for English. Non-English blog posts may produce inconsistent rewriting results and the audio output defaults to an English voice.

---

## Project Files

```
├── blog_to_podcast_agent.ipynb   # Main notebook
├── README.md                     # This file
├── architecture_diagram.png      # Architecture diagram (add your image here)
└── podcast_output.mp3            # Generated after running the agent
```

---

## Course Information

**Course:** ITAI2376  
**Institution:** Houston City College 
