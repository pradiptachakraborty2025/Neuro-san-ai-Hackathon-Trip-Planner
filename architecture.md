# Architecture

## What we're trying to do

Planning a trip usually means juggling several separate decisions at once — flights, where to stay, what to do, and whether all of it fits the budget. This project builds an **agentic trip-planning assistant** that takes a single natural-language request (e.g. *"Plan a 1-day trip from Bangalore for 4 people with a 5000 Rs budget"*) and returns a complete, budget-checked, day-by-day schedule — without the user having to manually research or reconcile flights, hotels, activities, and costs themselves.

## How it works: a coordinator + specialist agent network

The system is built as a **multi-agent network** using the AAOSA (Adaptive Agent-Oriented Software Architecture.) pattern, defined declaratively in HOCON config
(`registries/basic/trip_planner.hocon`) and run on the neuro-san agentic framework.

Rather than one large agent trying to do everything, the network is split into a **coordinator** and four **specialist agents**, each with a narrow, well-defined
responsibility:

| Agent | Role | Responsibility |
|---|---|---|
| **TripPlanner** | Coordinator | Gathers destination, dates/trip length, traveler count, and budget from the user. Delegates to the four specialists below, then merges their outputs into a single itinerary. |
| **FlightAgent** ("Flight Advisor") | Specialist | Suggests a plausible flight option (style, duration, price range) and booking considerations for the given route/dates. |
| **HotelAgent** ("Stay Finder") | Specialist | Suggests 1–2 accommodation options (budget to mid-range) that fit the destination, trip length, traveler count, and budget. |
| **ActivityAgent** ("Local Guide") | Specialist | Suggests sights/activities for the destination, organized so they can be spread across the trip's days, accounting for stated interests (relaxing, adventure, culture, food, etc). |
| **BudgetAgent** ("Budget Checker") | Specialist | Sums the estimated flight + hotel + activity costs against the user's stated budget and reports over/under, with cost-cutting suggestions if over. |

### Why this design (the agentic angle)

- **Separation of concerns**: each specialist agent has a single job and its own
  focused system prompt, so its behavior is easy to reason about, test, and improve
  independently — swapping out `HotelAgent`'s logic doesn't touch `FlightAgent`.
- **Coordinator-only delegation**: `TripPlanner` is explicitly instructed *not* to
  use its own knowledge for flights, hotels, or prices — it only orchestrates. This
  keeps the final answer grounded in what the specialists actually produced, rather
  than the coordinator hallucinating details on top.
- **Composable via config, not code**: the entire network — agent roles, their
  instructions, and who can call whom (`tools = [...]`) — is declared in HOCON. New
  specialists (e.g. a `VisaAgent` or `WeatherAgent`) could be added by extending the
  config, without changing the coordinator's code.
- **AAOSA call/response contract**: each specialist is wrapped with `${aaosa_call}` /
  `${aaosa_instructions}`, the shared machinery (from `aaosa_basic.hocon`) that lets
  agents call one another as callable "tools" with a consistent request/response
  shape, rather than each agent needing bespoke integration code.
- **Structured final output**: `TripPlanner` sets `structure_formats: "json"`, so the
  merged itinerary is returned in a predictable structure suitable for rendering in
  a UI or feeding downstream, not just free-form prose.

## Request flow

1. User sends a natural-language trip request to `TripPlanner`.
2. `TripPlanner` checks for missing required details (destination, dates/length,
   travelers, budget) and asks the user only for what's missing.
3. `TripPlanner` delegates in parallel/sequence to `FlightAgent`, `HotelAgent`, and
   `ActivityAgent`, each returning their specialist suggestion.
4. `TripPlanner` passes the estimated costs to `BudgetAgent`, which checks them
   against the stated total budget.
5. `TripPlanner` combines all four responses into a single day-by-day itinerary
   (Day 1, Day 2, ...) and returns it to the user.

## Tech stack

- **Framework**: neuro-san (AAOSA agent orchestration)
- **Config**: HOCON (`registries/basic/manifest.hocon`, `registries/basic/trip_planner.hocon`)
- **LLM**: [e.g. Claude / GPT-4o / whichever model is set in llm_config.hocon]
- **Language/runtime**: [e.g. Python 3.14.6]
- **Interface**: [e.g. CLI, neuro-san's built-in chat client, custom web UI]

## Known limitations

- Flight and hotel prices are LLM-estimated, not pulled from live APIs — they are realistic approximations, not bookable quotes.
- [any other limitations specific to your implementation, e.g. no persistence across sessions, no multi-currency support, etc.]
