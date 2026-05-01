# CLAUDE.md

This file guides Agent Coding in this repository using a "progressive disclosure" approach: prioritize retrieving high-level information from Serena memories first, then locate and read specific files/symbols only when needed, instead of expanding a large amount of context at once.

## Serena memories (keep context concise)
1. Prefer using `list_memories` to browse existing memories in the current project (do not read all of them by default).
2. Use `read_memory` to precisely read a specific memory only when needed (on-demand loading).
3. If memory information is insufficient or outdated, fall back to reading repository files or use Serena's symbol/search capabilities for targeted lookup, and maintain memory content with `write_memory` / `edit_memory` / `delete_memory`.

## High-level information in this repository (read corresponding memories first)
- `project_overview`: project purpose, platform requirements, build system, default backend, and repository-level working rules.
- `architecture_entrypoints`: top-level directory map, CMake/build entry points, runtime entry points, and shader entry points.
- `style_and_conventions`: C++/HLSL style, naming conventions, formatting expectations, and CPU/GPU contract cautions.
- `suggested_commands`: common PowerShell commands for navigation, CMake configuration/build, runtime invocation, and image regression scripts.
- `task_completion`: completion checklist, verification expectations, and cautions around build/test execution in this repository.

## "Source entry points" when memories are insufficient (query and read on demand)
- Build configuration: start from `CMakeLists.txt`, then `Rtxpt/CMakeLists.txt` for the `RtxptCore` library, shader compilation, and `Rtxpt` executable target.
- Runtime startup: inspect `Rtxpt/AdvancedSample.cpp` for `WinMain` / `main`, `AdvancedSample`, and `AdvancedPathTracer`.
- Main application flow: use Serena symbol lookup on `Rtxpt/Sample.h` and `Rtxpt/Sample.cpp` for scene loading, acceleration structures, lighting, path tracing, denoising, post-processing, and render-loop hooks.
- Command-line behavior: inspect `Rtxpt/SampleCommon/CommandLine.cpp` and related `SampleBaseApp` code only when command parsing or app initialization details are needed.
- Shader pipeline: start from `Rtxpt/shaders.cfg`, `Rtxpt/Shaders/PathTracerSample.hlsl`, and `Rtxpt/Shaders/PathTracer/PathTracer.hlsli`; then follow includes with Serena/search only for the specific shader feature under investigation.
- Feature modules: use targeted lookup under `Rtxpt/RTXDI`, `Rtxpt/NRD`, `Rtxpt/OpacityMicroMap`, `Rtxpt/Lighting`, `Rtxpt/Materials`, `Rtxpt/ProcessingPasses`, `Rtxpt/ToneMapper`, and `Rtxpt/Misc` based on the feature being changed.
- Tests and support scripts: start from `Support/tests/run_tests.ps1`, `_run.ps1`, `_1_render.ps1`, `_2_compare.ps1`, and `tests.json` when image regression behavior is relevant.
- External dependencies: inspect `External/` only after checking memories and local integration points; prefer dependency documentation/tools for API details instead of reading vendored sources broadly.

## Progressive disclosure key points
- Read memories first, then locate a single file/symbol; do not read the whole repository at once.
- Prefer Serena for code exploration (symbol overview/references/search), and read file contents only when necessary.
- Prefer Context7 for external dependency/library usage (query on demand).

## Important rules
- **ALWAYS** call Serena's `activate_project` on agent startup
