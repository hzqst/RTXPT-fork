# 代码风格与约定

- 主要语言：C++20 与 HLSL/HLSLI；CMake 构建。
- 编译设置：根 `CMakeLists.txt` 设置 `CMAKE_CXX_STANDARD 20`、`CMAKE_CXX_EXTENSIONS ON`、`CMAKE_COMPILE_WARNING_AS_ERROR ON`；MSVC 下使用 `/MP`，Debug 设置 `_ITERATOR_DEBUG_LEVEL=1`。
- 文件头：核心 C++ 和 HLSL 文件通常带 NVIDIA copyright/proprietary notice。
- 命名：类名常用 PascalCase，如 `Sample`、`AdvancedPathTracer`；成员函数常用 PascalCase，如 `CreateRTPipelines`、`SampleRenderCode`；成员变量多为 `m_` 前缀，如 `m_ui`、`m_renderTargets`；全局变量有 `g_` 前缀，如 `g_windowTitle`；常量有 `c_` 或 HLSL `k` 前缀，如 `c_envMapRadianceScale`、`kUseBSDFSampling`。
- 格式：C++ 类/函数通常换行开大括号；缩进以 4 空格为主；头文件中声明常有列对齐；CMake 中可见 tab/space 混用，保持局部文件风格即可。
- 注释：项目内注释主要是英文；不要无意义补充注释，只在复杂渲染流程、跨 API 差异、shader 宏条件等位置加入简短解释。
- C++ 结构：大量使用 `std::shared_ptr`、`std::unique_ptr`、NVRHI handles、Donut framework 类型；公共接口常通过继承/override 扩展。
- HLSL 结构：shader 代码大量使用 namespace、inline helper、compile-time macro、include guard。部分 HLSLI 使用传统 include guard 代替 `#pragma once`，原因是 DXC 兼容问题。
- 修改策略：优先保持现有架构、命名和局部风格；涉及 shader/C++ shared layout、resource bindings、CMake/SDK 配置时要格外谨慎，因为可能同时影响 CPU/GPU contract。
- 格式化/静态检查：仓库未发现 `.clang-format` 或明确 lint/format 命令；不要大范围机械格式化第三方或无关文件。