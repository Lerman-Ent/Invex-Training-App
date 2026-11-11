# Invex Training App

| Field   | Value |
|---------|-------|
| Purpose | Training site for Steel Warehouse end users of the Invex ERP: Day 1 overview and setup, sales order scenarios, quizzes and a shared Process Tracker |
| Users   | Steel Warehouse end users in Invex training, and their trainers |
| Data    | Trainee progress and quiz results, and the shared Process Tracker state, saved as JSON in Azure Blob Storage through the API; the Process Tracker also looks up order releases in the Invex test environment |
| Status  | In progress (to confirm) |
| Owner   | Mark Miller, Enterprise Applications |
| URL     | TBD |

Invex Training App, used for the Invex ERP implementation. This repo holds the Steel Warehouse version.
Two related repos cover the same ground: `Lerman-Ent/LJT-Training-App` is the Lock Joint Tube version of
the same app, and `Lerman-Ent/Training` (Invex ERP Training Hub) is a later app that covers both companies.
The last commit on `main` here is from May 2026, before the Training repo was created (June 2026), and
Training is the one still being changed, so it appears to have taken over from this repo. Nothing in this
repo says so outright; confirm before building on it.

## Layout

| Path | What it is |
| --- | --- |
| `index.html` | The training site: a single self-contained page (content, images, styles and script inline) |
| `process_tracker.html` | Shared Process Tracker page |
| `api/` | Azure Functions (JavaScript): `getProgress`, `saveProgress`, `listProgress`, `getTrackerState`, `saveTrackerState` |
| `staticwebapp.config.json` | Azure Static Web Apps routing |
| `.github/workflows/` | Azure Static Web Apps deploy workflow |

## Run locally

There is no build step. Serve the repo root with any local web server (for example
`python3 -m http.server`) and open `index.html`.

Saving progress and the Process Tracker need the API. Run it with the Azure Static Web Apps CLI:

```bash
npm install -g @azure/static-web-apps-cli
cd api && npm install && cd ..
swa start . --api-location api
```

The API reads a storage connection string from the `AZURE_STORAGE_CONNECTION_STRING` setting. Keep it in
a local settings file that is not committed.

## Deploy

Pushes to `main` run the GitHub Actions workflow in `.github/workflows/`, which deploys the site
(`app_location: /`) and the API (`api_location: api`) to Azure Static Web Apps. Pull requests get a
preview environment from the same workflow.
