# Project summary

## Project name
### Trip Planner


## Problem

Planning a trip requires pulling together several independent decisions — transportation, flights, accommodation, activities, and cost — and making sure they're all consistent with each other and with a budget. Doing this manually means researching each piece separately and then manually checking whether the combined plan is even affordable. It's tedious, and small changes (a longer trip, an extra traveler, a lower budget) mean redoing the math from scratch.

## Solution

We built an **agentic trip-planning assistant** that turns a single natural-language request — for example, *"Plan a 1-day trip near Bangalore for 4 people with a 5000 Rs budget"* — into a complete, organized, budget-aware itinerary, generated in one pass by a network of cooperating AI agents rather than a single monolithic prompt.

## How it works

The system is a **multi-agent network**, built on the AAOSA (Agent-as-a-Service Oriented Architecture) pattern and run on the neuro-san agentic framework. It consists of: - **TripPlanner** (coordinator) — gathers the trip's key parameters (destination, dates or length, number of travelers, budget), and delegates the actual research to four specialist agents, then combines their answers into one itinerary.
- **Flight Advisor** — suggests a realistic flight option and considerations
  (duration, approximate price range, best time to book, layovers).
- **Stay Finder** — suggests 1–2 accommodation options across a budget-to-mid-range spread, matched to trip length, traveler count, and stated budget.
- **Local Guide** — suggests a mix of must-see sights and activities, spaced out across the days of the trip, and tailored to stated interests (e.g. relaxing, adventure, culture, food).
- **Budget Checker** — totals the estimated costs from the other three agents against the user's stated budget, and reports whether the plan fits, and if not, suggests concrete ways to cut costs.

Each agent has a single, narrow responsibility and its own instructions, which keeps the system modular: any one specialist can be revised or replaced without touching the others. The coordinator is explicitly instructed to rely only on the specialists for factual content (flights/hotels/prices) rather than its own general knowledge, which keeps the final itinerary grounded in what was actually "looked up" by the specialist agents rather than invented on the spot.

## Why an agentic approach (rather than one large prompt)

- **Modularity**: each concern (flights, hotels, activities, budget) is isolated,
  so it's easier to debug, extend, or swap out any one piece.
- **Clarity of reasoning**: because each specialist only handles its own domain, its output is easier to trust and easier to check than one agent trying to reason about flights, hotels, activities, and math simultaneously. 
- **Extensibility**: new specialists (visa requirements, weather, local transit,
  currency conversion) can be added to the network declaratively, without
  rewriting the coordinator.
- **Traceability**: because delegation and responses happen as discrete steps,
  it's possible to see exactly which agent produced which part of the final
  itinerary — useful both for debugging and for building user trust.

## Example use case

**Input**: "Suggest a weekend trip near Chennai under ₹10,000."

**Output**: A short itinerary (Day 1 / Day 2) with a nearby destination
suggestion, an estimated travel option, a place to stay, a couple of activities,
and a final budget check confirming the plan is within ₹10,000 (or a note on
where to trim if it isn't).

## What's next / possible extensions

- [add a WeatherAgent]
- [add a web front-end, connect to real flight/hotel APIs]

## Team
[Agentforce 360]
