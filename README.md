# UdaPlay - AI Game Research Agent Project

## Project Overview

UdaPlay is an AI-powered research agent for the video game industry. The project is split into two notebook-based parts:

- Part 1: build an offline RAG pipeline with ChromaDB.
- Part 2: build an agent that combines local retrieval with web search.

The final agent should answer game-related questions using local game data when possible and external search when needed.

## Project Structure

```text
project/
|-- games/                    # JSON files with game data
|-- lib/                      # Project support code
|-- Udaplay_01_project.ipynb  # Part 1: offline RAG
|-- Udaplay_02_project.ipynb  # Part 2: agent workflow
|-- Dockerfile
|-- docker-compose.yml
|-- requirements.txt
|-- .env
`-- README.md
```

## Requirements

- Docker Desktop
- Python 3.11+ if running outside Docker
- API keys in a local `.env` file

Create `.env` in the project root with the keys required by the notebooks:

```text
OPENAI_API_KEY="YOUR_KEY"
CHROMA_OPENAI_API_KEY="YOUR_KEY"
TAVILY_API_KEY="YOUR_KEY"
```

Do not commit `.env`. The Docker setup loads it at runtime with `env_file`, and `.dockerignore` prevents it from being copied into the image.

## Running in Docker

From the project directory:

```powershell
cd "D:\TOLAREZ\Software Projects\Test projects\udacity-agentic-ai-research-agent"
docker compose up -d --build
```

JupyterLab will be available at:

```text
http://127.0.0.1:8888
```

This project disables the Jupyter token/password for local Docker development, so VS Code and your browser should not ask for one.

To stop the container:

```powershell
docker compose down
```

## Running Cells in VS Code

1. Start the container with `docker compose up -d --build`.
2. Open `Udaplay_01_project.ipynb` or `Udaplay_02_project.ipynb` in VS Code.
3. Click `Select Kernel` in the top-right of the notebook.
4. Choose `Select Another Kernel`.
5. Choose `Existing Jupyter Server`.
6. Enter:

```text
http://127.0.0.1:8888
```

7. Select the Python kernel from that Jupyter server.

After that, notebook cells run inside the Docker container. Changes are saved back to this folder because the project directory is mounted into the container at `/app`.

If VS Code still asks for a password, it is probably using an old cached Jupyter connection. Reload the VS Code window and add the existing Jupyter server again with `http://127.0.0.1:8888`.

## Executing a Notebook from the Container

To run Part 1 non-interactively:

```powershell
docker compose exec udacity-agentic-ai-research-agent jupyter nbconvert --execute --to notebook --inplace Udaplay_01_project.ipynb
```

To run Part 2:

```powershell
docker compose exec udacity-agentic-ai-research-agent jupyter nbconvert --execute --to notebook --inplace Udaplay_02_project.ipynb
```

These commands execute every cell and write outputs back into the notebook file.

## Project Tasks

Part 1 focuses on offline RAG:

- Set up ChromaDB as a persistent client.
- Create a collection with suitable embedding functions.
- Process and index game data from JSON files.
- Retrieve relevant game documents for user questions.

Part 2 focuses on agent development:

- Answer questions using internal game knowledge.
- Search the web when local retrieval is not enough.
- Maintain conversation state.
- Return structured outputs.
- Store useful information for future use.

Required tools to implement:

- `retrieve_game`: search the vector database for game information.
- `evaluate_retrieval`: assess the quality of retrieved results.
- `game_web_search`: search the web for additional information.

## Testing

After completing both notebooks, test the agent with questions like:

- "When was Pokemon Gold and Silver released?"
- "Which one was the first 3D platformer Mario game?"
- "Was Mortal Kombat X released for PlayStation 5?"

## Notes

- Keep API keys in `.env`.
- Use Docker if you want the same environment for VS Code and notebook execution.
- Avoid `docker compose config` if you do not want Compose to print expanded environment values.
- Run the notebooks in order: Part 1 first, then Part 2.
