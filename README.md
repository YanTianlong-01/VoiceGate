# VoiceGate: Cross-Language Video Dubbing Engine



[中文文档](README_zh.md)

> A cross-language video translation & dubbing pipeline powered by VoxCPM2 and ComfyUI

## Overview

VoiceGate is a cross-language video dubbing engine built on VoxCPM2 and ComfyUI. VoxCPM2 supports **30 languages** (including 8 Southeast Asian languages) and **9 Chinese dialects** (Cantonese, Sichuanese, Wu, Northeastern, Hokkien, etc.), with voice cloning and timbre design capabilities. The engine achieves frame-level alignment between TTS audio and SRT subtitle timestamps via the custom-built **VoiceBridge** plugin, ensuring dubbing stays perfectly in sync with the video.

The complete pipeline covers ASR subtitle extraction, LLM translation, multilingual TTS, and synchronized audio merging — all visually orchestrated as a drag-and-drop node graph, ready to use out of the box.

[Watch the demo on Bilibili (Chinese)](https://www.bilibili.com/video/BV1sc7C6VE9e/?share_source=copy_web&vd_source=94a8c00ee32b6b955ae0133d5103f92a)

## Use Cases

- **Content Going Global** — One-click dubbing of Chinese educational/knowledge videos into English, Japanese, Korean, etc., for publishing on YouTube/TikTok
- **Content Importing** — Dubbing overseas YouTube/Bilibili content into Chinese or regional dialects for the domestic market
- **Multilingual Localization** — Producing multi-language versions of documentaries, museum guides, and educational materials

## Architecture

![VoiceGate Architecture](assets/architecture_en.drawio.svg)

### Core Modules

| Module | Description |
|--------|-------------|
| VoiceBridge ASR | Speech-to-subtitle via Qwen3-ASR with forced alignment |
| LLM Translation | Subtitle translation with customizable prompt & model |
| VoxCPM2 TTS | 30 languages + 9 dialects, voice cloning & timbre design |
| VoiceBridge SRT Alignment | Frame-level merging of TTS audio with SRT timestamps |
| MelBandRoFormer | Vocal separation with optional ambient sound preservation |

## Key Innovation

The **VoiceBridge plugin** ([GitHub](https://github.com/YanTianlong-01/comfyui_voicebridge)) is the core contribution of this project — the first to bring SRT-timestamp-driven multilingual TTS alignment into ComfyUI's visual workflow:

- `VoiceBridgeAudioListMergerBySRT` — Merges per-sentence TTS audio strictly aligned to subtitle timelines
- `VoiceBridgeSRTSplitter` — Splits SRT subtitles by timestamp to drive per-sentence TTS generation
- `VoiceBridgeASRTranscribe` — ASR transcription with forced alignment, outputting time-stamped SRT
- Solves the classic "audio-video desync" pain point in AI dubbing, achieving frame-level synchronization

## Project Structure

```
VoiceGate/
├── comfyui_voicebridge/  # VoiceBridge ComfyUI plugin (git submodule)
├── workflows/             # ComfyUI workflow JSON files
├── README.md              # This file (English)
├── README_zh.md           # 中文文档 (Chinese documentation)
└── LICENSE
```

## Quick Start

### Option 1: Try Online (Recommended)

No local setup needed — just open in your browser:

👉 [Online App (Audio)](https://www.runninghub.cn/ai-detail/2062442306350964737?inviteCode=rh-v1455)
👉 [Online App (Video)](https://www.runninghub.cn/ai-detail/2062446982618238978?inviteCode=rh-v1455)

👉 [ComfyUI Online Workflow (Audio)](https://www.runninghub.cn/post/2062432233125928961?inviteCode=rh-v1455)
👉 [ComfyUI Online Workflow (Video)](https://www.runninghub.cn/post/2062445363042283522?inviteCode=rh-v1455)

👉 [Hugging Face Space](https://huggingface.co/spaces/build-small-hackathon/VoiceGate)

Open the link → Click "Run Now" → Upload video → Select target language → Generate dubbing

### Option 2: Local Deployment

#### 1. Clone the Repository (with submodules)

```bash
git clone --recursive https://github.com/YanTianlong-01/VoiceGate.git
cd VoiceGate
```

#### 2. Install ComfyUI & Dependencies

```bash
# Install ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI && pip install -r requirements.txt

# Install VoiceBridge plugin
cp -r ../VoiceGate/comfyui_voicebridge custom_nodes/
```

#### 3. Load Workflows

Drag the JSON files from `workflows/` into ComfyUI, configure your API key (for LLM translation), and hit run.

## Dependencies

- ComfyUI
- VoxCPM2 (via ComfyUI_RH_VoxCPM nodes)
- Qwen3-ASR / Qwen3-ForcedAligner
- MelBandRoFormer

## License

Apache-2.0

## Acknowledgements

- [VoxCPM2](https://github.com/OpenBMB/VoxCPM) — OpenBMB's open-source multilingual TTS model
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) — Visual workflow engine


