# 架构与入口

## 顶层目录

- `Rtxpt/`：项目核心应用、渲染流程、HLSL shader 与示例代码。README 指出 `Sample.cpp/.h/.hlsl` 是核心 entry points。
- `Rtxpt/Shaders/PathTracer/`：核心路径追踪 shader 逻辑。
- `Rtxpt/SampleCommon/`：应用基础设施、命令行、shader compiler utils、render targets、capture script、scene extension 等通用代码。
- `Rtxpt/RTXDI/`：RTXDI / ReSTIR DI 与 GI 相关 pass、resources、application settings、shader bridge。
- `Rtxpt/NRD/`：NRD denoiser integration。
- `Rtxpt/OpacityMicroMap/`：OMM baking/build queue。
- `Rtxpt/Lighting/`：lights baker、environment map、procedural sky、distant lighting。
- `Rtxpt/Materials/`：materials baker。
- `Rtxpt/ProcessingPasses/`：accumulation、post process、denoising guides 等 pass。
- `Rtxpt/ToneMapper/`：tone mapping pass 和相关 shader。
- `Rtxpt/Misc/`：shader debug、debug lines、zoom tool 等辅助功能。
- `External/`：第三方库和 SDK，包括 Donut、NRD、RTXDI、OMM、NVAPI、cxxopts、RTXTF、Streamline、DXC、AgilitySDK 等。
- `Assets/`：模型、贴图和 scene 文件，来自 `RTXPT-Assets` submodule。
- `Support/`：测试、ImageMagick、OptiX denoiser 等支持工具。
- `Docs/`：README 使用的项目图片。

## 构建入口

- 根 `CMakeLists.txt` 设置 C++20、MSVC flags、下载/配置 DXC 与 Agility SDK，添加 `External`，再添加 `Rtxpt`。
- `Rtxpt/CMakeLists.txt` 收集 `.cpp/.h/.md` 和 HLSL 文件，使用 Donut 的 `donut_compile_shaders` 编译 shader。
- `Rtxpt/CMakeLists.txt` 定义 `RtxptCore` static library，并通过 `add_rtxpt_sample(Rtxpt "AdvancedSample.cpp")` 创建主可执行目标。
- Debug 输出名为 `RtxptD`，Release 输出名为 `Rtxpt`，输出目录是仓库根 `bin/`。

## 运行入口

- `Rtxpt/AdvancedSample.cpp` 定义 `WinMain` / `main`，创建 `AdvancedSample`，调用 `Init`、`RunMainLoop`、`End`。
- `AdvancedSample` 继承 `SampleBaseApp`，覆写 `CreateMainRenderPass`，返回 `AdvancedPathTracer`。
- `AdvancedPathTracer` 继承 `Sample`，实现 `SampleRenderCode`、`CreateRTPipelines`、`DestroyRTPipelines`、`GetMaterialSpecializationShader`。
- `Rtxpt/Sample.cpp` / `Rtxpt/Sample.h` 是主渲染应用类，负责 scene load、camera/input、acceleration structures、lighting、RTXDI、path tracing、denoise、post process、render loop hooks 等。

## Shader 入口

- 主 path tracer variant 来自 `Rtxpt/Shaders/PathTracerSample.hlsl`。
- 核心共享逻辑在 `Rtxpt/Shaders/PathTracer/*.hlsli`，其中 `PathTracer.hlsli` 包含 path state 初始化、path tracing helper 和 compile-time 设置。
- `Rtxpt/shaders.cfg` 驱动 shader 编译配置。