# CrateDB Model Context Protocol Server

## About

This CrateDB MCP Server is based on the PostgreSQL Model Context Protocol (PG-MCP) Server.
[pg-mcp] uses [asyncpg], so it can also be used with CrateDB.

`server/resources/schema.py` received a few adjustments to compensate for
missing metadata features of CrateDB, nothing serious.

## Usage

Start CrateDB.
```shell
docker run --rm \
  --name=cratedb --publish=4200:4200 --publish=5432:5432 \
  --env=CRATE_HEAP_SIZE=2g crate/crate:nightly \
  -Cdiscovery.type=single-node
```

Initialize Python environment.
```shell
git clone https://github.com/crate-workbench/pg-mcp --branch=cratedb
cd pg-mcp
uv venv --python 3.13 --seed .venv
uv sync --frozen
```

Run MCP server and test program.
```shell
uv run -m server.app
uv run test.py "postgresql://crate@localhost/doc"
```

Run example Claude session (untested).
```shell
export DATABASE_URL=postgresql://crate@localhost
export ANTHROPIC_API_KEY=...
uv run -m client.claude_cli "Give me 5 Austria mountains (querying specific tables, like sys.summits)"
```


[asyncpg]: https://pypi.org/project/asyncpg
[pg-mcp]: https://github.com/stuzero/pg-mcp
