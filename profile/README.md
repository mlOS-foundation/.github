# mlOS Foundation

**Building the Universal Operating System for Machine Learning**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Axon](https://img.shields.io/badge/Axon-v3.1.9-brightgreen)](https://github.com/mlOS-foundation/axon/releases)
[![Core](https://img.shields.io/badge/Core-v6.2.0--alpha-blue)](https://github.com/mlOS-foundation/core-releases)
[![Kernel](https://img.shields.io/badge/Kernel-v6.2.0--alpha-purple)](https://github.com/mlOS-foundation/mlos-linux-kernel/releases)
[![E2E Tests](https://github.com/mlOS-foundation/system-test/actions/workflows/e2e-test.yml/badge.svg)](https://mlos-foundation.github.io/system-test/)

## 🎯 Mission

We're creating the foundational infrastructure that makes ML models truly portable across any framework, hardware, and deployment environment. Just as POSIX made software portable across Unix systems, **MLOS makes ML models portable across any framework and deployment environment**.

## 🔬 Core Innovation

MLOS introduces a paradigm shift: **ML frameworks don't integrate with deployment platforms - they integrate with MLOS through a universal standard interface (SMI)**.

### The MLOS Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Application Layer                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │   Axon CLI   │  │  MLOS API    │  │   Plugins    │          │
│  │  (v3.1.9)    │  │(HTTP/gRPC/IPC)│ │ (SMI-based)  │          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
└─────────┼──────────────────┼─────────────────┼──────────────────┘
          │                  │                 │
          │ 1. Install &     │ 2. Register     │ 3. Execute
          │    Convert       │    with MLOS    │    Inference
          │                  │                 │
┌─────────▼──────────────────▼─────────────────▼──────────────────┐
│                    MLOS Core Engine (v6.2.0-alpha)              │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Model Registry  │  Plugin Registry  │  Resource Mgr     │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │         Standard Model Interface (SMI)                    │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  ONNX Runtime Plugin  │  GGUF/llama.cpp Plugin (LLMs)    │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
          │
          │ Kernel-Level Optimizations
          │ (US-63/865,176)
          │
┌─────────▼───────────────────────────────────────────────────────┐
│              Operating System Kernel                             │
│  • ML-aware scheduler                                            │
│  • Tensor memory management                                      │
│  • GPU resource orchestration                                    │
└─────────────────────────────────────────────────────────────────┘
```

## ✨ Latest Features

### ⚡ Kernel Prefetch Plans (Core v6.2.0-alpha)

Activates kernel-level optimizations that were previously implemented but not enabled:

- ✅ **Prefetch Plans**: Adaptive tensor loading for ONNX and GGUF plugins (5-15% latency reduction)
- ✅ **High Priority Scheduling**: `MLOS_PRIO_HIGH` for inference (reduced jitter under load)
- ✅ **Performance Cleanup**: Removed 111 fflush() calls (5-10% I/O improvement)

| Plugin | Prefetch Strategy | Priority |
|--------|-------------------|----------|
| **ONNX** | 4-tensor adaptive plan with critical/optional flags | HIGH |
| **GGUF** | LLM-optimized (embeddings, KV cache, layer weights) | HIGH |

### ⚡ Enhanced Kernel Statistics (Core v6.1.0-alpha)

Comprehensive observability for ML kernel operations with 4 new ioctls (total: 23):

- ✅ **NUMA Memory Pool Statistics**: Per-node memory allocation tracking and migration metrics
- ✅ **Adaptive Prefetch Learning**: Self-tuning prefetch timing with learning rate visibility
- ✅ **Prediction Accuracy Tracking**: Deadline prediction quality metrics with calibration scores
- ✅ **Extended Latency Statistics**: Variance, stddev, and percentile calculations (P50/P90/P99)

| Feature | ioctl | Description |
|---------|-------|-------------|
| **NUMA Stats** | 70 | Per-node memory allocation and remote access tracking |
| **Prefetch Learning** | 71 | Timing adaptation and success rate metrics |
| **Prediction Accuracy** | 72 | False alarm/miss rates and calibration scores |
| **Extended Latency** | 73 | Variance analysis and outlier detection |

### ⚡ Advanced Kernel Optimizations (Core v6.0.0-alpha)

New ML-optimized kernel features for production inference workloads:

- ✅ **NUMA-Aware Tensor Placement**: Intelligent tensor allocation across NUMA nodes for multi-socket systems
- ✅ **Predictive Tensor Prefetching**: ML-based prediction of tensor access patterns with async prefetching
- ✅ **Deadline-Miss Prediction**: Early warning system for inference deadline violations with adaptive scheduling

| Feature | Benefit | Use Case |
|---------|---------|----------|
| **NUMA Placement** | Up to 40% latency reduction | Multi-socket servers |
| **Tensor Prefetch** | Reduced cache misses | Sequential inference |
| **Deadline Prediction** | SLA compliance | Real-time systems |

### 🔧 Docker ONNX Converter Fix (Axon v3.1.9)

Fixed ONNX conversion for HuggingFace models that don't have pre-exported ONNX files:

- ✅ **ResNet-50 Support**: `hf/microsoft/resnet-50` now converts and runs correctly
- ✅ **Absolute Container Paths**: Fixed path handling in Docker converter to prevent Optimum misinterpretation
- ✅ **E2E Validated**: Full pipeline tested with ResNet, BERT, GPT-2

### ⚡ Smart Plugin Selection (Core v6.0.0-alpha)

Improved plugin selection for native framework models:

- ✅ **ONNX Detection**: Check if ONNX model exists before selecting runtime
- ✅ **Native Preference**: Use native framework plugins (libtorch) when no ONNX available
- ✅ **Case-Insensitive**: Support both `pytorch`/`PyTorch` and `torchscript`/`TorchScript`

### 🚀 Universal ONNX Conversion (Axon v3.x)

Axon now features **universal ONNX conversion** across all major ML repositories:

| Repository | Converter | Coverage | Example |
|------------|-----------|----------|---------|
| **Hugging Face** 🤗 | optimum + torch.onnx | 100K+ models | `axon install hf/bert-base-uncased@latest` |
| **PyTorch Hub** 🔥 | TorchScript | Research | `axon install pytorch/vision/resnet50@latest` |
| **TensorFlow Hub** 🧠 | tf2onnx | Production | `axon install tfhub/google/universal-sentence-encoder/4@latest` |
| **ModelScope** 🎨 | Auto-detect | Multimodal | `axon install modelscope/damo/cv_resnet50@latest` |

**Key Features:**
- ✅ **Smart Repository Routing**: Automatic converter selection based on model namespace
- ✅ **Multi-Strategy Fallbacks**: Multiple conversion methods for maximum compatibility
- ✅ **Docker-Based Conversion**: Zero Python dependencies on host machine
- ✅ **Vision Model Support**: Full ImageNet input support (224×224×3 RGB)

### ⚡ Enhanced ONNX Runtime Plugin (Core v3.x)

The built-in ONNX Runtime plugin now supports **universal inference**:

| Tensor Type | Support | Use Case |
|-------------|---------|----------|
| **int64** | ✅ | NLP token IDs (GPT-2, BERT, T5) |
| **float32** | ✅ | Vision models, embeddings |
| **int32** | ✅ | TensorFlow models |
| **bool** | ✅ | Attention masks |

**Advanced Features:**
- ✅ Named input parsing (`{"input_ids": [...], "attention_mask": [...]}`)
- ✅ Multi-input models (BERT, RoBERTa, T5)
- ✅ Dynamic shape handling
- ✅ Large input support (16MB+)

### ⚡ Kernel Tensor Operations (v6.0.0-alpha)

MLOS now features **complete kernel-level tensor operations** for ML workloads:

| Operation | ioctl | Description |
|-----------|-------|-------------|
| **Tensor Pin** | `MLOS_TENSOR_PIN` | Pin tensors in memory for zero-copy mmap access |
| **Tensor Unpin** | `MLOS_TENSOR_UNPIN` | Release pinned tensors for eviction |
| **Memory Defrag** | `MLOS_MEMORY_DEFRAG` | Defragment GPU memory pool |
| **Memory Trim** | `MLOS_MEMORY_TRIM` | Trim memory with LRU eviction |
| **Inference Start** | `MLOS_INFERENCE_START` | Start kernel-scheduled inference |
| **Inference Wait** | `MLOS_INFERENCE_WAIT` | Wait for inference completion |
| **GPU List** | `MLOS_GPU_LIST` | Enumerate available GPUs |
| **Tensor Prefetch** | `MLOS_TENSOR_PREFETCH` | Predictive tensor loading |

**Kernel Module Releases:**
- Ubuntu 22.04 (kernel 5.15)
- Ubuntu 22.04 HWE (kernel 6.5)
- Ubuntu 24.04 (kernel 6.8)
- Flatcar (kernel 6.1)

### ⚡ Format-Agnostic Runtime (v6.0.0-alpha)

MLOS now features **native format execution** - models run in their optimal format without conversion overhead:

| Format | Extension | Runtime | Use Case | Performance |
|--------|-----------|---------|----------|-------------|
| **GGUF** | `.gguf` | llama.cpp | LLMs (TinyLlama, Phi-2, Qwen) | **Native** - No conversion |
| **ONNX** | `.onnx` | ONNX Runtime | Vision, NLP encoders | **Native** - Built-in |
| **SafeTensors** | `.safetensors` | Auto-convert | HuggingFace models | Converted to ONNX |
| **PyTorch** | `.pt`, `.pth` | Auto-convert | Research models | Converted to ONNX |

**Why Format-Agnostic Matters:**

```
Traditional Approach (Always Convert to ONNX):
┌──────────┐    ┌──────────────┐    ┌──────────────┐
│ GGUF LLM │ -> │ Convert ONNX │ -> │ ONNX Runtime │  ❌ Slow, quality loss
└──────────┘    │  (minutes)   │    └──────────────┘
                └──────────────┘

MLOS Format-Agnostic Approach:
┌──────────┐    ┌──────────────┐
│ GGUF LLM │ -> │ llama.cpp    │  ✅ Instant, native quality
└──────────┘    │  (direct)    │
                └──────────────┘
```

**Key Benefits:**
- **Faster Installation**: Skip conversion for execution-ready formats (GGUF, ONNX)
- **Native Quality**: LLMs run with llama.cpp optimizations (quantization-aware)
- **Smart Detection**: Axon auto-detects format via extension + magic bytes
- **Fallback Safety**: Unknown formats gracefully convert to ONNX

**LLM Example:**
```bash
# Install GGUF model (no conversion - native execution)
axon install hf/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF@latest

# Register and run inference
axon register hf/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF@latest
curl -X POST http://localhost:8080/models/.../inference \
  -d '{"prompt": "What is ML?", "max_tokens": 64}'
```

## 📊 E2E Validation

**📈 [View Latest Test Report](https://mlos-foundation.github.io/system-test/)**

| Model | Category | Format | Status | Inference Time |
|-------|----------|--------|--------|----------------|
| GPT-2 | NLP | ONNX | ✅ Pass | ~68ms |
| BERT | NLP | ONNX | ✅ Pass | ~98ms |
| RoBERTa | NLP | ONNX | ✅ Pass | ~85ms |
| T5 | NLP | ONNX | ✅ Pass | ~150ms |
| TinyLlama | LLM | **GGUF** | ✅ Pass | ~200ms |
| ResNet-50 | Vision | ONNX | ✅ Pass | ~45ms |
| ViT | Vision | ONNX | ✅ Pass | ~120ms |
| MobileNetV2 | Vision | ONNX | ✅ Pass | ~25ms |
| CLIP | Multimodal | ONNX | ✅ Pass | ~180ms |

## 🏗️ Key Components

### 🧠 Axon - The Neural Pathway for ML Models

**Version: v3.1.9** | [Repository](https://github.com/mlOS-foundation/axon) | [Releases](https://github.com/mlOS-foundation/axon/releases)

- **Universal Model Installer**: Works with 80%+ of ML repositories
- **Format-Agnostic Installation**: Auto-detects GGUF, ONNX, SafeTensors, PyTorch
- **Smart Format Detection**: Extension + magic byte detection for optimal runtime
- **Model Package Format (MPF)**: Standardized `.axon` packages with `manifest.yaml`
- **Manifest-First Architecture**: Format-agnostic model execution

**Installation:**
```bash
curl -sSL axon.mlosfoundation.org | sh
```

### ⚙️ MLOS Core - Kernel-Level ML Runtime

**Version: v6.2.0-alpha** | [Releases](https://github.com/mlOS-foundation/core-releases)

- **Multi-Protocol APIs**: HTTP REST, gRPC, IPC (ultra-low latency)
- **Format-Agnostic Runtime**: ONNX + GGUF/llama.cpp plugins for native execution
- **LLM Support**: Native GGUF execution for TinyLlama, Phi-2, Qwen models
- **Plugin Architecture**: Framework-agnostic via Standard Model Interface (SMI)
- **Kernel Integration**: 23 ioctl operations for tensor memory management and observability
- **Resource Management**: Kernel-level optimization for ML workloads

### 🐧 MLOS Kernel Module - Linux Kernel Integration

**Version: v6.2.0-alpha** | [Releases](https://github.com/mlOS-foundation/mlos-linux-kernel/releases)

- **Character Device**: `/dev/mlos-ml` for userspace communication
- **Tensor Memory Manager**: LRU-based eviction, pinning, zero-copy mmap
- **ML-Aware Scheduler**: Priority queues (REALTIME, HIGH, NORMAL, BATCH)
- **GPU Support**: Multi-GPU enumeration, simulated GPU for testing
- **Supported Kernels**: 5.15, 6.1, 6.5, 6.8

### 🧪 System Test - E2E Validation Framework

**Repository**: [system-test](https://github.com/mlOS-foundation/system-test)

- **Automated E2E Testing**: Full pipeline validation (Axon → Core → Inference)
- **Multi-Model Coverage**: NLP, Vision, and Multimodal models
- **GitHub Pages Reports**: [mlos-foundation.github.io/system-test](https://mlos-foundation.github.io/system-test/)
- **CI/CD Integration**: Scheduled and on-demand test runs

### 📐 SMI Spec - Standard Model Interface

- **Universal Contract**: Framework-agnostic interface for ML models
- **Multi-Language Support**: C, Python, Go, JavaScript
- **Status**: 🔒 Private - In Development

## 🚀 Quick Start

### Install & Run Inference

```bash
# 1. Install Axon CLI
curl -sSL axon.mlosfoundation.org | sh

# 2. Install a model (auto-converts to ONNX)
axon install hf/bert-base-uncased@latest

# 3. Register with MLOS Core
axon register hf/bert-base-uncased@latest

# 4. Run inference
curl -X POST http://localhost:8080/models/hf%2Fbert-base-uncased%40latest/inference \
  -H "Content-Type: application/json" \
  -d '{"input_ids": [101, 7592, 102], "attention_mask": [1, 1, 1]}'
```

### Vision Model Example

```bash
# Install vision model
axon install hf/google/vit-base-patch16-224@latest

# Run inference with ImageNet input (224×224×3)
curl -X POST http://localhost:8080/models/hf%2Fgoogle%2Fvit-base-patch16-224%40latest/inference \
  -H "Content-Type: application/json" \
  -d '{"pixel_values": [...]}'  # 150528 float values
```

## 📋 Component Versions

| Component | Version | Status | Repository |
|-----------|---------|--------|------------|
| **Axon** | v3.1.9 | ✅ Stable | [axon](https://github.com/mlOS-foundation/axon) |
| **MLOS Core** | v6.2.0-alpha | ✅ Alpha | [core-releases](https://github.com/mlOS-foundation/core-releases) |
| **MLOS Kernel Module** | v6.2.0-alpha | ✅ Alpha | [mlos-linux-kernel](https://github.com/mlOS-foundation/mlos-linux-kernel) |
| **System Test** | Active | ✅ CI/CD | [system-test](https://github.com/mlOS-foundation/system-test) |
| **SMI Spec** | v1.0.0 | 🔒 Private | [smi-spec](https://github.com/mlOS-foundation/smi-spec) |
| **MLOS Linux (Ubuntu)** | - | 🔄 Planning | [mlos-linux-ubuntu](https://github.com/mlOS-foundation/mlos-linux-ubuntu) |
| **MLOS Linux (Flatcar)** | - | 🔄 Planning | [mlos-linux-flatcar](https://github.com/mlOS-foundation/mlos-linux-flatcar) |

## 📅 Development Roadmap

### ✅ Phase 0: Foundation (Complete)
- Standard Model Interface (SMI) specification
- Axon v1.0.0 - Hugging Face Hub adapter
- MLOS Core v1.0.0 - Core runtime with HTTP API
- Patent applications (US-63/861,527, US-63/865,176)

### ✅ Phase 1: Universal Model Delivery (Complete)
- Multi-repository adapters (HuggingFace, PyTorch Hub, TF Hub, ModelScope)
- 80%+ ML model user base coverage
- Manifest-First Architecture

### ✅ Phase 2: Universal Inference (Complete)
- **Axon v3.0.0**: Universal ONNX conversion
- **Core v2.2.0-alpha**: Multi-type tensor support, named inputs
- **Axon v3.1.0**: Vision model support
- **Axon v3.1.3**: Enhanced seq2seq/multi-encoder model support, improved T5 conversion
- **Core v3.2.9-alpha**: Complete seq2seq inference with auto attention_mask generation
- **Axon v3.1.4**: Format-agnostic model installation (GGUF, ONNX, SafeTensors, PyTorch)
- **Core v3.2.10-alpha**: GGUF runtime plugin with llama.cpp backend for native LLM support
- E2E validation with automated testing

### ✅ Phase 3: Kernel Integration (Complete)
- **Core v6.2.0-alpha**: 23 kernel ioctls with enhanced statistics and observability
- **Kernel v6.2.0-alpha**: Binary releases for 4 kernel versions
- Tensor pinning/unpinning for zero-copy memory access
- Memory defragmentation and trim operations
- ML-aware scheduler with priority queues
- GPU enumeration and management
- Tested on Ubuntu 24.04 (kernel 6.8.0-60-generic)

### 🚧 Phase 4: Production Readiness (In Progress)
- **Timeline**: Q1-Q2 2026
- gRPC full implementation
- GPU memory management
- Batched inference optimization
- Model caching improvements

### 📅 Phase 5: Ecosystem Expansion (Planned)
- **Timeline**: Q2-Q3 2026
- MLOS Linux Distributions (Ubuntu & Flatcar-based)
- Additional repository adapters (Replicate, Kaggle)
- Cloud-native deployment options
- Model marketplace integration

## 🔒 Patent Status

| Patent | Status | Key Innovation |
|--------|--------|----------------|
| **US-63/861,527** | Pending | Model Package Format (MPF), Native lifecycle management |
| **US-63/865,176** | Pending (CIP) | ML-aware scheduler, Tensor memory management, GPU orchestration |

## 📚 Repositories

### Core Projects
- **[axon](https://github.com/mlOS-foundation/axon)** - Universal model installer & ONNX converter
- **[core-releases](https://github.com/mlOS-foundation/core-releases)** - Public releases for MLOS Core
- **[system-test](https://github.com/mlOS-foundation/system-test)** - E2E validation framework
- **[mlosfoundation.org](https://github.com/mlOS-foundation/mlosfoundation.org)** - Official website

### Distribution Projects (Planning)
- **[mlos-linux-ubuntu](https://github.com/mlOS-foundation/mlos-linux-ubuntu)** - Ubuntu-based MLOS distribution
- **[mlos-linux-flatcar](https://github.com/mlOS-foundation/mlos-linux-flatcar)** - Flatcar-based MLOS distribution
- **[mlos-linux-kernel](https://github.com/mlOS-foundation/mlos-linux-kernel)** - Shared kernel patches

### Supporting Projects
- **[smi-spec](https://github.com/mlOS-foundation/smi-spec)** - Standard Model Interface specification
- **[bindings](https://github.com/mlOS-foundation/bindings)** - Language bindings for SMI protocol
- **[examples](https://github.com/mlOS-foundation/examples)** - Examples, tutorials, and best practices

## 🌟 Why MLOS?

### The Problem
- **Framework Lock-in**: Models tied to specific frameworks
- **Deployment Complexity**: Different platforms require different integrations
- **No Standard Interface**: Each framework has its own API
- **Resource Inefficiency**: No unified resource management

### The Solution
- **Universal Interface**: SMI provides framework-agnostic contract
- **Standardized Packaging**: MPF (Model Package Format) via Axon
- **Universal Execution**: ONNX-based inference for all models
- **Kernel-Level Optimization**: OS-level ML resource management

## 📞 Contact & Resources

- **Website**: [mlosfoundation.org](https://mlosfoundation.org)
- **E2E Reports**: [mlos-foundation.github.io/system-test](https://mlos-foundation.github.io/system-test/)
- **Email**: info@mlos-foundation.org
- **Status**: Alpha Development
- **Public Release**: Q2 2026

## 📄 License

MLOS Foundation projects use the following licenses:
- **Axon**: GNU AGPL v3.0
- **MLOS Core**: MIT License
- **Documentation**: MIT License

---

**© 2025 mlOS Foundation. Building the future of ML infrastructure.**

*"Just as POSIX made software portable across Unix systems, MLOS makes ML models portable across any framework and deployment environment."*

**Signal. Propagate. Myelinate.** 🧠
