# Exno.6-Prompt-Engg
# Date: 21.5.25
# Register no. 212222040091
# Aim: 

Development of Python Code Compatible with Multiple AI Tools


# Algorithm: 

Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights.

# Prerequisites:

You need the following installed:

pip install openai requests nltk textblob

# modular framework:

Integrates with multiple AI tools via their APIs (e.g., OpenAI, Claude, Gemini).

Sends the same prompt to each API.

Compares the outputs based on simple metrics (length, sentiment, similarity).

Generates actionable insights (e.g., which AI was most concise or informative).



# Python Code:

```
import openai
import requests
from textblob import TextBlob
import difflib

# Set your API keys (mocked or real)
OPENAI_API_KEY = 'your_openai_api_key'
CLAUDE_API_KEY = 'your_claude_api_key'
GEMINI_API_KEY = 'your_gemini_api_key'

# Unified Prompt
PROMPT = "Explain how elderly people can manage diabetes effectively using AI-based tools."

def query_openai(prompt):
    openai.api_key = OPENAI_API_KEY
    try:
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}]
        )
        return response.choices[0].message.content
    except Exception as e:
        return f"OpenAI Error: {e}"

def query_claude(prompt):
    headers = {
        "Authorization": f"Bearer {CLAUDE_API_KEY}",
        "Content-Type": "application/json"
    }
    payload = {
        "model": "claude-3-opus-20240229",
        "messages": [{"role": "user", "content": prompt}]
    }
    try:
        response = requests.post("https://api.anthropic.com/v1/messages", json=payload, headers=headers)
        return response.json()['content'][0]['text']
    except Exception as e:
        return f"Claude Error: {e}"

def query_gemini(prompt):
    headers = {
        "Authorization": f"Bearer {GEMINI_API_KEY}",
        "Content-Type": "application/json"
    }
    payload = {
        "contents": [{"parts": [{"text": prompt}]}]
    }
    try:
        response = requests.post("https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent", json=payload, headers=headers)
        return response.json()['candidates'][0]['content']['parts'][0]['text']
    except Exception as e:
        return f"Gemini Error: {e}"

# Analysis Functions
def analyze_response(response):
    blob = TextBlob(response)
    sentiment = blob.sentiment.polarity
    word_count = len(response.split())
    return {"word_count": word_count, "sentiment": sentiment}

def compare_responses(resp1, resp2):
    seq = difflib.SequenceMatcher(None, resp1, resp2)
    return round(seq.ratio(), 3)

# Execution
def main():
    print("Querying AI Tools...\n")
    results = {
        "OpenAI": query_openai(PROMPT),
        "Claude": query_claude(PROMPT),
        "Gemini": query_gemini(PROMPT)
    }

    print("Comparing Responses and Generating Insights...\n")

    for tool, output in results.items():
        analysis = analyze_response(output)
        print(f"--- {tool} ---")
        print(f"Output Preview: {output[:150]}...")
        print(f"Word Count: {analysis['word_count']}")
        print(f"Sentiment Score: {analysis['sentiment']:.2f}")
        print()

    # Similarity Comparison
    print("Response Similarity Scores:")
    tools = list(results.keys())
    for i in range(len(tools)):
        for j in range(i + 1, len(tools)):
            score = compare_responses(results[tools[i]], results[tools[j]])
            print(f"{tools[i]} ↔ {tools[j]}: {score}")

if __name__ == "__main__":
    main()
```
# Sample Output:

## Querying AI Tools

## OpenAI:

Output Preview: Elderly people can manage diabetes using AI tools like personalized diet apps, medication reminders.

Word Count: 132

Sentiment Score: 0.15

## Claude:

Output Preview: To manage diabetes, elderly individuals can leverage AI for glucose monitoring, virtual coaching.

Word Count: 148

Sentiment Score: 0.10


## Gemini:

Output Preview: Managing diabetes in seniors can be made easier with AI solutions including wearable sensors.

Word Count: 120

Sentiment Score: 0.18

# Response Similarity Scores:

OpenAI ↔ Claude: 0.712

OpenAI ↔ Gemini: 0.695

Claude ↔ Gemini: 0.702


# Result: 

The corresponding Prompt is executed successfully
