# llama.cpp 源码副本 (mtp-22673 分支)

此文件夹是从 `E:\Genshin Impact Game\work\llama.cpp` 整理出来的一份**干净源代码副本**，
供独立查看/构建使用。

## 来源与分支

- 上游: https://github.com/ggml-org/llama.cpp (ghfast.top 镜像)
- 分支: **`mtp-22673`**  — Qwen3.6 APEX-MTP 专用分支(对应 PR #22673)
- 提交: `2dff7ff8f90ce6daefd6adb097d58a4276e5dd2d` ("conversion: fix type annotations")
- 文件数: 2852 (含本地对 `vendor/cpp-httplib/httplib.cpp` 的兼容性修改)

> ⚠️ 此模型(Qwen3.6-35B-A3B-APEX-MTP-I-Compact.gguf)必须用该 MTP 分支构建，
> 普通 `master` 分支无法加载。

## 已排除内容

- `.git/` 目录(历史与远程配置)
- `build/`、`build2/`、`build-hbm2/` 构建产物
- 编译中间文件

## 构建命令(已验证成功)

```bat
cmake -S . -B build-hbm2 -G Ninja -DCMAKE_BUILD_TYPE=Release ^
  -DGGML_VULKAN=ON -DGGML_NATIVE=OFF ^
  -DLLAMA_BUILD_UI=OFF -DLLAMA_BUILD_WEBUI=OFF -DLLAMA_BUILD_SERVER=ON ^
  -DCMAKE_CXX_FLAGS=-D_WIN32_WINNT=0x0A00 -DCMAKE_C_FLAGS=-D_WIN32_WINNT=0x0A00
cmake --build build-hbm2 --target llama-server -j4
```

三个坑(都在本机实测过):
1. **必须 `-DLLAMA_BUILD_UI=OFF`**,否则构建会跑 npm + HuggingFace 下载并崩溃(`0xc0000139`)。
2. **必须 `-D_WIN32_WINNT=0x0A00`**,否则 cpp-httplib 报 "doesn't support Windows 8 or lower"。
3. 编译出的 `llama-server.exe` 需要旁边放 3 个 MinGW DLL:
   `libgcc_s_seh-1.dll`、`libstdc++-6.dll`、`libwinpthread-1.dll`。

## 运行时环境变量(AMD HBM2 必设)

```bat
set GGML_VK_DISABLE_COOPMAT=1
set GGML_VK_DISABLE_COOPMAT2=1
```

否则 AMD Radeon Pro 5600M 的 Vulkan 输出会乱码。

## 参考

- 可用的预编译二进制仍在 E 盘: `E:\Genshin Impact Game\work\llama.cpp\build-hbm2\bin\llama-server.exe`
- 模型: `C:\GLM\Qwen3.6-35B-A3B-APEX-MTP-I-Compact.gguf`
- 网关: `C:\GLM\llama_gateway.py`(端口 2026)

_由 Hermes 于 2026-09-07 整理导出_
