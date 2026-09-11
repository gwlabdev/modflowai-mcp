<div align="center">
  <img src="./assets/modflow-ai-wordmark-cream.png" alt="MODFLOW AI" width="400">
</div>

<br>

# MODFLOW-AI MCP Server

A hosted Model Context Protocol (MCP) server that gives AI assistants grounded access to MODFLOW and PEST documentation, to the FloPy and PyEMU Python API, to the Fortran source of MODFLOW 6 and MODFLOW-USG-Transport (GSI), to tutorials, and to the ModelMuse Help. Your assistant searches and retrieves real sources instead of guessing.

## What It Does

MODFLOW-AI MCP Server exposes nine tools over the [Model Context Protocol](https://modelcontextprotocol.io/). An AI assistant calls them to search documentation, retrieve files, and return cited answers.

### Key Features

- **Multi-repository search** across MODFLOW 6, MODFLOW-USG, PEST, PEST++, PEST_HP, plproc, gwutils, FloPy, PyEMU, and the ModelMuse Help.
- **Source code, not just documentation**: the FloPy and PyEMU Python sources and the Fortran sources of MODFLOW 6 and MODFLOW-USG-Transport (GSI) are indexed and retrievable in full, so an assistant can read what a package actually does rather than what the manual says about it.
- **Text and semantic search**, each tuned for a specific content type (docs, code, tutorials).
- **Acronym expansion** for MODFLOW/PEST terms (WEL, RIV, MAW, CHD, DRN, UZF, …).
- **GitHub URLs** returned with every code or tutorial result.
- **File retrieval by exact path**, with pagination for files over 30 KB.
- **Indexed ModelMuse Help**, with ranked search, page retrieval, and internal links.
- **Authenticated access**, limited to approved users.
- **Usage tracking**: tool calls are traced on our own infrastructure to monitor
  reliability and improve results. Traces record the account and the search
  arguments. They are never sold or shared with third parties.

## Getting Started

### 1. Request access

For access, visit [www.modflow.ai](https://www.modflow.ai). You'll receive configuration instructions by email.

### 2. Compatible AI Assistants

**HTTP transport** (direct connection):
- VS Code
- Cursor
- Codex
- ChatGPT

**MCP-Remote required**:
- Claude Desktop
- Claude.ai (Claude Code)

In ChatGPT the server also exposes the OpenAI-compatible `search` and `fetch`
tools, so results appear as citable sources.

### 3. Configuration

Your access email includes the endpoint URL and the exact configuration block for your client.

## 📚 Available Tools

### Search

#### search_docs
Full-text search across documentation, Python modules, and tutorial notebooks.
- Ultra-flexible `repository` parameter (array, comma / space / pipe / semicolon separated).
- Wildcards (`*`) and boolean operators (`AND` / `OR` / `NOT`).
- Acronym expansion (`UZF` → Unsaturated Zone Flow).
- Omit `repository` to search everything.

#### search_code
API and module search for FloPy and PyEMU, plus Fortran source for MODFLOW 6
and MODFLOW-USG-Transport (GSI).
- Returns signatures, parameters, docstrings.
- Python results include package codes (WEL, RCH, …) and model families.
- Fortran results search subroutine and module names across the full file.
- Direct GitHub links to source, pinned to the indexed commit.

#### search_tutorials
Tutorials and workflows.
- Filters by complexity (beginner / intermediate / advanced).
- Shows prerequisites and common modifications.
- Array search inside use cases and implementation tips.

#### semantic_search_docs
Concept-based documentation search using OpenAI embeddings. Best for "how to" and exploratory queries.

#### semantic_search_tutorials
Semantic search over tutorials with domain-aware matching (e.g., uncertainty vs. flow modeling).

#### search_modelmuse_help
Full-text search over the indexed ModelMuse HTML Help.
- Best for ModelMuse dialogs, menu commands, objects, formulas, and package setup.
- Expands acronyms such as `MAW` automatically.
- Returns exact `href` values for page retrieval.

### Retrieval

#### get_file_content
Fetch a complete file by exact path. Paginates files over 30 KB.
- Works for documentation files, Python modules, and Fortran source
  (`.f`, `.for`, `.f90`, `.inc`).

#### get_modelmuse_help_page
Fetch an indexed ModelMuse Help page using an exact `href` from `search_modelmuse_help`. Large pages are paginated and can include up to 100 internal links.

#### get_modflow_ai_info
Server overview: available repositories, tools, and statistics. No parameters.

## 💡 Usage Examples

### How AI agents use these tools

**User**: "How do I set up a pumping well in MODFLOW 6?"
**Agent calls**: `search_docs` with `query="WEL package MODFLOW 6"`
→ WEL package docs, examples, API.

**User**: "Show me a beginner tutorial for FloPy"
**Agent calls**: `search_tutorials` with `query="getting started"`, `complexity="beginner"`
→ Step-by-step FloPy tutorials with code.

**User**: "Explain how particle tracking works in groundwater models"
**Agent calls**: `semantic_search_docs` with a conceptual query
→ Theory and mathematical explanations.

**User**: "I need the NPF package documentation file"
**Agent calls**: `get_file_content` with the exact path
→ Full NPF docs.

**User**: "How does MODFLOW 6 actually solve for the well flow rate?"
**Agent calls**: `search_code` with `query="WEL"`, `repository="mf6"`, then
`get_file_content` on the returned path
→ The Fortran subroutine itself, read from the indexed release.

**User**: "What is MODFLOW AI?"
**Agent calls**: `get_modflow_ai_info`
→ Server overview.

**User**: "Where do I configure the MAW package in ModelMuse?"
**Agent calls**: `search_modelmuse_help` with `query="MAW"`, then `get_modelmuse_help_page` with the returned `href`
→ The indexed ModelMuse Help topic and its internal links.

### Query tips

- Use `search_docs` without a `repository` to search everything at once.
- Use specific terms or acronyms (`UZF`, `WEL package`) rather than long sentences.
- Start with `get_modflow_ai_info` to see what's available.
- Use `semantic_search_docs` for "how / why" conceptual questions.
- Use `search_modelmuse_help` for ModelMuse interface and setup questions.
- Avoid overlapping the same query across multiple tools in one turn.
- Use `search_code` — not semantic search — for exact function or class names.

## 📊 Available Repositories

### Code
- **FloPy** — Python package for MODFLOW (modules and tutorials).
- **pyEMU** — Python tools for uncertainty analysis and PEST++ integration.
- **MODFLOW 6** — Fortran source from the latest stable USGS release.
- **MODFLOW-USG-Transport** — Fortran source from the official GSI Environmental
  distribution. This is the GSI transport build, not the USGS MODFLOW-USG release.

Python sources are re-indexed daily from upstream. Fortran sources follow each
new published release.

### Documentation
- **MODFLOW AI** — Server documentation and guides.
- **MODFLOW 6** — USGS modular groundwater flow model.
- **MODFLOW-USG** — USGS unstructured grid version. Documentation only; its
  source is not indexed.
- **PEST** — Parameter estimation toolkit.
- **PEST++** — Next-generation PEST tools.
- **PEST_HP** — High-performance computing version.
- **gwutils** — Groundwater utility programs.
- **plproc** — Pilot point processor.

### Graphical interface
- **ModelMuse Help** — the USGS ModelMuse HTML Help, indexed page by page with
  its internal links. Covers dialogs, menu commands, objects, formulas, and
  package setup from the GUI side. Served by its own pair of tools rather than
  by `search_docs`.

## 🔍 Search Intelligence

### Acronym Recognition

The server expands common MODFLOW/PEST acronyms automatically:

- `WEL` → Well Package
- `RIV` → River Package
- `MAW` → Multi-Aquifer Well
- `CHD` → Constant Head Boundary
- `DRN` → Drain Package
- `EVT` → Evapotranspiration
- `RCH` → Recharge
- `SFR` → Streamflow Routing
- … and more.

### Method Selection

- **Text search** for exact terms, acronyms, quoted phrases.
- **Semantic search** for conceptual / "how to" questions.
- **Hybrid search** when a query benefits from both.

### GitHub URLs

Code results include direct links:
- FloPy modules: `github.com/modflowpy/flopy/blob/<commit>/…`
- PyEMU modules: `github.com/pypest/pyemu/blob/<commit>/…`
- MODFLOW 6 source: linked at the indexed release commit.

Links point at the exact commit that was indexed, so a result keeps matching
the code it came from. MODFLOW-USG-Transport ships as a download rather than a
public repository, so those results carry the distribution version instead of a
link.

## 💬 Feedback & Support

- **Issues and questions**: reach out via the contact in your access email.
- **Feature requests**: tell us what would help your workflow.
- **Corrections**: suggest improvements to docs or coverage.

## 📄 License & Terms

MODFLOW-AI MCP Server is a proprietary hosted service. By using it you agree to:
- Use the service within rate limits.
- Not reverse-engineer or abuse the service.

The service is provided as-is. No source code is licensed for redistribution.

For questions or access: [LinkedIn](https://www.linkedin.com/in/dlz800).

## 🙏 Acknowledgments

Built with data from:
- [USGS MODFLOW](https://www.usgs.gov/mission-areas/water-resources/science/modflow-and-related-programs)
- [FloPy Project](https://github.com/modflowpy/flopy)
- [PEST Suite](https://pesthomepage.org/)
- The broader groundwater modeling community.

---

*For access, visit [www.modflow.ai](https://www.modflow.ai).*
