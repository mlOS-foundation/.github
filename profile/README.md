# mlOS Foundation

**Building the Universal Operating System for Machine Learning**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Axon](https://img.shields.io/badge/Axon-v3.1.3-brightgreen)](https://github.com/mlOS-foundation/axon/releases)
[![Core](https://img.shields.io/badge/Core-v3.2.8--alpha-blue)](https://github.com/mlOS-foundation/core-releases)
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
│  │  (v3.1.3)    │  │(HTTP/gRPC/IPC)│ │ (SMI-based)  │          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
└─────────┼──────────────────┼─────────────────┼──────────────────┘
          │                  │                 │
          │ 1. Install &     │ 2. Register     │ 3. Execute
          │    Convert       │    with MLOS    │    Inference
          │                  │                 │
┌─────────▼──────────────────▼─────────────────▼──────────────────┐
│                    MLOS Core Engine (v3.2.8-alpha)              │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Model Registry  │  Plugin Registry  │  Resource Mgr     │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │         Standard Model Interface (SMI)                    │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │    ONNX Runtime Plugin (Multi-Type Tensor Support)       │  │
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

## 📊 E2E Validation

**📈 [View Latest Test Report](https://mlos-foundation.github.io/system-test/)**

| Model | Category | Status | Inference Time |
|-------|----------|--------|----------------|
| GPT-2 | NLP | ✅ Pass | ~68ms |
| BERT | NLP | ✅ Pass | ~98ms |
| RoBERTa | NLP | ✅ Pass | ~85ms |
| ResNet-50 | Vision | ✅ Pass | ~45ms |
| ViT | Vision | ✅ Pass | ~120ms |
| MobileNetV2 | Vision | ✅ Pass | ~25ms |

## 🏗️ Key Components

### 🧠 Axon - The Neural Pathway for ML Models

**Version: v3.1.3** | [Repository](https://github.com/mlOS-foundation/axon) | [Releases](https://github.com/mlOS-foundation/axon/releases)

- **Universal Model Installer**: Works with 80%+ of ML repositories
- **Universal ONNX Conversion**: Docker-based multi-framework converter
- **Model Package Format (MPF)**: Standardized `.axon` packages with `manifest.yaml`
- **Manifest-First Architecture**: Format-agnostic model execution

**Installation:**
```bash
curl -sSL axon.mlosfoundation.org | sh
```

### ⚙️ MLOS Core - Kernel-Level ML Runtime

**Version: v3.2.8-alpha** | [Releases](https://github.com/mlOS-foundation/core-releases)

- **Multi-Protocol APIs**: HTTP REST, gRPC, IPC (ultra-low latency)
- **Enhanced ONNX Plugin**: Multi-type tensor support, named inputs
- **Plugin Architecture**: Framework-agnostic via Standard Model Interface (SMI)
- **Resource Management**: Kernel-level optimization for ML workloads

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
| **Axon** | v3.1.3 | ✅ Stable | [axon](https://github.com/mlOS-foundation/axon) |
| **MLOS Core** | v3.2.8-alpha | ✅ Alpha | [core-releases](https://github.com/mlOS-foundation/core-releases) |
| **System Test** | Active | ✅ CI/CD | [system-test](https://github.com/mlOS-foundation/system-test) |
| **SMI Spec** | v1.0.0 | 🔒 Private | [smi-spec](https://github.com/mlOS-foundation/smi-spec) |
| **MLOS Linux (Ubuntu)** | - | 🔄 Planning | [mlos-linux-ubuntu](https://github.com/mlOS-foundation/mlos-linux-ubuntu) |
| **MLOS Linux (Flatcar)** | - | 🔄 Planning | [mlos-linux-flatcar](https://github.com/mlOS-foundation/mlos-linux-flatcar) |
| **MLOS Kernel Patches** | - | 🔄 Planning | [mlos-linux-kernel](https://github.com/mlOS-foundation/mlos-linux-kernel) |

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
- **Core v3.2.8-alpha**: Complete seq2seq inference with auto attention_mask generation
- E2E validation with automated testing

### 🚧 Phase 3: Production Readiness (In Progress)
- **Timeline**: Q1-Q2 2026
- gRPC full implementation
- GPU memory management
- Batched inference optimization
- Model caching improvements

### 📅 Phase 4: Ecosystem Expansion (Planned)
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
