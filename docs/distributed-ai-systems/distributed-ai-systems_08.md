## 第 6 章：分布式推理与 vLLM

跨 GPU 内存分配，并在请求完成时回收——PagedAttention 消除了困扰早期服务系统的碎片化问题。基于块的设计还消除了填充 FLOPs：注意力内核只遍历实际存在的块，绝不会在填充位置上浪费计算。这些创新加在一起，相比传统方法可带来 2-4 倍的吞吐量提升。

接着我们探讨了三种基本的并行策略。张量并行将每一层横向切分到多个 GPU 上，降低每 GPU 内存需求和延迟，代价是每一层都要进行全归约通信。数据并行将模型复制到多个 GPU 上，以零通信开销线性扩展吞吐量，但要求每个副本存储完整权重。流水线并行沿着层将模型纵向切分，支持在节点间带宽受限的场景下进行多节点部署，但会引入流水线气泡，必须通过请求组和分块预填充来缓解。

对于 MoE 模型，专家并行改变了专家的分布方式和 token 的路由方式。如果希望每个请求都使用所有 GPU（更低延迟），请选择 TP+EP；如果希望获得按请求分区的 KV 缓存和更高吞吐量，尤其是对于 MLA/MQA 模型，请选择 DP+EP。

决策框架非常直接：从适配模型所需的最小并行度开始（先 TP，再为多节点添加 PP），然后通过增加 DP 来扩展吞吐量。对你的工作负载进行剖析，了解通信与计算之比，并根据预填充/解码的混合情况调整 `max_num_batched_tokens`。不要随意设置参数——要为你的特定部署进行测量、迭代和优化。

展望未来，vLLM 持续演进，包括分离式预填充/解码（将计算密集型的预填充与内存密集型的解码分到不同硬件上）、改进专家并行（EP）在模型/量化/硬件组合上的稳定性，以及与 AMD MI300X、Intel Gaudi 等新兴硬件的更紧密集成。

vLLM 的模型并行方法在大型模型和高吞吐量工作负载上表现出色。但如果你需要交互式应用的超低延迟，或者想通过复杂的路由和会话持久化来处理数千个并发请求，该怎么办？下一章将介绍 SGLang，它采用了一种不同的方法：SGLang 不专注于模型分片，而是强调请求级路由、前缀缓存和工作负载分离，以实现不同的性能特性。

### 代码摘要

- `vllm.LLM`：用于模型加载和推理的 vLLM LLM 类
- `vllm.SamplingParams`：文本生成采样的配置
- `vllm.engine.LLMEngine`：vLLM 核心推理引擎
- `vllm.worker.worker.Worker`：用于分布式推理的 vLLM worker 进程
- `vllm.engine.arg_utils`：vLLM 命令行参数工具
- `vllm.distributed.parallel_state`：vLLM 并行状态管理
- `vllm.engine.async_llm_engine.AsyncLLMEngine`：用于服务的异步推理引擎

### 有用链接

**vLLM 与 PagedAttention**

- 使用 PagedAttention 的大语言模型服务的高效内存管理（2023）：https://arxiv.org/abs/2309.06180
- vLLM 文档：https://docs.vllm.ai/
- vLLM GitHub：https://github.com/vllm-project/vllm
- vLLM 路线图：https://roadmap.vllm.ai

**分布式推理**

- vLLM 文档 - 并行与扩展：https://docs.vllm.ai/en/stable/serving/parallelism_scaling/
- vLLM 文档 - 数据并行部署：https://docs.vllm.ai/en/stable/serving/data_parallel_deployment.html
- vLLM 文档 - 分布式服务：https://docs.vllm.ai/en/stable/serving/distributed_serving.html
- NVIDIA Dynamo KV 缓存管理器：https://docs.nvidia.com/dynamo/archive/0.2.0/architecture/kv_cache_manager.html

**研究**

- 何时推理：vLLM 的语义路由器（2025）：https://arxiv.org/abs/2510.08731
- 使用 vLLM 进行分布式推理（Red Hat，2025）：https://developers.redhat.com/articles/2025/02/06/distributed-inference-with-vllm

#### 参考资料

- https://docs.vllm.ai/en/latest/models/supported_models.html
- Kwon 等人，*使用 PagedAttention 的大语言模型服务的高效内存管理*，2023。https://arxiv.org/abs/2309.06180

### 练习

**实现 KV 缓存管理**

实现一个简化的 KV 缓存管理器，以理解 vLLM 在推理过程中如何管理内存。

**要求：**

类签名：

```python
class KVCacheManager:
    def __init__(self, num_layers: int, num_heads: int, head_dim: int, block_size: int, num_blocks: int):
        """Initialize KV cache with block-based allocation."""
        pass

    def allocate(self, seq_id: int, num_tokens: int) -> list[int]:
        """Allocate blocks for a sequence. Return block indices."""
        pass

    def free(self, seq_id: int):
        """Free all blocks for a sequence."""
        pass

    def get_cache(self, seq_id: int) -> tuple[torch.Tensor, torch.Tensor]:
        """Get K and V tensors for a sequence."""
        pass
```

- 使用基于块的分配（类似 PagedAttention）
- 跟踪空闲和已分配的块
- 支持序列生成 token 时的动态增长
- 实现碎片整理（可选）

**测试你的实现：**

```python
import torch

# Initialize cache manager
cache_mgr = KVCacheManager(
    num_layers=32,
    num_heads=32,
    head_dim=128,
    block_size=16,
    num_blocks=1000
)

# Allocate for multiple sequences
seq1_blocks = cache_mgr.allocate(seq_id=1, num_tokens=100)
seq2_blocks = cache_mgr.allocate(seq_id=2, num_tokens=50)
print(f"Seq 1: {len(seq1_blocks)} blocks")
print(f"Seq 2: {len(seq2_blocks)} blocks")
print(f"Free blocks: {cache_mgr.num_free_blocks}")

# Free sequence 1
cache_mgr.free(seq_id=1)
print(f"After free: {cache_mgr.num_free_blocks} free blocks")
```

**实现连续批处理**

创建一个简单的连续批处理调度器，按请求到达顺序处理它们。

**要求：**

类签名：

```python
class ContinuousBatchingScheduler:
    def __init__(self, max_batch_size: int, max_seq_len: int):
        pass

    def add_request(self, request_id: int, prompt_tokens: list[int]):
        """Add a new request to the queue."""
        pass

    def step(self) -> dict:
        """Execute one generation step. Return completed requests."""
        pass

    def get_batch(self) -> list[int]:
        """Get current batch of request IDs."""
        pass
```

- 维护一个待处理请求的队列
- 当有空间时将新请求添加到运行批次
- 立即移除已完成的请求
- 优先为新请求进行预填充

**测试你的实现：**

```python
scheduler = ContinuousBatchingScheduler(max_batch_size=8, max_seq_len=2048)

# Simulate incoming requests
import time
import random

for i in range(20):
    # Add request with random prompt length
    prompt_len = random.randint(10, 100)
    scheduler.add_request(request_id=i, prompt_tokens=list(range(prompt_len)))

    # Run generation steps
    for _ in range(5):
        completed = scheduler.step()
        if completed:
            print(f"Completed: {completed}")
    print(f"Step {i}: batch size = {len(scheduler.get_batch())}")
```

**基准测试 vLLM 吞吐量**

编写一个全面的基准测试程序，用于在不同配置下测试 vLLM 推理吞吐量。

**要求：**

测试变量：

- 批次大小：1, 4, 8, 16, 32




- 输入长度：128、512、1024、2048
- 输出长度：64、128、256、512
- 张量并行：1、2、4 个 GPU
- 测量指标：
  - 吞吐量（token/秒）
  - 首个 token 生成时间（TTFT）
  - 每个输出 token 的时间（TPOT）
  - GPU 内存利用率
- 生成图表并进行分析

#### 测试你的实现：

```python
from vllm import LLM, SamplingParams

def benchmark_vllm(
    model_name: str,
    batch_size: int,
    input_len: int,
    output_len: int,
    tp_size: int = 1
) -> dict:
    """Benchmark vLLM with specific configuration."""
    llm = LLM(model=model_name, tensor_parallel_size=tp_size)

    # Generate test prompts
    prompts = ["Hello " * (input_len // 2)] * batch_size
    sampling_params = SamplingParams(max_tokens=output_len)

    # Warmup
    _ = llm.generate(prompts[:1], sampling_params)

    # Benchmark
    import time
    start = time.time()
    outputs = llm.generate(prompts, sampling_params)
    elapsed = time.time() - start

    total_tokens = sum(len(o.outputs[0].token_ids) for o in outputs)
    throughput = total_tokens / elapsed

    return {
        "throughput": throughput,
        "latency": elapsed / batch_size,
        "ttft": outputs[0].metrics.first_token_time,
    }

# Run benchmarks
results = []
for batch_size in [1, 4, 8, 16]:
    for input_len in [128, 512, 1024]:
        metrics = benchmark_vllm(
            "Qwen/Qwen2.5-0.5B-Instruct",
            batch_size, input_len, output_len=128
        )
        results.append({"batch": batch_size, "input": input_len, **metrics})
        print(f"Batch={batch_size}, Input={input_len}: {metrics['throughput']:.1f} tok/s")
```

#### 实现投机解码

创建一个简单的投机解码实现，以理解该技术。

#### 要求：

- 使用一个小型草稿模型生成候选 token
- 使用目标模型验证候选 token
- 实现拒绝采样以确保正确性
- 测量与标准解码相比的加速比
- 处理可变的接受率

#### 测试你的实现：

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

class SpeculativeDecoder:
    def __init__(self, target_model, draft_model, tokenizer, num_speculative: int = 4):
        self.target = target_model
        self.draft = draft_model
        self.tokenizer = tokenizer
        self.num_speculative = num_speculative

    def generate(self, prompt: str, max_tokens: int) -> str:
        """Generate with speculative decoding."""
        pass

    def _draft_tokens(self, input_ids: torch.Tensor) -> torch.Tensor:
        """Generate speculative tokens with draft model."""
        pass

    def _verify_tokens(self, input_ids: torch.Tensor, draft_tokens: torch.Tensor) -> tuple[torch.Tensor, int]:
        """Verify draft tokens with target model. Return accepted tokens."""
        pass

# Test speculative decoding
target = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-3.2-1B").cuda()
draft = AutoModelForCausalLM.from_pretrained("Qwen/Qwen2.5-0.5B").cuda()
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-3.2-1B")
decoder = SpeculativeDecoder(target, draft, tokenizer, num_speculative=4)

# Compare with standard decoding
prompt = "The capital of France is"
speculative_output = decoder.generate(prompt, max_tokens=50)
print(f"Speculative: {speculative_output}")
```

#### 部署带 OpenAI 兼容 API 的 vLLM

搭建一个提供 OpenAI 兼容 API 的 vLLM 服务器，并编写客户端与之交互。

#### 要求：

- 使用 OpenAI API 启动 vLLM 服务器
- 实现用于聊天补全的客户端
- 支持流式响应
- 添加请求超时和重试逻辑
- 测量延迟分布

#### 测试你的实现：

```bash
# Start server (in terminal)
python -m vllm.entrypoints.openai.api_server \
    --model Qwen/Qwen2.5-0.5B-Instruct \
    --port 8000
```

```python
import httpx
import asyncio
from typing import AsyncIterator

class VLLMClient:
    def __init__(self, base_url: str = "http://localhost:8000"):
        self.base_url = base_url
        self.client = httpx.AsyncClient(timeout=60.0)

    async def chat(self, messages: list[dict], stream: bool = False) -> str | AsyncIterator[str]:
        """Send chat completion request."""
        pass

    async def list_models(self) -> list[str]:
        """List available models."""
        pass

async def main():
    client = VLLMClient()
    # List models
    models = await client.list_models()
    print(f"Available models: {models}")

    # Chat completion
    messages = [{"role": "user", "content": "What is machine learning?"}]
    response = await client.chat(messages)
    print(f"Response: {response}")

    # Streaming
    print("Streaming: ", end="")
    async for chunk in await client.chat(messages, stream=True):
        print(chunk, end="", flush=True)
    print()

asyncio.run(main())
```

#### 预期学习成果

完成这些练习后，你应该能够：

- 理解 KV 缓存管理和基于块的分配
- 实现连续批处理以实现高效推理
- 对 vLLM 吞吐量进行基准测试和优化
- 理解投机解码及其权衡
- 部署并与 vLLM 的 OpenAI 兼容 API 交互
- 分析推理性能瓶颈

#### 订阅 Deep Engineering

加入成千上万希望理解软件如何变化、深化专业知识并构建持久系统的开发者和架构师。

Deep Engineering 是一份由专家主导的每周通讯，面向经验丰富的从业者，提供关于架构、系统设计和现代编程实践的原创分析、技术访谈和精选见解。

扫描二维码或访问链接即可免费订阅。

https://packt.link/deep-engineering-newsletter

![](img/522d0e44234972210ef66d7489ddbd04_354_0.png)
