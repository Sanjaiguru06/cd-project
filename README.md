# Compiler Phase Visualizer

A full-stack web application that visualizes all 6 phases of a C compiler for complex C programs — including nested loops, conditionals, logical operators, and formatted output.

## Features

- **6 Compiler Phases** visualized end-to-end:
  1. **Lexical Analysis** — Tokenizes C source code (keywords, operators, strings, char literals, preprocessor directives)
  2. **Syntax Analysis** — Recursive-descent parser producing a visual SVG parse tree
  3. **Semantic Analysis** — Scoped symbol table with type inference; SVG annotated tree with type chips (INT/FLOAT/STRING/CHAR)
  4. **Intermediate Code Generation (ICG)** — Three-Address Code (TAC) with sequential line numbering, labels & gotos; Quadruple, Triple, and Indirect-Triple table views
  5. **Code Optimization** — Constant folding, dead temp elimination, and goto target remapping
  6. **Target Code Generation** — Assembly-style output (MOVI, MOV, ADD, CMP, JMP, CALL, RET, HALT, etc.)

- Auto-compiles on page load with a recompile button
- Phase navigation to step through each stage
- SVG trees with kind-based node coloring and a legend

## Tech Stack

### Frontend
- React 19, React Router, Tailwind CSS, shadcn/ui (Radix UI), Axios
- Single self-contained HTML visualizer at `frontend/public/compiler_visualizer.html` (vanilla JS, no build step)

### Backend
- FastAPI (Python), MongoDB (via Motor async driver)
- Status check API at `/api/status`

## Project Structure
├── backend/
│   ├── server.py          # FastAPI app with MongoDB integration
│   └── requirements.txt   # Python dependencies
├── frontend/
│   ├── public/
│   │   └── compiler_visualizer.html  # Main visualizer (self-contained)
│   ├── src/
│   │   ├── App.js         # React entry point
│   │   └── components/ui/ # shadcn/ui components
│   └── package.json
├── tests/                 # Test suite
└── memory/PRD.md          # Product requirements

## Getting Started

### Prerequisites
- Node.js & Yarn
- Python 3.10+
- MongoDB

### Backend Setup

```bash
cd backend
pip install -r requirements.txt
# Create a .env file with:
# MONGO_URL=<your_mongo_connection_string>
# DB_NAME=<your_db_name>
# CORS_ORIGINS=http://localhost:3000
uvicorn server:main --reload
```

### Frontend Setup

```bash
cd frontend
yarn install
# Create a .env file with:
# REACT_APP_BACKEND_URL=http://localhost:8000
yarn start
```

The app will be available at `http://localhost:3000`.  
The compiler visualizer is served at `http://localhost:3000/compiler_visualizer.html` (or via the backend URL).

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/` | Health check |
| POST | `/api/status` | Create a status check entry |
| GET | `/api/status` | Retrieve all status checks |

## Supported C Syntax

The compiler handles a meaningful subset of C including:

- Preprocessor directives (`#include`, `#define`)
- Function definitions and calls (`printf`, `scanf`, and user-defined)
- Data types: `int`, `float`, `char`, `double`
- Control flow: `if/else`, `for`, `while`, `do-while`, `break`, `continue`, `return`
- Operators: arithmetic, relational, logical (`||`, `&&`, `!`), ternary, postfix (`i++`)
- Arrays and array indexing
- String and char literals
