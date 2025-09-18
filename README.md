# 🚀 Groq-Powered Conversation Magic: Task 1 & Task 2 Adventure! 🌟

[![Colab](https://img.shields.io/badge/Run%20in-Colab-brightblue?style=for-the-badge&logo=googlecolab)](https://colab.research.google.com/github/yourusername/conversation-management-assignment/blob/main/assignment.ipynb)
[![GitHub Commits](https://img.shields.io/github/commit-activity/y/yourusername/conversation-management-assignment?color=green)](https://github.com/yourusername/conversation-management-assignment/commits/main)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

<div align="center">
  <img src="https://media.giphy.com/media/3o7aDgf116I0i2eQ8k/giphy.gif" alt="Fun AI Chatbot GIF" width="300">
  <p><em>Watch our AI chat and extract secrets like a digital detective! 🕵️‍♂️</em></p>
</div>

## 🎉 Hey There, Future AI Wizard! What's This Project?

Imagine building a super-smart chatbot that remembers your chats, summarizes boring long talks, and even pulls out personal info like a pro detective—**all for free with Groq's lightning-fast AI**! This project is your ticket to that magic. It's an assignment split into two epic quests:

- **Task 1: Chat Memory Master** – Keep conversations alive without forgetting (or exploding in tokens!).
- **Task 2: PII Detective** – Spot and grab personal details from chats like name, email, and age, safely and smartly.

We started simple, hit some bumps (like chats vanishing or info getting lost), fixed them like heroes, and ended up with a beast-mode version. No fancy frameworks—just pure Python and Groq's OpenAI-style API. Run it in [Google Colab](https://colab.research.google.com) for free, and see the fireworks! 🎆

<div align="center">
  <img src="https://media.giphy.com/media/l0HlRnAWXxn0MhKLK/giphy.gif" alt="Success GIF" width="200">
</div>

---

## 🛤️ The Journey: What We Did, Bumps We Hit, and Wins We Scored

This project was like a video game—levels, bosses (bugs!), power-ups (fixes!), and epic loot (achievements!). Here's the story in simple bites.

### Level 1: Task 1 – The Chat Memory Quest 📝
**What We Did**: Built a "ConversationManager" that stores your back-and-forth chats like a diary. It chops long talks (truncation by turns, chars, or words), summarizes them to keep things snappy, and auto-summarizes every 3 chats to avoid "brain overload."

- **Cool Powers**: 
  - **Basic Mode**: Just chat, history grows forever (until you hit limits).
  - **Truncation Tricks**: Keep last 4 messages, or under 500 chars/100 words.
  - **Smart Summaries**: Hierarchical magic—split chats into chunks, summarize each, then mash into a mega-summary with tables! 📊
  - **Bonus Fun**: Semantic smarts (scores chats for relevance, drops boring jokes), and markdown visuals (bold, tables, lists) for pretty outputs.

**Bumps We Hit** (The Boss Fights! 😤):
- **Infinite Loop Trap**: Semantic scoring called the API too many times, hanging the demo. Fix: Capped it at 5 messages, added "internal" calls (no screen spam).
- **Vanishing History**: Length truncation ate everything (empty chats!). Fix: Added `min_messages=2` rule—always keep at least one user-assistant pair.
- **Stray Numbers**: Relevance scores (like "10" or "0") leaked into outputs. Fix: Hidden "internal" function for scoring, clean displays only.

**Wins We Scored** (Epic Loot! 🏆):
- **100% Requirement Check**: Full history storage, customizable truncation (turns/chars/words), periodic summaries (k=3 with replacement), 4 sample chats per demo.
- **Super Advanced**: Hierarchical summaries (no info loss!), semantic pruning (keeps only juicy bits), markdown magic (Colab shines with tables!).
- **Fun Factor**: Demos show real chats (weather, jokes, quantum physics)—try it, it's like talking to a witty robot! Outputs: Clean lengths (e.g., basic=8 messages), pretty markdown tables.

<details>
<summary>🔍 Quick Peek at Task 1 Output (Spoiler: It's Awesome!)</summary>

| Demo | What It Does | Cool Output Example |
|------|--------------|---------------------|
| Basic | No cuts, full chat | History: 8 messages, quantum table with **bold** qubits! |
| Semantic | Scores & prunes | Drops joke (low score=2), keeps quantum (score=10) |
| Length | Caps at 500 chars | Retains last 2-4, no empty disaster! |
| Periodic | Summarizes k=3 | Table: "Weather? No data. Joke? Skeletons! Capital? Paris!" |

</details>

---

### Level 2: Task 2 – The PII Detective Quest 🔍
**What We Did**: Created a JSON "blueprint" (schema) to hunt for personal info in chats—like a digital Sherlock. Uses Groq's function calling to pull out name, email, phone, location, age in neat JSON, then checks if it's legit.

- **Cool Powers**:
  - **Schema Superpower**: Required name/location, optional email/phone/age (null if missing), regex checks (e.g., email must have @domain.com).
  - **Extraction Engine**: Groq API "calls" a function to grab PII—fast and structured!
  - **Validation Guard**: `jsonschema` scans for fakes (wrong formats? Busted!).
  - **Bonus Fun**: 4 sample chats (detective cases: full info, missing age, no phone, mixed bag).

**Bumps We Hit** (Sneaky Traps! 😩):
- **Ghost Extractions**: PII vanished to `null` even when staring us in the face (e.g., email became null). Fix: Ditched pre-API masking (it confused the AI), sent original chats, beefed up prompts with "extract EVERY mention."
- **API Meltdowns**: Chat 4 bombed with JSON parse errors (400 Bad Request). Fix: Added fallback (raw completion if function call flops), exponential retry (wait longer each time), low temp for steady outputs.
- **Validation Sneak**: Passed invalid extractions as "valid" (schema allowed nulls). Fix: Detailed error reports (e.g., "Field 'email' failed pattern"), but since extraction now correct, all green!

**Wins We Scored** (Boss Down! 🎮):
- **100% Requirement Check**: Schema for 5 fields, function calling with Groq, 4 chats parsed (>3), full validation with details.
- **Super Advanced**: Privacy masking for displays (shows [EMAIL] in outputs), retry magic (handles rate limits), step-by-step prompts (AI thinks like a pro detective).
- **Fun Factor**: Outputs like a game—markdown tables, ✅ Valid badges, masked "clues" (e.g., [PHONE] hides secrets). All chats now extract perfectly: Chat 1 gets email/phone/age right!

<details>
<summary>🔍 Quick Peek at Task 2 Output (Detective Files!)</summary>

| Chat | Extracted Goods | Status |
|------|-----------------|--------|
| 1 (Full) | Name: John Doe, Email: john.doe@..., Phone: +1-..., Location: NY, Age: 28 | ✅ Valid |
| 2 (Partial) | Name: Jane Smith, Email: jane@..., Phone: 123-..., Location: London, Age: null | ✅ Valid |
| 3 (Minimal) | Name: Alice Johnson, Email: null, Phone: null, Location: Seattle, Age: null | ✅ Valid |
| 4 (Mixed) | Name: Bob Wilson, Email: bob@..., Phone: +1-..., Location: Toronto, Age: 35 | ✅ Valid |

</details>

---

## 🛠️ Tech Stack: The Hero Tools
- **Groq API**: Super-fast AI brain (free tier rocks for this!).
- **OpenAI SDK**: For chat completions and function calling (easy as pie).
- **Python Stdlib**: JSON, regex, jsonschema for validation—no extra fluff.
- **Google Colab**: Run it free, see markdown magic (tables, bold, GIFs!).
- **GitHub**: Tracks our adventure with commits (9+ levels cleared!).

No frameworks like LangChain—kept it pure and simple, just like the assignment says.

## 🚀 How to Play (Run It Yourself!)
1. **Grab the Notebook**: Click the Colab badge above or [open here](https://colab.research.google.com/github/yourusername/conversation-management-assignment/blob/main/assignment.ipynb).
2. **Plug in Power**: Replace `YOUR_GROQ_API_KEY` with your free key from [console.groq.com](https://console.groq.com).
3. **Hit Play**: Run all cells—watch chats flow, summaries pop, and PII get nabbed!
4. **Tweak & Have Fun**: Change samples, try k=5 for summaries, or add your own detective chats.

<div align="center">
  <img src="https://media.giphy.com/media/26ufnwz3wDUllfIzK/giphy.gif" alt="Level Up GIF" width="200">
  <p><em>Level complete! What's your high score? 😎</em></p>
</div>

## 📈 Achievements Unlocked
- **Boss Slayed**: 100% on both tasks—requirements nailed, criteria crushed.
- **Innovation Badge**: Advanced goodies like hierarchical summaries, semantic smarts, PII masking, and retry heroes.
- **Bug Hunter Gold**: Turned fails (vanishing history, ghost PII) into wins with smart fixes.
- **Free & Fast**: All on Groq's free tier, runs in seconds!

## 🎁 Thanks & Next Quest?
Shoutout to Groq for the speedy AI—made this project fly! Got ideas? Fork the repo and level up. Questions? Open an issue or chat me up. What's next: AI that predicts your jokes? Let's build it! 🚀

<div align="right">
  <em>Built with ❤️ by Myron Correia on September 14, 2025</em>
</div>

---

<details>
<summary>💡 Pro Tip: Commit History Explorer</summary>

Our 9 commits tell the full story—from basic code to epic fixes. Check 'em out:

| Commit # | What Happened | Emoji Vibe |
|----------|---------------|------------|
| 1 | Basic Task 1: History & summaries | 🛠️ |
| 2 | Task 1 Fixes: No more vanishing chats | 🔧 |
| 3 | Advanced Task 1: Hierarchical magic | ✨ |
| 4 | Task 1 Final: Clean & capped | ✅ |
| 5 | Initial Task 2: Schema & calling | 🔍 |
| 6 | Task 2 Fixes: Null busters | 🛡️ |
| 7 | Advanced Task 2: Masking & retries | 🕶️ |
| 8 | Task 2 Final: Perfect extraction | 🎯 |
| 9 | Polish: README glow-up | 🌟 |

</details>

