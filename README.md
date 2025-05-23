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
