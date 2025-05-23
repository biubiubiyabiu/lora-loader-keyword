如何安装 (How to Install)

1.  **下载节点文件**:
    *   您可以直接克隆此 GitHub 仓库，或者下载 `lora_utils_nodes.py` 和 `__init__.py` 文件。
2.  **放置文件**:
    *   在您的 ComfyUI 安装目录下，找到 `custom_nodes` 文件夹。
    *   在 `custom_nodes` 文件夹内创建一个名为 `MyAwesomeNodes` (或者您喜欢的任何名称，但要与 `__init__.py` 中的导入路径对应) 的新文件夹。
    *   将下载的 `lora_utils_nodes.py` 和 `__init__.py` 文件放入您创建的这个新文件夹中。
    *   最终路径结构应如下所示:
        ```
        ComfyUI/
        └── custom_nodes/
            └── MyAwesomeNodes/  <-- 您创建的文件夹
                ├── __init__.py
                └── lora_utils_nodes.py
        ```
3.  **重启 ComfyUI**:
    *   完全关闭 ComfyUI 后台（关闭命令行窗口或停止相关服务）。
    *   重新启动 ComfyUI。
    *   启动时，您应该能在 ComfyUI 的终端/命令行窗口看到类似 "### 正在加载: My Awesome Nodes (lora_utils_nodes) ###" 的消息。

## 如何使用 (How to Use)

1.  在 ComfyUI 画布上，右键点击 -> "Add Node" -> "My Awesome Nodes" (或您在代码中设置的 `CATEGORY`) -> "Loaders" -> "LoRA Loader w/ Keyword Injection (Awesome)" (或您设置的显示名称)。
2.  **连接输入**:
    *   将您的 `Load Checkpoint` 节点的 `MODEL` 和 `CLIP` 输出分别连接到此节点的 `model` 和 `clip` 输入。
    *   将您的主 `CLIPTextEncode` (Positive) 节点的 `CONDITIONING` 输出连接到此节点的 `positive_conditioning` 输入。
    *   将您的主 `CLIPTextEncode` (Negative) 节点的 `CONDITIONING` 输出连接到此节点的 `negative_conditioning` 输入。
3.  **配置节点**:
    *   从 `lora_name` 下拉菜单中选择您想要使用的 LoRA。
    *   根据需要调整 `strength_model` 和 `strength_clip`。
    *   在 `lora_keywords` 文本框中输入该 LoRA 的特定触发词，例如 "character_name, armor, solo"。
4.  **连接输出**:
    *   将此节点的 `MODEL` 输出连接到 `KSampler` 节点的 `model` 输入。
    *   将此节点的 `POSITIVE_CONDITIONING_ENHANCED` 输出连接到 `KSampler` 节点的 `positive` 输入。
    *   将此节点的 `NEGATIVE_CONDITIONING_PASSTHROUGH` 输出连接到 `KSampler` 节点的 `negative` 输入。
5.  **链式使用 (Chaining for Multiple LoRAs)**:
    *   如果您需要使用多个 LoRA，可以串联多个 `LoraLoaderWithKeywordInjection_Awesome` 节点。
    *   将前一个 LoRA 节点的 `MODEL` 输出连接到下一个 LoRA 节点的 `model` 输入。
    *   将前一个 LoRA 节点的 `CLIP` 输出连接到下一个 LoRA 节点的 `clip` 输入。
    *   将前一个 LoRA 节点的 `POSITIVE_CONDITIONING_ENHANCED` 输出连接到下一个 LoRA 节点的 `positive_conditioning` 输入。
    *   Negative Conditioning 通常可以从最初的 Negative Prompt 节点一直透传到最后一个 KSampler。
## 概述 (Overview)

这是一个为 [ComfyUI](https://github.com/comfyanonymous/ComfyUI) 设计的自定义节点，旨在简化使用多个 LoRA 模型及其特定触发关键词（trigger words）的工作流程。

当您在项目中使用多个 LoRA 时，将每个 LoRA 的激活词手动添加到主提示文本中可能会变得混乱且容易出错。此节点允许您在加载每个 LoRA 的同时，为其指定独立的关键词，这些关键词会自动注入到正向提示（Positive Conditioning）中。

**主要功能:**
*   从您的 `ComfyUI/models/loras` 文件夹中选择并加载 LoRA 文件。
*   为加载的 LoRA 设置模型（Model）和 CLIP 的应用强度。
*   为该 LoRA 输入特定的触发关键词或短语。
*   将这些关键词编码后的 Conditiong 附加到传入的 Positive Conditioning 之后。
*   输出已应用 LoRA 的模型（Model）、CLIP，以及增强后的 Positive Conditioning 和透传的 Negative Conditioning。
*   可以链式连接多个此节点，以分别管理和应用多个 LoRA 及其关键词。
*   包含一个 "enabled" 开关，方便快速禁用/启用节点效果进行对比。

## 痛点解决 (Problem Solved)

*   **简化多 LoRA 工作流**: 无需在全局提示框中手动混合所有 LoRA 的关键词。
*   **提高可维护性**: 每个 LoRA 及其关键词都在一个节点内管理，使工作流更清晰。
*   **模块化**: 方便地添加、移除或调整单个 LoRA 及其效果，而不影响其他部分。
