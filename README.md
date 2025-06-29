# pythonSynapse

This project provides a Python port of the goSynapse client. It exposes a `SynapseClient` class and a simple interactive CLI.

## Installation

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

## Running Tests

Install test dependencies and execute `pytest`:

```bash
pip install pytest
pytest
```

## Configuration

Both the CLI and the health check read configuration from environment variables.  
Place these in a `.env` file in the project root and they will be loaded automatically.

Example `.env`:

```dotenv
SYNAPSE_HOST=synapse.example.com
SYNAPSE_PORT=443
SYNAPSE_API_KEY=your-api-key
SYNAPSE_VIEW_ID=<view-guid>
```

## Launching the CLI

Execute the CLI after activating your environment and creating the `.env` file:

```bash
python scripts/storm_cli.py
```

At the `storm>` prompt type your Storm queries. Type `exit` or `quit` to leave.

Each command's output is appended to `storm_results.json`. The file now
records any text printed by the Cortex under a `"print"` key. Counts from
queries like `| count` appear here even though they don't emit nodes.
If you do not see the `"print"` list, ensure the CLI is using the local
`gosynapse` code by running `pip install -e .` or by setting
`PYTHONPATH=src` before launching the script.

## Health Check Utility

`scripts/health_check.py` verifies connectivity to your Cortex. It loads the `.env`
file automatically and requires the following variables:

```
SYNAPSE_HOST
SYNAPSE_PORT
SYNAPSE_API_KEY
SYNAPSE_VIEW_ID
```

The script checks `/api/v1/active`, executes a trivial Storm query via
`/api/v1/storm/call`, and performs a small streaming query against the
`/api/v1/storm` endpoint. If any check fails, it exits with status code `1`.

Run it after activating your environment:

```bash
python scripts/health_check.py
```
