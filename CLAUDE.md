# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Start the application:**
```bash
./run.sh
```

**Manual start (alternative):**
```bash
cd backend
uv run uvicorn app:app --reload --port 8000
```

**Install dependencies:**
```bash
uv sync
```

## Project Architecture

This is a full-stack RAG (Retrieval-Augmented Generation) system for course materials with the following key components:

### Backend Structure (`backend/`)
- **`app.py`** - FastAPI main application with `/api/query` and `/api/courses` endpoints
- **`rag_system.py`** - Main orchestrator class that coordinates all components
- **`vector_store.py`** - ChromaDB integration with embedding search using sentence-transformers
- **`document_processor.py`** - Processes course documents into chunks
- **`ai_generator.py`** - Anthropic Claude API integration for response generation
- **`session_manager.py`** - Manages conversation history and sessions
- **`search_tools.py`** - Tool management system for search functionality
- **`models.py`** - Pydantic models for Course, Lesson, and CourseChunk
- **`config.py`** - Configuration management with environment variables

### Frontend (`frontend/`)
- **`script.js`** - Vanilla JavaScript client that communicates with FastAPI backend
- Static HTML interface served by FastAPI

### Key Architecture Patterns
- **Component-based design**: Each major functionality (vector search, AI generation, document processing) is separated into its own module
- **Session-based conversations**: User queries maintain context through session management
- **Tool-based search**: Uses a ToolManager system for extensible search capabilities
- **Chunked document processing**: Course materials are split into semantic chunks for vector storage

### Configuration
- Uses `uv` as package manager with Python 3.13+
- ChromaDB for vector storage (stored in `./chroma_db`)
- Anthropic API key required in `.env` file
- Documents loaded from `docs/` directory on startup

### Data Flow
1. Course documents are processed into chunks by `DocumentProcessor`
2. Chunks are embedded and stored in ChromaDB via `VectorStore`
3. User queries trigger semantic search through `ToolManager`
4. Retrieved context is sent to Claude API via `AIGenerator`
5. Response with sources is returned to frontend via FastAPI

The system automatically loads course materials from the `docs/` folder on startup and provides both course analytics and query capabilities through a web interface.
- always use uv to run the surver. do not use pip directly.
- Make sure to use uv to manage all dependencies.