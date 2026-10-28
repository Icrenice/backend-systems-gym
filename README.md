# Backend Systems Gym

A local practice app for backend architecture design. Pick a challenge, describe your solution, and receive an AI-assisted review grounded in a small, built-in set of architecture sources.

## Features

- Four rotating challenges covering event synchronization, asynchronous reports, grounded assistants, and agent memory.
- Keyword-based retrieval of up to two local architecture cards, with links to their sources.
- Reviews covering recommendations, trade-offs, failure modes, missing questions, and a next exercise.
- Local history of the five most recent reviews and basic retrieval, citation, and latency metrics.

## Requirements

- Node.js and npm compatible with the dependencies in `package-lock.json`.
- An OpenAI API key with access to the model configured in `server.js` (`gpt-6-luna`). API calls may incur charges.

## Run locally

From the project directory, install the locked dependencies:

```powershell
npm ci
```

Set your API key for the current PowerShell session and start the app:

```powershell
$env:OPENAI_API_KEY = "your-api-key"
npm start
```

Open [http://127.0.0.1:3000](http://127.0.0.1:3000). Select a challenge, enter a design of at least 30 characters, and choose **Review My Design**. The server listens only on localhost and uses port 3000.

## How it works

The Node.js HTTP server serves the single-page interface and provides challenge, review, and history endpoints. It ranks the hard-coded source cards by matching keywords against the challenge and design. When at least one card matches, it sends the challenge, design, and selected cards to the configured OpenAI model. When no card matches, it saves a fallback response without calling the model.

Reviews, including the submitted challenge and design, are appended to `reviews.jsonl` in the project directory. The interface displays the five most recent entries. Citation coverage measures the proportion of selected source identifiers that appear in the model response; it does not verify whether the citations support the claims.

## Project structure

```text
server.js          HTTP server, challenges, source cards, and review logic
public/index.html  Browser interface
reviews.jsonl      Locally saved review history
package.json       Dependencies and start script
```

## Before publishing

Review `reviews.jsonl` before adding it to Git: it can contain personal or sensitive designs and reviews. Keep API keys out of the repository. This is a local practice project; the server has no authentication and should not be exposed publicly without further security work.

## Testing

No automated test script is currently defined in `package.json`. To check the basic flow, start the app, load a challenge, submit a design, and confirm that a review and history entry appear. A model-backed review requires a working API key and access to the configured model.
