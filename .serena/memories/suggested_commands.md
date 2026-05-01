# 常用命令

以下命令以 Windows PowerShell、仓库根 `D:\RTXPT` 为默认工作目录。

## 项目与 Git

```powershell
cd D:\RTXPT
git status --short
git submodule update --init --recursive
```

## 查找与阅读

```powershell
rg "pattern" Rtxpt
rg --files -g '!**/.git/**' -g '!**/build/**' -g '!**/bin/**' -g '!**/.vs/**'
Get-ChildItem -Force
Get-Content .\README.md -TotalCount 120
Select-String -Path .\Rtxpt\*.cpp -Pattern "WinMain"
```

优先使用 Serena：先 `list_memories`，再按需 `read_memory`；查源码优先 `get_symbols_overview`、`find_symbol`、`find_referencing_symbols`、`search_for_pattern`。

## 配置与构建

README 推荐：

```powershell
cmake CMakeLists.txt -B .\build
```

更常见的等价写法：

```powershell
cmake -S . -B .\build -A x64
cmake --build .\build --config Release --target Rtxpt
cmake --build .\build --config Debug --target Rtxpt
```

注意：用户规则要求，除非用户明确要求，不要在完成代码或修复后自行运行 build/test。需要时先说明建议或等待授权。

## 运行

构建后可执行文件输出到 `bin/`：

```powershell
.\bin\Rtxpt.exe --scene transparent-machines.scene.json
.\bin\Rtxpt.exe --width 1920 --height 1080
.\bin\Rtxpt.exe --width 3840 --height 2160 --fullscreen
.\bin\Rtxpt.exe --debug
.\bin\Rtxpt.exe --vk
```

Vulkan 需要先配置 `DONUT_WITH_VULKAN=ON` 与 `NVRHI_WITH_VULKAN=ON`，并安装/配置 Vulkan SDK。

## 测试脚本

图像回归脚本位于 `Support/tests`：

```powershell
Push-Location .\Support\tests
.\run_tests.ps1
.\run_tests.ps1 3
.\generate_golden.ps1
.\run_psnr.ps1
Pop-Location
```

注意：当前脚本中 `_1_render.ps1` 引用 `..\bin\pt_sdk.exe`，而当前 CMake 目标输出名是 `Rtxpt.exe` / `RtxptD.exe`；`_2_compare.ps1` 引用 `..\tools\ImageMagick\magick.exe`，而仓库中可见路径是 `Support\ImageMagick\magick.exe`。使用测试脚本前需要确认这些路径是否由本地环境另行映射，或按当前构建输出修正。