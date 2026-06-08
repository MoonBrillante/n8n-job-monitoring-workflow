# n8n Job Monitoring Workflow

## Overview

**n8n Job Monitoring Workflow** is an automated workflow built with n8n to collect job posts from selected public job sources, including Jooble, RemoteOK, and Remotive.

The workflow runs on a scheduled trigger, prepares search requests, handles different request methods for each source, parses and filters job listings, removes duplicates based on existing records, and appends only new matching jobs to Google Sheets.

This project demonstrates a practical automation pipeline for job data collection, filtering, deduplication, and structured storage.

## Features

- **Scheduled automation** – Runs automatically on a configurable schedule.
- **Config-driven search setup** – Search keywords, locations, and job sources are managed from Google Sheets.
- **Multiple job source ingestion** – Collects job posts from selected public sources such as Jooble, RemoteOK, and Remotive.
- **Job parsing and filtering** – Parses raw job data and filters results based on target keywords, locations, and exclusion rules.
- **Duplicate prevention** – Compares new results against existing Google Sheet records and only appends new job posts.
- **Structured output** – Saves filtered job listings into Google Sheets for review and tracking.

## Workflow Architecture

```txt
Schedule Trigger
      ↓
Read Search Config (Google Sheets)
      ↓
Prepare Search Requests
      ↓
Source Router
      ├── Jooble requests → HTTP POST
      └── Other sources → HTTP GET
              ↓
Merge Results
      ↓
Parse and Filter Jobs
      ↓
Deduplicate New Jobs
      ↓
Append New Jobs to Google Sheets
```

The number of items processed in each step may vary depending on the search configuration, job source responses, filtering rules, and existing records in Google Sheets.

## Node Reference

| Node                     | Type                              | Description                                                                                       |
| ------------------------ | --------------------------------- | ------------------------------------------------------------------------------------------------- |
| Schedule Trigger         | Trigger                           | Starts the workflow on a configurable schedule.                                                   |
| Read Search Config       | Google Sheets                     | Loads search keywords, locations, job sources, and active status from the configuration sheet.    |
| Prepare Search Requests  | Code                              | Transforms configuration rows into structured HTTP request parameters.                            |
| Check Source Is Jooble   | IF                                | Routes requests depending on whether the source is Jooble or another selected job source.         |
| HTTP Jooble POST         | HTTP Request                      | Fetches job listings from Jooble using a POST request.                                            |
| HTTP Other Sources - GET | HTTP Request                      | Fetches job listings from other selected public job sources using GET requests.                   |
| Parse and Filter Jobs    | Code                              | Normalizes job data and filters results based on target keywords, locations, and exclusion rules. |
| Filter New Jobs Only     | Remove Duplicates / Deduplication | Keeps only job listings that have not been processed before.                                      |
| Append New Jobs to Sheet | Google Sheets                     | Appends new filtered job listings to the Google Sheets output table.                              |

## Getting Started

### Prerequisites

* n8n, either self-hosted or n8n Cloud
* Google account with Google Sheets access
* Jooble API key, if Jooble is enabled as a source
* Access to the public job sources used in the workflow, such as RemoteOK and Remotive

### Setup

1. Create or import the workflow in n8n.
2. Create a Google Sheets configuration table with the required columns.

Example configuration:

| keyword            | location | platform | active |
| ------------------ | -------- | -------- | ------ |
| frontend developer | remote   | remoteok | true   |
| react developer    | remote   | remotive | true   |
| software engineer  | Spain    | jooble   | true   |

3. Configure Google Sheets credentials in n8n.
4. Add the Jooble API key in the HTTP request configuration, if Jooble is enabled.
5. Replace the Google Sheet URLs or IDs in the workflow nodes with your own configuration sheet and output sheet.
6. Run the workflow manually to test the output.
7. Activate the workflow schedule only after confirming that the filtered results are correct.

## Configuration Reference

| Field      | Description                                   | Example                          |
| ---------- | --------------------------------------------- | -------------------------------- |
| `keyword`  | Job search keyword used to build the request. | `frontend developer`             |
| `location` | Target location or remote search area.        | `remote`, `Spain`                |
| `platform` | Job source identifier used by the workflow.   | `jooble`, `remoteok`, `remotive` |
| `active`   | Enables or disables a search row.             | `true`, `false`                  |

## Notes on Item Counts

The number of items shown in n8n may vary between executions. This depends on the search configuration, responses from each job source, filtering rules, duplicate checks, and existing records in Google Sheets.

For this reason, the workflow documentation focuses on the automation process rather than fixed item counts.

## Tech Stack

* **Automation platform**: n8n
* **Job sources**: Jooble API, RemoteOK API, Remotive API
* **Data processing**: JavaScript Code nodes in n8n
* **Storage and configuration**: Google Sheets
* **Deduplication**: n8n item filtering and job ID comparison

## License

No license specified.

