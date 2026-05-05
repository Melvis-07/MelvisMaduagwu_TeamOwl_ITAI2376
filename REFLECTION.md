# 📝 Project Reflection — Blog-to-Podcast Agent

**Course:** ITAI2376
**Team:** Munachimso Benson, Melvis Maduagwu, Christen Robinson, Kevin Moreira

---

## What Worked Well

The core pipeline came together more smoothly than expected. Once the environment was set up and the OpenAI API key was configured, the three main steps; scraping, rewriting, and audio conversion; connected cleanly without major integration issues.

BeautifulSoup performed reliably on open, publicly accessible sites. We tested on Wikipedia and Typeface, and both returned clean, readable paragraph text that the agent could work with immediately. The scraper correctly stripped HTML tags and returned only the content that mattered.

The most pleasant surprise was how well GPT-4o-mini handled the rewriting step. From the first test run, the output already sounded natural and conversational. The system prompt we wrote;  which instructed the model to remove visual references, convert bullet points to flowing sentences, and rewrite headers as spoken transitions; produced clean podcast-ready scripts without needing multiple rounds of tuning. The before-and-after difference between raw Wikipedia text and the rewritten script was immediately obvious and compelling.

OpenAI TTS then converted those scripts into audio that sounded professional and human. The `alloy` voice in particular struck a good balance between clarity and warmth, making the output feel genuinely listenable rather than robotic.

The human checkpoint we built into the pipeline; where the agent pauses after rewriting and shows the script before converting to audio; turned out to be a practical decision. It gave us the ability to catch any rewriting issues before spending API credits on TTS, and it made the demo more interactive and easier to narrate.

---

## What Did Not Work and How We Handled It

The most immediate failure we encountered was an issue with OpenAI billing during early development, where a card was declined during the initial attempt to add credits. This temporarily blocked us from testing the paid API calls. We resolved this by troubleshooting the payment method and loading credits as soon as the billing issue was cleared.

We also encountered an issue with Medium blocking our scraper. When we attempted to scrape a Medium article URL, BeautifulSoup returned either an empty response or a login wall instead of the actual article content. Medium actively detects automated HTTP requests and prevents access to its content without authentication, which a simple requests-based scraper cannot bypass.

We handled this access issue in two ways. First, we added a character count check; if the scraped text returned fewer than 100 characters, the agent automatically detected the failure and switched to manual input mode, prompting the user to paste the article text directly. This meant the rest of the pipeline could still run even when scraping failed. Second, we shifted our testing to sites that do not block scrapers; Wikipedia and Typeface; both of which worked reliably and gave us enough content to demonstrate the full pipeline end to end.

---

## The Biggest Technical Challenge and How We Solved It

The biggest technical challenge was the Medium scraping failure, and the broader problem it represented; the agent's input layer was fragile because it depended entirely on external websites cooperating with our HTTP requests. A tool that only works on some websites is not a reliable tool.

The solution was to design the input layer defensively rather than optimistically. Instead of assuming the scraper would always succeed, we built the manual paste fallback directly into the agent's logic as a first-class option rather than an afterthought. The agent now presents the user with a clear choice at the start of every run; URL or manual paste. If the URL path fails, it catches the failure gracefully and reroutes without crashing.

This shift in thinking; from "the scraper is the tool" to "getting clean text into the agent is the goal, and scraping is just one way to do it" was the key insight that made the agent more robust. It also aligned with how real-world agents are built: you design for failure, not just the happy path.

---

## Why We Switched from Multi-Agent to Single Agent

The original project blueprint submitted at Midterm proposed a Multi-Agent Priority Briefing system that used Gmail to read emails, extract deadlines and tasks, and produce a daily briefing. We switched to the Blog-to-Podcast Agent for several interconnected reasons.

The Gmail-based approach ran into infrastructure blockers before we wrote a single line of agent code. Our school email runs on Outlook through a JW Murphy institutional policy, which restricts third-party API access and made OAuth authentication impractical. Moving to a shared personal Gmail account introduced new problems; whose account to use, how to safely share OAuth credentials across the team, and how to populate a fresh inbox with realistic test emails quickly enough to meet the project timeline.

These were not coding problems. They were infrastructure and access problems that were eating into the time we needed to actually build the agent. We made the decision to switch to a project where the input was simple, open, and required no authentication; a blog URL or pasted text, so we could spend our time on the agent logic rather than fighting API setup.

The Blog-to-Podcast Agent fully satisfies the course rubric under the Single Agent option: it uses one LLM (GPT-4o-mini) as its reasoning brain, two tools (BeautifulSoup and OpenAI TTS), and LangChain as the agent framework. The switch turned out to be the right call; it unblocked the team immediately and resulted in a cleaner, more demonstrable project.

## Potential Future Projects
In the future, Or If we had more time, we could probably make this project into an app. We would accomplish this by wrapping a user interface or conversational interface arroung the agent. The app could then be used by regular individuals on their edge devices who prefer the convinience of audio or those busy professionals who would appriciate the ability to engage with information while on the go.