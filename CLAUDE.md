# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pip install -e .              # install project + crewai deps
cp .env.example .env          # then set OPENAI_API_KEY (and any tool keys)

python -m crewai_lab.main     # run the crew (equivalent to `crewai run` if CLI installed)
```

There is no test suite, linter, or CI config yet — `tests/` exists but is empty.

## Architecture

This follows the standard `crewai create crew` layout: agents and tasks are defined declaratively in YAML, then wired together in code.

- `src/crewai_lab/config/agents.yaml` — agent definitions (`role`/`goal`/`backstory`), templated with `{topic}`.
- `src/crewai_lab/config/tasks.yaml` — task definitions, each bound to an agent by name; `reporting_task` writes to `report.md` via `output_file`.
- `src/crewai_lab/crew.py` — `CrewaiLab` class. Uses `@CrewBase`/`@agent`/`@task`/`@crew` decorators from `crewai.project`; each `@agent`/`@task` method pulls its config from the YAML by key (e.g. `self.agents_config["researcher"]`). The `crew()` method assembles `self.agents`/`self.tasks` (populated by the decorators) into a `Crew` with `Process.sequential`.
- `src/crewai_lab/main.py` — entry points (`run`, `train`, `replay`, `test`) that instantiate `CrewaiLab().crew()` and call the corresponding CrewAI kickoff/train/replay/test method. `inputs = {"topic": ...}` here must match the `{topic}` placeholders in the YAML.
- `src/crewai_lab/tools/custom_tool.py` — example custom tool (`BaseTool` subclass with a Pydantic `args_schema`). Add new tools here and attach them to an agent via the `tools=[...]` kwarg in `crew.py`.
- `knowledge/` — drop knowledge-source files here for agents to reference (currently empty).

To add a new agent/task: add an entry to the relevant YAML, then add a matching `@agent`/`@task` method in `crew.py` that reads `self.agents_config[...]` / `self.tasks_config[...]`.
