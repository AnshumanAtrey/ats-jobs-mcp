# ATS Jobs MCP: Greenhouse, Lever, Workday, Ashby and 4 more

One remote MCP server that gives an AI agent live job listings from eight applicant tracking systems (ATS):
Greenhouse, Lever, Workday, Ashby, SmartRecruiters, Workable, Cornerstone and Deel. Together that is about 1.4 million
open jobs, read from the companies' own job boards, newest first. Each ATS is one tool, so an agent can search one of
them or all eight from Claude, Cursor, VS Code or any other MCP client.

**Server URL** (Streamable HTTP, hosted by Apify):

```
https://mcp.apify.com/?tools=anshumanatrey/ashby-jobs-search,anshumanatrey/cornerstone-jobs-search,anshumanatrey/deel-jobs-search,anshumanatrey/greenhouse-jobs-search,anshumanatrey/lever-jobs-search,anshumanatrey/smartrecruiters-jobs-search,anshumanatrey/workable-job-search,anshumanatrey/workday-job-search
```

Sign in with Apify (OAuth), or send `Authorization: Bearer <YOUR_APIFY_TOKEN>` with a token from
[Apify Console](https://console.apify.com/account/integrations). Searches bill your own Apify account per job returned,
from $0.0012 a job. Free Apify accounts get $5 of credit every month, enough for 2,000 to 4,000 jobs.

## Tools

| Tool | What it searches | Per job | With full details | Per run |
|---|---|---|---|---|
| `anshumanatrey--greenhouse-jobs-search` | 8,357 boards on boards.greenhouse.io, 210,000+ open jobs | $0.0018 | $0.0019 | $0.01 |
| `anshumanatrey--lever-jobs-search` | 2,300+ companies on jobs.lever.co, 76,000+ open jobs | $0.0022 | $0.0023 | $0.005 |
| `anshumanatrey--workday-job-search` | 1,688 employers on myworkdayjobs.com, 659,000+ open jobs | $0.0015 | $0.0018 | none |
| `anshumanatrey--ashby-jobs-search` | 3,478 companies on jobs.ashbyhq.com, 61,000+ open jobs | $0.0012 | $0.00135 | none |
| `anshumanatrey--smartrecruiters-jobs-search` | 2,900+ companies on careers.smartrecruiters.com, 222,000+ open jobs | $0.0025 | $0.0035 | none |
| `anshumanatrey--workable-job-search` | every company on apply.workable.com and jobs.workable.com, 170,000+ listings | $0.0025 | $0.0035 | $0.01 |
| `anshumanatrey--cornerstone-jobs-search` | 605 employers on csod.com, 61,000+ open jobs | $0.002 | $0.003 | none |
| `anshumanatrey--deel-jobs-search` | 159 companies on jobs.deel.com, 1,300+ open jobs | $0.0025 | $0.0035 | none |

Prices are Apify's free-plan prices: Silver plans pay 10% less and Gold plans 20% less. Coverage counts come from each
actor's Store listing and move every day. The server also carries Apify's `get-actor-run`, `get-dataset-items`,
`get-key-value-store-record` and `abort-actor-run` tools, which an agent uses to collect a long search.

## How a search works

Every tool takes the same inputs, all optional: `searchTerms` (job titles), `location`, `workType` (remote or onsite),
`postedWithin`, `employmentType` (where the ATS has job types), `excludeTerms`, `companies`, `searchDescriptions`,
`includeDetails` and `maxJobs` (100 by default). Pass `jobUrls` instead to check whether job links are still open. An
empty call returns the newest jobs on that ATS.

A call starts a run of the matching [Apify actor](https://apify.com/anshumanatrey). A narrow search answers within the
call. A keyword search across every board of a platform can take a few minutes, so the call returns a run ID and the
agent collects the jobs with `get-actor-run` and `get-dataset-items`. Measured through this URL on 2026-11-03: a
Greenhouse search for "data engineer" with `maxJobs` 3 ran 202 seconds and was charged 3 jobs plus the $0.01 run start
(run `ulCgXmY6tYEtJMIRr`).

## Connect

**Claude (web or desktop):** Settings, Connectors, Add custom connector. Paste the server URL and sign in with Apify.

**Claude Code:**

```bash
claude mcp add --transport http ats-jobs "https://mcp.apify.com/?tools=anshumanatrey/ashby-jobs-search,anshumanatrey/cornerstone-jobs-search,anshumanatrey/deel-jobs-search,anshumanatrey/greenhouse-jobs-search,anshumanatrey/lever-jobs-search,anshumanatrey/smartrecruiters-jobs-search,anshumanatrey/workable-job-search,anshumanatrey/workday-job-search" --header "Authorization: Bearer YOUR_APIFY_TOKEN"
```

**Cursor** (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "ats-jobs": {
      "url": "https://mcp.apify.com/?tools=anshumanatrey/ashby-jobs-search,anshumanatrey/cornerstone-jobs-search,anshumanatrey/deel-jobs-search,anshumanatrey/greenhouse-jobs-search,anshumanatrey/lever-jobs-search,anshumanatrey/smartrecruiters-jobs-search,anshumanatrey/workable-job-search,anshumanatrey/workday-job-search",
      "headers": { "Authorization": "Bearer YOUR_APIFY_TOKEN" }
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`, VS Code asks for the token once):

```json
{
  "servers": {
    "ats-jobs": {
      "type": "http",
      "url": "https://mcp.apify.com/?tools=anshumanatrey/ashby-jobs-search,anshumanatrey/cornerstone-jobs-search,anshumanatrey/deel-jobs-search,anshumanatrey/greenhouse-jobs-search,anshumanatrey/lever-jobs-search,anshumanatrey/smartrecruiters-jobs-search,anshumanatrey/workable-job-search,anshumanatrey/workday-job-search",
      "headers": { "Authorization": "Bearer ${input:apify-token}" }
    }
  },
  "inputs": [{ "type": "promptString", "id": "apify-token", "description": "Apify API token", "password": true }]
}
```

**Gemini CLI** (asks for the Apify token once and keeps it in the system keychain):

```bash
gemini extensions install https://github.com/AnshumanAtrey/ats-jobs-mcp
```

## Prompts to try

- Find remote data engineer jobs posted in the last 24 hours on Greenhouse and Lever.
- Which companies on Ashby are hiring product designers in London?
- List Workday jobs at Nvidia with "machine learning" in the title.
- Check which of these job links are still open: (paste links).

## Each ATS on its own

Every source is also its own MCP server, Apify actor and open-source repo:

| ATS | Apify Store | Source | MCP Registry |
|---|---|---|---|
| Greenhouse | [greenhouse-jobs-search](https://apify.com/anshumanatrey/greenhouse-jobs-search) | [repo](https://github.com/AnshumanAtrey/greenhouse-jobs-search) | `io.github.AnshumanAtrey/greenhouse-jobs-search` |
| Lever | [lever-jobs-search](https://apify.com/anshumanatrey/lever-jobs-search) | [repo](https://github.com/AnshumanAtrey/lever-jobs-search) | `io.github.AnshumanAtrey/lever-jobs-search` |
| Workday | [workday-job-search](https://apify.com/anshumanatrey/workday-job-search) | [repo](https://github.com/AnshumanAtrey/workday-job-search) | `io.github.AnshumanAtrey/workday-job-search` |
| Ashby | [ashby-jobs-search](https://apify.com/anshumanatrey/ashby-jobs-search) | [repo](https://github.com/AnshumanAtrey/ashby-jobs-search) | `io.github.AnshumanAtrey/ashby-jobs-search` |
| SmartRecruiters | [smartrecruiters-jobs-search](https://apify.com/anshumanatrey/smartrecruiters-jobs-search) | [repo](https://github.com/AnshumanAtrey/smartrecruiters-jobs-search) | `io.github.AnshumanAtrey/smartrecruiters-jobs-search` |
| Workable | [workable-job-search](https://apify.com/anshumanatrey/workable-job-search) | [repo](https://github.com/AnshumanAtrey/workable-job-search) | `io.github.AnshumanAtrey/workable-job-search` |
| Cornerstone | [cornerstone-jobs-search](https://apify.com/anshumanatrey/cornerstone-jobs-search) | [repo](https://github.com/AnshumanAtrey/cornerstone-jobs-search) | `io.github.AnshumanAtrey/cornerstone-jobs-search` |
| Deel | [deel-jobs-search](https://apify.com/anshumanatrey/deel-jobs-search) | [repo](https://github.com/AnshumanAtrey/deel-jobs-search) | `io.github.AnshumanAtrey/deel-jobs-search` |

For a single ATS, use `https://mcp.apify.com/?tools=anshumanatrey/<actor>`. All eight run on the open-source
[ats-jobs-engine](https://github.com/AnshumanAtrey/ats-jobs-engine), and setup pages for every server are at
[actorly.atrey.dev/mcp](https://actorly.atrey.dev/mcp).

## About

Listed in the official [MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.AnshumanAtrey/ats-jobs-mcp)
as `io.github.AnshumanAtrey/ats-jobs-mcp`; the workflow in `.github/workflows/mcp-registry.yml` publishes `server.json`
there. The actors read public job boards, the pages any visitor sees. Not affiliated with or endorsed by Greenhouse,
Lever, Workday, Ashby, SmartRecruiters, Workable, Cornerstone, Deel or Apify.

Made by [Anshuman Atrey](https://atrey.dev). MIT license.
