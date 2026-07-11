# [Project name] — Trip planner

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
├── registries/
│   └── trip_planner.hocon
│   └── manifest.hocon
└── .env
```

## Prerequisites

- Python [I used Python 3.14.6]
- An API key for [e.g. OpenAI / Anthropic, I used Mistral Api Key], set as an environment variable
  (see below)
- neuro-san installed (see `requirements.txt`)

I did this in Gitub Codespaces
For that you have to follow this steps.
## Setup

1. **Create a Codespace in Github**

2. **Create and activate a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```
3. **Download Neuro San Studio**
    ```bash
    pip install neuro-san-studio
   ```
5. **Install dependencies**
   ```bash
   pip install langchain-mistralai==1.1.2
   ```

6. **Set your LLM API key**

   Crete one .env file
   ```bash
   touch .env
   ```
   Set your key in this file
   ```bash
   MISTRAL_API_KEY=Write your key here
   ```
8. **Active Agent Registry**

   Each entry is "path-to-hocon-file": true/false. Setting yours to true tells neuro-san:

    "load basic/trip_planner.hocon"
    "make the top-level agent defined in it (TripPlanner) available as a callable network"

## Running it

```bash
ns run
```
Then interact with the `TripPlanner` agent via the neuro-san client UI at http://localhost:XXXX

## Example prompts to try

- "I want a trip near bangalore can you guide me."
- "I want a relaxing beach vacation in Goa for a week."
- "Suggest a weekend trip near Chennai under 10000 rupees."
- "Plan a 5-day trip to Japan for 2 people with a $3000 budget."

## Notes

- Flight and hotel prices are LLM-estimated approximations, not live bookable
  quotes.
