# mcp_oi_assistant
一个为编程竞赛（OI）选手设计的 MCP 服务器，能够安全地编译、运行并测试 C++ 代码。

## 📋 功能列表

### 核心功能
- ✅ **编译 C++ 代码**：调用 MinGW-w64 的 `g++` 编译器
- ✅ **运行程序**：提供输入数据，获取程序输出
- ✅ **输出比较**：自动比对实际输出与预期输出
- ✅ **GDB 调试**：支持使用 GDB 进行代码调试
- ✅ **测试用例管理**：读取预设的测试用例文件

### 安全特性
- ✅ **目录隔离**：所有文件操作限制在 `./tmp/` 目录内
- ✅ **命令过滤**：阻止执行危险系统命令
- ✅ **资源限制**：自动限制运行时间、内存和输出大小
- ✅ **文件名消毒**：防止路径遍历攻击

## 🛠️ 依赖的命令行工具

### 必需工具
| 命令 | 用途 | 安装方式 |
|------|------|----------|
| `g++` | C++ 编译器 | [MinGW-w64](https://www.mingw-w64.org/) |
| `gdb` | 调试器 | MinGW-w64 自带 |

### 工具路径要求
确保以下可执行文件存在：
```
/mingw64/bin/g++.exe
/mingw64/bin/gdb.exe
```

## 🚀 快速开始

### 1. 安装依赖
```bash
pip install mcp PyYAML psutil
```

### 2. 目录结构
```
oi-assistant/
├── main.py              # 启动入口
├── mcp_server.py        # 服务器核心
├── runner.py            # 代码运行器
├── security.py          # 安全模块
├── config.yaml          # 配置文件
├── requirements.txt     # Python依赖
├── tmp/                 # 临时工作区（自动创建）
└── mingw64/             # MinGW 工具链（需自行放置）
    └── bin/
        ├── g++.exe      # C++编译器
        └── gdb.exe      # 调试器
```

## 📚 MCP 工具说明

### 1. `compile_and_run`
编译并运行 C++ 代码。

**参数：**
- `code`：C++ 源代码（必需）
- `input`：程序输入数据（必需）
- `expected_output`：预期输出（可选，自动比对）
- `filename`：自定义文件名（可选）
- `time_limit`：时间限制（毫秒，默认 5000）
- `memory_limit`：内存限制（MB，默认 256）

### 2. `debug_with_gdb`
使用 GDB 调试 C++ 程序。

**参数：**
- `code`：C++ 源代码（必需）
- `gdb_script`：自定义 GDB 脚本（可选）

### 3. `compare_outputs`
比较两个输出是否一致。

**参数：**
- `actual`：实际输出（必需）
- `expected`：预期输出（必需）
- `ignore_whitespace`：是否忽略空白字符（默认 true）
- `ignore_case`：是否忽略大小写（默认 false）

### 4. `read_test_case`
读取预设的测试用例。

**参数：**
- `test_case_id`：测试用例 ID（必需）

## ⚙️ 配置文件

`config.yaml` 主要配置项：

```yaml
# 编译设置
compilation:
  compiler_path: "./mingw64/bin/g++.exe"  # g++路径
  cpp_standard: "c++17"                   # C++标准
  optimization_level: "-O2"                # 优化级别

# 运行限制
execution:
  max_time: 5000        # 最大运行时间（毫秒）
  max_memory: 256       # 最大内存（MB）
  max_output_size: 1048576  # 最大输出（1MB）

# 路径设置
paths:
  temp_dir: "./tmp"      # 临时文件目录
  mingw_dir: "./mingw64"  # MinGW目录
```

## 📁 文件存储

程序运行时会自动在 `./tmp/` 下创建以下子目录：

```
tmp/
├── sources/     # 源代码文件（*.cpp）
├── execute/     # 可执行文件（*.exe）
├── inputs/      # 输入数据文件（*.in）
├── outputs/     # 程序输出文件（*.out）
└── tests/       # 测试用例文件（*.txt）
```

## 🔧 常见问题

### 找不到编译器
```
错误：编译失败，无法找到 g++.exe
```
✅ **解决**：确保 `mingw64/bin/g++.exe` 存在且路径正确

### 运行超时
```
错误：运行超时（5000ms）
```
✅ **解决**：检查代码是否有死循环，或增加 `time_limit` 参数

### 输出不匹配
```
❌ 输出不匹配
```
✅ **解决**：检查输出格式（换行符、空格），或设置 `ignore_whitespace=true`

## 📄 许可证

MIT License