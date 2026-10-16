## 11. 部署多智能体系统

在上一章中，我们介绍了多智能体系统的测试、调试和故障排除。我们探讨了常见的故障模式（如幻觉和工具误用）、仪表盘和可观测性策略、协调失败的调试技术，以及通过冗余和优雅降级设计弹性。我们全程使用 MAKDO 作为具体示例来说明这些概念。

现在，是时候真正部署 MAKDO 了。但在生产环境中运行多智能体系统会带来新的挑战，涉及部署架构、服务发现、网络和安全。如何跨多个环境部署智能体？它们如何安全通信？如何管理凭据和访问控制？

在本章中，我们将在接近真实生产环境的配置中部署 MAKDO。我们将使用 **Docker 中的 Kubernetes**（**KinD**）创建两个 Kubernetes 集群。控制集群运行 MAKDO 本身。工作集群是 MAKDO 监控和管理的目标集群。k8s-ai（一个智能体到智能体服务器）运行在工作集群上，通过 **Agent-to-Agent**（**A2A**）协议为 MAKDO 提供诊断和修复问题的能力。这种设置模拟了真实的生产场景，即管理系统与其管理的工作负载分开运行。

我们将逐步完成两个集群的设置、将 MAKDO 组件部署到控制集群、将 k8s-ai 部署到工作集群、配置安全的跨集群通信，以及管理服务发现和访问控制。最终，你将拥有一个功能完备的多智能体系统，能够管理单独的 Kubernetes 集群，并自动检测和修复问题。

本章涵盖以下要点：

- 为多智能体部署设置双 KinD 集群
- 将 MAKDO 部署到控制集群
- 将 k8s-ai 部署到工作集群
- 实现集群间的安全通信

### 技术要求

请按照 `https://github.com/PacktPublishing/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/tree/main/ch11/makdo` 上的说明操作。

### 为多智能体部署设置双 KinD 集群

让我们构建一个接近生产环境的部署，让 MAKDO 在一个集群上运行并管理另一个集群。在真实环境中，这种分离对于将管理基础设施与生产工作负载隔离至关重要。它缩小了爆炸半径，即使 MAKDO 无法运行，生产系统也能继续运转。此外，如果生产集群遇到资源耗尽或类似问题，MAKDO 将能够进行分析并可能修复它，因为它不受相同问题的影响。

### 安装 KinD 和先决条件

在创建集群之前，你需要安装 Docker、kubectl 和 KinD。我们不会逐一介绍每个工具的安装步骤（这些步骤因平台而异并随时间变化），而是提供一个验证脚本来检查一切是否就绪。

首先，如果你还没有克隆本书的仓库，请先克隆：

```
git clone git@github.com:PacktPublishing/Design-Multi-Agent-AI-Systems-using-MCP-and-A2A.git
```

使用官方文档安装所需工具：

- **Docker**：`https://docs.docker.com/get-docker/`
- **kubectl**：`https://kubernetes.io/docs/tasks/tools/`
- **KinD**：`https://kind.sigs.k8s.io/docs/user/quick-start/#installation`

安装完成后，使用我们的先决条件检查脚本验证一切是否正常：

```
cd ch11
chmod +x check-prerequisites.sh
./check-prerequisites.sh
```

该脚本会检查 Docker 是否已安装并运行、kubectl 是否可用、KinD 是否已安装（建议使用 0.20.0 或更高版本）、磁盘空间是否足够（两个集群约需 10 GB）、Docker 内存分配是否充足（至少需要 4 GB），以及现有 KinD 集群是否可能与我们的设置冲突。

如果脚本报告任何缺失的先决条件，请使用前面的链接安装它们，然后再次运行脚本。当你看到 **All prerequisites met!** 时，就可以创建集群了。

### 创建控制集群配置

控制集群托管 MAKDO 智能体。它需要配置为允许外部访问 MAKDO 的 API 端点。KinD 使用 YAML 配置文件来定义集群拓扑和网络。

创建 `control-cluster.yaml`：

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: makdo-control
nodes:
  - role: control-plane
    kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "cluster-role=control"
    extraPortMappings:
      - # 用于访问 MAKDO API/仪表板的端口（如需要）
        containerPort: 30080
        hostPort: 8080
        protocol: TCP
      - # 用于 MAKDO 健康检查端点的端口
        containerPort: 30090
        hostPort: 9090
        protocol: TCP
```

以下是一些关键配置点：

- `name`：`makdo-control` 用于标识该集群。KinD 将这个名字用于 Docker 容器和 kubectl 上下文。
- `node-labels`：标签有助于识别 Pod 运行在哪个集群中，对于调试和监控很有用。
- `extraPortMappings`：将集群中的端口暴露到宿主机。端口 `8080` 映射到容器端口 `30080`（Kubernetes 中的 `NodePort` 范围）。端口 `9090` 映射到 `30090` 用于健康检查。这些映射让你可以从本地机器访问 MAKDO 服务，或者在需要时让工作集群也能访问 MAKDO。

接下来，创建控制集群：

```
kind create cluster --config control-cluster.yaml
```

这需要 1–2 分钟。KinD 会下载 Kubernetes 节点镜像（如果未缓存）、创建 Docker 容器并初始化 Kubernetes。完成后，你会看到以下输出：

```
Creating cluster "makdo-control" ...
 ✓ Ensuring node image (kindest/node:v1.33.1) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-makdo-control"
```

验证集群：

```
kubectl cluster-info --context kind-makdo-control
kubectl get nodes --context kind-makdo-control
```

> **注意**：确切的 Kubernetes 版本（本例中为 v1.33.1）取决于你安装的 KinD 版本。KinD 会自动使用兼容的 Kubernetes 版本。




预期输出如下：

```
Kubernetes control plane is running at https://127.0.0.1:60905
CoreDNS is running at https://127.0.0.1:60905/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

| NAME | STATUS | ROLES | AGE | VERSION |
|------|--------|-------|-----|---------|
| makdo-control-control-plane | Ready | control-plane | 1m | v1.33.1 |

一个节点处于 `Ready` 状态。集群正在运行，但为空。

### 创建工作集群配置

工作集群是您实际工作负载运行的地方。MAKDO 会监控此集群，并在问题出现时进行修复。此集群需要暴露 k8s-ai 的 MCP 服务器，以便 MAKDO 与其通信。

创建 `worker-cluster.yaml`：

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: makdo-worker
nodes:
  - role: control-plane
    kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "cluster-role=worker"
    extraPortMappings:
      # Port for k8s-ai MCP server
      - containerPort: 30100
        hostPort: 8100
        protocol: TCP
      # Port for exposing problematic workloads (for testing)
      - containerPort: 30200
        hostPort: 8200
        protocol: TCP
```

与控制集群相比，主要区别如下：

- `name`：`makdo-worker` 用于区别于控制集群。
- `node-labels`：`cluster-role=worker` 将此集群标识为受管集群。
- `Port 8100`：映射到容器端口 `30100`，k8s-ai 的 A2A 服务器将在此运行。MAKDO 的 Analyzer 和 Fixer 代理通过此端口使用 A2A 协议调用 k8s-ai。
- `Port 8200`：用于测试工作负载。我们将在此部署有问题的 Pod，供 MAKDO 检测和修复。

创建工作集群：

```bash
kind create cluster --config worker-cluster.yaml
```

现在您有两个独立的 Kubernetes 集群以 Docker 容器形式运行：

```bash
kind get clusters
```

预期输出如下（您也可能会看到其他集群）：

```
makdo-control
makdo-worker
```

检查两个集群的上下文：

```bash
kubectl config get-contexts | grep makdo
```

输出如下：

```
  kind-makdo-control   kind-makdo-control   kind-makdo-control
* kind-makdo-worker    kind-makdo-worker    kind-makdo-worker
```

`*` 符号表示当前激活的上下文。使用以下命令在它们之间切换：

```bash
# Use control cluster
kubectl config use-context kind-makdo-control

# Use worker cluster
kubectl config use-context kind-makdo-worker
```

两个集群相互隔离。如果没有显式网络配置，一个集群中的 Pod 无法直接访问另一个集群中的 Pod。我们接下来将进行此类配置。

### 设置集群网络和端口转发

KinD 集群以 Docker 容器形式运行在宿主机上。它们位于同一个 Docker 网络中，这意味着它们可以通过 Docker 网络互相访问。但集群内的服务需要特殊配置才能被访问。

首先，找到两个集群使用的 Docker 网络：

```bash
docker network ls | grep kind
```

输出如下：

```
aadf3e99800d   kind   bridge   local
```

两个集群容器都连接到此 `kind` 网络：

```bash
docker network inspect kind | grep -A 3 "makdo"
```

这会显示两个集群容器的 IP 地址。但是，我们不希望硬编码 IP。相反，我们将使用前面配置的端口映射。

对于跨集群通信，MAKDO（位于控制集群上）需要访问 k8s-ai（位于工作集群上）。由于两个集群位于同一宿主机上，MAKDO 可以通过 `host.docker.internal:8100` 访问 k8s-ai。这个特殊的 DNS 名称会从 Docker 容器内部解析到宿主机。

让我们验证端口映射是否正常工作。检查映射端口上的监听情况：

```bash
# Control cluster ports
lsof -i :8080 2>/dev/null || echo "Port 8080: not yet in use"
lsof -i :9090 2>/dev/null || echo "Port 9090: not yet in use"

# Worker cluster ports
lsof -i :8100 2>/dev/null || echo "Port 8100: not yet in use"
lsof -i :8200 2>/dev/null || echo "Port 8200: not yet in use"
```

目前这些端口尚未使用，因为我们还没有部署任何服务。端口映射已配置，但在服务绑定到内部端口（`30080`、`30090`、`30100` 和 `30200`）之前不会激活。

检查 Docker 容器本身是否在运行并具有端口映射：

```bash
docker ps --format "table {{.Names}}\t{{.Ports}}" | grep makdo
```

输出如下：

| NAMES | PORTS |
|-------|-------|
| makdo-worker-control-plane | 127.0.0.1:61462->6443/tcp, 0.0.0.0:8100->30100/tcp, 0.0.0.0:8200->30200/tcp |
| makdo-control-control-plane | 127.0.0.1:60905->6443/tcp, 0.0.0.0:8080->30080/tcp, 0.0.0.0:9090->30090/tcp |

您可以看到两个容器及其端口映射。`6443` 端口是 Kubernetes API 服务器。我们自定义的映射（`8080`->`30080`、`8100`->`30100` 等）已准备好供服务使用。

### 验证跨集群连通性

在部署 MAKDO 和 k8s-ai 之前，请验证两个集群是否健康并且可以相互通信。

检查集群健康状态：

```bash
kubectl cluster-info --context kind-makdo-control
kubectl get nodes --context kind-makdo-control
```

预期输出如下：

```
Kubernetes control plane is running at https://127.0.0.1:60905
CoreDNS is running at https://127.0.0.1:60905/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

| NAME | STATUS | ROLES | AGE | VERSION |
|------|--------|-------|-----|---------|
| makdo-control-control-plane | Ready | control-plane | 15m | v1.33.1 |

现在检查工作集群：

```bash
kubectl cluster-info --context kind-makdo-worker
kubectl get nodes --context kind-makdo-worker
```

预期输出如下：

```
Kubernetes control plane is running at https://127.0.0.1:61462
CoreDNS is running at https://127.0.0.1:61462/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

| NAME | STATUS | ROLES | AGE | VERSION |
|------|--------|-------|-----|---------|
| makdo-worker-control-plane | Ready | control-plane | 12m | v1.33.1 |

两个集群的控制平面都在运行，并且有一个节点处于 `Ready` 状态。

测试每个集群内的基本网络：

```bash
# Test DNS in control cluster
kubectl run dns-test --image=busybox:latest --restart=Never --rm -i --context \
  kind-makdo-control --command -- \
  nslookup kubernetes.default.svc.cluster.local

# Test DNS in worker cluster
kubectl run dns-test --image=busybox:latest --restart=Never --rm -i --context \
  kind-makdo-worker --command -- \
  nslookup kubernetes.default.svc.cluster.local
```

两者都应成功解析 Kubernetes 服务 DNS 名称。

对于跨集群通信，MAKDO 不会直接访问工作集群中的 Pod。而是通过暴露的端口调用 k8s-ai 的 A2A 服务器。我们将在 k8s-ai 部署后测试这一点。集群已准备好进入下一步。

### 向工作集群部署测试工作负载

为了验证 MAKDO 是否正常工作，我们需要包含问题的工作负载供其检测和修复。让我们部署一个包含三个 Pod 的测试命名空间：一个崩溃的 Pod、一个镜像拉取错误的 Pod 和一个健康的 Pod。

创建 `test-workload.yaml`：

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-workload
---
# A pod that will crashloop - missing required environment variable
apiVersion: v1
kind: Pod
metadata:
  name: crashloop-pod
  namespace: test-workload
  labels:
    app: crashloop-test
spec:
  containers:
    - name: app
      image: busybox:latest
      command: ["sh", "-c"]
      args:
        - |
          if [ -z "$REQUIRED_VAR" ]; then
            echo "ERROR: REQUIRED_VAR not set"
            exit 1
          fi
          echo "Running successfully"
          sleep 3600
---
# A pod with an image that doesn't exist
apiVersion: v1
kind: Pod
metadata:
  name: imagepull-pod
  namespace: test-workload
  labels:
    app: imagepull-test
spec:
  containers:
    - name: app
      image: nonexistent-registry.example.com/fake-image:v1.0.0
      command: ["sleep", "3600"]
---
# A healthy pod for comparison
apiVersion: v1
kind: Pod
metadata:
  name: healthy-pod
  namespace: test-workload
  labels:
    app: healthy-test
spec:
  containers:
```


```yaml
- name: app
  image: nginx:alpine
  ports:
    - containerPort: 80
```

部署到工作集群：

```bash
kubectl apply -f test-workload.yaml --context kind-makdo-worker
```

检查 Pod 状态：

```bash
kubectl get pods -n test-workload --context kind-makdo-worker
```

输出如下：

| NAME | READY | STATUS | RESTARTS | AGE |
|------|-------|--------|----------|-----|
| crashloop-pod | 0/1 | CrashLoopBackOff | 7 (27s ago) | 11m |
| healthy-pod | 1/1 | Running | 0 | 11m |
| imagepull-pod | 0/1 | ImagePullBackOff | 0 | 11m |

完美！我们得到了以下结果：

- `crashloop-pod`：`CrashLoopBackOff`（因缺少环境变量而立即退出）
- `healthy-pod`：Running（`nginx` 成功启动）
- `imagepull-pod`：`ImagePullBackOff`（镜像不存在）

观察 `crashloop-pod` 反复重启：

```bash
kubectl get pods -n test-workload --context kind-makdo-worker --watch
```

按 *Ctrl* + *C* 停止观察。这些故障 Pod 是 MAKDO 的理想目标。Analyzer 会检测它们、分类故障类型，Fixer 可能会尝试修复。

查看 Pod 日志以了解失败原因：

```bash
kubectl logs crashloop-pod -n test-workload --context kind-makdo-worker
```

输出如下：

```
ERROR: REQUIRED_VAR not set
```

工作集群现在存在有问题的负载。接下来，我们将 MAKDO 部署到控制集群、将 k8s-ai 部署到工作集群，然后观察 MAKDO 自动检测并处理这些问题。

### 将 MAKDO 部署到控制集群

现在两个集群都已运行，测试负载也在工作集群中持续失败，是时候部署 MAKDO 了。MAKDO 将在控制集群中以 Deployment 形式运行，并通过 k8s-ai 的 A2A 服务器监控工作集群。

#### 准备 MAKDO 配置

MAKDO 需要配置文件来了解要监控哪些集群、如何连接 k8s-ai，以及如何通过 Slack 通信。我们将根据示例配置来适配我们的双集群环境。

查看 `https://github.com/PacktPublishing/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/tree/main/ch11/makdo` 了解详情。

以下是关键配置点：

- **Clusters**：只列出工作集群。MAKDO 不需要监控自身的控制集群。`kind-makdo-worker` 上下文与 KinD 创建的 kubectl 上下文一致。
- `kubeconfig_path`：设置为 `/root/.kube/config`，因为 `kubeconfig` 将被挂载到 MAKDO 容器内此路径。我们会将 `kubeconfig` 作为卷挂载。
- `k8s_ai.base_url`：使用 `host.docker.internal:8100` 访问工作集群上的 k8s-ai。这个特殊 DNS 名称在 Docker 容器内解析为主机。端口 `8100` 是 k8s-ai 的 A2A 服务器映射端口。
- **环境变量**：API 密钥和令牌使用 `${VAR}` 语法。MAKDO 从环境变量加载这些值，我们将通过 Kubernetes secrets 提供。
- `monitoring.check_interval`：设置为 120 秒（2 分钟）。MAKDO 每两分钟轮询一次工作集群，以便快速发现问题。

接下来，我们需要代理配置文件。MAKDO 为每个代理的提示词和工具配置使用单独的 YAML 文件。

创建 `makdo-config/coordinator.yaml`：

```yaml
name: "MAKDO Coordinator"
description: |
  You are the MAKDO Coordinator, the central orchestrator for multi-cluster Kubernetes operations.
  Your responsibilities:
  1. Periodically check cluster health across all registered clusters
  2. Coordinate between Analyzer, Fixer, and Slack Bot agents
  3. Escalate critical issues to appropriate agents
  4. Maintain operational awareness of all clusters
  When asked to perform health checks:
  1. Use the Analyzer agent to assess cluster health
  2. If issues are found, use the Slack Bot to notify users
  3. For critical issues, use the Fixer agent if remediation is needed
  4. Always report status back to the Slack channel
  Be proactive, clear, and focused on cluster reliability.
model_id: "gpt-4o"
sub_agents:
  - name: "MAKDO_Analyzer"
    config_path: "src/makdo/agents/analyzer.yaml"
  - name: "MAKDO_Fixer"
    config_path: "src/makdo/agents/fixer.yaml"
  - name: "MAKDO_Slack_Bot"
    config_path: "src/makdo/agents/slack.yaml"
history:
  max_messages: 10
  summarize_after: 5
```

创建 `makdo-config/analyzer.yaml`：

```yaml
name: "MAKDO Analyzer"
description: |
  You are the MAKDO Analyzer agent. Your role is to diagnose Kubernetes cluster health issues.
  Your responsibilities:
  1. Check cluster resource health (pods, deployments, services, nodes)
  2. Identify problems like CrashLoopBackOff, ImagePullBackOff, resource exhaustion
  3. Analyze logs and events to understand root causes
  4. Provide clear diagnostic reports to the Coordinator
  When analyzing issues:
  1. Always check ALL namespaces, not just default
  2. Look at pod status, events, and logs
  3. Identify patterns across multiple failing pods
  4. Classify issues by severity (critical, warning, info)
  5. Provide actionable recommendations
  Use the k8s diagnostic tools available to you through the A2A protocol.
model_id: "gpt-4o"
tools:
  - name: "kubernetes_resource_health"
    type: "a2a"
    endpoint: "http://host.docker.internal:8100"
    description: "Check health of Kubernetes resources (pods, deployments, services, nodes)"
  - name: "kubernetes_diagnose_issue"
    type: "a2a"
    endpoint: "http://host.docker.internal:8100"
    description: "Diagnose specific Kubernetes issues by analyzing events, logs, and resource state"
history:
  max_messages: 8
  summarize_after: 4
```

创建 `makdo-config/fixer.yaml`：

```yaml
name: "MAKDO Fixer"
description: |
  You are the MAKDO Fixer agent. Your role is to remediate Kubernetes cluster issues safely.
  Your responsibilities:
  1. Apply fixes for diagnosed issues from the Analyzer
  2. Always prioritize safety and avoid destructive actions
  3. Request approval for critical operations
  4. Verify fixes were successful after applying
  Common remediation actions:
  - Restart failing pods
  - Update pod configurations to fix missing env vars
  - Scale deployments
  - Apply configuration changes
  CRITICAL SAFETY RULES:
  1. Never delete resources without explicit approval
  2. Always verify before applying changes
  3. If unsure, ask for human approval
  4. Test fixes on non-production resources first when possible
  Use the k8s remediation tools available through the A2A protocol.
model_id: "gpt-4o"
tools:
  - name: "kubernetes_apply_fix"
    type: "a2a"
    endpoint: "http://host.docker.internal:8100"
    description: "Apply fixes to Kubernetes resources (restart pods, update configs, scale)"
  - name: "kubernetes_verify_fix"
    type: "a2a"
    endpoint: "http://host.docker.internal:8100"
    description: "Verify that applied fixes resolved the issue"
history:
  max_messages: 6
  summarize_after: 3
```

创建 `makdo-config/slack.yaml`：

```yaml
name: "MAKDO Slack Bot"
description: |
  You are the MAKDO Slack Bot agent. Your role is to communicate with users via Slack.
  Your responsibilities:
  1. Post cluster health reports to the *#makdo-devops channel*
  2. Alert users to critical issues
  3. Provide status updates on ongoing operations
  4. Respond to user questions about cluster health
  Message formatting:
  - Use clear, concise language
  - Highlight critical issues with appropriate urgency
  - Include relevant details (pod names, namespaces, error messages)
  - Provide actionable next steps when possible
  Always post to *#makdo-devops channel unless told otherwise.*
model_id: "gpt-4o"
mcp_servers:
  slack:
    command: "npx"
    args: ["@korotovsky/slack-mcp-server"]
    env:
      SLACK_BOT_TOKEN: "${AI6_BOT_TOKEN}"
      SLACK_TEAM_ID: "${SLACK_TEAM_ID}"
history:
  max_messages: 5
  summarize_after: 3
```

这些配置文件定义了每个代理的行为、工具及通信方式。Analyzer 和 Fixer 代理使用指向 k8s-ai 的 A2A 工具。`Slack_Bot` 代理使用 MCP 服务器进行 Slack 集成。

#### 为 API 密钥创建 Kubernetes Secrets

MAKDO 需要 OpenAI 的 API 密钥（用于运行基于 LLM 的代理）和 Slack 的 API 密钥（用于发布消息）。我们将把这些密钥作为 Kubernetes secrets 存储在控制集群中。

首先，创建一个包含你的 API 密钥的 `.env` 文件：

```bash
cat > .env <<EOF
OPENAI_API_KEY=sk-...your-key-here...
AI6_BOT_TOKEN=xoxb-...your-slack-bot-token...
AI6_APP_TOKEN=xapp-...your-slack-app-token...
SLACK_TEAM_ID=T...your-team-id...
EOF
```



将占位符替换为你的实际密钥。如果你还没有这些密钥，请按以下方式获取：

- **OpenAI API 密钥**：从 `https://platform.openai.com/api-keys` 获取
- **Slack 令牌**：在 `https://api.slack.com/apps` 创建一个 Slack 应用，并配置机器人令牌作用域（`chat:write` 和 `channels:read`），以及用于 socket 模式的应用级令牌
- **Slack 团队 ID**：在你的 Slack 工作区设置中的 **工作区 ID** 下查找

在控制集群中创建 Kubernetes secret：

```bash
# Switch to control cluster
kubectl config use-context kind-makdo-control

# Load environment variables
set -a
source .env
set +a

# Create secret
kubectl create secret generic makdo-secrets \
  --from-literal=openai-api-key="$OPENAI_API_KEY" \
  --from-literal=slack-bot-token="$AI6_BOT_TOKEN" \
  --from-literal=slack-app-token="$AI6_APP_TOKEN" \
  --from-literal=slack-team-id="$SLACK_TEAM_ID" \
  -n default
```

验证 secret 是否已创建：

```bash
kubectl get secret makdo-secrets -n default
```

输出如下：

| 名称 | 类型 | 数据 | 存活时间 |
| --- | --- | --- | --- |
| makdo-secrets | Opaque | 4 | 5s |

secret 已创建。不必担心值会被隐藏，这正是 secret 的意义所在。你可以验证这些密钥是否存在：

```bash
kubectl describe secret makdo-secrets -n default
```

输出如下：

```
Name: makdo-secrets
Namespace: default
Labels: <none>
Annotations: <none>
Type: Opaque

Data
====
openai-api-key: 51 bytes
slack-app-token: 53 bytes
slack-bot-token: 56 bytes
slack-team-id: 11 bytes
```

secret 已就绪。我们将在 MAKDO 部署中把这些值挂载为环境变量。

### 创建 kubeconfig ConfigMap

MAKDO 需要访问工作集群的 `kubeconfig` 文件来管理它。我们将创建一个包含 `kubeconfig` 文件的 `ConfigMap` 资源，并将其挂载到 MAKDO Pod 中。请注意，在生产环境中，出于安全考虑，最好将其存储在 secret 中。

提取工作集群的 `kubeconfig` 并创建一个 `ConfigMap`：

```bash
# Create kubeconfig ConfigMap from your local kubeconfig
kubectl create configmap makdo-kubeconfig \
  --from-file=config=$HOME/.kube/config \
  --context kind-makdo-control
```

该 `ConfigMap` 将被挂载到 MAKDO 容器中的 `/root/.kube` 目录下，使其能够同时访问控制集群和工作集群。

验证 `ConfigMap` 是否已创建：

```bash
kubectl get configmap makdo-kubeconfig --context kind-makdo-control
```

输出如下：

| 名称 | 数据 | 存活时间 |
| --- | --- | --- |
| makdo-kubeconfig | 1 | 5s |

现在，MAKDO 在连接工作集群时可以使用该 `kubeconfig` 文件。

### 将 MAKDO 部署为 Kubernetes 部署

MAKDO 将作为一个单副本的 Deployment 运行。如果 Pod 崩溃，Deployment 会自动重启，并且可以通过滚动发布新版本来轻松更新配置。

以下是部署清单：

`https://github.com/PacktPublishing/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/blob/main/ch11/makdo/deployment.yaml`

以下是关键的部署配置：

- `ConfigMap`：包含 `makdo.yaml` 配置文件。这让我们无需重新构建 Docker 镜像即可更新配置。
- `replicas: 1`：MAKDO 的单个实例。多个副本会导致重复的健康检查和协调冲突。在生产环境中，建议使用 leader 选举，以便有一个待命实例随时接管。
- `imagePullPolicy: Never`：我们将在本地构建 MAKDO 镜像并加载到 KinD 中。永不从镜像仓库拉取，以获取最新代码。
- **`env` 部分**：从 `makdo-secrets` secret 加载的环境变量。这些变量会覆盖配置中的 `${VAR}` 占位符。
- `volumeMounts`：挂载了两个卷：
  - `/app/config`：来自 `ConfigMap` 的 MAKDO 配置
  - `/root/.kube`：来自 `ConfigMap` 的 `kubeconfig` 文件，以便 MAKDO 可以访问两个集群
- `resources`：内存和 CPU 限制可防止 MAKDO 消耗过多资源。对于 Python 进程和 LLM 调用来说，512 Mi 到 1 Gi 是合理的。
- `volumes`：挂载了两个 `ConfigMap` 资源：
  - `makdo-config`：包含 MAKDO 配置文件
  - `makdo-kubeconfig`：包含用于访问两个集群的 `kubeconfig` 文件

在部署之前，我们需要构建 MAKDO Docker 镜像并将其加载到控制集群中。

### 构建并加载 MAKDO 镜像

MAKDO 需要被打包为 Docker 镜像。我们将创建一个 Dockerfile，用于安装所有依赖并运行 MAKDO 主进程。

创建 `makdo/Dockerfile`：

```dockerfile
FROM python:3.13-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
  curl \
  git \
  kubectl \
  && rm -rf /var/lib/apt/lists/*

# Install Node.js for Slack MCP server
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
  && apt-get install -y nodejs \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy MAKDO source from the book repository
# Adjust path based on where you cloned the book repo
COPY ../../packt/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/ch09/makdo/src ./src
COPY ../../packt/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/ch09/makdo/pyproject.toml .

# Install MAKDO dependencies
RUN pip install --no-cache-dir ai-six==0.14.4 pyyaml python-dotenv requests

# Create required directories
RUN mkdir -p /app/config /app/logs

# Set Python to run unbuffered so logs appear immediately
ENV PYTHONUNBUFFERED=1

# Run MAKDO main entry point
CMD ["python", "-m", "src.makdo.main"]
```

Dockerfile 使用 Python 3.13。该版本为所有依赖提供了预构建的二进制 wheel，因此无需编译。我们安装 kubectl 用于访问 Kubernetes 集群，安装 Node.js 用于 Slack MCP 服务器。Python 依赖（`ai-six`、`pyyaml`、`python-dotenv` 和 `requests`）可直接从 wheel 安装，无需构建工具。

构建镜像：

```bash
cd ch11/makdo
docker build -t makdo:latest .
```

这需要几分钟时间下载基础镜像并安装依赖。请留意错误。如果构建因缺少 MAKDO 源码而失败，请确认 `src` 目录存在。

构建完成后，将镜像加载到控制集群中：

```bash
kind load docker-image makdo:latest --name makdo-control
```

这会将镜像从本地 Docker 传输到 KinD 集群的内部镜像缓存。输出如下：

```
Image: "makdo:latest" with ID "sha256:..." not yet present on node "makdo-control-control-plane", loading...
```

验证镜像是否可用：

```bash
docker exec -it makdo-control-control-plane crictl images | grep makdo
```

你应该能看到 `makdo` 镜像出现在列表中。现在我们可以进行部署了。

应用部署：

```bash
kubectl apply -f deployment.yaml --context kind-makdo-control
```

输出如下：

```
configmap/makdo-config created
deployment.apps/makdo created
```

检查部署状态：

```bash
kubectl get deployment makdo --context kind-makdo-control
```

输出如下：

| 名称 | 就绪 | 最新 | 可用 | 存活时间 |
| --- | --- | --- | --- | --- |
| makdo | 1/1 | 1 | 1 | 15s |

MAKDO 正在运行。一个副本已就绪。

### 配置 MAKDO 以连接到工作集群

MAKDO 需要 `kubeconfig` 文件来访问工作集群，并需要一个 k8s-ai 会话令牌来使用 A2A 工具。`kubeconfig` 文件已从主机挂载。现在，我们需要确保 MAKDO 能够与 k8s-ai 创建会话。

k8s-ai 服务器尚未部署。我们将在下一节部署它。目前，请先验证 MAKDO 至少能够启动，并能通过 kubectl 访问工作集群。

检查 MAKDO Pod 的日志：

```bash
kubectl logs -l app=makdo --context kind-makdo-control --tail=50
```

你很可能会看到关于 k8s-ai 无法访问的错误。这是正常现象。请寻找表明 MAKDO 成功启动的日志：

```
2025-11-02 10:15:32,123 - makdo - INFO - Starting MAKDO - Multi-Agent Kubernetes DevOps System
2025-11-02 10:15:32,456 - makdo - INFO - Coordinator and sub-agents created successfully
2025-11-02 10:15:32,500 - makdo - INFO - Setting up tool call monitoring...
2025-11-02 10:15:32,550 - makdo - INFO - ✅ Tool call monitoring enabled for all agents
2025-11-02 10:15:32,600 - makdo - INFO - Getting kubeconfig for context: kind-makdo-worker
```

如果你看到有关 kubectl 或 `kubeconfig` 的错误，则 `kubeconfig ConfigMap` 可能缺失或不正确。确认你已使用自己的 `kubeconfig` 文件创建了 `makdo-kubeconfig ConfigMap`。

要从 MAKDO Pod 内部测试 kubectl 访问，请使用以下命令：

```bash
kubectl exec -it deployment/makdo --context kind-makdo-control -- kubectl get nodes --context kind-makdo-worker
```




此命令在 MAKDO 容器内运行 kubectl 来查询工作集群。输出如下：

| 名称 | 状态 | 角色 | 年龄 | 版本 |
| --- | --- | --- | --- | --- |
| makdo-worker-control-plane | 就绪 | 控制平面 | 45m | v1.33.1 |

成功！MAKDO 可以访问工作集群。一旦我们部署 k8s-ai，k8s-ai 连接就会正常工作。

### 验证 MAKDO 部署和日志

让我们验证 MAKDO 是否健康并准备好监控工作集群。检查 Pod 状态：

`kubectl get pods -l app=makdo --context kind-makdo-control`

输出如下：

| 名称 | 就绪 | 状态 | 重启次数 | 年龄 |
| --- | --- | --- | --- | --- |
| makdo-7c9f8b6d5d-x2k4j | 1/1 | 运行中 | 0 | 2m |

Pod 正在运行。检查资源使用情况：

`kubectl top pod -l app=makdo --context kind-makdo-control`

输出如下（需要 `metrics-server`，在 KinD 中可能无法工作）：

```
Error from server (ServiceUnavailable): the server is currently unable to handle the request (get pods.metrics.k8s.io)
```

没关系。KinD 默认不包含 `metrics-server`。我们可以从 Pod 规范中检查资源限制：

`kubectl describe pod -l app=makdo --context kind-makdo-control | grep -A 5 "Limits"`

输出如下：

```
 Limits:
   cpu: 500m
   memory: 1Gi
 Requests:
   cpu: 250m
   memory: 512Mi
```

资源配置正确。现在，检查 MAKDO 日志是否有错误：

`kubectl logs -l app=makdo --context kind-makdo-control --tail=100`

查找以下关键指标：

- **协调器已启动**：`Starting MAKDO Multi-Agent Kubernetes DevOps System`
- **子代理已创建**：`Coordinator and sub-agents created successfully`
- **正在尝试 k8s-ai 会话**：`Creating k8s-ai session for kind-makdo-worker…`
- **健康检查循环已启动**：`Starting health check loop`

你会看到关于 k8s-ai 连接失败的错误。在我们部署 k8s-ai 之前，这是预期的。

重要的是 MAKDO 已启动且没有崩溃。

如果 MAKDO 正在崩溃循环，请检查常见问题：

- **缺少密钥**：验证 `makdo-secrets` 是否存在，并包含所有键：`kubectl get secret makdo-secrets --context kind-makdo-control`
- **缺少 kubeconfig ConfigMap**：在部署 MAKDO 之前，验证是否使用你的 `kubeconfig` 文件创建了 `makdo-kubeconfig ConfigMap`。
- **Python 错误**：检查日志中是否有导入错误或缺少依赖项。你可能需要重新构建镜像。

要实时跟踪日志以查看 MAKDO 正在做什么，请使用以下命令：

`kubectl logs -l app=makdo --context kind-makdo-control --follow`

按 *Ctrl* + *C* 停止。MAKDO 已部署并等待 k8s-ai。接下来，我们将 k8s-ai 部署到工作集群，以便 MAKDO 可以开始监控。

#### 将 k8s-ai 部署到工作集群

既然 MAKDO 已在控制集群上运行，我们需要将 k8s-ai 部署到工作集群。k8s-ai 通过 A2A 协议暴露 Kubernetes 诊断能力，允许 MAKDO 的 Analyzer 和 Fixer 代理检查集群健康状况并执行修复操作。

我们将容器化 k8s-ai，将其部署为 Kubernetes 服务，并暴露它以便 MAKDO 可以跨集群与其通信。

#### 构建 k8s-ai Docker 镜像

k8s-ai 需要在工作集群中以容器形式运行。我们将创建一个 Dockerfile，该文件将 k8s-ai 及其所有依赖项打包，安装 kubectl 用于集群访问，并将其配置为作为 A2A 服务器运行。

创建 `k8s-ai/Dockerfile`：

```dockerfile
FROM python:3.12-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl" && \
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    rm kubectl

WORKDIR /app

# Copy k8s-ai source
COPY k8s_ai ./k8s_ai
COPY pyproject.toml .

# Install k8s-ai and dependencies
RUN pip install --no-cache-dir -e .

# Create directory for API keys
RUN mkdir -p /app/data

# Expose A2A server port and Admin API port
EXPOSE 9999 9998

# Run k8s-ai server in A2A mode
CMD ["k8s-ai-server", "--host", "0.0.0.0", "--port", "9999", "--admin-port", "9998"]
```

以下代码片段的关键点如下：

- **基础镜像**：Python 3.12-slim 提供了一个最小的 Python 环境。
- **kubectl 安装**：k8s-ai 执行 kubectl 命令与集群交互。我们下载最新的稳定版本并安装它。
- **源代码**：复制 `k8s_ai` 包和 `pyproject.toml` 文件。我们使用 `pip install -e .` 将 k8s-ai 安装为可编辑包。
- **数据目录**：`/app/data` 将存储用于身份验证的 API 密钥（`keys.json`）。
- **端口**：端口 `9999` 用于 A2A 协议服务器，端口 `9998` 用于 Admin API（用于创建会话）。
- **CMD**：启动 `k8s-ai-server`，并使用标志绑定到所有接口（`0.0.0.0`），以便可以从容器外部访问。

k8s-ai 源代码位于主 k8s-ai 仓库中。将其复制到 `ch11/k8s-ai` 目录：

```bash
# From the k8s-ai repository root
cp -r k8s_ai /path/to/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/ch11/k8s-ai/
cp pyproject.toml /path/to/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/ch11/k8s-ai/
```

构建镜像：

```bash
cd ch11/k8s-ai
docker build -t k8s-ai:latest .
```

这需要几分钟来下载基础镜像并安装依赖项。完成后，验证镜像：

`docker images | grep k8s-ai`

输出如下：

```
k8s-ai latest abc123def456 2 minutes ago 450MB
```

镜像已构建完成，可以部署到工作集群。

#### 创建 k8s-ai 部署清单

k8s-ai 需要多种 Kubernetes 资源才能运行：一个具有访问集群权限的服务账户，一个用于运行容器的 Deployment，以及一个用于暴露它的 Service。我们将使用**基于角色的访问控制**（**RBAC**）授予 k8s-ai 只读集群访问权限。

以下是 k8s-ai 部署清单：

`https://github.com/PacktPublishing/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/blob/main/ch11/k8s-ai/deployment.yaml`.

关键配置点包括以下内容：

- **服务账户和 RBAC**：`k8s-ai` 服务账户通过 `ClusterRole k8s-ai-reader` 获得只读权限。这允许 k8s-ai 运行 `kubectl get/list/watch` 命令，但阻止任何修改操作。`ClusterRoleBinding` 资源将 `ServiceAccount` 连接到 `ClusterRole`。
- **API 密钥 Secret**：包含带有 `.env` 文件中 API 密钥的 `keys.json`。MAKDO 的 Analyzer 和 Fixer 代理将使用此密钥进行身份验证。该密钥存储为 `${K8S_AI_API_KEY}` 占位符，在应用清单时将从你的环境中替换。
- **Deployment**：运行一个 k8s-ai 副本，`imagePullPolicy: Never`（KinD 使用本地镜像）。容器从 Secret 资源中获取 OpenAI API 密钥。资源限制防止 k8s-ai 消耗过多的内存或 CPU。
- **健康探针**：存活探针通过获取 `/.well-known/agent.json` 处的代理卡片来检查 k8s-ai 是否响应。就绪探针确保 Pod 在标记为可用之前准备好接收流量。
- **Service**：使用 `NodePort` 类型暴露 k8s-ai。端口 `30100` 映射到 A2A 服务器（`9999`），端口 `30099` 映射到 Admin API（`9998`）。记住，在 `worker-cluster.yaml` 中，端口 `30100` 映射到主机端口 `8100`，使得 k8s-ai 可以从控制集群通过 `http://host.docker.internal:8100` 访问。

让我们看看如何部署我们刚刚构建的 k8s-ai 镜像。

#### 将 k8s-ai 部署到工作集群

镜像已构建，清单已创建，我们准备将 k8s-ai 部署到工作集群。首先，将镜像加载到工作集群，创建所需的 Secret，然后应用部署。




将 k8s-ai 镜像加载到 worker 集群：

```bash
kind load docker-image k8s-ai:latest --name makdo-worker
```

以下是输出：

```
Image: "k8s-ai:latest" with ID "sha256:1fe7b6c86eb2..." not yet present on node "makdo-worker-control-plane", loading...
```

这会将镜像从本地 Docker 传输到 worker 集群的内部镜像仓库。

如果没有这一步，pod 将因 `ImagePullBackOff` 而失败，因为 KinD 集群默认情况下无法访问外部镜像仓库。

创建保存 OpenAI API 密钥的 Secret。k8s-ai 需要它来调用 LLM 进行自然语言处理：

```bash
kubectl create secret generic k8s-ai-secrets \
  --from-literal=openai-api-key="${OPENAI_API_KEY}" \
  --context kind-makdo-worker
```

验证 Secret 是否已创建：

```bash
kubectl get secret k8s-ai-secrets --context kind-makdo-worker
```

以下是输出：

| 名称 | 类型 | 数据 | 存活时间 |
| --- | --- | --- | --- |
| k8s-ai-secrets | Opaque | 1 | 5s |

部署前，请确保 `ch11` 目录中存在包含所需凭据的 `.env` 文件。复制示例文件：

```bash
cd ch11
cp .env.example .env
```

编辑 `.env` 并设置你的凭据。

- 将 `K8S_AI_API_KEY` 设置为 `test-key` 以用于开发。
- 对于生产环境，请运行 `k8s-ai-server --generate-key` 生成密钥，并将其赋给 `K8S_AI_API_KEY`。
- 将 `OPENAI_API_KEY` 设置为你的 OpenAI API 密钥。

仅在使用 Slack 机器人时才需要 Slack 凭据。

`.env` 文件位于 `.gitignore` 中，因此你的机密信息不会被提交。

现在，使用 `envsubst` 替换环境变量来应用 `deployment` 清单：

```bash
cd k8s-ai
export $(grep -v '^#' ../.env | xargs)
envsubst < deployment.yaml | kubectl apply -f - --context kind-makdo-worker
```

以下是输出：

```
serviceaccount/k8s-ai created
clusterrole.rbac.authorization.k8s.io/k8s-ai-reader created
clusterrolebinding.rbac.authorization.k8s.io/k8s-ai-reader-binding created
secret/k8s-ai-api-keys created
deployment.apps/k8s-ai created
service/k8s-ai created
```

共创建了六个资源：`ServiceAccount`、`ClusterRole`、`ClusterRoleBinding`、`Secret`（用于 API 密钥）、`Deployment` 和 `Service`。

检查 pod 状态：

```bash
kubectl get pods -l app=k8s-ai --context kind-makdo-worker
```

以下是输出：

| 名称 | 就绪 | 状态 | 重启次数 | 存活时间 |
| --- | --- | --- | --- | --- |
| k8s-ai-7b9c8d6f5d-x2k4j | 1/1 | Running | 0 | 45s |

pod 正在运行。如果你看到 `ImagePullBackOff`，请验证镜像是否已通过 `kind load` 加载。如果你看到 `CrashLoopBackOff`，请检查日志中的错误（通常是缺少 OpenAI API 密钥）。

检查服务：

```bash
kubectl get svc k8s-ai --context kind-makdo-worker
```

以下是输出：

| 名称 | 类型 | 集群 IP | 外部 IP | 端口 | 存活时间 |
| --- | --- | --- | --- | --- | --- |
| k8s-ai | NodePort | 10.96.145.123 | `<none>` | 9999:30100/TCP,9998:30099/TCP | 1m |

该服务暴露了两个节点端口，其中 `30100` 用于 A2A 服务器，`30099` 用于 Admin API。由于我们在 `worker-cluster.yaml` 文件中为 worker 集群配置了 `extraPortMappings`，因此可以从主机通过 `localhost:8100` 和 `localhost:8099` 访问这些端口。

### 暴露 k8s-ai 服务以实现跨集群访问

k8s-ai 现在运行在 worker 集群中，但控制集群中的 MAKDO 需要访问它。在我们的 KinD 设置中，跨集群通信通过 Docker 主机网络进行。worker 集群的节点端口 `30100` 通过我们之前配置的 `extraPortMappings` 字段映射到主机端口 `8100`。

从控制集群的角度来看，worker 集群可通过 `host.docker.internal:8100` 访问。这个特殊的 DNS 名称会解析为 Docker 主机的 IP 地址，从而允许一个 KinD 集群中的容器访问另一个集群中的服务。

让我们验证网络是否正常工作。首先，检查 k8s-ai 服务是否响应 agent card 端点：

```bash
curl http://localhost:8100/.well-known/agent.json
```

以下是输出（为便于阅读已格式化）：

```json
{
  "name": "k8s-ai Diagnostic Agent",
  "description": "Kubernetes AI diagnostic agent with read-only cluster analysis with session-based cluster management",
  "url": "http://0.0.0.0:9999/",
  "version": "2.0.0",
  "capabilities": {
    "streaming": false
  },
  "skills": [
    {
      "id": "kubernetes_diagnostics",
      "name": "Kubernetes Diagnostics",
      "description": "Perform read-only Kubernetes cluster diagnostics, troubleshooting, and health analysis with detailed insights",
      "tags": ["kubernetes", "diagnostics", "troubleshooting", "monitoring", "health"]
    }
  ]
}
```

agent card 确认 k8s-ai 正在响应。现在，通过调用 Admin API 创建会话来测试身份验证。Admin API 需要来自 `.env` 文件的 API 密钥：

```bash
curl -X POST http://localhost:8099/sessions \
  -H "Authorization: Bearer test-key" \
  -H "Content-Type: application/json" \
  -d '{
    "cluster_name": "makdo-worker",
    "ttl_hours": 24
  }'
```

如果你尚未配置 `ServiceAccount` 属性，此请求将因身份验证错误而失败。这是预期行为，因为我们只是在验证端点是否可达。我们将在下一小节中测试完整的会话创建。

这里的关键点是，MAKDO 代理将使用 `http://host.docker.internal:8100` 来访问 k8s-ai 的 A2A 服务器。这已在 MAKDO 的 `coordinator.yaml` 文件中配置：

```yaml
a2a_servers:
  - name: "kind-makdo-worker"
    url: "http://host.docker.internal:8100"
    timeout: 30.0
    api_key: "test-key"
```

网络已就绪。MAKDO 现在可以与 k8s-ai 进行跨集群通信。

### 验证 k8s-ai 部署

部署 k8s-ai 后，验证所有组件是否正常运行且服务可访问。

#### 检查 pod 状态

首先，验证 k8s-ai pod 是否在 worker 集群中运行：

```bash
kubectl get pods -l app=k8s-ai --context kind-makdo-worker -o wide
```

以下是预期输出：

| 名称 | 就绪 | 状态 | 重启次数 | 存活时间 | IP | 节点 |
| --- | --- | --- | --- | --- | --- | --- |
| k8s-ai-75558cb559-n8jl9 | 1/1 | Running | 0 | 20m | 10.244.0.5 | makdo-worker-control-plane |

pod 应显示 `1/1 Running` 状态且无重启。

#### 验证 pod 健康状态

检查 pod 的详细状态和健康探针：

```bash
kubectl describe pod -l app=k8s-ai --context kind-makdo-worker | grep -A 5 "Conditions:"
```

以下是预期输出：

| 类型 | 状态 |
| --- | --- |
| PodReadyToStartContainers | True |
| Initialized | True |
| Ready | True |
| ContainersReady | True |
| PodScheduled | True |

所有条件都应为 `True`，表明 pod 已通过其存活探针和就绪探针。

#### 检查服务器日志

查看 k8s-ai 日志，确认两个服务器都已成功启动：

```bash
kubectl logs -l app=k8s-ai --context kind-makdo-worker --tail=50
```

查找显示 A2A 端点正在响应的健康探针活动：

```
INFO: 10.244.0.1:41262 - "GET /.well-known/agent.json HTTP/1.1" 200 OK
```

Kubernetes 发出的定期健康探针请求确认服务运行正常。

#### 测试 A2A 端点

验证 k8s-ai agent card 是否可从集群外部访问：

```bash
curl -s http://localhost:8100/.well-known/agent.json | python3 -m json.tool | head -30
```

以下是预期响应：

```json
{
  "capabilities": {
    "streaming": false
  },
  "defaultInputModes": [
    "text/plain"
  ],
  "defaultOutputModes": [
    "text/plain"
  ],
  "description": "Kubernetes AI diagnostic agent with read-only cluster analysis with session-based cluster management",
  "name": "k8s-ai Diagnostic Agent",
  "preferredTransport": "JSONRPC",
  "protocolVersion": "0.3.0",
  "security": [
    {
      "BearerAuth": []
    }
  ],
  "securitySchemes": {
    "BearerAuth": {
      "scheme": "bearer",
      "type": "http"
    }
  },
  "skills": [
    {
      "description": "Perform read-only Kubernetes cluster diagnostics..."
    }
  ]
}
```


这确认了 A2A 服务器正在 Pod 内的端口 `9999` 上运行，`NodePort` 服务将端口 `30100` 映射到容器端口，主机端口映射（`8100` → `30100`）允许外部访问，代理卡片遵循 A2A 协议规范，并且已启用 Bearer Token 认证。

#### 验证服务配置

检查 k8s-ai 服务配置：

```bash
kubectl get svc k8s-ai --context kind-makdo-worker -o yaml | grep -A 10 "spec:"
```

以下是预期输出：

```yaml
spec:
  ports:
    - name: a2a
      nodePort: 30100
      port: 9999
      protocol: TCP
      targetPort: 9999
    - name: admin
      nodePort: 30099
      port: 9998
      protocol: TCP
      targetPort: 9998
  selector:
    app: k8s-ai
  type: NodePort
```

这表明 A2A 端口（`9999`）和 Admin API 端口（`9998`）均已通过 `NodePort` 暴露。k8s-ai 已验证并运行，现在工作集群中已有一个诊断代理，可以接收来自控制集群中 MAKDO 的请求。

#### 测试 k8s-ai Admin API 和会话管理

k8s-ai Admin API 提供会话管理功能，允许 MAKDO 创建隔离的会话以与工作集群交互。每个会话都有自己的 `kubeconfig` 和过期时间。

#### 暴露 Admin API 端口

默认情况下，工作集群配置会暴露 A2A 端口（`8100`），但不会暴露 Admin API 端口。更新工作集群配置以添加管理员 API 端口 `8099`：

```yaml
# ch11/worker-cluster.yaml
extraPortMappings:
  # Port for k8s-ai A2A server
  - containerPort: 30100
    hostPort: 8100
    protocol: TCP
  # Port for k8s-ai Admin API
  - containerPort: 30099
    hostPort: 8099
    protocol: TCP
```

如果你已经创建了工作集群，则需要使用更新后的配置重新创建它：

```bash
kind delete cluster --name makdo-worker
kind create cluster --config worker-cluster.yaml
```

然后，按照之前的部署步骤重新部署 k8s-ai。

#### 创建会话

要创建会话，你需要提供集群的 `kubeconfig`。首先，提取工作集群的 `kubeconfig`：

```bash
kubectl config view --context kind-makdo-worker --minify --flatten > /tmp/makdo-worker-kubeconfig.yaml
```

创建会话请求 JSON 文件：

```python
import json

# Read kubeconfig
with open('/tmp/makdo-worker-kubeconfig.yaml', 'r') as f:
    kubeconfig = f.read()

# Create session request
request = {
    "cluster_name": "makdo-worker",
    "ttl_hours": 24,
    "kubeconfig": kubeconfig
}

# Write to file
with open('/tmp/session-request.json', 'w') as f:
    json.dump(request, f)

print("Session request created")
```

现在，使用 Admin API 创建会话：

```bash
curl -s -X POST 'http://localhost:8099/sessions' \
  -H 'Authorization: Bearer test-key' \
  -H 'Content-Type: application/json' \
  -d '@/tmp/session-request.json' | python3 -m json.tool
```

以下是预期响应：

```json
{
  "success": true,
  "session_token": "k8s-ai-session--qSTOZ0ggxnL-w5mMmMNboUBz0GVIGXvxkxfjYY5ND0",
  "cluster_name": "makdo-worker",
  "api_server": "https://127.0.0.1:65089",
  "namespace": "default",
  "connectivity_status": "connected",
  "expires_at": "2025-11-10T04:37:27.297559Z",
  "error": null
}
```

会话令牌（`k8s-ai-session--...`）将用于对 A2A 端点的请求进行身份验证。

#### 列出活动会话

查看所有活动会话：

```bash
curl -s 'http://localhost:8099/sessions' \
  -H 'Authorization: Bearer test-key' | python3 -m json.tool
```

以下是预期响应：

```json
{
  "total_sessions": 1,
  "sessions": [
    {
      "session_token": "k8s-ai-session--qSTOZ0ggxnL-w5mMmMNboUBz0GVIGXvxkxfjYY5ND0",
      "cluster_name": "makdo-worker",
      "api_server": "https://127.0.0.1:65089",
      "namespace": "default",
      "created_at": "2025-11-09T04:37:27.297568Z",
      "expires_at": "2025-11-10T04:37:27.297559Z",
      "is_expired": false
    }
  ]
}
```

#### 删除会话

要在会话过期前手动删除它，请使用以下命令：

```bash
SESSION_TOKEN="k8s-ai-session--qSTOZ0ggxnL-w5mMmMNboUBz0GVIGXvxkxfjYY5ND0"
curl -s -X DELETE "http://localhost:8099/sessions/${SESSION_TOKEN}" \
  -H 'Authorization: Bearer test-key' | python3 -m json.tool
```

以下是预期响应：

```json
{
  "success": true,
  "message": "Session deleted successfully"
}
```

让我们看看如何针对 k8s-ai 服务器运行 A2A 会话。

#### 将会话与 A2A 协议结合使用

MAKDO 在向 k8s-ai 发起 A2A 请求时将使用会话令牌。该令牌通过 `Authorization` 标头传递，并附带特定于方法的参数：

```bash
curl -s -X POST 'http://localhost:8100/' \
  -H 'Authorization: Bearer test-key' \
  -H 'Content-Type: application/json' \
  -d '{
    "jsonrpc": "2.0",
    "method": "kubernetes_diagnose_issue",
    "params": {
      "session_token": "'"${SESSION_TOKEN}"'",
      "issue_description": "Check pod health in default namespace"
    },
    "id": 1
  }'
```

基于会话的方法具有以下几个优点：

- **隔离性**：每个 MAKDO 实例都可以拥有自己的会话，而不会干扰其他实例
- **安全性**：会话会自动过期，从而限制访问的时间窗口
- **灵活性**：不同的会话可以面向不同的命名空间或集群
- **可审计性**：会话的创建和使用都会被记录，以进行安全审计

在 Admin API 测试通过并正常运行的情况下，k8s-ai 现已准备好跨集群为 MAKDO 提供诊断请求服务。

### 在集群之间启用安全通信

MAKDO 部署在控制集群中，k8s-ai 部署在工作集群中，我们需要确保它们之间的安全通信。本节介绍服务发现、网络、访问控制以及已经具备的安全机制，并演示它们如何在 KinD 环境中工作。

#### 服务发现与网络

在多集群部署中，代理需要跨集群边界发现彼此并进行通信。我们的部署使用显式配置和主机网络来实现跨集群通信。

#### MAKDO 如何发现 k8s-ai

MAKDO 使用静态配置来发现 k8s-ai 服务。查看配置：

```bash
kubectl get configmap makdo-config --context kind-makdo-control \
  -o jsonpath='{.data.makdo\.yaml}' | grep -A 5 "k8s_ai:"
```

以下是预期输出：

```yaml
k8s_ai:
  base_url: "http://host.docker.internal:8100"
  admin_api_url: "http://host.docker.internal:8100/admin"
```

关键网络组件包括以下内容：

- `host.docker.internal`：一种特殊的 DNS 名称，在容器内解析为 Docker 主机
- **端口** `8100`：从工作集群中的 `NodePort 30100` 映射到主机端口 `8100`
- **端口** `8099`：从 `NodePort 30099` 映射，用于 Admin API

#### 跟踪请求路径

让我们跟踪一个诊断请求从 MAKDO 流向 k8s-ai 的过程：

```text
MAKDO Pod (control cluster)
  ↓ http://host.docker.internal:8100
Docker Host Network
  ↓ localhost:8100
Worker Cluster Node (Docker container)
  ↓ NodePort 30100
k8s-ai Service (worker cluster)
  ↓ ClusterIP:9999
k8s-ai Pod (worker cluster)
```

验证每个步骤：

```bash
# 1. Check MAKDO can resolve host.docker.internal
kubectl exec deployment/makdo --context kind-makdo-control -- \
    python3 -c "import socket; print(f'host.docker.internal resolves to: {socket.gethostbyname(\"host.docker.internal\")}')"
# 2. Check host can access port 8100
```


```bash
curl -s http://localhost:8100/.well-known/agent.json | python3 -m json.tool | head -10

# 3. Check NodePort service in worker cluster
kubectl get svc k8s-ai --context kind-makdo-worker -o yaml | grep -A 5 "nodePort: 30100"

# 4. Check k8s-ai pod is receiving traffic
kubectl logs deployment/k8s-ai --context kind-makdo-worker --tail=5
```

在生产系统中，这些网络调用链可能会给延迟敏感的操作增加显著延迟。设置一个延迟预算并明确监控和管理它可能会很有用。

为了访问 k8s-ai 服务器，我们需要先发现它。有几种常见模式适用于不同的用例。让我们来探讨一下。

### 服务发现模式

我们的部署演示了静态配置服务发现。生产环境中的其他模式包括：

- **基于 DNS 的发现**：对集群内服务使用 Kubernetes DNS：

  ```yaml
  # For services in the same cluster
  k8s_ai:
    base_url: "http://k8s-ai.default.svc.cluster.local:9999"
  ```

- **服务网格发现**：使用服务网格，如 Istio 或 Linkerd：

  ```yaml
  apiVersion: networking.istio.io/v1beta1
  kind: ServiceEntry
  metadata:
    name: k8s-ai-external
  spec:
    hosts:
    - k8s-ai.worker-cluster.global
    location: MESH_EXTERNAL
    ports:
    - number: 9999
      name: a2a
      protocol: HTTP
    resolution: DNS
    endpoints:
    - address: k8s-ai.worker-cluster.svc.cluster.local
  ```

- **动态服务注册**：使用服务注册中心，如 Consul：

  ```python
  import consul

  # Register k8s-ai service
  client = consul.Consul()
  client.agent.service.register(
      name='k8s-ai',
      service_id='k8s-ai-worker-1',
      address='k8s-ai.worker-cluster',
      port=9999,
      tags=['a2a', 'diagnostics']
  )

  # Discover services
  services = client.health.service('k8s-ai', passing=True)
  for service in services:
      print(f"Found k8s-ai at {service['Service']['Address']}:{service['Service']['Port']}")
  ```

让我们考虑暴露 k8s-ai 服务器的各种网络方法。

### 跨集群网络方法

不同的环境需要不同的网络策略：

- **KinD（当前设置）**：使用 Docker 主机网络（`host.docker.internal`）、带 `extraPortMappings` 的 `NodePort` 服务。简单，适用于本地开发：

  ```yaml
  extraPortMappings:
  - containerPort: 30100
    hostPort: 8100
  ```

- **云提供商（AWS、GCP 和 Azure）**：使用具有公网 IP 的 `LoadBalancer` 服务，或使用 VPC 对等的私有网络，或使用传输网关实现多集群连接：

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: k8s-ai
    annotations:
      service.beta.kubernetes.io/aws-load-balancer-type: "nlb"
      service.beta.kubernetes.io/aws-load-balancer-internal: "true"
  spec:
    type: LoadBalancer
    selector:
      app: k8s-ai
    ports:
    - port: 9999
      targetPort: 9999
  ```

- **多集群服务网格**：服务可在集群间自动发现。集群间使用双向 TLS。流量管理和可观测性：

  ```yaml
  apiVersion: install.istio.io/v1alpha1
  kind: IstioOperator
  spec:
    values:
      global:
        multiCluster:
          clusterName: worker-cluster
          network: network1
  ```

对于分布式系统，测试端到端连通性非常重要。

### 测试端到端连通性

验证从 MAKDO 到 k8s-ai 的完整连通性：

```bash
# Create a test session
kubectl config view --context kind-makdo-worker --minify --flatten > /tmp/test-conn.yaml

python3 << 'EOF'
import json

with open('/tmp/test-conn.yaml', 'r') as f:
    kubeconfig = f.read()

request = {
    "cluster_name": "connectivity-test",
    "ttl_hours": 1,
    "kubeconfig": kubeconfig
}

with open('/tmp/test-conn.json', 'w') as f:
    json.dump(request, f)
EOF

# Create session via Admin API
RESPONSE=$(curl -s -X POST 'http://localhost:8099/sessions' \
  -H 'Authorization: Bearer test-key' \
  -H 'Content-Type: application/json' \
  -d '@/tmp/test-conn.json')
echo $RESPONSE | python3 -m json.tool

# Extract session token
SESSION_TOKEN=$(echo $RESPONSE | python3 -c "import sys, json; print(json.load(sys.stdin)['session_token'])")
echo "Session token: ${SESSION_TOKEN}"
```

现在，测试 MAKDO（在控制集群中运行）能否通过相同路径访问此端点：

```bash
# Test from host (simulating MAKDO's perspective)
curl -s 'http://localhost:8100/.well-known/agent.json' \
  -H 'Authorization: Bearer test-key' | python3 -m json.tool | head -15
```

以下为显示代理卡的命令输出：

```json
{
  "capabilities": {
    "streaming": false
  },
  "defaultInputModes": [
    "text/plain"
  ],
  "defaultOutputModes": [
    "text/plain"
  ],
  "description": "Kubernetes AI diagnostic agent with read-only cluster analysis with session-based cluster management",
  "name": "k8s-ai Diagnostic Agent",
}
```

这确认了完整路径：MAKDO → Docker 主机 → Worker 集群 → k8s-ai pod。

工业化强度的系统必须提供纵深防御以满足安全要求。让我们看看 MAKDO 和 k8s-ai 提供了什么。

### 了解安全架构

我们的部署实现了多层安全：

1. **API 密钥认证**：MAKDO 和 k8s-ai 都使用不记名令牌认证。
2. **基于会话的访问**：k8s-ai 使用带过期时间的临时会话。
3. **RBAC**：k8s-ai 在 worker 集群中拥有只读权限。
4. **网络隔离**：通信通过受控的特定端口进行。
5. **机密管理**：凭据存储在 Kubernetes Secrets 和 `.env` 文件中。

这遵循安全最佳实践，并最大限度地降低了远程访问、数据泄露、权限提升、横向移动和敏感数据暴露的风险。

### API 密钥认证

API 密钥认证是提供远程服务安全访问的常用方法。让我们看看 k8s-ai 如何实现它。

#### k8s-ai 认证

k8s-ai 的 A2A 端点和 Admin API 都需要不记名令牌认证。API 密钥存储在部署期间创建的 Kubernetes secret 中。

查看存储的 API 密钥：

```bash
kubectl get secret k8s-ai-api-keys --context kind-makdo-worker \
  -o jsonpath='{.data.keys\.json}' | base64 -d | python3 -m json.tool
```

以下是预期输出：

```json
{
  "api_keys": [
    {
      "key": "test-key",
      "name": "MAKDO Client",
      "created": "2025-01-01T00:00:00"
    }
  ]
}
```

通过不带凭据的请求测试认证：

```bash
curl -s 'http://localhost:8099/sessions' | python3 -m json.tool
```

以下是预期响应：

```json
{
  "detail": "Not authenticated"
}
```

现在，使用有效凭据测试：

```bash
curl -s 'http://localhost:8099/sessions' \
  -H 'Authorization: Bearer test-key' | python3 -m json.tool
```

这将返回活动会话列表，确认认证正常。

#### MAKDO 配置

MAKDO 将其 k8s-ai API 密钥存储在配置中。检查 MAKDO 的 `configMap`：

```bash
kubectl get configmap makdo-config --context kind-makdo-control \
  -o jsonpath='{.data.makdo\.yaml}' | grep -A 3 "a2a_servers:"
```

以下是显示 API 密钥配置的预期输出：

```yaml
a2a_servers:
  - name: "kind-makdo-worker"
    url: "http://host.docker.internal:8100"
    timeout: 30.0
    api_key: "test-key"
```

在生产环境中，请记住，API 密钥应使用强大的随机值生成（而不是 `test-key`），定期轮换，存储在专用的机密管理系统中（例如 HashiCorp Vault、AWS Secrets Manager 等），并且切勿提交到版本控制。

### 基于会话的安全性

k8s-ai 项目实现了基于会话的访问控制，在 API 密钥之外增加了一个额外的安全层。会话生命周期包括以下内容：

1. **创建**：MAKDO 通过 Admin API 创建会话，提供 `kubeconfig`。
2. **使用**：会话令牌用于所有诊断操作。
3. **过期**：会话会在其**生存时间**（**TTL**）之后自动过期。
4. **清理**：过期的会话会自动删除。

现在让我们演示会话安全。

使用较短的 TTL 创建会话：

```bash
# Extract kubeconfig
kubectl config view --context kind-makdo-worker --minify --flatten > /tmp/test-kubeconfig.yaml

# Create session with 1-minute TTL
python3 << 'EOF'
import json

with open('/tmp/test-kubeconfig.yaml', 'r') as f:
    kubeconfig = f.read()
EOF
```



```text
request = {
    "cluster_name": "test-session",
    "ttl_hours": 0.0167, # 1 minute = 1/60 hours
    "kubeconfig": kubeconfig
}

with open('/tmp/short-session.json', 'w') as f:
    json.dump(request, f)
EOF
```

```bash
curl -s -X POST 'http://localhost:8099/sessions' \
  -H 'Authorization: Bearer test-key' \
  -H 'Content-Type: application/json' \
  -d '@/tmp/short-session.json' | python3 -m json.tool
```

响应中包含一个 `expires_at` 时间戳。超过该时间后，会话将失效：

```bash
# Wait 90 seconds
sleep 90

# Try to use the expired session
SESSION_TOKEN="<token-from-creation-response>"

curl -s -X POST 'http://localhost:8100/' \
  -H 'Authorization: Bearer test-key' \
  -H 'Content-Type: application/json' \
  -d '{
    "jsonrpc": "2.0",
    "method": "kubernetes_diagnose_issue",
    "params": {
      "session_token": "'"${SESSION_TOKEN}"'",
      "issue_description": "test"
    },
    "id": 1
  }'
```

以下是表明会话已过期的预期响应：

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32002,
    "message": "Session not found or expired"
  },
  "id": 1
}
```

这种限时访问可以降低凭据泄露造成的影响。

### RBAC 与最小权限

k8s-ai 在工作集群中仅以最小权限运行。

#### 检查 RBAC 配置

查看分配给 k8s-ai 的 `ClusterRole` 源：

```bash
kubectl get clusterrole k8s-ai-reader --context kind-makdo-worker -o yaml
```

以下是关键权限：

```yaml
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log", "services", "nodes", "namespaces", "events"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["apps"]
  resources: ["deployments", "replicasets", "statefulsets", "daemonsets"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["batch"]
  resources: ["jobs", "cronjobs"]
  verbs: ["get", "list", "watch"]
```

请注意这些只读动词：`get`、`list`、`watch`。这里没有 `create`、`update`、`delete` 或 `patch` 权限。

#### 测试权限边界

验证 k8s-ai 无法修改资源：

```bash
# Get a pod name
POD_NAME=$(kubectl get pods --context kind-makdo-worker -o jsonpath='{.items[0].metadata.name}')

# Try to delete it using k8s-ai's service account
kubectl delete pod ${POD_NAME} --context kind-makdo-worker \
  --as=system:serviceaccount:default:k8s-ai
```

以下是预期错误：

```text
Error from server (Forbidden): pods "k8s-ai-f77d65f7d-p6lkk" is forbidden:
User "system:serviceaccount:default:k8s-ai" cannot delete resource "pods"
in API group "" in the namespace "default"
```

这表明，即使攻击者获得了 k8s-ai 的访问权限，也无法修改或删除集群资源。

### KinD 中的网络分段

虽然 KinD 集群运行在同一个 Docker 网络上，但我们通过端口映射和服务配置来控制通信。

#### 端口映射安全

查看已暴露的端口：

```bash
docker ps --filter name=makdo-control-plane --format "table {{.Names}}\t{{.Ports}}"
docker ps --filter name=makdo-worker-control-plane --format "table {{.Names}}\t{{.Ports}}"
```

输出显示了受控的暴露范围：

| 名称 | 端口 |
| --- | --- |
| makdo-control-control-plane | 127.0.0.1:xxx->6443/tcp, 0.0.0.0:9000->30000/tcp |
| makdo-worker-control-plane | 127.0.0.1:xxx->6443/tcp, 0.0.0.0:8099->30099/tcp, 0.0.0.0:8100->30100/tcp |

以下是一些关键观察：

- **Kubernetes API**：绑定到 `127.0.0.1`（仅本机可访问，外部无法访问）
- **应用端口**：绑定到 `0.0.0.0`（可从宿主机访问，模拟外部访问）
- **仅暴露特定端口**：并非所有容器端口都被暴露

这演示了如何在 KinD 环境中管理 Kubernetes 的连接和网络方面的同时保持隔离。现在，让我们看看如何测试它。

#### 测试网络隔离

尝试直接从宿主机访问 Kubernetes API（应失败）：

```bash
# Get the worker cluster API server address
WORKER_API=$(kubectl config view --context kind-makdo-worker --minify \
  -o jsonpath='{.clusters[0].cluster.server}')
echo "Worker API: ${WORKER_API}"

# Try to access from outside the cluster (without proper credentials)
curl -k "${WORKER_API}/api/v1/namespaces"
```

由于身份认证要求，我们会得到预期错误：

```json
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}
```

Kubernetes API 受到保护，只有使用正确的 `kubeconfig` 凭据才能访问。

### 生产环境安全增强

对于超出 KinD 的生产部署，请实施以下额外安全措施：

1. **TLS/SSL 加密**：在本地环境（KinD）中，MAKDO 与 k8s-ai 之间使用 HTTP 通信，而在生产环境中则为两个端点启用 TLS：

   ```yaml
   # k8s-ai with TLS
   containers:
   - name: k8s-ai
     env:
     - name: TLS_CERT_FILE
       value: "/certs/tls.crt"
     - name: TLS_KEY_FILE
       value: "/certs/tls.key"
     volumeMounts:
     - name: tls-certs
       mountPath: /certs
       readOnly: true
   volumes:
   - name: tls-certs
     secret:
       secretName: k8s-ai-tls
   ```

   使用 `cert-manager` 或您的云提供商提供的证书管理服务。

2. **网络策略**：限制 Pod 之间的通信：

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: k8s-ai-network-policy
   spec:
     podSelector:
       matchLabels:
         app: k8s-ai
     policyTypes:
     - Ingress
     ingress:
     - from:
       - namespaceSelector:
           matchLabels:
             name: makdo-namespace
       ports:
       - protocol: TCP
         port: 9999
   ```

3. **Pod 安全标准**：强制执行安全上下文：

   ```yaml
   securityContext:
     runAsNonRoot: true
     runAsUser: 1000
     fsGroup: 1000
     seccompProfile:
       type: RuntimeDefault
     capabilities:
       drop:
       - ALL
   ```

4. **密钥管理**：用外部密钥管理器替换 Kubernetes 密钥：

   ```yaml
   apiVersion: external-secrets.io/v1beta1
   kind: ExternalSecret
   metadata:
     name: k8s-ai-secrets
   spec:
     refreshInterval: 1h
     secretStoreRef:
       name: vault-backend
       kind: SecretStore
     target:
       name: k8s-ai-secrets
     data:
     - secretKey: api-key
       remoteRef:
         key: k8s-ai/api-key
   ```

5. **审计日志**：启用全面的日志记录：

   ```python
   # In k8s-ai code
   import logging
   logging.basicConfig(
       level=logging.INFO,
       format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
   )

   # Log all authentication attempts
   def authenticate(request):
       logger.info(f"Authentication attempt from {request.client.host}")
       # ... authentication logic
   ```

6. **速率限制**：防止滥用：

   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: k8s-ai-rate-limits
   data:
     rate_limits.yaml: |
       limits:
       - key: ip
         rate: 100
         per: minute
       - key: api_key
         rate: 1000
         per: hour
   ```

这里的教训是，保持一个严密而安全的生产环境并非易事，必须采取多种措施。

以下是部署到生产环境前需要验证的安全检查清单：

- 所有通信都使用 TLS/SSL 加密
- API 密钥强健、随机，并定期轮换
- 密钥由外部管理（Vault、AWS Secrets Manager 等）
- RBAC 遵循最小权限原则
- 网络策略限制不必要的通信
- Pod 安全上下文强制执行非 root 运行
- 审计日志记录所有访问尝试
- 速率限制防止滥用
- 容器镜像已扫描漏洞




- 依赖项已更新至包含安全补丁的最新版本
- 出口过滤以减少数据外泄

本 KinD 部署中所展示的安全架构奠定了坚实的基础，但生产环境还需要以下额外的加固措施。

#### 摘要

在本章中，我们使用 KinD 部署了一个跨越两个 Kubernetes 集群的完整多智能体系统，展示了分布式 AI 智能体架构的实战模式。我们首先设置具有正确网络和端口映射的双集群，然后将 MAKDO 部署到控制集群，它带有四个专用智能体以及用于记忆和 Slack 通信的 MCP 服务器。在工作集群上，我们将 k8s-ai 部署为具有只读 RBAC 权限的诊断智能体，并同时暴露其 A2A 协议端点和用于会话管理的 Admin API。在整个部署过程中，我们实现了多层安全机制，包括 API 密钥认证、带自动过期机制的基于会话的访问控制，以及通过受控端口暴露和服务边界实现的网络隔离。

我们构建的架构在负责编排和决策的控制智能体与拥有直接集群访问权限并执行专门任务的工作智能体之间进行了关注点分离。通信通过 Agent-to-Agent 协议进行，该协议使用基于 HTTP 的 JSON-RPC，并通过 `host.docker.internal` 为 KinD 环境提供静态服务发现。我们演示了如何跨集群边界追踪请求路径、测试身份验证和授权机制、验证 RBAC 权限以防止未经授权的修改，以及使用 Kubernetes Secrets 和环境变量安全地管理机密。该部署还展示了跨集群网络连接的实用模式，从适合本地开发的简单 `NodePort` 方法，到面向生产环境的服务网格和动态服务注册等更复杂的选项。

虽然这种基于 KinD 的部署为理解多智能体系统架构提供了极好的基础，但迁移到生产环境还需要额外的考量，包括对所有通信进行 TLS 加密、采用具有轮换策略的更强大 API 密钥、外部机密管理系统、全面的监控和日志记录、高可用性配置以及自动化部署流水线。这里展示的模式和实践可从本地开发扩展到跨云提供商、本地基础设施和混合环境的企业生产系统。

在下一章也是最后一章中，我们将探讨多智能体 AI 系统的未来，以及将塑造这些架构演进的新兴趋势。

#### 获取本书 PDF 版本和独家附赠内容

扫描二维码（或访问 `packtpub.com/unlock`）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

*注意：请将发票放在手边。直接从 Packt 购买无需发票。*

![](img/840072a5db7cd7dead6a4daec17c5a79_497_0.png)

![](img/840072a5db7cd7dead6a4daec17c5a79_497_1.png)
