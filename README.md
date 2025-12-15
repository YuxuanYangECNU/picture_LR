# 🎨 Color Grading Style Matcher / 调色风格匹配

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

上传你的原图和目标风格图，自动计算 Lightroom 调整参数，帮你快速复刻调色风格。

Upload your source image and target style image, automatically calculate Lightroom adjustment parameters.

## ✨ 功能特点 / Features

- 📊 **直方图分析** - RGB 和亮度直方图对比
- 🎚️ **参数计算** - 自动计算曝光、对比度、色温等 12+ 项基本参数
- 🌈 **HSL 调整** - 8 种颜色的色相/饱和度/明度独立调整建议
- 📈 **色调曲线** - 阴影/暗部/亮部/高光的曲线调整值
- 🎨 **分区色调** - 阴影和高光的色彩倾向分析
- 📱 **Web 界面** - 现代化响应式网页，支持拖拽上传

## 🖼️ 截图 / Screenshot

![Demo](demo.png)

## 🚀 快速开始 / Quick Start

### 方式一：本地命令行

```bash
# 安装依赖
pip install -r requirements.txt

# 运行分析
python analyzer.py 原图.jpg 目标图.jpg

# 指定输出路径
python analyzer.py my_photo.jpg reference.jpg result.png
```

输出示例：
```
Lightroom 调整参数:
==================================================
exposure       : +0.35
contrast       : +25
highlights     : -30
shadows        : +40
whites         : +10
blacks         : -5
temperature    : +15
tint           : -3
vibrance       : +20
saturation     : +10
clarity        : +15
dehaze         : +8
==================================================
```

### 方式二：Python 代码调用

```python
from analyzer import match_style

# 分析并获取参数
adjustments, report_bytes = match_style("my_photo.jpg", "target_style.jpg")

# 查看参数
print(adjustments.to_dict())

# 保存报告
with open("report.png", "wb") as f:
    f.write(report_bytes)
```

### 方式三：Web 网站

```bash
# 启动服务器
python app.py

# 访问 http://localhost:5000
```

## 📦 部署到服务器 / Deployment

### 使用 Gunicorn

```bash
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

### 使用 Docker

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY . .
RUN pip install -r requirements.txt

EXPOSE 5000
CMD ["gunicorn", "-w", "2", "-b", "0.0.0.0:5000", "app:app"]
```

### 部署到 Vercel / Railway / Render

本项目可以直接部署到各种 PaaS 平台。

## 📐 分析原理 / How It Works

1. **图片加载** - 支持 JPG/PNG/TIFF 等常见格式
2. **颜色空间转换** - 转换到 RGB、HSV、LAB 颜色空间
3. **特征提取**:
   - 直方图分布
   - 亮度统计（均值、标准差、百分位数）
   - HSL 各颜色区域统计
   - 分区色调（阴影/中间调/高光）
   - 清晰度和朦胧度指标
4. **差异计算** - 对比原图和目标图的特征差异
5. **参数映射** - 将差异映射到 Lightroom 参数范围

## 🎛️ 输出参数说明 / Parameters

| 参数 | 说明 | 范围 |
|------|------|------|
| Exposure | 曝光度 | -5 ~ +5 EV |
| Contrast | 对比度 | -100 ~ +100 |
| Highlights | 高光 | -100 ~ +100 |
| Shadows | 阴影 | -100 ~ +100 |
| Whites | 白色 | -100 ~ +100 |
| Blacks | 黑色 | -100 ~ +100 |
| Temperature | 色温调整方向 | 相对值 |
| Tint | 色调调整方向 | 相对值 |
| Vibrance | 自然饱和度 | -100 ~ +100 |
| Saturation | 饱和度 | -100 ~ +100 |
| Clarity | 清晰度 | -100 ~ +100 |
| Dehaze | 去朦胧 | -100 ~ +100 |

## ⚠️ 注意事项 / Notes

- 参数是基于图片特征差异的**估算值**，可能需要微调
- 建议原图和目标图的内容类型相似（如都是人像、都是风景）
- 色温参数是相对调整方向，不是绝对 K 值
- 生成的参数适用于 Lightroom Classic / Adobe Camera Raw

## 📄 License

MIT License

## 🙏 致谢 / Acknowledgments

- OpenCV
- Matplotlib
- Flask
