# 🧩 Certificate-Template

### 🌟 功能简介  
该工作流可根据一张 **证书风格参考图** 自动生成同风格的 **证书模板底图**，保持边框、版式、配色等风格一致。  
适用于 AI 证书生成、定制化证书模板设计、视觉风格迁移。

---

### 🧠 原理说明  
- 使用 **IPAdapter + CLIPVisionLoader** 提取风格特征；  
- 使用 **ControlNet (Canny)** 保持几何轮廓；  
- 使用 **SDXL Base + Refiner** 生成空白模板；  
- 支持 Prompt 自定义内容风格（如 “minimalist” / “classic”）。

---

### 🧱 节点结构  

| 模块 | 节点 | 功能说明 |
|------|------|-----------|
| 模型加载 | SDXL Base / Refiner | 生成与细化图像 |
| 风格提取 | IPAdapter + CLIPVisionLoader | 提取模板视觉风格 |
| 几何约束 | ControlNet (Canny) | 保持边框与结构 |
| 文本提示 | CLIPTextEncode | 自定义语义控制 |
| 输出 | VAEDecode + SaveImage | 输出生成结果 |

---

### ⚙️ 使用步骤  
1. 在 **LoadImage** 上传证书风格参考图；  
2. 在 **Prompt** 中描述希望生成的风格（如 “clean modern certificate layout”）；  
3. 运行生成即可获得新模板；  
4. 可作为后续文字与徽章排版的底图。

---

### 💡 小技巧  
- 如果想强化边框细节，可增加 Canny ControlNet 权重。  
- 若生成区域不平衡，可微调 `Seed` 或 `CFG` 值以获得更多样式。

