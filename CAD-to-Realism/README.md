# 🧩 CAD-to-Realism

### 🌟 功能简介  
该工作流可将 **未渲染的模型图（CAD 基底图）** 与一张 **风格参考图** 结合，生成既保留模型结构与细节、又具备逼真质感的风格化真实感图像。  
适用于产品渲染预览、工业设计视觉化、建筑模型可视化等场景。

---

### 📸 效果示例  

**输入基底图：**  
[![1.jpg](https://i.postimg.cc/pTGnzjK3/1.jpg)](https://postimg.cc/zbhB1Vtk)  

**风格参考图 1 → 生成结果：**  
[![1-comfyui.jpg](https://i.postimg.cc/SsvJJhhh/1-comfyui.jpg)](https://postimg.cc/mz7b5KkX)  

**风格参考图 2 → 生成结果：**  
[![1-comfyui2.jpg](https://i.postimg.cc/2jg7tpZm/1-comfyui2.jpg)](https://postimg.cc/c6M3Kbh5)

---

### 🧠 核心原理  
该工作流通过以下模块协同实现：
- **IPAdapter + CLIPVisionLoader**：提取风格图特征并融合入生成模型；  
- **ControlNet (Canny + Depth)**：保持 CAD 图的轮廓与空间深度；  
- **DepthAnythingV2 模型**：生成深度图信息；  
- **LoRA + SDXL 模型**：增强真实感与纹理细节；  
- **Upscaler**：对最终图像进行超分放大。

---

### 🧱 节点结构概览  

| 模块 | 节点名称 | 功能说明 |
|------|-----------|-----------|
| 模型加载 | CheckpointLoaderSimple (`sd_xl_base_1.0`, `sd_xl_refiner_1.0`) | 加载基础与精修模型 |
| 风格输入 | LoadImage (参考图) + CLIPVisionLoader | 读取风格图并提取视觉嵌入 |
| 风格控制 | IPAdapterModelLoader + IPAdapterAdvanced | 将风格特征注入生成流程 |
| CAD 输入 | LoadImage + ResizeAndPadImage | 读取并适配基底 CAD 图尺寸 |
| 结构保持 | ControlNet (Canny/Depth) | 保持几何与深度结构 |
| 深度估计 | DownloadAndLoadDepthAnythingV2Model + DepthAnything_V2 | 自动生成深度引导图 |
| 文本提示 | CLIPTextEncodeSDXL/Refiner | 语义控制与伪影去除 |
| 图像生成 | KSamplerAdvanced (两阶段) | 基础生成 + Refiner 微调 |
| 超分辨率 | 4x-UltraSharp Upscaler | 清晰化与细节增强 |
| 输出 | SaveImage | 保存最终生成结果 |

---

### ⚙️ 使用步骤  
1. 打开 ComfyUI，导入 `CAD-to-Realism.json`。  
2. 在节点 **LoadImage (基底图)** 上传 CAD 模型图。  
3. 在节点 **LoadImage (参考图)** 上传风格图像。  
4. 调整 Prompt / Negative Prompt。  
5. 点击 `Queue Prompt` 生成。  
6. 输出图像保存在 `ComfyUI/output`。

---

### 🧩 模型依赖  

| 模型类别 | 文件名 |
|-----------|---------|
| Base 模型 | `sd_xl_base_1.0.safetensors` |
| Refiner 模型 | `sd_xl_refiner_1.0.safetensors` |
| IPAdapter | `ip-adapter-plus_sdxl_vit-h.safetensors` |
| DepthAnything | `depth_anything_v2_vitl_fp32.safetensors` |
| ControlNet-Canny | `controlnet-canny-sdxl-1.0.safetensors` |
| ControlNet-Depth | `controlnet-depth-sdxl-1.0.safetensors` |
| Upscaler | `4x-UltraSharp.pth` |
| LoRA | `detailed_notrigger.safetensors` |

---

### 💡 Prompt 建议  
**正向 Prompt：**  
> (masterpiece, ultra-detailed, photorealistic:1.2), professional photo, realistic texture lighting, high-tech, interior rendering  

**负向 Prompt：**  
> (low quality, blurry, anime, text watermark, lowres, shadow artifacts)

