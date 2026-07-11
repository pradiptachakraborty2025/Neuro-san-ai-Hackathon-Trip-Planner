# [Project name] — Agentic trip planner

An AAOSA multi-agent network (coordinator + flight/hotel/activity/budget
specialists) that turns a natural-language trip request into a complete,
budget-checked, day-by-day itinerary.

See [`architecture.md`](./architecture.md) for how the agent network is designed,
and [`summary.md`](./summary.md) for the project overview.

## Repo structure

```
.
├── README.md
├── architecture.md
├── summary.md
├── requirements.txt
├── manifest.hocon
├── registries/
│   └── trip_planner.hocon      # agent network definition (TripPlanner + 4 specialists)
└── config/
    └── llm_config.hocon        # LLM/model configuration
```

## Prerequisites

- Python [FILL IN version, e.g. 3.10+]
- An API key for [FILL IN — e.g. OpenAI / Anthropic], set as an environment variable
  (see below)
- neuro-san installed (see `requirements.txt`)

## Setup

1. **Clone the repo**
   ```bash
   git clone <your-repo-url>
   cd <your-repo-name>
   ```

2. **Create and activate a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate      # on Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set your LLM API key**
   ```bash
   export OPENAI_API_KEY="your-key-here"   # or ANTHROPIC_API_KEY, etc — match llm_config.hocon
   ```

5. **Register the agent network**
   Make sure `manifest.hocon` includes a reference to `registries/trip_planner.hocon`
   (this should already be in place).

## Running it

[FILL IN — the actual command you use to start neuro-san / your server, e.g.:]
```bash
python -m neuro_san.service.main_loop --config manifest.hocon
```

Then interact with the `TripPlanner` agent via [FILL IN — e.g. the neuro-san client
UI at http://localhost:XXXX, or a CLI command].

## Example prompts to try

- "Plan a 5-day trip to Japan for 2 people with a $3000 budget."
- "I want a relaxing beach vacation in Goa for a week."
- "Suggest a weekend trip near Chennai under 10000 rupees."

## Notes

- Flight and hotel prices are LLM-estimated approximations, not live bookable
  quotes.
- [FILL IN any other setup quirks, known issues, or notes for evaluators]
