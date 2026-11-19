<p align="center">
  <img src="img/logotype_atlas.png" alt="Atlas AI" width="320">
</p>

# Atlas AI — Travel Recommendation Expert System

A knowledge-based expert system that recommends travel destinations from vague, everyday preferences like *"warm"*, *"cheap"* or *"not too far"*. Users talk to a chatbot, and the system reasons over expert rules with fuzzy logic to rank the destinations that fit them best.

Built for the **Knowledge-Based Systems** course of the BSc in Artificial Intelligence & Data Science at La Salle – Universitat Ramon Llull (2025–2026).

## Why fuzzy logic?

Travel preferences are subjective. "Cheap" or "warm" don't have a fixed threshold, and a classic rule-based system that treats them as true or false either discards good options or returns nothing. Atlas AI uses fuzzy logic so that a preference can be *partly* satisfied: a destination with mild weather still scores for a user who asked for warm weather, just less than a hot one.

## How it works

```mermaid
flowchart LR
    A[Chatbot] --> B[NLP parsing<br/>NLTK CFG]
    B --> C[Fuzzification<br/>trapezoidal sets]
    C --> D[Rule evaluation<br/>forward chaining]
    D --> E[Defuzzification<br/>mean of activations]
    E --> F[Top 3 ranking]
```

1. **Chatbot.** A Streamlit interface asks one question at a time: month, budget, weather, trip length, distance and type of trip. A sidebar shows the answers given so far and lets the user restart at any point.
2. **Natural language parsing.** Free-text answers are parsed with a context-free grammar built in NLTK, which accepts several ways of saying the same thing. If an answer can't be parsed, the chatbot asks again.
3. **Fuzzification.** Each answer is mapped to a 1–10 scale (1–12 for months) and converted into membership degrees with overlapping trapezoidal fuzzy sets, so one value can belong to two categories at once (e.g. 60% *cool*, 40% *mild*).
4. **Rule evaluation.** The inference engine applies expert-defined IF-THEN rules with forward chaining. Each rule combines one to three preferences and contributes an activation strength to every destination it recommends.
5. **Defuzzification and ranking.** The activations of each destination are averaged into a single score, and the top 3 destinations are shown with their match confidence and attributes.

## Knowledge base

The knowledge is stored in plain JSON, so destinations and rules can be added or changed without touching the inference code.

| File | Content |
|---|---|
| `fuzzy/countries.json` | Destination facts: continent, best months, budget, weather, duration, distance and trip type |
| `fuzzy/fuzzy_sets.json` | Linguistic variables and their fuzzy sets (e.g. weather: cold, cool, mild, warm) |
| `fuzzy/rules.json` | Expert IF-THEN rules linking preferences to destinations |

## Example

| Preferences | Top 3 |
|---|---|
| Summer · medium budget · warm · short trip · close · relaxing | Portugal (62.2%) · Spain (59.0%) · France (56.4%) |
| Winter · low budget · warm · long trip · far · adventure | Thailand (37.1%) · Argentina (32.6%) · Morocco (31.0%) |

Even contradictory inputs (cold weather in summer, close and cheap) still return a ranked list instead of failing, because every rule can fire partially.

## Project structure

```
app.py                      Streamlit entry point, sidebar and page navigation
nav/
  chat.py                   Chatbot view
  recommendations.py        Results view
  utils.py                  Grammar, questions and answer extraction
fuzzy/
  fuzzification.py          Trapezoidal membership functions
  rule_evaluation.py        Rule activation strengths
  inference_engine.py       Forward chaining over the rule base
  defuzzification.py        Score aggregation and ranking
  countries.json            Destination facts
  fuzzy_sets.json           Fuzzy sets
  rules.json                Expert IF-THEN rules
img/                        Logos
```

## Run it

```bash
pip install streamlit nltk
streamlit run app.py
```

Run it from the project root so the JSON files in `fuzzy/` are found.

## Limitations

- All rules have the same weight, so a key constraint like budget counts as much as trip type.
- Results depend on the hand-written rules and fuzzy sets, which reflect the designers' assumptions.
- The system doesn't learn from user feedback.

## Team

Alex Vilamitjana, Sara Gibert and Oriol Pros