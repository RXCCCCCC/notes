# 模型训练指南

了解如何使用纯 PyTorch 从零训练 MiniMind 语言模型。

## 📊 训练概览

MiniMind 实现了一套完整的训练流程:

```
Tokenizer 训练
        ↓
   预训练(学习知识)
        ↓
   SFT(学习对话)
        ↓
    ┌───────────────────┬─────────────────────┬──────────────┐
    ↓                   ↓                     ↓              ↓
  LoRA              DPO/RLHF         RLAIF (PPO/GRPO/SPO)  蒸馏
(领域适配)         (偏好对齐)       (强化学习)            (推理)
```

## 💰 训练成本(单张 NVIDIA 3090)

| 模型 | 数据集 | 时长 | 成本(元) | 质量 |
|-------|---------|----------|-----------|---------|
| MiniMind2-Small | pretrain_hq + sft_mini_512 | 2.1h | ≈3 | 😊😊 |
| MiniMind2-Small | 完整数据集 | 38h | ≈50 | 😊😊😊😊😊😊 |
| MiniMind2 | pretrain_hq + sft_mini_512 | 3.3h | ≈5 | 😊😊 |
| MiniMind2 | 完整数据集 | 122h | ≈160 | 😊😊😊😊😊😊😊 |

!!! success "超快训练"
    **只需 2.1 小时 + $3 = 可用的聊天机器人!**
    
    使用 `pretrain_hq.jsonl` + `sft_mini_512.jsonl` 可获得最快的复现

## 📋 数据准备

### 1. 下载数据集

从 [ModelScope](https://www.modelscope.cn/datasets/gongjy/minimind_dataset) 或 [HuggingFace](https://huggingface.co/datasets/jingyaogong/minimind_dataset) 下载:

```bash
mkdir -p dataset
cd dataset
# 下载所需文件
```

### 2. 数据集目录结构

```
./dataset/
├── pretrain_hq.jsonl ✨ (1.6GB,预训练必需)
├── sft_mini_512.jsonl ✨ (1.2GB,最快的 SFT)
├── sft_512.jsonl (7.5GB,标准 SFT)
├── sft_1024.jsonl (5.6GB,更长序列的 SFT)
├── sft_2048.jsonl (9GB,超长序列的 SFT)
├── dpo.jsonl (909MB,DPO 训练)
├── r1_mix_1024.jsonl (340MB,推理蒸馏)
├── rlaif-mini.jsonl (1MB,RLAIF 算法)
├── lora_identity.jsonl (22.8KB,身份 LoRA)
└── lora_medical.jsonl (34MB,医疗领域 LoRA)
```

### 3. 数据格式

**预训练数据**(`pretrain_hq.jsonl`):
```json
{"text": "如何克服拖延症?克服拖延症并不容易,但这些建议或许有帮助..."}
```

**SFT 数据**(`sft_*.jsonl`):
```json
{
  "conversations": [
    {"role": "user", "content": "你好!"},
    {"role": "assistant", "content": "你好!我能帮你什么?"},
    {"role": "user", "content": "给我讲个笑话。"},
    {"role": "assistant", "content": "为什么稻草人获奖了?因为他在自己的领域里出类拔萃!"}
  ]
}
```

**DPO 数据**(`dpo.jsonl`):
```json
{
  "chosen": [
    {"role": "user", "content": "2+2 等于多少?"},
    {"role": "assistant", "content": "2+2 等于 4。"}
  ],
  "rejected": [
    {"role": "user", "content": "2+2 等于多少?"},
    {"role": "assistant", "content": "2+2 等于 5。"}
  ]
}
```

**LoRA 领域数据**(`lora_*.jsonl`):
```json
{
  "conversations": [
    {"role": "user", "content": "颈椎病的治疗方法是什么?"},
    {"role": "assistant", "content": "颈椎病的治疗通常包括..."}
  ]
}
```

## 🎯 完整训练流程

所有训练脚本都位于 `./trainer` 目录。

```bash
cd trainer
```

### 阶段 1:预训练

**目的**:学习基础知识(续写)

```bash
# 单 GPU
python train_pretrain.py

# 多 GPU (DDP)
torchrun --nproc_per_node 2 train_pretrain.py

# 多 GPU (DeepSpeed)
deepspeed --master_port 29500 --num_gpus=2 train_pretrain.py
```

**关键参数**:
- `max_seq_len`: 512(根据 GPU 显存调整)
- `learning_rate`: 1e-4
- `epochs`: 根据数据集大小调整

**输出**:`./out/pretrain_*.pth`

**训练时长**:
- MiniMind2-Small (26M):约 1.1h
- MiniMind2 (104M):约 3.9h

!!! tip "预训练小贴士"
    - 从 `pretrain_hq.jsonl` 开始可获得最佳效果
    - 预训练数据质量 > 数量
    - 监控损失曲线以检测过拟合

### 阶段 2:监督微调(SFT)

**目的**:教会模型对话模式和聊天模板

```bash
# 单 GPU
python train_full_sft.py

# 多 GPU
torchrun --nproc_per_node 2 train_full_sft.py
```

**配置**:
- 加载阶段 1 的预训练模型
- 使用 SFT 数据集(`sft_mini_512.jsonl` 或 `sft_512.jsonl`)
- 调整 `max_seq_len` 以匹配训练数据

**输出**:`./out/full_sft_*.pth`

**训练时长**:
- 使用 sft_mini_512:1-3 小时
- 使用完整 sft_512:20-25 小时

!!! warning "SFT 数据选择"
    - `sft_mini_512.jsonl`:最快,约 1.2GB,最长 512 token
    - `sft_512.jsonl`:标准,约 7.5GB,最长 512 token
    - `sft_1024.jsonl`:更长,约 5.6GB,最长 1024 token
    - `sft_2048.jsonl`:最长,约 9GB,最长 2048 token

### 阶段 3:LoRA 微调(可选)

**目的**:参数高效的领域适配

**应用场景**:
- 医疗问答知识
- 个人身份/自我认知
- 专有领域知识

```bash
# 编辑 train_lora.py 设置正确的数据集和基础模型
python train_lora.py

# 多 GPU
torchrun --nproc_per_node 2 train_lora.py
```

**输出**:`./out/lora/lora_*.pth`

**示例 1:医疗领域**

准备 `dataset/lora_medical.jsonl`:
```json
{
  "conversations": [
    {"role": "user", "content": "颈椎病患者正确的枕头高度应该是多少?"},
    {"role": "assistant", "content": "对于颈椎病,枕头高度应该是..."}
  ]
}
```

训练:
```bash
# 修改 train_lora.py: lora_name = 'medical'
python train_lora.py
```

**示例 2:身份/自我认知**

准备 `dataset/lora_identity.jsonl`:
```json
{
  "conversations": [
    {"role": "user", "content": "你是谁?"},
    {"role": "assistant", "content": "我是 MiniMind..."}
  ]
}
```

### 阶段 4:直接偏好优化(DPO)

**目的**:让模型回复与人类偏好对齐

DPO 通过直接优化偏好对,消除了对单独奖励模型的需求。

```bash
python train_dpo.py

# 多 GPU
torchrun --nproc_per_node 2 train_dpo.py
```

**输出**:`./out/rlhf_*.pth`

**关键特性**:
- 离线策略训练(可跨 epoch 复用数据)
- 无需单独的奖励模型
- 比 PPO 更高的样本效率
- 训练收敛稳定

**训练时长**:约 1-3 小时

### 阶段 5:基于 AI 反馈的强化学习(RLAIF)

RLAIF 是一种使用 AI 生成奖励(而非人工标注)的先进训练方法。MiniMind 实现了三种现代算法:

#### 5.1 PPO(近端策略优化)

经典的同策略(on-policy)强化学习算法,稳定性经过验证。

```bash
python train_ppo.py

# 多 GPU
torchrun --nproc_per_node 2 train_ppo.py
```

**算法**:
$$\mathcal{L}_{PPO} = -\mathbb{E}\left[\min(r_t \cdot A_t, \text{clip}(r_t, 1-\varepsilon, 1+\varepsilon) \cdot A_t)\right] + \beta \cdot \mathbb{E}[\text{KL}]$$

**特性**:
- 稳定但奖励提升较慢
- 需要 Actor 和 Critic 两个网络
- 显存占用高(单网络的 1.5-2 倍)
- 有利于探索

**输出**:`./out/ppo_actor_*.pth`

**训练时长**:约 1-3 小时

#### 5.2 GRPO(群体相对策略优化)

DeepSeek-R1 使用的现代算法,收敛更快。

```bash
python train_grpo.py

# 多 GPU
torchrun --nproc_per_node 2 train_grpo.py
```

**算法**:
$$\mathcal{L}_{GRPO} = -\mathbb{E}\left[r_t \cdot A_t - \beta \cdot \text{KL}_t\right]$$

其中优势(advantage)计算如下:
$$A_t = \frac{R - \mu_{group}}{\sigma_{group}}$$

**特性**:
- 单网络设计(省显存)
- 奖励提升更快
- 群体归一化消除了偏差
- 收敛稳定性更好

**输出**:`./out/grpo_*.pth`

**训练时长**:约 1-3 小时

#### 5.3 SPO(单流策略优化)

最新算法(2025),解决了 GRPO 的退化组问题。

```bash
python train_spo.py

# 多 GPU
torchrun --nproc_per_node 2 train_spo.py
```

**算法**:
$$\mathcal{L}_{SPO} = -\mathbb{E}\left[\log \pi_\theta(a_t|s) \cdot A_t - \beta \cdot \text{KL}_t\right]$$

带自适应基线: $B_t^{adaptive}$

**特性**:
- 无组依赖(1 输入 → 1 训练样本)
- 自适应价值跟踪
- 更好地处理困难样本
- 在小模型上属于实验性功能

**输出**:`./out/spo_*.pth`

**训练时长**:约 1-3 小时

#### RLAIF 数据集准备

所有 RLAIF 算法都使用 `rlaif-mini.jsonl`(1MB,10k 条样本):

```bash
# 下载数据集
# 格式:与 SFT 相同,但 assistant 内容为 "无"
{
  "conversations": [
    {"role": "user", "content": "简要解释一下光合作用。"},
    {"role": "assistant", "content": "无"}
  ]
}
```

模型在训练过程中生成样本,并由**奖励模型(Reward Model)**(例如 InternLM2-1.8B-Reward)进行评分。

**奖励模型设置**:

```bash
# 将奖励模型下载到上级目录
cd ../
git clone https://huggingface.co/internlm/internlm2-1_8b-reward

# 目录结构应为:
# project/
# ├── minimind/
# └── internlm2-1_8b-reward/
```

#### RLAIF 与 DPO 对比

| 方面    | DPO              | RLAIF (PPO/GRPO/SPO) |     |
| ----- | ---------------- | -------------------- | --- |
| 训练类型  | 离线策略(Off-policy) | 同策略(On-policy)       |     |
| 数据新鲜度 | 静态配对             | 动态(生成)               |     |
| 奖励来源  | 隐式               | 显式模型                 |     |
| 收敛速度  | 快                | 较慢                   |     |
| 显存占用  | 较低               | 较高                   |     |
| 最适合   | 偏好精炼             | 能力提升                 |     |
