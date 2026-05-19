# SpacemiT AI Lab

**SpacemiT AI Lab** 是一个基于 Web 的 AI 评估平台。用户通过网页申请云 K3 实例，无需任何硬件准备，即可在线体验 SpacemiT K3 AI CPU 上的模型推理效果与真实性能数据，实现 **<big>零配置、即开即用</big>** 的评估体验。

K3 设备本地同样内置 AI Lab 桌面应用，支持下载模型后在本地直接运行体验。

## ✨ 核心能力

- **云上真机体验**：申请云 K3 实例，网页直接调用真实硬件进行推理，实时返回结果与性能数据
- **视觉模型体验**：目标检测、实例分割、姿态估计、目标跟踪、图像分类
- **大语言模型对话**：智能问答、文本生成，流式输出
- **语音识别（ASR）**：音频转文字、实时录音识别
- **语音合成（TTS）**：文字转语音播放
- **语音活动检测（VAD）**：语音片段检测与切分
- **模型性能看板**：查看各模型在 K3 真实硬件上的性能指标（FPS / RTF / token/s）

## 平台支持情况

| 平台 & 系统          | 是否支持 |
| -------------------- | -------- |
| K1 Buildroot         | ❌ 不支持 |
| K1 OpenHarmony       | ❌ 不支持 |
| K1 Bianbu LXQT/GNOME | ❌ 不支持 |
| K3 Buildroot         | ❌ 不支持 |
| K3 OpenHarmony       | ❌ 不支持 |
| K3 Bianbu LXQT/GNOME | ✅ 支持   |

## 技术架构

### 业务架构图

![业务架构图](../static/ailab-framework.png)

### 技术架构图

![技术架构图](../static/ailab-tech.png)

### 应用技术栈

- **桌面框架**：Electron 41（RISC-V 优化版本，K3 本地应用）
- **前端技术**：原生 JavaScript + HTML5 + CSS3
- **后端服务**：SpacemiT AI Gateway（端口 18790）
- **模型推理**：ONNX Runtime（视觉/语音模型）+ llama.cpp（大语言模型，SpacemiT 加速版本）

### 依赖服务

- **AI Gateway**：统一推理网关，通过 HTTP/WebSocket 提供 ASR / TTS / VAD / Vision / LLM 域 API（`/v1/asr`、`/v1/tts`、`/v1/vad`、`/v1/vision`、`/v1/chat/completions`）
- **llama-server**：独立 LLM 数据面服务，由 AI Gateway 代理转发推理请求（端口 8080）
- **模型数据源**：从 SpacemiT Model Zoo 获取最新模型信息与性能数据

### 工作流程

```
1. 云上体验流程：Bianbu Cloud → 官网入口 → AI Lab 主页 → 在线体验模型推理 → 查看实时性能数据
                                                               ↓
                                                          查看官方性能数据

2. 本地体验流程：启动 AI Lab 本地应用 → 本地 APP 页面 → 下载模型 → 试用模型 → 体验模型推理 → 查看实时性能数据
                                                               ↓
                                                          查看官方性能数据

3. 局域网分享流程：启动应用 → 复制分享链接 → 其他设备浏览器访问
```

## 🚀 安装（K3 本地应用）

### 系统要求

- **操作系统**：Bianbu 4.0 rc4 之后的 LXQt 或 GNOME
- **硬件平台**：SpacemiT K3 RISC-V 设备
- **内存要求**：建议 8 GB 及以上
- **存储空间**：至少 10 GB 可用空间（用于模型下载）

### 安装步骤

```bash
sudo apt update
sudo apt install spacemit-ailab spacemit-ai-gateway
```

安装完成后会自动配置 systemd 服务并创建桌面快捷方式。

### 验证安装

```bash
# 检查 AI Gateway 服务
systemctl status spacemit-ai-gateway

# 检查服务端口
curl -s localhost:18790/healthz
```

## 快速开始

### 1) 云上体验（推荐）

访问云平台，无需任何硬件准备：

1. 进入 SpacemiT 开发者平台，找到 **AI Lab** 入口
2. 点击"立即体验"，申请体验实例，等待系统分配云 K3 实例（< 3 秒）
3. 实例就绪后自动跳转到模型中心页面，即可开始体验

> **注意**：每次体验时长最长 **2 小时**，超时或关闭页面后实例自动回收。

### 2) K3 本地应用启动

**方式一：从应用菜单启动**

点击系统菜单，搜索 **SpacemiT AI Lab** 或 **AI Lab**，点击启动。

**方式二：从终端启动**

```bash
/opt/spacemit-ailab/spacemit-ailab
```

### 3) 界面概览

应用启动后显示模型中心主页，包含：

- **顶部导航栏**：局域网分享链接及复制按钮、开机自启开关、语言切换按钮
- **实例状态栏**：云上体验时显示剩余可用时间
- **模型分类标签**：热门、视觉、大语言模型、语音
- **模型卡片网格**：展示各类 AI 模型及下载/体验状态
- **性能数据看板**：各模型在 K3 真实硬件上的性能指标

![应用主页](../static/ailab.png)

## 功能使用

### 云入口
进入 SpacemiT 开发者平台，找到 **AI Lab** 入口：
打开https://www.spacemit.com/，点击"体验中心"，选择"AI 体验"，进入https://www.spacemit.com/ailab 页面

![云入口](../static/ailab-inter.png)

### 云实例管理

#### 1) 申请体验实例

在云平台首页查看当前可用实例数量，点击"立即体验"按钮，申请体验实例，获取专属 K3 实例。

![申请体验实例](../static/ailab-1.png)

#### 2) 实例使用时间

进入模型中心后，顶部状态栏显示剩余体验时间（最长 2 小时）。时间即将耗尽时会提前提醒。

![实例状态栏](../static/ailab-2.png)

#### 3) 释放实例

- **自动释放**：2 小时到期后或关闭模型中心页面时自动回收

![释放实例](../static/ailab-free.png)

> **隐私保护说明**：实例释放时，应用自动清除本次会话产生的所有用户数据，包括 LLM 对话历史、上传的图片、录音文件及音频临时文件，数据仅在内存中处理，**不会在本地持久化保存**。

### 模型中心

#### 1) 分类浏览

点击顶部分类标签快速筛选：

- **热门**：最受欢迎的模型
- **视觉**：目标检测、实例分割、姿态估计、图像分类等
- **大语言模型**：对话、翻译、文本生成
- **语音**：ASR 语音识别、TTS 语音合成、VAD 语音检测

![模型分类浏览](../static/ailab-3.png)

#### 2) 模型卡片信息

每个模型卡片显示：

- 模型名称与任务类型
- 输入规格（尺寸/格式）
- 部署精度（INT8、FP16 等）
- 下载状态：**未下载**（显示"下载模型"按钮）/ **下载中**（显示进度）/ **已下载**（显示"试用模型"按钮）

### 视觉模型体验

1. 在模型列表中找到视觉模型（如 YOLOv8n、YOLOv11s），点击"试用模型"
2. 在左侧边栏点击示例图片，或点击"上传图片"选择本地文件
3. 对于目标检测模型，可调整**置信度阈值**（默认 0.35）和 **IoU 阈值**（默认 0.45）
4. 推理完成后查看标注结果（检测框 / 关键点 / 分割区域）及性能指标

![视觉模型体验](../static/ailab-4.png)

**支持的视觉任务：**

| 任务类型 | 代表模型                                                 |
| -------- | -------------------------------------------------------- |
| 目标检测 | YOLOv8n/s/m、YOLOv11n/s/m 、YOLOv5-Gesture、YOLOv5n-Face |
| 图像分割 | YOLOv8n/s/m-seg 系列                                     |
| 姿态估计 | YOLOv8n/s/m-pose 系列                                    |
| 人脸识别 | ArcFace-MobileFaceNet                                    |
| 图像分类 | ResNet50                                                 |

### 大语言模型对话

1. 找到大语言模型（如 Qwen-3-0.6B），点击"试用模型"
2. 在底部输入框输入问题，支持添加文档链接，按回车发送
3. AI 流式返回回复，支持多轮连续对话
4. 点击消息右侧复制按钮可复制回复内容

![LLM 对话界面](../static/ailab-5.png)

**支持的 LLM 模型：** Qwen2.5、Qwen3、Qwen3.5 系列等

### 语音识别（ASR）

1. 找到 ASR 模型（如 SenseVoice），点击"试用模型"
2. 选择体验方式：
   - **示例音频**：点击左侧示例直接识别
   - **上传音频**：支持 WAV格式
   - **实时录音**：点击"开始录音" → 说话 → "停止录音"，自动转写
3. 识别结果显示转写文本及处理耗时

![ASR 识别结果](../static/ailab-6.png)

### 语音合成（TTS）

1. 找到 TTS 模型（如 Matcha Icefall EN-US、Matcha Icefall ZH-Baker、Matcha Icefall ZH-EN），点击"试用模型"
2. 在文本框输入内容（最多 500 字）
3. 根据模型选择中文或英文
4. 点击"生成音频"，等待合成完成后自动播放

![TTS 合成界面](../static/ailab-7.png)

### 语音活动检测（VAD）

1. 找到 VAD 模型（Silero VAD），点击"试用模型"
2. 录音或上传音频文件
3. 检测结果以可视化形式展示语音活动段及时间边界

![VAD 检测结果](../static/ailab-8.png)

### 模型性能看板

在主页下方查看所有模型在 K3 上的性能指标：

- 点击分类标签筛选视觉 / LLM / 语音模型
- 指标包含PP128(token/s)、TG128(token/s)、音频时长、处理时间、RTF、输入形状、帧率(FPS)、量化类型 等

![模型性能看板](../static/ailab-9.png)

### 模型下载（K3 本地应用）

- 点击模型卡片上的"下载模型"按钮开始下载
- 所有模型存储于 `~/.cache/models/`（按类别分目录）

![模型下载进度](../static/ailab-10.png)

## 高级功能

### 开机自启动

点击顶部"开机自启动"开关，系统启动时自动运行 AI Lab，方便局域网内其他设备随时访问。

### 语言切换

点击顶部右上角语言按钮（EN / 中文），切换界面语言。

### 局域网分享

应用启动后自动开启局域网分享：

1. 查看顶部显示的访问地址（如 `http://192.168.1.100:8889`）
2. 同一局域网内的其他设备浏览器访问该地址，无需安装应用

![局域网分享](../static/ailab-11.png)

## 常见问题

### 应用无法启动？

```bash
# 检查 AI Gateway 服务状态
systemctl status spacemit-ai-gateway

# 重启服务
sudo systemctl restart spacemit-ai-gateway

# 检查端口占用
netstat -tulpn | grep 18790
```

### 模型下载失败？

- 检查网络连接是否正常
- 确认磁盘剩余空间充足：`df -h`
- 删除不需要的模型文件后重试

### 推理速度较慢？

- 选择较小参数量的模型（如 YOLOv8n 而非 YOLOv8m）
- 关闭其他占用资源的应用
- 同一时间只运行一个推理任务

### 局域网分享无法访问？

- 确认访问设备与 K3 在同一局域网
- 检查防火墙是否开放 8889 端口

### 如何查看日志？

```bash
# 查看 AI Gateway 日志
journalctl -u spacemit-ai-gateway -f
```

### 如何卸载应用？

```bash
sudo apt remove spacemit-ailab
# 同时删除下载的模型（可选）
rm -rf ~/.cache/models/
```

## 支持的模型

| 类型 | 代表模型                     | 格式   |
| ---- | ---------------------------- | ------ |
| LLM  | Qwen2.5/3/3.5、              | GGUF   |
| 视觉 | YOLOv5/v8/v11 系列、ResNet50 | ONNX   |
| ASR  | SenseVoice、Qwen3-ASR        | tar.gz |
| TTS  | Matcha-TTS（中文/英文）      | tar.gz |
| VAD  | Silero VAD                   | tar.gz |

## 技术支持

- **官方文档**：https://www.spacemit.com/community/document
- **开发者社区**：https://www.spacemit.com/community
- **问题反馈**：通过社区论坛或 GitLab Issues 提交
