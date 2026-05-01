# RTXPT 项目概览

- 项目名称：RTX Path Tracing / RTXPT，当前 README 标题为 `RTX Path Tracing v1.8.1`。
- 项目目的：NVIDIA 实时路径追踪代码示例，用作 path tracer 集成起点、SDK 参考和学习实验项目。
- 核心能力：纯 path tracer，不依赖 rasterization 主路径；支持 DirectX 12 和可选 Vulkan；集成 RTXDI/ReSTIR、NRD、OMM、RTXTF、Streamline/DLSS、OptiX denoiser 支持等。
- 平台要求：Windows 10 20H1+；DXR 1.1+ GPU；README 提到 GeForce Game Ready Driver 595.71+；Visual Studio 2022 v143 或更新；CMake v4.02+；Windows SDK 10.0.20348.0 或 10.0.26100.0+。
- 默认构建系统：CMake，根 `CMakeLists.txt` 声明 `project(RTXPathTracing)`，C++20，MSVC，多配置 Debug/Release，runtime 输出到仓库根 `bin/`。
- 默认图形后端：DX12；Vulkan 默认关闭，需要显式启用 `DONUT_WITH_VULKAN` 与 `NVRHI_WITH_VULKAN`。
- 重要项目规则：`CLAUDE.md` 要求始终先 `activate_project`，优先读 Serena memories，再按需使用 Serena symbol/search 工具定位文件或符号，避免一次性展开大量上下文。
- 用户全局规则中明确：除非用户明确要求，完成代码或修复后不要自行运行 test/build 命令；如需验证，先说明建议或在用户授权后执行。