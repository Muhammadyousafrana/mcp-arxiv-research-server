[![Deploy to GitHub Pages](https://github.com/Muhammadyousafrana/mcp-arxiv-research-server/actions/workflows/main.yml/badge.svg)](https://github.com/Muhammadyousafrana/mcp-arxiv-research-server/actions/workflows/main.yml)
# mcp-arxiv-research-server

A custom MCP (Model Context Protocol) server that searches arXiv, stores research papers by topic, and exposes tools and resources for structured academic discovery and analysis.

## 📋 Features

- **🔍 Search arXiv Papers**: Search for academic papers on arXiv based on topics
- **📚 Store Paper Information**: Automatically organize and store paper metadata by topic
- **🔧 MCP Tools**: Expose search and extraction tools via MCP protocol
- **📊 MCP Resources**: Access stored papers through structured resources
- **💡 Prompt Templates**: Pre-built prompts for research assistance

### Available Tools

1. **search_papers**: Search for papers on arXiv and store their information
2. **extract_info**: Retrieve information about a specific paper by ID

### Available Resources

1. **papers://folders**: List all available topic folders
2. **papers://{topic}**: Get detailed information about papers on a specific topic

## 🚀 Installation

### Prerequisites

- Python 3.12 or higher
- pip or uv package manager

### Option 1: Using pip (Recommended for Production)

```bash
# Clone the repository
git clone https://github.com/Muhammadyousafrana/mcp-arxiv-research-server.git
cd mcp-arxiv-research-server

# Create a virtual environment
python -m venv venv

# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Option 2: Using uv (Faster Alternative)

```bash
# Clone the repository
git clone https://github.com/Muhammadyousafrana/mcp-arxiv-research-server.git
cd mcp-arxiv-research-server

# Install uv if you haven't already
pip install uv

# Create virtual environment and install dependencies
uv venv
uv pip install -e .
```

## 🔄 Converting Dependencies from uv to pip

If you're using `uv` and want to convert your project to use `pip` instead, follow these steps:

### Step 1: Generate requirements.txt from pyproject.toml

The project uses `pyproject.toml` to define dependencies. To convert to a `requirements.txt` file:

```bash
# Install uv if not already installed
pip install uv

# Compile pyproject.toml to requirements.txt
uv pip compile pyproject.toml -o requirements.txt
```

This command will:
- Read the dependencies from `pyproject.toml`
- Resolve all dependencies and their versions
- Create a pinned `requirements.txt` file with all dependencies

### Step 2: Understanding runtime.txt

The `runtime.txt` file specifies the Python version for deployment platforms (like Heroku, Render, etc.):

```text
python-3.12.3
```

**Important Notes about runtime.txt:**

1. **Purpose**: Tells deployment platforms which Python version to use
2. **Format**: Must be in the format `python-X.Y.Z` (e.g., `python-3.12.3`)
3. **Location**: Place in the root directory of your project
4. **Compatibility**: Ensure the version matches your `pyproject.toml` requirement (`>=3.12`)

### Step 3: Using pip with requirements.txt

Once you have `requirements.txt`, you can install dependencies using standard pip:

```bash
# Create a virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # On macOS/Linux
# or
venv\Scripts\activate  # On Windows

# Install from requirements.txt
pip install -r requirements.txt
```

### Step 4: Keeping Dependencies in Sync

When you add new dependencies:

**Option A: Update pyproject.toml first (Recommended)**
```bash
# 1. Edit pyproject.toml and add your dependency
# 2. Regenerate requirements.txt
uv pip compile pyproject.toml -o requirements.txt

# 3. Install the new dependencies
pip install -r requirements.txt
```

**Option B: Direct pip install**
```bash
# Install new package
pip install package-name

# Update requirements.txt
pip freeze > requirements.txt
```

## 📦 Project Structure

```
mcp-arxiv-research-server/
├── research_server.py      # Main MCP server implementation
├── main.py                 # Entry point
├── pyproject.toml          # Project configuration and dependencies
├── requirements.txt        # Pip-compatible dependency list
├── runtime.txt             # Python version specification
├── uv.lock                 # uv lock file (if using uv)
├── .python-version         # Python version for pyenv
├── papers/                 # Directory for stored papers (created at runtime)
│   └── {topic}/
│       └── papers_info.json
├── .gitignore
├── LICENSE
└── README.md
```

## 🎯 Usage

### Starting the Server

```bash
python research_server.py
```

The server will start on port 8001 using SSE (Server-Sent Events) transport.

### Example: Searching for Papers

```python
# The MCP server exposes the following tool
search_papers(topic="machine learning", max_results=5)
```

This will:
1. Search arXiv for papers on "machine learning"
2. Store metadata in `papers/machine_learning/papers_info.json`
3. Return a list of paper IDs

### Example: Extracting Paper Information

```python
# Extract information about a specific paper
extract_info(paper_id="2301.12345")
```

### Example: Accessing Resources

Access stored papers through MCP resources:
- `papers://folders` - List all available topics
- `papers://machine_learning` - Get all papers on machine learning

## 🔧 Configuration

### Dependencies

The project has two main dependencies defined in `pyproject.toml`:

```toml
[project]
name = "mcp-arxiv-research-server"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "arxiv>=2.4.0",
    "mcp>=1.26.0",
]
```

### Python Version

- **Minimum Required**: Python 3.12
- **Runtime Version**: Python 3.12.3 (as specified in `runtime.txt`)
- **Specified in**: 
  - `pyproject.toml`: `requires-python = ">=3.12"`
  - `runtime.txt`: `python-3.12.3`
  - `.python-version`: `3.12` (for pyenv users)

## 📝 Data Storage

Papers are stored in the following structure:

```
papers/
└── {topic_name}/
    └── papers_info.json
```

Each `papers_info.json` contains:
```json
{
  "paper_id": {
    "title": "Paper Title",
    "authors": ["Author 1", "Author 2"],
    "summary": "Abstract text...",
    "pdf_url": "https://arxiv.org/pdf/...",
    "published": "2024-01-15"
  }
}
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Resources

- [arXiv API Documentation](https://arxiv.org/help/api/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [FastMCP Documentation](https://github.com/jlowin/fastmcp)

## 🐛 Troubleshooting

### Common Issues

1. **Import Error for mcp.server.fastmcp**
   - Ensure you have `mcp>=1.26.0` installed
   - Try reinstalling: `pip install --upgrade mcp`

2. **Python Version Mismatch**
   - Check your Python version: `python --version`
   - Must be Python 3.12 or higher
   - Update `runtime.txt` if deploying to a platform

3. **Papers Directory Not Found**
   - The `papers/` directory is created automatically
   - Ensure you have write permissions in the project directory

4. **Dependency Conflicts**
   - Delete `venv/` and reinstall:
     ```bash
     rm -rf venv
     python -m venv venv
     source venv/bin/activate
     pip install -r requirements.txt
     ```

## 📊 Development

### Using uv for Development

```bash
# Install in editable mode
uv pip install -e .

# Add a new dependency
# Edit pyproject.toml, then:
uv pip compile pyproject.toml -o requirements.txt
uv pip install -e .
```

### Using pip for Development

```bash
# Install in editable mode
pip install -e .

# Add a new dependency
pip install new-package
pip freeze > requirements.txt
```

## 🚀 Deployment

When deploying to platforms like Heroku, Render, or Railway:

1. Ensure `runtime.txt` specifies the correct Python version
2. Use `requirements.txt` for dependency installation
3. Set environment variables if needed
4. Ensure the `papers/` directory is writable (or configure persistent storage)

### Example Heroku Deployment

```bash
# Create Heroku app
heroku create your-app-name

# Ensure files are present
# - runtime.txt (python-3.12.3)
# - requirements.txt

# Deploy
git push heroku main
```

---

**Made with ❤️ for the research community**
