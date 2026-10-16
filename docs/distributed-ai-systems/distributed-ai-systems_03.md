# 2 GPU 硬件、网络与并行策略

##### 需求：

函数签名：

`analyze_gpu_topology()`

使用 `subprocess` 运行 `nvidia-smi topo -m` 并捕获输出。

解析拓扑矩阵以识别：

- 通过 NVLink 连接的 GPU（查找 NV18、NV12、NV4）
- 仅通过 PCIe 连接的 GPU（查找 PIX、PXB）
- 全连接（所有 GPU 均可通过 NVLink 与其他所有 GPU 通信）

返回一个字典，包含：

- `num_gpus`：GPU 数量
- `nvlink_pairs`：通过 NVLink 连接的 GPU 对列表
- `pcie_only_pairs`：仅通过 PCIe 连接的 GPU 对列表
- `has_all_to_all_nvlink`：布尔值，表示是否所有 GPU 之间都有 NVLink
- `recommended_parallelism`：基于拓扑的建议并行策略

##### 测试你的函数：

```python
import subprocess

topology = analyze_gpu_topology()
print(f"Number of GPUs: {topology['num_gpus']}")
print(f"NVLink pairs: {topology['nvlink_pairs']}")
print(f"PCIe-only pairs: {topology['pcie_only_pairs']}")
print(f"All-to-all NVLink: {topology['has_all_to_all_nvlink']}")
print(f"Recommended strategy: {topology['recommended_parallelism']}")
```

#### 并行策略选择器

实现一个函数，根据模型大小、硬件拓扑和训练需求推荐并行策略。

**需求：**

函数签名：

```python
recommend_parallelism_strategy(model_size_gb, num_gpus, topology_info,
                               has_nvlink_all_to_all, training_type='training')
```

考虑：

- 模型大小与单 GPU 内存
- 拓扑（NVLink 全连接 vs. 仅 PCIe）
- 训练与推理需求

返回一个字典，包含：

- `primary_strategy`：主要并行方式（DDP、FSDP、TP、PP 或混合）
- `reasoning`：选择该策略的原因说明
- `alternative_strategies`：其他可行选项列表
- `estimated_gpu_count`：所需的最少 GPU 数量
- `memory_per_gpu_gb`：每块 GPU 的估算内存

**测试你的函数：**

```python
# 70B model (140 GB in BF16) on 8 GPUs with NVLink all-to-all
strategy = recommend_parallelism_strategy(
    model_size_gb=140,
    num_gpus=8,
    topology_info={'has_all_to_all_nvlink': True},
    has_nvlink_all_to_all=True,
    training_type='training'
)
print(f"Recommended: {strategy['primary_strategy']}")
print(f"Reasoning: {strategy['reasoning']}")
print(f"Estimated GPUs needed: {strategy['estimated_gpu_count']}")
print(f"Memory per GPU: {strategy['memory_per_gpu_gb']:.1f} GB")
```

#### 预期学习成果

完成这些练习后，你应该能够：

- 使用 PyTorch 和 CUDA API 以编程方式查询 GPU 硬件属性
- 计算使用不同精度和优化器训练大型模型所需的内存
- 解析并解读来自 `nvidia-smi` 的 GPU 拓扑信息
- 识别 GPU 之间的 NVLink 与 PCIe 连接
- 根据模型大小和硬件拓扑推荐合适的并行策略
- 理解 GPU 互连与并行效率之间的关系
