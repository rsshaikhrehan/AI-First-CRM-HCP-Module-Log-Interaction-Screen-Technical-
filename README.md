# HCP Interaction Logger

A production-ready React application with a LangGraph-powered AI assistant that controls an HCP (Healthcare Professional) interaction logging form via natural language.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     React Frontend                          │
│  ┌─────────────────────┐    ┌──────────────────────────┐  │
│  │   FormPanel         │    │   ChatPanel              │  │
│  │   (Left - Form)     │◄──►│   (Right - AI Assistant) │  │
│  │                     │    │                          │  │
│  │  • HCP Name         │    │  • Natural language      │  │
│  │  • Interaction Type │    │  • Tool call visibility  │  │
│  │  • Date/Time        │    │  • Suggestion chips      │  │
│  │  • Materials        │    │                          │  │
│  │  • Samples          │    │                          │  │
│  │  • Sentiment        │    │                          │  │
│  └─────────────────────┘    └──────────────────────────┘  │
│                           │                                 │
│                    LangGraph Agent                          │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  classify_node → extract_node → tool_node → respond │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

## 5 LangGraph Tools

| Tool | Purpose | Example Prompt |
|------|---------|---------------|
| `log_interaction` | Extract entities from natural language and fill the entire form | *"Today I met with Dr. Smith and discussed Product X efficiency. Sentiment was positive and I shared brochures."* |
| `edit_interaction` | Update only specified fields while preserving the rest | *"Sorry, the name was actually Dr. John and the sentiment was negative."* |
| `add_material` | Add shared materials to the list | *"Add brochure and clinical study as materials shared."* |
| `add_sample` | Add distributed samples with quantities | *"Add 5 samples of Product X."* |
| `generate_followups` | Generate AI-suggested next steps based on current form data | *"Generate follow-up actions for this interaction."* |

## Project Structure

```
hcp-interaction-logger/
├── public/
│   └── vite.svg
├── src/
│   ├── agents/
│   │   └── InteractionAgent.js      # LangGraph agent with 5 tools
│   ├── components/
│   │   ├── ChatPanel.jsx            # AI assistant chat UI
│   │   ├── FormPanel.jsx            # Interaction form UI
│   │   └── Header.jsx               # App header
│   ├── utils/
│   │   └── LLMExtractor.js          # Simulated LLM NER engine
│   ├── App.jsx                      # Main app component
│   ├── index.css                    # Tailwind + custom styles
│   └── main.jsx                     # Entry point
├── index.html
├── package.json
├── vite.config.js
├── tailwind.config.js
├── postcss.config.js
├── .eslintrc.cjs
├── .gitignore
└── README.md
```

## Getting Started

### Prerequisites
- Node.js 18+ and npm

### Installation

```bash
# 1. Extract the zip file
cd hcp-interaction-logger

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

The app will open at `http://localhost:3000`.

### Build for Production

```bash
npm run build
```

Output will be in the `dist/` folder.

## How to Use

1. **Type a natural language prompt** in the chat panel (right side)
2. **The AI assistant extracts entities** and fills the form on the left
3. **Updated fields pulse with a highlight** so you can see what changed
4. **Tool calls and results are visible** in the chat for transparency
5. **Use suggestion chips** at the bottom for quick prompts

### Example Workflow

```
User:  "Today I met with Dr. Smith and discussed Product X efficiency.
        The sentiment was positive and I shared brochures."

AI:    [Tool Call: log_interaction]
       ✅ Filled: hcpName: Dr. Smith
       ✅ Filled: date: 2025-07-14
       ✅ Filled: sentiment: positive
       ✅ Filled: materials: Brochure

User:  "Sorry, the name was actually Dr. John and the sentiment was negative."

AI:    [Tool Call: edit_interaction]
       ✅ Updated: hcpName -> "Dr. John"
       ✅ Updated: sentiment -> "negative"

User:  "Add 5 samples of Product X"

AI:    [Tool Call: add_sample]
       ✅ Added: 5x Product X

User:  "Generate follow-up actions"

AI:    [Tool Call: generate_followups]
       ✅ Suggestions: Schedule follow-up, Send PDF, Add to board
```

## Key Features

- **Split-screen layout** matching the original screenshot
- **LangGraph agent simulation** with visible tool calls
- **5 fully implemented tools** with entity extraction
- **Field highlighting** on updates for visual feedback
- **Suggestion chips** for common workflows
- **Responsive design** using Tailwind CSS
- **Lucide icons** throughout the UI
- **No manual form filling required** — AI controls everything

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | React 18 + Vite |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Agent | Simulated LangGraph (JavaScript) |
| LLM | Pattern-based NER engine (simulated) |

## Notes

- The LLM is simulated using regex patterns and heuristics for demonstration.
- In production, replace `LLMExtractor` with actual API calls to OpenAI, Anthropic, or a local LLM.
- The agent architecture mirrors real LangGraph: classify → extract → tool → respond.
