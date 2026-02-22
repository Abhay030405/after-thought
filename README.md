Afterthought
===========

Long-term memory operating system for AI coding agents. This is
AI infrastructure: a shared developer memory server that persists
project knowledge and injects it into LLM prompts so agents behave like
continuity-aware teammates.

At a Glance
-----------
- Problem: LLMs forget project context between sessions.
- Solution: Persistent, queryable memory for architecture, decisions,
  bugs, and standards.
- Outcome: Agents answer with project-aware, consistent code.

What This Builds
----------------
Afterthought sits between the developer, the IDE, and the LLM. The LLM
asks the memory server what it already knows, receives enriched context,
then generates a project-aware answer.

Key idea: this is Retrieval-Augmented Tool-Using Agents with hybrid
memory (semantic + structural), not just a vector database.

High-Level Flow
---------------
Developer prompt in IDE
  -> IDE Extension
  -> MCP Client
  -> Afterthought Memory Server
  -> Hybrid Retrieval Engine
	  -> Vector Search (semantic)
	  -> Graph Reasoning (structural)
	  -> Ranking
	  -> Context Builder
  -> LLM (with enriched prompt)

Core Subsystems
---------------
1) Memory Ingestion Engine
	- Converts developer activity into structured knowledge.
	- Inputs: prompts, code, commits, docs.
	- Uses LLM extraction + schemas to produce entities and relations.

2) Embedding and Vector Memory
	- Stores semantic meaning for similarity search.
	- Example: "authentication module" ~= "login system".

3) Knowledge Graph Memory
	- Stores relationships (dependencies, causes, decisions).
	- Enables reasoning beyond text similarity.

4) Hybrid Retrieval Engine
	- Combines vector search and graph traversal.
	- Ranks and builds the final LLM context payload.

5) MCP Server (Tool Layer)
	- Exposes tools like remember(), recall(), project_summary(),
	  get_dependencies(), bug_history().
	- Allows LLMs to call memory like an API.

6) IDE Extension
	- Captures prompts, calls memory server, injects context into the
	  LLM prompt in the editor.

Data Flow Example
-----------------
User prompt: "Add authentication middleware"

1) Extension intercepts prompt and queries memory server.
2) Vector search finds prior JWT decisions and related notes.
3) Graph traversal finds auth dependencies (user DB, token policies).
4) Context builder assembles a concise, ranked memory pack.
5) LLM receives enriched prompt and writes project-correct code.

Technology Stack
----------------
Backend
- Python
- FastAPI
- Pydantic

AI and ML
- SentenceTransformers
- BGE embedding model (or similar)
- Optional: OpenAI API or local LLM for extraction

Databases
- Qdrant (vector search)
- Neo4j (knowledge graph)

Agent Protocol
- MCP (Model Context Protocol)
- JSON-RPC
- SSE streaming

Dev Tools
- Docker
- Docker Compose
- GitPython
- Tree-sitter

Integration
- VSCode Extension (TypeScript)
- Node.js

Repository Layout
-----------------
server/                 MCP server and session management
memory_core/            Long-term memory core (entities, graph, vectors)
ingestion_pipeline/     Memory creation from prompts, code, docs, git
retrieval_engine/       Hybrid retrieval and context building
mcp_tools/              Tool functions exposed to LLMs
api_gateways/           REST API and SSE access layer
ide_extensions/         VSCode extension integration
sdk/                    Client SDKs for agents

Project Goals
-------------
- Persistent agent memory across sessions.
- Hybrid memory: semantic search + structural reasoning.
- Tool-using agents that query memory before responding.
- IDE-first integration that improves real developer workflows.

Status
------
This repository implements the system architecture and scaffolding for
the Afterthought memory server and its IDE integration.
