<!--
 * Copyright 2022-2023 SPACEMIT. All rights reserved.
 * Use of this source code is governed by a BSD-style license
 * that can be found in the LICENSE file.
 * 
 * @Author: David(qiang.fu@spacemit.com)
 * @Date: 2026-05-12 20:12:39
 * @LastEditTime: 2026-05-16 00:00:00
 * @FilePath: \doc\docs-ai\en\solutions\aicomputer_solution\hermes.md
 * @Description: 
-->
sidebar_position: 8

# Hermes Agent

**Hermes Agent** is an open-source AI agent framework that supports multiple LLM providers, with capabilities including tool calling, scheduled tasks, MCP protocol, and web access. It can be used via terminal CLI or integrated into messaging platforms (Telegram, Discord, etc.).

## Platform Support

| Platform & OS         | Supported |
| --------------------- | --------- |
| K1 Buildroot          | ❌ No     |
| K1 OpenHarmony        | ❌ No     |
| K1 Bianbu LXQT/GNOME  | ✅ Yes    |
| K3 Buildroot          | ❌ No     |
| K3 OpenHarmony        | ❌ No     |
| K3 Bianbu LXQT/GNOME  | ✅ Yes    |

## Installation

### Option 1: apt Install (Recommended)

Install directly via apt on Bianbu OS:

```bash
sudo apt update
sudo apt install hermes-agent
```

Once installed, the `hermes` command is available immediately — no additional environment setup required.
![alt text](../../../zh/solutions/static/hermes/hermes-agent.png)

### Option 2: Source Install (Developer)

For those who need to modify the code or contribute to development.

#### Prerequisites

- Python 3.11+ (uv will download it automatically)
- `libffi8` (runtime library, included in the system)
- `libffi-dev` headers (must be extracted manually, see below)

#### 1. Clone the Repository

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
```

#### 2. Install uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
```

#### 3. Extract libffi-dev Headers

Building `cffi` / `cryptography` on riscv64 requires `ffi.h`, but the system only ships the runtime library without headers. Use `apt-get download` to fetch the `.deb` package (no install, no sudo required), then extract the headers with `dpkg-deb -x`:

```bash
apt-get download libffi-dev
mkdir -p /tmp/libffi-dev
dpkg-deb -x libffi-dev_*.deb /tmp/libffi-dev
```

> `/tmp/libffi-dev` is lost on reboot. Re-run this step if you need to reinstall dependencies. An already-installed venv is not affected.

#### 4. Create a Virtual Environment and Install Dependencies

```bash
cd hermes-agent

export PKG_CONFIG_PATH="/tmp/libffi-dev/usr/lib/riscv64-linux-gnu/pkgconfig:$PKG_CONFIG_PATH"
export C_INCLUDE_PATH="/tmp/libffi-dev/usr/include/riscv64-linux-gnu:$C_INCLUDE_PATH"
export CPATH="/tmp/libffi-dev/usr/include/riscv64-linux-gnu:$CPATH"
export LIBRARY_PATH="/tmp/libffi-dev/usr/lib/riscv64-linux-gnu:$LIBRARY_PATH"

uv venv venv --python 3.11
source venv/bin/activate

uv pip install -e ".[cron,cli,dev,mcp,web]"
```

> The `voice` and `messaging` extras have limitations on riscv64. See [Unavailable Features](#unavailable-features).

## Configuration

### 1. Set API Key

Edit `~/.hermes/.env` (auto-generated on first run, or create it manually). Example using the MiniMax domestic endpoint:

```env
MINIMAX_CN_API_KEY=your_key_here
MINIMAX_CN_BASE_URL=https://api.minimaxi.com/v1
```

For the source install, you can also edit `.env` in the project directory directly:

```bash
cp .env.example .env
```

### 2. Configure the Model

Edit `~/.hermes/config.yaml` (auto-generated on first run, or create it manually):

```yaml
model:
  default: MiniMax-M2.7-highspeed
  provider: custom
  base_url: https://api.minimaxi.com/v1
  api_mode: chat_completions

custom_providers:
- name: minimax-cn
  base_url: https://api.minimaxi.com/v1
  api_key: your_key_here
  api_mode: chat_completions
  model: MiniMax-M2.7-highspeed
```

> The MiniMax API only accepts plain model names (e.g. `MiniMax-M2.7-highspeed`). Provider-prefixed names (e.g. `minimax-cn/MiniMax-M2.7-highspeed`) are not accepted.

## Starting the Agent

**apt install** — run directly:

```bash
hermes
```

**Source install** — activate the virtual environment each time you open a new terminal:

```bash
cd hermes-agent
source venv/bin/activate
./hermes
```

## Supported LLM Providers

Set the corresponding key in `.env` to switch providers:

| Provider          | Environment Variable   |
| ----------------- | ---------------------- |
| OpenRouter        | `OPENROUTER_API_KEY`   |
| Google Gemini     | `GOOGLE_API_KEY`       |
| Kimi / Moonshot   | `KIMI_API_KEY`         |
| MiniMax (Global)  | `MINIMAX_API_KEY`      |
| MiniMax (China)   | `MINIMAX_CN_API_KEY`   |
| GLM / z.ai        | `GLM_API_KEY`          |
| Hugging Face      | `HF_TOKEN`             |

## Unavailable Features

The following features are not available due to riscv64 platform limitations:

| Feature                        | Reason                                                                   |
| ------------------------------ | ------------------------------------------------------------------------ |
| `voice` (speech recognition)   | `ctranslate2` has no riscv64 wheel; `faster-whisper` depends on it      |
| Local STT (speech-to-text)     | Same as above                                                            |
| `messaging` Discord encryption | `pynacl` requires `cffi` to build; skipped due to missing libffi headers |

### Messaging (Optional)

The `messaging` feature connects Hermes Agent to messaging platforms (Telegram, Discord, Slack, WhatsApp, Signal, Matrix). Once a bot token is configured for the target platform, you can chat with the agent directly from those apps.

To enable it in a source install, set the libffi environment variables and install separately:

```bash
export PKG_CONFIG_PATH="/tmp/libffi-dev/usr/lib/riscv64-linux-gnu/pkgconfig:$PKG_CONFIG_PATH"
export LIBRARY_PATH="/tmp/libffi-dev/usr/lib/riscv64-linux-gnu:$LIBRARY_PATH"
export CPATH="/tmp/libffi-dev/usr/include/riscv64-linux-gnu:$CPATH"
source venv/bin/activate
uv pip install -e ".[messaging]"
```

Start the messaging gateway:

```bash
./hermes gateway
```


[def]: ../static/hermes/hermes-agent.png