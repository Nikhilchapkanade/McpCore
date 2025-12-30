🚀 MCP-RAG-Nexus

Welcome to MCP-RAG-Nexus! This is a fully dockerized proof-of-concept designed to explore the power of the Model Context Protocol (MCP).

I built this project to demonstrate how a local AI Agent (using Llama 3.2) can "wake up," realize it needs external information, and autonomously query a "Knowledge Server" to get answers. It simulates a real-world scenario where an AI looks up data from tools like Google Drive or Slack without hallucinating.

🧠 How It Works

Think of this system as three friends talking in a private room (a Docker network):

The Brain (rag-agent): This is the LangChain agent. When you ask it a question, it decides if it needs to use a tool.

The Toolbox (mcp-knowledge): This acts as the "Knowledge Server." It holds mock data for "Google Drive" and "Slack" and serves it via MCP.

The Engine (ollama): This runs the actual Llama 3.2 model locally. It does the heavy lifting of processing text.

🛠️ Prerequisites

You don't need much to get this running. Just make sure you have:

Docker Desktop (This is essential as everything runs in containers).

Git (To clone the repo).

⚡ Quick Start Guide

1. Clone the Repo
First, grab the code and jump into the directory:

Bash

git clone https://github.com/Nikhilchapkanade/mcp-rag-nexus.git
cd mcp-rag-nexus
2. Build the Stack
Fire up the containers. This might take a minute or two the first time as it downloads the Python libraries and sets up the network.

Bash

docker-compose up --build

3. ⚠️ Important: Download the Brain
Don't skip this step! By default, the Ollama container starts empty. You need to pull the Llama 3.2 model inside the container.

While the containers are running, open a new terminal window and run:

Bash

docker exec -it mcp-rag-system-ollama-1 ollama pull llama3.2
(Note: If Docker complains about the name, run docker ps to double-check the Ollama container name).

🎮 Let's Test It!
Once everything is up and running (look for Uvicorn running on http://0.0.0.0:8080 in your logs), it's time to see the magic happen.

You can test it right from your browser or your terminal.

🧪 Scenario A: The "Google Drive" Search

Ask the agent about enterprise pricing. It will realize it doesn't know the answer, call the "Product Docs" tool, and give you the mock data.

Browser Link: Click here to ask about Pricing

Terminal Command:

Bash

curl "http://localhost:8080/query?q=What%20is%20the%20pricing%20for%20Enterprise"
Expected Response:

"The Enterprise plan costs $50/user/month."

🧪 Scenario B: The "Slack" Check

Ask about the dev team's progress. The agent will switch tools and search the "Slack" history.

Browser Link: Click here to ask about the Memory Leak

📂 Project Structure

Here is a quick look at how the code is organized if you want to tinker with it:

Plaintext

├── docker-compose.yml      # The blueprint that connects the 3 services
├── rag-agent/              # The "Brain"
│   ├── Dockerfile
│   ├── server.py           # FastAPI server handling your requests
│   ├── agent.py            # Where LangChain decides which tool to use
│   └── requirements.txt
└── mcp-knowledge/          # The "Toolbox"
    ├── Dockerfile
    ├── main.py             # Defines the mock Google/Slack tools via FastMCP
    └── requirements.txt
👋 Contributing
This is a playground project! Feel free to fork it, add new tools to the mcp-knowledge folder, or swap out Llama 3.2 for a different model. If you run into issues, open an issue or drop a PR!

Happy coding! 🚀
