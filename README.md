# VoiceGate：跨语言视频智能配音引擎

> 基于 VoxCPM2 与 ComfyUI 的跨语言视频翻译配音流水线

## 项目简介

VoiceGate 是基于 VoxCPM2 与 ComfyUI 构建的跨语言视频智能配音引擎。VoxCPM2 支持 30 种语言（含东南亚八国语言）与 9 种中文方言（粤语、四川话、吴语、东北话、闽南语等），具备声音克隆与音色设计能力。引擎通过自研 VoiceBridge 插件实现 TTS 语音与 SRT 字幕时间戳的帧级对齐，确保配音与画面精准同步。

完整链路覆盖 ASR 字幕提取、LLM 翻译、多语言 TTS 到音频对齐合并，节点图可视化编排，开箱即用。

## 应用场景

- **内容出海**：中文知识类视频一键翻配为英文、日文、韩文等版本，发布到 YouTube/TikTok
- **视频引进**：海外 YouTube/B 站内容翻配为中文或方言版本，引进国内
- **多语言本地化**：纪录片、博物馆讲解、教育内容的多语言版本制作

## 技术架构

```mermaid
flowchart LR
    subgraph input [输入]
        A[("🎬 输入视频")]
    end

    subgraph pipeline [处理流水线]
        direction LR
        B[("🎙️ VoiceBridge ASR<br/>字幕提取 + Forced Alignment")]
        C[("🌐 LLM 翻译<br/>字幕跨语言翻译")]
        D[("🔊 VoxCPM2 TTS<br/>多语言语音合成")]
        E[("🎛️ VoiceBridge SRT 对齐<br/>时间戳驱动音频合并")]
    end

    subgraph output [输出]
        F[("🎧 配音音频 + 同步字幕")]
    end

    A -->|"提取语音"| B
    B -->|"SRT 字幕"| C
    C -->|"翻译后文本"| D
    D -->|"逐句音频"| E
    E -->|"对齐合并"| F

    classDef input fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef pipeline fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef output fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    class A input
    class B,C,D,E pipeline
    class F output
```

### 核心模块

| 模块 | 说明 |
|------|------|
| VoiceBridge ASR | 基于 Qwen3-ASR 的语音转字幕，支持 forced alignment |
| LLM 翻译 | 字幕翻译（支持自定义 prompt 与模型） |
| VoxCPM2 TTS | 30 语言 + 9 方言，声音克隆与音色设计 |
| VoiceBridge SRT 对齐 | TTS 语音与 SRT 时间戳帧级对齐合并 |
| MelBandRoFormer | 人声分离，支持保留环境音 |

## 核心创新

**VoiceBridge 插件**（[GitHub](https://github.com/YanTianlong-01/comfyui_voicebridge)）是本项目核心贡献，首次将 SRT 字幕时间戳驱动的多语言 TTS 对齐引入 ComfyUI 可视化工作流：

- `VoiceBridgeAudioListMergerBySRT`：TTS 生成的逐句音频严格按字幕时间轴合并
- `VoiceBridgeSRTSplitter`：SRT 字幕按时间戳拆分，驱动 TTS 逐句生成
- `VoiceBridgeASRTranscribe`：ASR 转录 + forced alignment，输出带时间戳的 SRT
- 解决传统 AI 配音"音画脱节"的痛点，实现配音与原片节奏帧级同步

## 项目结构

```
VoiceGate/
├── comfyui_voicebridge/  # VoiceBridge ComfyUI 插件 (git submodule)
├── workflows/             # ComfyUI 工作流 JSON
├── README.md              # 本文件
└── LICENSE
```

## 快速开始

### 1. 克隆仓库（含子模块）

```bash
git clone --recursive https://github.com/YanTianlong-01/VoiceGate.git
cd VoiceGate
```

### 2. 安装 ComfyUI 与依赖

```bash
# 安装 ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI && pip install -r requirements.txt

# 安装 VoiceBridge 插件
cp -r ../VoiceGate/comfyui_voicebridge custom_nodes/
```

### 3. 加载工作流

将 `workflows/` 中的 JSON 文件拖入 ComfyUI，配置 API Key 后即可运行。

## 依赖

- ComfyUI
- VoxCPM2 (通过 ComfyUI_RH_VoxCPM 节点)
- Qwen3-ASR / Qwen3-ForcedAligner
- MelBandRoFormer

## 许可证

Apache-2.0

## 鸣谢

- [VoxCPM2](https://github.com/OpenBMB/VoxCPM) — OpenBMB 开源多语言 TTS 模型
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) — 可视化工作流引擎
