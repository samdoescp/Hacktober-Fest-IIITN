# Diagram2Code — Hand-Drawn Physics & Circuit Simulation Compiler

> **Hacktoberfest Hack Day Nagpur X Elevate IIITN — Qualifier Submission**[cite: 1]  
> **Track:** Track 2 — Best Use of Gemma 4[cite: 1]  
> **Problem Statement:** Multimodal Visual Reasoning & Executable Code Synthesis for STEM Tools[cite: 1]

---

## 1. Team Details

* **Team Name:** `The Infinite loops`
* **Team Leader:** Samarth Malviya ([@github-username](https://github.com/samdoescp))


## 2. Problem Statement & Target Audience

### The Problem
Engineers, educators, and students routinely sketch technical systems—such as electric circuit schematics, free-body force vectors, and kinematic linkages—on paper or whiteboards during problem-solving and brainstorming. Translating these graphical concepts into executable computer simulations requires tedious manual translation into SPICE netlists or mathematical differential equations. Existing OCR and vision tools treat diagrams as generic images or unstructured text, failing to capture underlying graph topologies, node connectivity, and physical constraints.

### Target Users
* **Electrical & Mechanical Engineers:** Rapid, offline visual prototyping of circuit sub-blocks and physical systems without launching heavy CAD software.
* **STEM Educators & Students:** Instant interactive validation of physics and electronics homework problems.
* **Privacy-Constrained R&D Teams:** Local execution ensures proprietary circuit IP is never uploaded to external cloud APIs.

---

## 3. Proposed Solution & Role of Gemma 4

### Core Concept
**Diagram2Code** is an edge-native, privacy-preserving visual compiler powered by **Google Gemma 4**[cite: 1]. It parses hand-drawn diagrams, extracts underlying spatial node connections and component parameters into an Abstract Syntax Tree (AST), and compiles this intermediate representation into deterministic, executable Python simulation code (`PySpice`, `SymPy`, `SciPy`) for real-time numerical analysis and dynamic rendering.

### The Role of Open-Source AI
Google Gemma 4 serves as the system's visual reasoning engine[cite: 1]. Rather than generating code directly from images in a single non-deterministic step, Gemma 4 operates as an Intermediate Representation (IR) extractor[cite: 1]:
1. **Visual Topological Extraction:** Identifies discrete component symbols (resistors, capacitors, force vectors, masses) and maps directional connectivity nodes across hand-drawn lines.
2. **Value Normalization:** Parses handwritten numerical labels, engineering prefixes (e.g., $k\Omega$, $\mu\text{F}$, $N$), and directional angles into standardized units.
3. **Structured JSON Emission:** Outputs strict, machine-readable Abstract Syntax Trees (AST) that feed directly into deterministic Python code generation templates.

---

## 4. System Architecture & Workflow

1. **Ingestion & Preprocessing:** The user captures or uploads a hand-drawn diagram via the web UI. OpenCV applies adaptive thresholding, contrast adjustment, and noise filtering to clean hand-drawn strokes.
2. **Visual Processing (Gemma 4):** The cleaned image tensor is passed to Gemma 4 running locally[cite: 1]. Guided by structured vision prompts, Gemma 4 identifies components, pins, values, and net connections[cite: 1].
3. **AST Generation:** Gemma 4 emits a validated JSON object representing the system graph (nodes, edges, component parameters)[cite: 1].
4. **Deterministic Compilation:** The domain compiler maps the JSON AST directly into domain-specific Python code templates (`PySpice` for electrical circuits, `SymPy` for physics differential equations).
5. **Execution & Plotting Sandbox:** The synthesized Python script is executed inside a local, resource-capped subprocess sandbox, returning interactive Plotly charts and waveform plots back to the user interface.

---

## 5. Pipeline & Data Flow Matrix

| Stage | Input Format | Processing Engine | Output Format |
| :--- | :--- | :--- | :--- |
| **Ingestion** | Raw photo / sketch upload | OpenCV thresholding & noise filtering | Cleaned 1024x1024 image tensor |
| **Visual Inference** | Cleaned image buffer | Gemma 4 Vision-Language Model[cite: 1] | Structured JSON AST |
| **Code Generation** | JSON AST | AST Template Compiler | Native Python script (`.py`) |
| **Simulation** | Native Python script | `PySpice` / `SciPy` / `Ngspice` | Interactive Plotly graph / waveform |

---

## 6. Technology Stack & Dependencies

* **Primary AI Engine:** Google Gemma 4 (Lightweight Open-Weight Vision Model)[cite: 1]
* **Inference Runtime:** Ollama / `llama.cpp` (4-bit/8-bit GGUF quantization for local edge execution)
* **Mathematical & Circuit Engines:** `PySpice` (SPICE wrapper), `Ngspice` runtime, `SymPy` (Symbolic Mathematics), `SciPy` (Numerical Integration)
* **Application Framework:** Python 3.10+ / Streamlit / Plotly.js / OpenCV

---

## 7. Expected Deliverables

1. **Interactive Compilation Web App:** A web portal allowing users to upload drawings, view real-time extracted AST netlists, adjust values manually if needed, and run live simulations.
2. **Automated Code Generator:** A modular backend that outputs commented, standalone Python scripts (`PySpice` or `SymPy`) for offline execution in external Jupyter notebooks.
3. **Dynamic Visualizer:** Live rendering of transient AC/DC circuit responses, Bode plot frequency curves, and dynamic trajectory graphs.

---

## 8. Implementation Roadmap

- [x] **Stage 1: Qualifier Phase (Oct 3–8)**[cite: 1]
  - Problem scope definition, system architecture design, and AST specification.
  - Setup of public GitHub repository with zero implementation code[cite: 1].
- [ ] **Stage 2: Model System Prompt Optimization**
  - Prompt engineering for Gemma 4 to enforce deterministic JSON AST outputs across messy sketches[cite: 1].
  - Preprocessing pipeline setup (Otsu thresholding and line thinning).
- [ ] **Stage 3: Compiler Development**
  - Building deterministic AST parsers for `PySpice` netlists and `SymPy` symbolic differential equations.
  - Pre-execution static syntax validation.
- [ ] **Stage 4: Execution Sandbox & UI Integration**
  - Isolated subprocess execution environment setup with resource bounds.
  - Streamlit dashboard integration with dynamic Plotly rendering.

---

## 9. Scalability & Edge Strategy

* **Low-Resource Deployment:** Deploying Gemma 4 using 4-bit quantization (Q4_K_M GGUF) enables the system to run on standard consumer laptops without requiring high-end cloud GPU infrastructure[cite: 1].
* **Domain Extensibility:** The AST JSON schema acts as a universal intermediate layer. Adding support for new domains (e.g., chemical kinetics or digital logic gate truth tables) requires adding a target compiler module without retraining the core vision model.

---

## 10. Expected Challenges & Risk Mitigation

| Risk Event | Severity / Impact | Technical Mitigation Strategy |
| :--- | :--- | :--- |
| **Ambiguous Hand-Drawn Node Topology** | High: Disconnected circuit netlist or missing ground node | Human-in-the-loop AST editor UI allowing manual JSON node updates before script generation |
| **Malformed Code Generation** | Medium: Runtime syntax error during script execution | Pydantic validation schema on JSON AST prior to template compilation |
| **Inference Bottleneck on Edge Devices** | Low: Latency delay during visual parsing | Input image downsampling & 4-bit GGUF quantization via `llama.cpp` |

---

## 11. License
*An appropriate open-source license (e.g., MIT License) will be selected and added to the repository during the final implementation phase.*[cite: 1]
