# dmqbe

## 项目简介
qbe 是一个轻量级的C语言编译器后端，支持多种架构的代码生成和优化。项目旨在提供一个简单高效的编译器后端实现。

## 功能特性
- 支持 x86_64、ARM64 和 RISC-V 64 架构
- 包含多种优化pass：GVN、GCM、SSA等
- 提供丰富的测试用例
- 支持交叉编译

## 构建说明
### 依赖
- CMake 3.10+
- GCC/Clang 编译器

### 构建步骤
```bash
# 使用CMake构建
mkdir build && cd build
cmake ..
make
```

## 使用示例
```bash
# 编译C代码到汇编
./qbe input.c -o output.s
```

## 测试说明
项目包含大量测试用例，位于test/目录下。可以使用以下命令运行测试：
```bash
./test.sh
```

## 贡献指南
欢迎提交issue和pull request。请确保代码风格一致，并通过所有测试。

## 许可证
本项目采用MIT许可证，详见LICENSE文件。
