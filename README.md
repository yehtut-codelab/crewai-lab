# crewai_lab

A CrewAI agent project scaffold.

## Setup

```bash
pip install -e .
cp .env.example .env
# then edit .env and set OPENAI_API_KEY (and any other provider/tool keys)
```

## Run

```bash
python -m crewai_lab.main
```

Or, if the `crewai` CLI is installed:

```bash
crewai run
```

## Structure

- `src/crewai_lab/config/agents.yaml` — agent definitions (role, goal, backstory)
- `src/crewai_lab/config/tasks.yaml` — task definitions
- `src/crewai_lab/crew.py` — wires agents/tasks into a `Crew`
- `src/crewai_lab/tools/` — custom tools
- `src/crewai_lab/main.py` — `run`/`train`/`replay`/`test` entry points
- `knowledge/` — knowledge source files for agents
