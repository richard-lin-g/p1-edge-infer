# onnx-arm-inference-engine

> 把 PyTorch 模型量化后跑在 ARM 工控机上，给出 p99 延迟、内存、精度对比的开源推理引擎。

**状态：骨架（D1 建仓，2026-11-02）。实现从 D11 开始，每天提交。**

## 做什么

- 输入一个 PyTorch 分类模型，导出 ONNX，做 INT8 动态 / 静态（QDQ + 校准）量化
- 用 C++17 封装 ONNX Runtime 会话，导出 C ABI 共享库，供采集程序直接调用
- 在 x86 与 ARM aarch64 上给出可复现的延迟 / 内存 / 体积 / 精度对比

## 怎么跑

```bash
git clone <repo-url> && cd onnx-arm-inference-engine
make setup     # 装 Python 依赖 + 拉基础镜像
make export    # PyTorch → ONNX
make bench     # 跑基准，输出 results/*.json
```

> 骨架阶段这三条命令还不可用（`Makefile` 在 D11 补）。现在只保证仓库结构存在、提交时间线从今天开始。

## 产出什么

| 产物 | 位置 |
|---|---|
| 基准报告（p50/p95/p99、内存、体积、精度） | `docs/benchmark.md` |
| 原始数据 | `results/*.json` |
| 架构说明 | `docs/architecture.md` |

## 目录

```
src/export/     PyTorch → ONNX
src/quantize/   INT8 动态 / 静态量化
src/bench/      基准测量
cpp/            C++17 推理封装（C ABI）
scripts/        环境与权重下载
docs/           架构、基准报告、图
results/        基准原始数据
tests/
```

## 红线

- 不放模型权重（> 100 MB）与数据集，走 Release 附件 + 下载脚本
- 不放任何前雇主的真实数据、图纸与测点数据
- 不放密钥；配置用 `.env.example` 给模板

## License

MIT
