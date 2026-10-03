# WeGo

A co-planning companion for teachers supporting multilingual learners. Start with a classroom moment, connect it to a language purpose and domain, and shape one next move, one practice, and one thing to look for.

## GitHub Pages preview

The public Pages site runs in prepared-example mode. It does not call the AI server, send lesson notes anywhere, or save them. The editable fields stay in the browser; use **Copy plan** if you want to keep a note. The examples are planning prompts, not student ratings or official WIDA descriptors.

GitHub Pages serves the static `index.html` file. It cannot run the optional Node server or provide live AI suggestions.

## Run the sample locally

The prototype has an optional server-side OpenAI connection. The browser sends lesson context to WeGo's `/api/suggestions` route only when it is served from `localhost` or `127.0.0.1`; the Node server reads `OPENAI_API_KEY` and makes the OpenAI request. Teachers never enter or receive the API key.

Use Node.js 20 or later. From this folder, run:

```sh
node server.mjs
```

Then open `http://localhost:4173`. With no API key configured, the app stays in sample mode and uses prepared Earth-spheres, narrative, and literary-essay examples. The language-use menu includes Inform, Explain, Narrate, and Argue.

## Turn on live suggestions locally

1. Copy `.env.example` to `.env` in this folder.
2. Put your OpenAI API key after `OPENAI_API_KEY=` in `.env`.
3. Restart the server and refresh the page.

The key stays in `.env` on the server and is ignored by Git. Do not paste it into `index.html`, a browser setting, GitHub, a screenshot, or a chat message. You may instead provide `OPENAI_API_KEY` through the hosting provider's private environment-variable or secret settings.

`OPENAI_MODEL` can select the server-side model; it defaults to `gpt-5-mini`. `WEGO_AI_ENABLED=false` disables live suggestions without removing the key.

## What the AI receives

When live suggestions are enabled, WeGo sends the teacher's lesson/task note, classroom observation, grade cluster, subject, selected language use, domain, and an optional teacher-selected reference. WeGo does not save those notes. The request uses `store: false`, and the server does not log the lesson text or OpenAI response. Please keep student names and private details out of the notes.

The model returns one focused plan: a next language move, one short practice, one observable look-for, a brief reason, and an assumption for the teacher to check. The plan is prefilled in editable fields. In live mode, teachers can ask for more support, a shorter practice, or a stretch without starting over; each requested revision makes another API request and may use paid credits. In sample mode, prepared examples are labeled and are not generated from the teacher's notes.

The result distinguishes the classroom observation that the teacher entered, the selected WIDA planning lens, and an assumption that needs checking. The prototype sends the selected grade band, Key Language Use, domain, and optional WeGo reference paraphrase; it does not retrieve an official WIDA descriptor for each suggestion. WeGo wording is an original planning interpretation, not official descriptor text. The result links to the Grades 4–5 framework for teacher review and flags that other grade bands need their own source check. The optional proficiency reference is not a student rating or a required progression. For a literary essay, include the actual prompt and a recent observation; WeGo is instructed not to invent story events, quotations, or a student's interpretation.

## Before a hosted pilot

The server includes basic in-memory request limits, but this prototype does not yet have teacher sign-in or a school access gate. Keep a live deployment private until an access-control step and persistent usage limits are in place. Set spending controls in the OpenAI API project as well. A static-only host cannot run this API route; the Node server must be hosted with the page or deployed as a separate server.

For local use, the server listens only on `127.0.0.1`. A hosting provider generally needs `HOST=0.0.0.0`; only set that when deploying behind an appropriate private access gate.
