## gencyber-ETL (MongoDB)

This repo hosts the MongoDB instance shared by the rest of the
`gencyber-*` stack. It is the single source of truth for:

- **LangGraph checkpoints** written by `gencyber-Agent` (via the
  official `MongoDBSaver` checkpointer).

### Prereqs

Create the shared docker network once:

```bash
docker network create generative-cybersecurity-network
```

### Run

```bash
docker compose up -d
```

Mongo listens on host port `9000` by default.

### Database / collections

| Database | Collection | Owner | Purpose |
|----------|------------|-------|---------|
| `gencyber` | `checkpoints*` (managed by LangGraph `MongoDBSaver`) | `gencyber-Agent` | Per-thread workflow checkpoints used to resume + visualize the LangGraph state. |
| `gencyber` | `benchmark_runs` | *(optional / legacy)* | Reserved for custom or historical benchmark-run exports. The default **workbench + agent** stack does not require this collection. |

The database name can be overridden by setting `MONGODB_DATABASE` on
`gencyber-Agent`. The agent expects `MONGODB_URI` to point at this instance:

```bash
export MONGODB_URI="mongodb://mongouser:mongopassword@mongodb:27017/?authSource=admin"
export MONGODB_DATABASE="gencyber"
```
