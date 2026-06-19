# DatoVero

An all-in-one, AI-powered operating system for small nonprofits — client intake, inventory, volunteers, donors, time tracking, financials, and reporting in a single platform instead of five disconnected tools.

## The problem

Small nonprofits run their operations across a patchwork of spreadsheets, paper logs, and single-purpose apps that don't talk to each other. The cost shows up at reporting time: pulling a single board report meant manually stitching numbers together from every one of those sources — a process that took **10+ hours** a month. Staff time that should go to the mission goes to data entry and reconciliation instead.

## What it does

DatoVero consolidates the core functions a nonprofit needs to operate into one system:

- **Client intake & case management** — profiles, intake forms, program enrollment, service records
- **Inventory** — cataloging, categories, conditions, and checkout workflows
- **Volunteer tracking** — scheduling, hour logging, and verification
- **Time clock** — staff clock-in/out and timesheet workflows
- **Donors, grants & financials** — giving history, grant pipelines, budgets
- **AI reporting** — board-ready reports generated and narrated automatically
- **Org Brain** — an AI agent that reads both the organization's live data and its own written notes to answer questions and track goals

## How it works

The frontend is a React 19 + TypeScript single-page app (Vite, Tailwind CSS) hosted on Vercel. The backend runs on Supabase: a PostgreSQL database with authentication, serverless edge functions, and real-time updates so data stays live across users. Each organization's data is isolated at the database layer, and row-level security enforces who can see what.

The AI layer is built on the Anthropic Claude API, accessed only through a server-side proxy that validates every request. Two designs matter most:

- **AI reporting** runs two ways: standard reports are generated from preset queries, while ad-hoc requests have Claude generate the SQL itself. In both cases the backend executes the query under row-level security — so the model never has direct database access — and Claude turns the returned rows into a written narrative of trends, risks, and recommendations rather than just a table of numbers.
- **The Org Brain** is a retrieval-over-org-data agent: it pulls together the organization's structured records *and* its written knowledge (goals, processes, meeting notes), retrieves the pieces relevant to a question, and answers in the organization's own context — something a generic chatbot can't do because it has no view of the org's data.

## Impact

DatoVero grew out of a system I built for **Dress for Success NWA**, a Northwest Arkansas nonprofit. That deployment is still in daily production:

- Board report preparation dropped from **10+ hours a month to 1–2 minutes**.
- Roughly **10 staff** use it daily across 16 active modules.
- It continues to run the organization day-to-day, and it's what inspired me to build DatoVero as a startup — now in active development.

## What I'd do differently / what I learned

Building for one real organization taught me that adoption lives and dies on simplicity: features mean nothing if a non-technical staff member can't find them, so I cut and streamlined far more than I added. I also learned to trace every number back to the exact query that produced it: early reports read an imported record's date as a "joined" date when it was really just the date the data was imported, and the AI confidently summarized figures that didn't mean what I assumed they did. The data was never wrong — my understanding of what it represented was, and nothing caught that until I checked each value against its source. And as a solo builder, I underestimated how much breadth costs; I'd now ship a smaller, sharper core and let real usage pull the next feature rather than building ahead of demand.
