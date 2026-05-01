# 任务完成检查

- 遵循 `CLAUDE.md`：先用 Serena memories 获取高层上下文；源码分析优先用 Serena symbol/search；仅在需要时读取具体文件或符号。
- 遵循用户全局规则：除非用户明确要求，不要在完成代码或修复后自行运行 build/test；如验证对交付很关键，应说明建议执行的命令和原因，等待用户授权或明确记录未执行。
- 修改范围保持局部，避免无关重构、第三方依赖改动、生成物改动和大规模格式化。
- 涉及公共 CPU/GPU contract 的改动要额外检查：HLSL shared headers、C++ struct/layout、resource bindings、shader macro、CMake shader compile config、RTXDI/NRD/OMM integration。
- C++/HLSL 修改后建议的验证路径按风险选择：
  - 低风险只读/文档：无需 build/test，检查相关引用即可。
  - 局部 C++/shader 改动：建议 `cmake --build .\build --config Release --target Rtxpt` 或 Debug 对应目标。
  - 图像行为改动：建议构建后手动运行 sample，必要时运行 `Support\tests\run_tests.ps1`，但需先确认测试脚本中的 executable/ImageMagick 路径。
  - 后端/API 改动：DX12 默认路径优先；Vulkan 改动需要启用 `DONUT_WITH_VULKAN` 与 `NVRHI_WITH_VULKAN` 后单独验证。
- 完成说明必须如实区分：已运行的命令、未运行的命令、无法运行或未获授权的验证。没有实际验证证据时，不要声称测试通过或可合并。
- Commit 规范来自用户全局规则：`<type>(scope): <summary>`，summary 使用中文、动词开头、长度不超过 50 字、不加句号。