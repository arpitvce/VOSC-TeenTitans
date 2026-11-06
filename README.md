# Synaptic — Intelligent Repository Exploration

<p align="center">
  <strong>Understand unfamiliar codebases. Find relevant code. Explore how everything connects.</strong>
  <br />
  An evidence-grounded code intelligence platform for exploring complex software repositories.
</p>

<p align="center">
  ![Hackathon](https://img.shields.io/badge/Project-Hackathon-blueviolet)
  ![Domain](https://img.shields.io/badge/Domain-Developer%20Tools-black)
  ![Focus](https://img.shields.io/badge/Focus-Code%20Intelligence-blue)
</p>

---

## 1. Problem Statement

### Official Problem Statement

Developers approaching an unfamiliar open-source repository often struggle to identify where a feature is implemented, how modules relate to one another, and which files are relevant to a small change. Large file listings and generic code summaries rarely provide enough information to guide a contributor toward a specific entry point.

Build a repository-exploration tool that indexes a bounded source-code project and helps users locate relevant files and functions for a natural-language question or a specified feature. The tool should provide evidence from actual source files rather than generate speculative architectural descriptions.

The principal technical challenge is combining source-code structure, lexical or semantic retrieval, and explainable ranking. A prototype should support a limited set of programming languages and demonstrate retrieval on questions whose answers can be verified against the repository.

### Requirements

1. Accept a local repository or a provided archive of source files.

2. Exclude common generated files, build outputs, dependency directories, and files beyond a configurable size limit.

3. Index source-file paths, function or class names, and relevant code snippets.

4. Support natural-language and keyword queries about a feature or behaviour.

5. Rank relevant files and code regions using lexical search, symbol information, embeddings, or a hybrid retrieval method.

6. Show the matching code snippet, file path, and line range for each result.

7. Distinguish retrieved source evidence from any generated explanation and avoid claiming that a file is responsible for behaviour without supporting evidence.

8. Demonstrate retrieval quality on at least ten repository-specific questions with manually verified relevant files or symbols.

### Expected Output

An interactive code intelligence dashboard that indexes local or archived repositories, executes hybrid code retrieval for natural-language queries, displays matched file paths with exact code snippets and line ranges, and provides AI-generated code explanations grounded in verified source evidence.

### Suggested Featherless Usage

Use Featherless chat completions to interpret a developer's question and summarise retrieved code snippets. For semantic code retrieval, use an embedding model only if the required embedding capability is available in the selected service; otherwise use local embeddings or lexical ranking. The model should receive relevant snippets rather than the entire repository, and all file paths and line references should come from the actual index.

---

## 2. The Problem We Are Solving

Understanding an unfamiliar software repository is difficult because the information developers need is distributed across files, functions, classes, modules and dependencies.

Consider a developer joining a large open-source project who wants to answer:

- Where is authentication implemented?
- Which function handles incoming API requests?
- How does a particular feature interact with other modules?
- Which files should be modified to introduce a small feature?
- Where is a particular configuration value used?
- Which functions depend on a specific component?

Traditional file explorers help locate files, but they do not directly explain which code regions are relevant to a particular question.

Conversational code assistants can generate explanations, but an explanation without verifiable source references is insufficient for reliable engineering work.

**The challenge is to retrieve the right code, explain why it is relevant and provide evidence that developers can independently verify.**

---

## 3. Our Solution — Synaptic

Synaptic is an interactive repository intelligence platform designed to help developers explore unfamiliar codebases through evidence-grounded retrieval and visual code relationships.

It combines three complementary capabilities:

### 3.1 Hybrid Code Retrieval

Developers can ask natural-language questions or enter keywords describing a feature or behaviour.

Synaptic searches the indexed repository using source-code information, lexical matching and semantic retrieval where available.

Results are ranked to identify relevant files, functions and code regions.

### 3.2 Interactive Repository Graph

Synaptic provides a visual representation of repository entities and their relationships.

Developers can explore connected parts of a codebase rather than relying exclusively on directory navigation.

The graph complements retrieval by providing additional structural context.

### 3.3 Evidence-Grounded Explanations

Synaptic is designed to provide explanations based on retrieved source code.

Each result should include:

- Repository-relative file path.
- Matching code snippet.
- Exact starting and ending line numbers.
- Relevant function, class or symbol where available.
- Retrieval relevance information.
- An explanation grounded in the displayed evidence.

The retrieved source is the evidence. AI-generated explanations are a separate interpretation of that evidence.

---

## 4. Why Synaptic?

### Beyond conversational codebase exploration

Conversational repository assistants primarily organize interaction around questions and answers.

Synaptic emphasizes a combination of code retrieval, verifiable source evidence and visual repository exploration.

| Dimension | Conversational exploration | Synaptic |
|---|---|---|
| Primary interaction | Questions and answers | Search, evidence and graph exploration |
| Code discovery | Conversation-driven | Hybrid retrieval |
| Source evidence | Depends on result presentation | File paths, snippets and line ranges |
| Code relationships | Often explained through text | Interactive graph representation |
| Result verification | Depends on source references | Direct inspection of retrieved source |
| Architecture exploration | Primarily explanation-driven | Graph-based exploration |
| Retrieval transparency | Depends on implementation | Explicit ranking and evidence presentation |

These approaches are complementary. Synaptic's objective is not to eliminate conversational AI, but to make AI-assisted code understanding more transparent and verifiable.

---

## 5. Core Features

### 5.1 Repository Ingestion

Accept a bounded source-code project through a local repository or uploaded archive.

The ingestion process should:

- Read repository structure.
- Identify supported source-code files.
- Detect file types and programming languages.
- Exclude generated and irrelevant files.
- Apply configurable file-size limits.
- Preserve repository-relative paths.
- Record indexing metadata.

Example exclusions:

```text
node_modules/
.git/
dist/
build/
target/
vendor/
__pycache__/
coverage/
.next/
```

The exclusion list should be configurable.

Binary files, generated assets and unsupported formats should not be treated as source-code documents.

### 5.2 Source-Code Indexing

Extract useful information from supported source files.

Index:

- File paths.
- Programming languages.
- Function names.
- Class names.
- Method names.
- Relevant code snippets.
- Symbol locations.
- Line ranges.
- Import relationships where supported.

Use language-aware parsing where possible rather than relying exclusively on regular expressions.

The indexing system should preserve the original source positions so retrieved snippets can be traced to exact lines.

### 5.3 Natural-Language Code Search

Allow developers to ask questions such as:

```text
Where is user authentication handled?

Which function processes incoming API requests?

Where are database connections initialized?

Which files implement the repository upload functionality?

How are dependencies resolved?

Where is the configuration for application startup?
```

The system should convert the query into useful retrieval signals without inventing repository facts.

### 5.4 Hybrid Retrieval Engine

Synaptic is designed to combine multiple retrieval strategies.

**Lexical retrieval**

Find relevant terms, identifiers, function names and matching source-code text.

**Symbol-based retrieval**

Use indexed functions, classes, methods and other structural entities to identify relevant code regions.

**Semantic retrieval**

Where an embedding model is available, retrieve code snippets with semantically related meaning even when they do not contain the exact query terms.

**Hybrid ranking**

Combine the retrieval signals to produce a ranked list of relevant source regions.

A conceptual scoring model:

\[
S(d,q)=w_lL(d,q)+w_sY(d,q)+w_eE(d,q)
\]

Where:

- \(L\) = lexical relevance.
- \(Y\) = symbol relevance.
- \(E\) = semantic relevance.
- \(w_l,w_s,w_e\) = configurable weights.

The final ranking should be evaluated against manually verified repository-specific questions.

Do not claim that a particular scoring algorithm is implemented until it has actually been built and tested.

### 5.5 Evidence-Based Results

Each search result should display:

```text
Result: Authentication Handler

File:
src/auth/authentication.py

Symbol:
authenticate_user()

Location:
Lines 42–78

Matched source:
[Actual source-code snippet]

Why retrieved:
Relevant identifier and code-context matches.

Relevance:
[Actual ranking information]
```

All paths, snippets and line references must come from the repository index.

Never fabricate line numbers or source code.

### 5.6 AI-Assisted Code Explanations

Use an LLM to explain retrieved source code.

The process should be:

1. Receive a developer question.
2. Retrieve relevant code regions.
3. Select the strongest evidence.
4. Pass only those snippets and their metadata to the LLM.
5. Generate an explanation grounded in the supplied evidence.
6. Display citations to the actual source locations.

If the retrieved evidence is insufficient, the system should say so instead of generating a speculative answer.

### 5.7 Interactive Repository Graph

Synaptic also provides a graph-based interface for exploring repository structure.

The graph can represent:

- Files.
- Directories.
- Modules.
- Functions.
- Classes.
- Packages.
- Dependencies.

Relationships can include:

- IMPORTS.
- CALLS.
- EXTENDS.
- IMPLEMENTS.
- DEPENDS_ON.
- CONTAINS.

The graph should support:

- Pan and zoom.
- Node selection.
- Relationship exploration.
- Entity-type filtering.
- Connected-node highlighting.
- Search.
- Node details.
- Navigation between files and graph entities.

The graph is an additional exploration mechanism, not a substitute for exact code retrieval.

---

## 6. System Architecture

The proposed processing architecture is:

```text
             SOURCE REPOSITORY
                     |
                     v
             REPOSITORY INGESTION
                     |
                     v
             FILE FILTERING
                     |
                     v
             LANGUAGE DETECTION
                     |
                     v
              SOURCE PARSING
                     |
             +-------+-------+
             |               |
             v               v
       SYMBOL INDEX      CODE CHUNKS
             |               |
             v               v
      STRUCTURAL DATA    LEXICAL INDEX
             |               |
             |          EMBEDDINGS
             |          (optional)
             |               |
             +-------+-------+
                     |
                     v
              HYBRID RETRIEVAL
                     |
                     v
              RESULT RANKING
                     |
                     v
             SOURCE VERIFICATION
                     |
             +-------+-------+
             |               |
             v               v
       CODE EVIDENCE     AI EXPLANATION
             |               |
             +-------+-------+
                     |
                     v
             SYNAPTIC DASHBOARD
```

### 6.1 Repository Ingestion

The system accepts a repository or archive and creates a bounded indexing job.

It should validate archive paths, avoid path traversal, enforce file-count and size limits, and safely handle unsupported files.

### 6.2 Parsing and Chunking

Supported source files are parsed into useful code entities.

Prefer syntax-aware parsing for extracting functions, classes and methods.

Chunking should preserve symbol boundaries and line-number metadata.

Avoid arbitrary chunk splitting that destroys the context needed to understand a function.

### 6.3 Index Construction

Build the necessary indexes:

- File metadata index.
- Symbol index.
- Lexical search index.
- Semantic vector index, if available.
- Structural relationship graph.

### 6.4 Query Processing

When a developer submits a query:

1. Normalize the query.
2. Extract important keywords and identifiers.
3. Identify relevant symbols where possible.
4. Execute lexical retrieval.
5. Execute semantic retrieval if configured.
6. Combine retrieval results.
7. Rank candidate code regions.
8. Deduplicate overlapping results.
9. Verify source locations.
10. Return evidence-backed results.

### 6.5 Explanation Generation

Only verified retrieved snippets should be passed into the explanation stage.

The LLM should not receive an unrestricted instruction to invent an architecture description.

Its response should distinguish:

- Directly observed source evidence.
- Reasonable interpretation.
- Unverified assumptions.
- Missing evidence.

---

## 7. Technical Approach

The implementation should prioritize correctness, traceability and bounded resource consumption.

### Repository parsing

Use language-aware parsers, such as Tree-sitter where appropriate, to extract code structure.

Start with a limited set of languages and expand only after reliable extraction is demonstrated.

### Retrieval

Begin with lexical retrieval and symbol-aware indexing.

Add semantic embeddings when a suitable embedding model is available.

A hybrid retrieval system can improve recall by combining exact identifier matches with semantically related code.

### Ranking

Use normalized retrieval signals and configurable weights.

Evaluate ranking quality against manually verified answers instead of relying on subjective impressions.

### Evidence verification

Maintain source offsets and line mappings during indexing.

Before displaying a result, verify that:

- The referenced file exists in the indexed repository version.
- The source range is valid.
- The displayed snippet corresponds to the stored source.
- The result belongs to the selected repository.

---

## 8. Evaluation and Testing

Retrieval quality is a central part of the project.

Synaptic should be evaluated using at least ten repository-specific questions with manually verified relevant files or symbols.

Example evaluation questions:

| No. | Question | Expected evidence |
|---|---|---|
| 1 | Where is authentication handled? | Authentication function or module |
| 2 | Where are API requests processed? | Request handlers |
| 3 | Where are database connections initialized? | Database initialization code |
| 4 | Where is configuration loaded? | Configuration loader |
| 5 | Where are user permissions checked? | Authorization functions |
| 6 | Where is repository upload implemented? | Upload handler |
| 7 | Where are dependencies resolved? | Dependency resolution code |
| 8 | Where is application startup defined? | Entry point |
| 9 | Which functions handle error responses? | Error handlers |
| 10 | Where is logging configured? | Logging configuration |

These are example questions. Their expected answers must be verified against the actual repository used in the evaluation.

### Suggested evaluation metrics

- Recall@K.
- Precision@K.
- Mean Reciprocal Rank (MRR).
- Correct file retrieval rate.
- Correct symbol retrieval rate.
- Source-line accuracy.
- Indexing time.
- Query latency.
- Memory consumption.

The evaluation should compare lexical-only retrieval with hybrid retrieval where applicable.

---

## 9. Suggested Featherless Integration

Featherless chat completions can be used to interpret developer questions and explain retrieved code.

The intended flow is:

```text
Developer Query
       |
       v
Hybrid Retrieval
       |
       v
Relevant Code Snippets
       |
       v
Featherless Chat Completion
       |
       v
Evidence-Grounded Explanation
```

The model should receive only relevant snippets rather than the entire repository.

For semantic retrieval, use an embedding model only when the selected service supports the required embedding capability.

Otherwise, use a local embedding model or lexical and symbol-based retrieval.

The LLM must not invent:

- File paths.
- Line numbers.
- Function names.
- Repository relationships.
- Source-code evidence.

These must originate from the actual repository index.

---

## 10. User Interface

The Synaptic interface combines traditional repository navigation with graph-based code exploration.

### Repository explorer

A file explorer allows developers to browse directories and inspect source files.

### Interactive graph

The graph provides a visual representation of repository entities and their connections.

### Search interface

A natural-language and keyword search interface lets developers ask questions about code.

### Evidence panel

Search results display matching files, symbols, snippets and line ranges.

### AI explanation panel

A separate panel provides an explanation grounded in the retrieved source evidence.

### Design philosophy

The interface follows a premium developer-infrastructure aesthetic:

- Minimal dark interface.
- Clear typography.
- Compact technical information.
- Subtle borders.
- High information density.
- Consistent navigation.
- Interactive visualizations.

---

## 11. Comparison with Existing Approaches

Synaptic explores a complementary approach to conversational repository assistants.

| Capability | Conversational repository assistant | Synaptic |
|---|---|---|
| Ask questions about code | Yes | Yes, as implemented |
| Retrieve relevant source | Depends on implementation | Core objective |
| Exact file and line evidence | Depends on implementation | Required |
| Hybrid retrieval | Varies | Core technical approach |
| Interactive code graph | Varies | Visual exploration capability |
| Source-grounded explanation | Depends on implementation | Core design principle |

The key distinction is the combination of explainable retrieval, exact source evidence and visual repository exploration.

Synaptic aims to make it easier not only to ask what code does, but also to locate and inspect the actual implementation.

---

## 12. Development Roadmap

| Stage | Objective |
|---|---|
| 1 | Repository ingestion and safe file filtering |
| 2 | Source parsing and symbol extraction |
| 3 | Code chunking and line mapping |
| 4 | Lexical and symbol-based indexing |
| 5 | Hybrid retrieval and ranking |
| 6 | Evidence-backed result presentation |
| 7 | AI explanations using Featherless |
| 8 | Interactive repository graph |
| 9 | Ten-question retrieval evaluation |
| 10 | Performance, reliability and usability improvements |

Future capabilities may include GitHub integration, advanced dependency analysis, impact analysis, collaborative exploration and AI coding agents.

---

## 13. Demonstration

### Existing conversational repository exploration

![Existing repository exploration](https://github.com/user-attachments/assets/7187e678-1752-4edf-aac7-f9df466bc354)

### Synaptic — Interactive repository visualization

![Synaptic repository graph](https://github.com/user-attachments/assets/2a9f0524-1363-4b26-a3d0-b02d2bf210f1)

The screenshots demonstrate the different interaction models: conversational exploration and visual repository exploration.

The complete hackathon demonstration should additionally show natural-language retrieval, exact source snippets, line references and grounded explanations.

---

## 14. Expected Impact

Synaptic aims to reduce the effort required to understand unfamiliar software repositories.

Potential benefits include:

- Faster onboarding for new contributors.
- More efficient navigation of large codebases.
- Easier identification of relevant implementation files.
- Better visibility into code relationships.
- More reliable AI-assisted code understanding.
- Reduced dependence on speculative architectural summaries.

---

## 15. Team

**Team Name:** TeenTitans

**Project Name:** Synaptic

**Repository:** [VOSC-TeenTitans](https://github.com/arpitvce/VOSC-TeenTitans)

---

## 16. Vision

We believe developers should be able to understand a software repository before attempting to modify it.

Synaptic aims to make code discovery evidence-driven, repository exploration visual and AI explanations verifiable.

**Find the code. Understand the connections. Make changes with confidence.**

---

<p align="center">
  <strong>Synaptic — See the connections behind your code.</strong>
</p>
