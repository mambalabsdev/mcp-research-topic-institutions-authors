# Research Topic Institutions and Authors MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-research-topic-institutions-authors)](https://www.npmjs.com/package/@mambalabsdev/mcp-research-topic-institutions-authors) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-research-topic-institutions-authors)](https://github.com/mambalabsdev/mcp-research-topic-institutions-authors/blob/main/LICENSE)

An MCP server for the Mamba Labs Research Topic Institutions and Authors actor on Apify. Turn a research topic into ranked institutions and authors from OpenAlex.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give a research topic as a keyword or OpenAlex topic ID. The actor groups matching OpenAlex works and returns one row per institution or author, ranked by works on the topic, with ROR, ORCID, total works, citations, h-index, and newest papers on the topic. It returns only what OpenAlex publishes, and no email or phone. A free OpenAlex key from the buyer is optional and gives that buyer a larger daily budget.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-research-topic-institutions-authors": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-research-topic-institutions-authors"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `find_research_topic_institutions_authors` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Which institutions publish the most on perovskite solar cells?"
- "Find the top authors on CRISPR base editing since 2022, with their newest papers."
- "List companies publishing on solid state batteries in Germany."

## Tool

`find_research_topic_institutions_authors`. One tool. Example arguments:

```json
{
  "topics": [
    "perovskite solar cells"
  ],
  "max_institutions": 5,
  "max_authors": 5,
  "recent_papers": 2
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `topics` | array | no | Keywords such as perovskite solar cells, or OpenAlex topic IDs such as T10247, up to 20. Each one gets its own ranked rows. |
| `row_types` | array | no | institutions, authors, or both. Default both. |
| `max_institutions` | number | no | Institution rows per topic, up to 200. Default 25. |
| `max_authors` | number | no | Author rows per topic, up to 200. Default 25. |
| `recent_papers` | number | no | Newest papers on the topic to add to each row, 0 to 10. Default 3. 0 skips them and uses far less of the OpenAlex budget. |
| `from_year` | number | no | Count works published from this year. Default 5 years before the current year. |
| `to_year` | number | no | Count works published up to this year. Default no end. |
| `countries` | array | no | Two letter codes such as US or DE. Institution rows must be in one of these. Author rows come from works with an institution in one of these, and the author may be elsewhere. |
| `institution_types` | array | no | Any of education, healthcare, company, archive, nonprofit, government, facility, funder, other. Institution rows must be one of these. Author rows come from works with such an institution. |
| `search_scope` | string | no | title_abstract (default): a keyword must be in the title or abstract. fulltext: also the full text where OpenAlex holds it, which matches more and less precisely. Ignored for topic IDs. |
| `openalex_api_key` | string | no | Optional. Your own free key from openalex.org/settings/api, with its own 1 USD a day of OpenAlex usage. Without one, the run shares a 0.10 USD daily budget with everyone on its address and keyword search can be limited. Never stored or logged. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `entity-row`, one institution or author row | $0.006 | $0.0057 | $0.0054 | $0.0051 |
| `apify-actor-start`, per run start | $0.003 | $0.003 | $0.003 | $0.003 |

Topic summaries, refused or failed topics, and refused input are free. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/research-topic-institutions-authors](https://apify.com/mambalabs/research-topic-institutions-authors)

## License

MIT. See [LICENSE](LICENSE).
