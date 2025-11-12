# mlOS Foundation

**Building the Universal Operating System for Machine Learning**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Private MVP](https://img.shields.io/badge/Status-Private%20MVP-orange)](https://github.com/mlOS-foundation)
[![Public Release: Q2 2026](https://img.shields.io/badge/Public%20Release-Q2%202026-blue)](https://github.com/mlOS-foundation)

## 🎯 Mission

We're creating the foundational infrastructure that makes ML models truly portable across any framework, hardware, and deployment environment. Just as POSIX made software portable across Unix systems, **MLOS makes ML models portable across any framework and deployment environment**.

## 🔬 Core Innovation

MLOS introduces a paradigm shift: **ML frameworks don't integrate with deployment platforms - they integrate with MLOS through a universal standard interface (SMI)**.

### The MLOS Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Application Layer                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │   Axon CLI   │  │  MLOS API    │  │   Plugins    │          │
│  │ (v1.5.0+)    │  │ (HTTP/gRPC/IPC)│  │ (SMI-based) │          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
└─────────┼──────────────────┼─────────────────┼──────────────────┘
          │                  │                 │
          │ 1. Install        │ 2. Register      │ 3. Execute
          │    Model          │    with MLOS     │    Inference
          │                  │                 │
┌─────────▼──────────────────▼─────────────────▼──────────────────┐
│                    MLOS Core Engine                              │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Model Registry  │  Plugin Registry  │  Resource Mgr  │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │         Standard Model Interface (SMI)                    │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │         Axon Manifest Reader (MPF)                       │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
          │
          │ Kernel-Level Optimizations
          │ (US-63/865,176)
          │
┌─────────▼───────────────────────────────────────────────────────┐
│              Operating System Kernel                             │
│  • ML-aware scheduler                                           │
│  • Tensor memory management                                     │
│  • GPU resource orchestration                                   │
└─────────────────────────────────────────────────────────────────┘
```

### Key Components

#### 🧠 **Axon** - The Neural Pathway for ML Models
- **Universal Model Installer**: Works with 80%+ of ML repositories
  - Hugging Face Hub (100,000+ models, 60%+ coverage)
  - PyTorch Hub (research models, 5%+ coverage)
  - TensorFlow Hub (production models, 7%+ coverage)
  - ModelScope (multimodal AI, 8%+ coverage)
- **Model Package Format (MPF)**: Standardized `.axon` packages with `manifest.yaml`
- **Version**: v1.5.0+ (Latest: [v1.5.0](https://github.com/mlOS-foundation/axon/releases))
- **Repository**: [mlOS-foundation/axon](https://github.com/mlOS-foundation/axon)

#### ⚙️ **MLOS Core** - Kernel-Level ML Runtime
- **Multi-Protocol APIs**: HTTP REST, gRPC, IPC (ultra-low latency)
- **Plugin Architecture**: Framework-agnostic via Standard Model Interface (SMI)
- **Resource Management**: Kernel-level optimization for ML workloads
- **Axon Integration**: Reads MPF packages per patent US-63/861,527
- **Version**: v1.0.0+ (Private repository)
- **Distribution**: Docker images (`ghcr.io/mlOS-foundation/mlos-core`) + Binary releases

#### 📐 **SMI Spec** - Standard Model Interface
- **Universal Contract**: Framework-agnostic interface for ML models
- **Multi-Language Support**: C, Python, Go, JavaScript, and more
- **Status**: 🔒 Private - In Development
- **Repository**: Private (will be open-sourced in Phase 3)

## 🏗️ Architecture Overview

### The MLOS Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    Delivery Layer (Axon)                     │
│  • Universal model installer                                 │
│  • Creates standardized .axon packages (MPF)              │
│  • Handles all repository adapters                           │
│  • Provides manifest.yaml with metadata                     │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           │ Axon Package (MPF)
                           │ • manifest.yaml (metadata)
                           │ • model files (framework-specific)
                           │
┌──────────────────────────▼──────────────────────────────────┐
│              Execution Layer (MLOS Core)                    │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Axon Manifest Reader                                │  │
│  │  • Reads manifest.yaml                               │  │
│  │  • Extracts metadata (framework, requirements)      │  │
│  │  • Converts to SMI metadata format                   │  │
│  └──────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Model Registry                                      │  │
│  │  • Stores model metadata from Axon manifest         │  │
│  │  • Maps model_id → plugin_id → model_path          │  │
│  └──────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Plugin Registry                                     │  │
│  │  • Manages framework plugins                         │  │
│  │  • Routes models to appropriate plugins             │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           │ SMI Interface (unchanged)
                           │ • metadata (from Axon manifest)
                           │ • path (to Axon package location)
                           │
┌──────────────────────────▼──────────────────────────────────┐
│              Framework Layer (Plugins)                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │ PyTorch      │  │ TensorFlow   │  │ ONNX         │     │
│  │ Plugin       │  │ Plugin       │  │ Plugin       │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
│                                                             │
│  Plugins:                                                   │
│  • Receive path to model files (from Axon package)         │
│  • Load framework-specific model format                    │
│  • Don't need to know about Axon                           │
│  • SMI interface unchanged                                 │
└─────────────────────────────────────────────────────────────┘
```

### Design Principles

1. **Separation of Concerns**
   - **Axon (Delivery)**: Repository interactions, MPF package creation
   - **MLOS Core (Execution)**: Lifecycle management, plugin routing
   - **Plugins (Framework)**: Framework-specific model loading

2. **Plugin Independence**
   - Plugins receive path to model files (from Axon package)
   - Plugins don't need to know about Axon
   - SMI interface unchanged

3. **Standardized Packaging**
   - All models use MPF format (Axon packages)
   - Consistent metadata structure
   - Universal distribution mechanism

## 📋 Development Phases & Drops

### Phase 0: Foundation (Completed ✅)

**Status**: Complete  
**Timeline**: Q3-Q4 2025

**Deliverables:**
- ✅ Standard Model Interface (SMI) specification
- ✅ Axon v1.0.0 - Hugging Face Hub adapter
- ✅ MLOS Core v1.0.0 - Core runtime with HTTP API
- ✅ Patent applications filed (US-63/861,527, US-63/865,176)

**Key Features:**
- Basic model installation from Hugging Face
- Model Package Format (MPF) implementation
- Plugin architecture foundation
- Multi-protocol API layer (HTTP, IPC)

### Phase 1: Universal Model Delivery (Completed ✅)

**Status**: Complete  
**Timeline**: Q4 2025  
**Release**: Axon v1.1.0 - v1.5.0

**Deliverables:**
- ✅ Axon v1.1.0 - PyTorch Hub adapter (5%+ coverage)
- ✅ Axon v1.2.0 - TensorFlow Hub adapter (7%+ coverage)
- ✅ Axon v1.3.0 - Adapter framework refactoring
- ✅ Axon v1.4.0 - ModelScope adapter (8%+ coverage)
- ✅ Axon v1.5.0 - MLOS Core E2E integration

**Coverage Achievement:**
- **80%+ of ML model user base** covered
- Hugging Face Hub: 60%+ coverage
- PyTorch Hub: 5%+ coverage
- TensorFlow Hub: 7%+ coverage
- ModelScope: 8%+ coverage

**Key Features:**
- Pluggable adapter architecture
- Universal model installer
- Standardized MPF packages
- E2E integration with MLOS Core

### Phase 2: Kernel-Level Optimizations (In Progress 🚧)

**Status**: Active Development  
**Timeline**: Q1 2026

**Planned Deliverables:**
- 🔄 MLOS Core v1.1.0 - Advanced resource scheduling
- 🔄 ML-aware kernel scheduler integration
- 🔄 Tensor memory management optimizations
- 🔄 GPU resource orchestration
- 🔄 MLOS Distribution repository (mlos-distro)

**Key Features:**
- Kernel-level ML optimizations (US-63/865,176)
- Advanced resource pooling
- Multi-model GPU coordination
- Unified MLOS distribution

### Phase 3: Production Readiness (Planned 📅)

**Status**: Planned  
**Timeline**: Q2 2026  
**Target**: Public Release

**Planned Deliverables:**
- 📅 gRPC full implementation
- 📅 Distributed plugin support
- 📅 Advanced monitoring dashboard
- 📅 Automatic scaling
- 📅 ML model versioning
- 📅 Package manager support (Homebrew, APT, etc.)

**Key Features:**
- Production-grade stability
- Enterprise features
- Comprehensive documentation
- Community ecosystem

### Phase 4: Ecosystem Expansion (Future 🔮)

**Status**: Future  
**Timeline**: Q3-Q4 2026+

**Planned Deliverables:**
- 🔮 Additional repository adapters (Replicate, Kaggle, etc.)
- 🔮 Cloud-native deployment options
- 🔮 ML workflow orchestration
- 🔮 Model marketplace integration
- 🔮 Advanced security features

## 📊 Current Status

### Component Versions

| Component | Version | Status | Repository |
|-----------|---------|--------|------------|
| **Axon** | v1.5.0 | ✅ Stable | [axon](https://github.com/mlOS-foundation/axon) |
| **MLOS Core** | v1.0.0 | ✅ Stable (Private) | Private |
| **SMI Spec** | v1.0.0 | 🔒 Private (In Development) | Private |
| **MLOS Distro** | - | 🔄 Planned | Future |

### Patent Status

- 🔒 **US-63/861,527**: Machine Learning Model Operating System
  - **Status**: Pending Provisional Application
  - **Filing Date**: August 11, 2025
  - **Key Innovation**: Model Package Format (MPF), Native lifecycle management

- 🔒 **US-63/865,176**: Kernel-Level Optimizations for ML Workloads
  - **Status**: Pending Provisional Application (Continuation-in-Part)
  - **Related**: US-63/861,527
  - **Key Innovation**: ML-aware scheduler, Tensor memory management, GPU orchestration

## 🚀 Quick Start

### Install Axon

```bash
# Install Axon CLI
curl -sSL axon.mlosfoundation.org | sh

# Install a model
axon install hf/bert-base-uncased@latest

# Register with MLOS Core
axon register hf/bert-base-uncased@latest
```

### Use MLOS Core

```bash
# Run inference via MLOS Core API
curl -X POST http://localhost:8080/models/hf/bert-base-uncased@latest/inference \
  -H "Content-Type: application/json" \
  -d '{"input": "Hello, MLOS!", "batch_size": 1}'
```

## 📚 Repositories

### Core Projects

- **[axon](https://github.com/mlOS-foundation/axon)** - Universal model installer (v1.5.0+)
- **[mlosfoundation.org](https://github.com/mlOS-foundation/mlosfoundation.org)** - Official website

### Private Projects

- **SMI Spec** - Standard Model Interface specification (🔒 Private, in development)
- **MLOS Core** - Kernel-level ML runtime (🔒 Private)

### Supporting Projects

- **[bindings](https://github.com/mlOS-foundation/bindings)** - Language bindings for SMI protocol
- **[examples](https://github.com/mlOS-foundation/examples)** - Examples, tutorials, and best practices

### Private Repositories

- **core** - MLOS Core runtime (kernel-level ML OS)
- **patent-docs** - Patent documentation

## 🌟 Why MLOS?

### The Problem

- **Framework Lock-in**: Models tied to specific frameworks (PyTorch, TensorFlow, etc.)
- **Deployment Complexity**: Different deployment platforms require different integrations
- **No Standard Interface**: Each framework has its own API and lifecycle
- **Resource Inefficiency**: No unified resource management for ML workloads

### The Solution

- **Universal Interface**: SMI provides framework-agnostic contract
- **Standardized Packaging**: MPF (Model Package Format) via Axon
- **Kernel-Level Optimization**: OS-level ML resource management
- **Deployment-Agnostic**: Models run identically across environments

### The Vision

Just as POSIX made software portable across Unix systems, **MLOS makes ML models portable across any framework and deployment environment**.

## 📞 Contact & Resources

- **Website**: [mlosfoundation.org](https://mlosfoundation.org)
- **Email**: info@mlos-foundation.org
- **Status**: Private MVP Development
- **Public Release**: Q2 2026

## 📄 License

All MLOS Foundation projects are licensed under the MIT License.

---

**© 2025 mlOS Foundation. Building the future of ML infrastructure.**

*"Just as POSIX made software portable across Unix systems, MLOS makes ML models portable across any framework and deployment environment."*
