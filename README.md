# 🌍 Travel-RAG-Orchestrator

![Python](https://img.shields.io/badge/python-3.11%2B-blue)
![Milvus](https://img.shields.io/badge/VectorDB-Milvus-orange)
![uv](https://img.shields.io/badge/package_manager-uv-purple)

> 一个专为复杂地理空间规划设计的高性能 RAG（检索增强生成）智能体引擎。通过混合检索与多模型协作，实现秒级的定制化旅行路线生成。

## 🎯 核心架构

本项目摒弃了传统的单链生成模式，采用**异步多智能体架构**：

1. **Query Expansion (HyDE)**: 针对用户简短的自然语言输入，系统首先利用大模型生成假设性回答，以此构建高维度的查询向量，大幅解决 "Query-Document Mismatch" 问题。
2. **Hybrid Retrieval (RRF)**: 底层依托 **Milvus** 向量数据库。系统并行发起 Dense Vector（语义）与 Sparse Vector（BM25/关键词）检索，最终通过 Reciprocal Rank Fusion (RRF) 算法合并打分，确保召回内容的精准度。
3. **Async Orchestration**: 基于 `asyncio` 实现全异步 I/O 流水线，最大化提升多 Agent 并行调用的网络效率。

## 📦 技术栈

- **包管理与运行**: `uv` (极致的 Python 依赖解析与构建速度)
- **并发框架**: `asyncio`, `FastAPI`
- **向量基建**: `Milvus` (Standalone 模式)
- **大模型生态**: Claude 3.5 (意图理解), DeepSeek (检索增强), Gemini 1.5 Pro (超长上下文编排)

## 🚦 快速运行

使用 `uv` 极速初始化环境：

```bash
# 1. 克隆项目
git clone [https://github.com/yourusername/Travel-RAG-Orchestrator.git](https://github.com/yourusername/Travel-RAG-Orchestrator.git)

# 2. 使用 uv 同步依赖
uv sync

# 3. 启动 Milvus 容器并初始化索引
docker-compose up -d milvus
python scripts/init_vector_store.py

# 4. 运行 API 服务
uv run uvicorn src.main:app --workers 4
