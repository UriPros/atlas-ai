# Atlas AI. Fuzzy Logic Vacation Expert System

Code by Alex Vilamitjana, Sara Gibert and Oriol Pros
Group: 7

---

Description

This project implements an intelligent vacation recommendation system based on Fuzzy Logic, combined with a conversational interface developed using Streamlit.

The system interacts with the user through a natural language chat, extracts key vacation preferences, converts them into numerical values, applies fuzzy inference rules, and finally recommends the top 3 destinations that best match the user profile.

The architecture follows a Knowledge-Based System (KBS) paradigm, separating data, inference logic, and presentation layers for clarity and modularity.

---

Execution Instructions

Prerequisites

* Python 3
* Libraries:

  * `streamlit`
  * `nltk`
  * `json`

Note: Make sure to install Streamlit and NLTK before running the application. NLTK may also require downloading specific resources (e.g., tokenizers or grammars).


pip install streamlit nltk


If it doesn't work try: 

pip3 install streamlit nltk


Run the Application

From the project root directory, execute:


streamlit run app.py


If it doesn't try:

python3 -m streamlit run app.py

The chatbot interface will open in your default browser.

It might have a dark app theme and to change and make it light the following steps must be done:

1. On the top right of the screen click the 3 bottons that appear.
2. Go to setting and the choose app theme part.
3. Click it and change it to "Light"
4. You are done.

---

System Overview

The application is structured into several main components:

* Chatbot Interface
  Guides the user through a structured conversation, validating answers using natural language parsing.

* Fuzzy Logic Engine
  Converts numeric inputs into fuzzy sets, evaluates fuzzy rules, and defuzzifies results to compute destination relevance scores.

* Recommendation View
  Displays the top destination along with a ranked list of the top 3 recommendations with confidence levels.

---

Project Structure

The system is modularized into Python scripts and data files:

Main Application

* File: `app.py`
  Entry point of the project.
  Responsibilities:

  * Configures the Streamlit page
  * Manages navigation between Chat and Recommendations
  * Maintains session state
  * Handles global UI settings and layout

---

Chat & NLP Processing

* File: `chat.py` *(if present in extended version)*
  Manages the conversation:

  * Displays questions and receives answers
  * Validates responses using NLP parsing
  * Stores processed answers in session state

* File: `utils.py`
  Contains:

  * Context-Free Grammar definitions for parsing
  * Functions for extracting and converting user input into numeric values
  * Question templates

---

Fuzzy Logic Core

* File: `fuzzification.py`
  Implements trapezoidal membership functions and fuzzifies numeric input values.

* File: `rule_evaluation.py`
  Evaluates fuzzy rules to compute the degree of match between user preferences and destinations.

* File: `defuzzification.py`
  Aggregates fuzzy rule outputs and selects the **top 3 destinations**.

* File: `inference_engine.py`
  Coordinates the complete inference process:

  * Fuzzification
  * Rule evaluation
  * Defuzzification

* File: `fuzzy_sets.json`
  Defines the fuzzy sets for:

  * Months / Seasons
  * Budget
  * Weather
  * Duration
  * Distance
  * Vacation type

* File: `rules.json`
  Contains expert-defined rules mapping user preferences to recommended destinations.

---

Data Files

* File: `countries.json`
  List of destination countries available in the system.

---

How the System Works

1. The user answers a series of vacation-related questions through the chatbot.
2. Answers are parsed and converted into numerical values.
3. Inputs are fuzzified using trapezoidal membership functions.
4. Fuzzy rules are evaluated to generate destination relevance scores.
5. Scores are defuzzified and ranked.
6. The top 3 destinations are presented to the user.

---
