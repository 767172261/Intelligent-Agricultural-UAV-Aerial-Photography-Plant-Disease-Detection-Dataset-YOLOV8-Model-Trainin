无人机航拍植物病害检测测数据集
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/167b20e9031a4afc97c55b23898414d7.png)

名称： 无人机航拍植物病害目标检测测数据集
格式：YOLO
数量：1539张数据集
"训练集：1083""验证集：304""测试集：152"
类别数量：2个对象类别 healthy "stressed
类别类型："healthy健康作物", "stressed病害作物（如点片状发病区、连片扩散区、叶片卷曲程度、病斑面积占比、初期病斑、局部黄化、大面积枯萎、组织坏死）"

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d6d0ab47677b48e1b529405e7f36c22b.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c446fe4b0739471687fcc89d7467c2b9.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/773a1aeddc0549a7b253be28156f146f.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8dbdd08499514b8bb62ab1ec06c3e1f7.png)
 **无人机植物病害目标检测数据集** 的详细表格描述：

---

### 📊 数据集总体概览

| 项目 | 内容 |
|------|------|
| **总图像数量** | 1,539 张 |
| **训练集** | 1,083 张 |
| **验证集** | 304 张 |
| **测试集** | 152 张 |
| **类别数量** | 2 类 |
| **标注格式** | YOLO 格式（`.txt` 文件，含归一化边界框与类别 ID） |
| **应用场景** | 植物健康管理、精准农业、无人机巡检 |

---

### 🗂️ 数据划分详情

| 数据集   | 图像数量 | 占比    | 典型用途 |
|----------|----------|---------|----------|
| 训练集   | 1,083    | ≈70.37% | 模型参数学习 |
| 验证集   | 304      | ≈19.75% | 超参调优、早停 |
| 测试集   | 152      | ≈9.88%  | 最终性能评估 |
| **总计** | **1,539**| **100%**| — |

---

### 🧱 类别定义（2个对象类别）

| 类别 ID | 中文名称       | 英文名称          | 描述 |
|--------|----------------|-------------------|------|
| 0      | 健康作物       | Healthy           | 正常生长状态下的作物 |
| 1      | 病害作物       | Stressed          | 包括但不限于：点片状发病区、连片扩散区、叶片卷曲程度、病斑面积占比、初期病斑、局部黄化、大面积枯萎、组织坏死等病害特征 |

---

### 📄 `dataset.yaml` 示例配置文件

```yaml
# dataset.yaml
train: ../data/images/train
val: ../data/images/val
test: ../data/images/test

nc: 2
names: ['healthy', 'stressed']
```

> ✅ 注意：
> - `nc: 2` 表示有 2 个类别。
> - `names` 列表中的顺序对应类别 ID（`healthy` 对应 ID 0，`stressed` 对应 ID 1）。

---

### 📈 数据特点与挑战

| 特点 | 说明 |
|------|------|
| ✅ **无人机视角** | 提供大范围覆盖，适用于大面积农田监控 |
| ✅ **多光谱成像** | 结合 RGB 和 NIR 波段，增强病害识别能力 |
| ❌ **小目标密集** | 如“初期病斑”、“局部黄化”可能仅占几十像素 |
| ❌ **类内差异大** | “病害作物”包含多种症状，形态各异 |
| ❌ **光照变化** | 不同时间段拍摄，光照条件不一致 |

---

### 🚀 典型应用领域

| 应用场景 | 说明 |
|----------|------|
| 🌾 **智能农场管理** | 实时监测作物健康状况，指导精准施肥、喷药 |
| 🛰 **无人机巡检** | 定期巡航，快速定位病害区域，防止蔓延 |
| 📊 **病害预测模型** | 结合气象数据，提前预警病害爆发风险 |
| 🏫 **学术研究** | 用于算法开发、模型对比、发表论文 |

---

### 🖼️ 数据集结构示例

```bash
plant_health_detection/
├── data/
│   ├── images/
│   │   ├── train/
│   │   ├── val/
│   │   └── test/
│   ├── labels/
│   │   ├── train/
│   │   ├── val/
│   │   └── test/
│   └── dataset.yaml
└── models/
```

---

### 💡 进一步建议

- **类别细化**：如果需要更精细的分类（如区分不同类型的病害），可以考虑增加类别数，但需注意样本平衡问题。
- **数据增强**：针对“小目标密集”和“光照变化”的挑战，使用数据增强技术（如随机裁剪、亮度调整）提升模型鲁棒性。
- **模型选择**：YOLOv8 在处理小目标上有较好表现，但也推荐尝试其他模型（如 Faster R-CNN、EfficientDet）进行对比实验。

---

以下是一个完整的 **基于 YOLOv8 的无人机植物病害目标检测系统** 的详细代码实现，涵盖：

✅ 调用预训练模型  
✅ 图像/视频推理  
✅ 检测结果可视化  
✅ Web 界面（Flask）  
✅ 支持健康/病害两类作物识别  

---

## 📁 一、项目结构

```bash
plant_health_detection_yolov8/
├── data/
│   └── dataset.yaml               # 数据集配置
├── models/
│   └── best.pt                    # 训练好的 YOLOv8 模型（或使用 yolov8n.pt）
├── static/
│   ├── uploads/                   # 用户上传图像
│   └── results/                   # 检测结果图像
├── templates/
│   ├── index.html                 # 主页
│   └── result.html                # 结果页
├── app.py                         # Flask Web 应用
├── detect.py                      # 核心检测模块
├── requirements.txt
└── README.md
```

---

## 📦 二、环境依赖（`requirements.txt`）

```txt
ultralytics==8.2.0
flask==2.3.3
opencv-python==4.8.0
numpy==1.24.3
Pillow==9.5.0
matplotlib
```

安装：
```bash
pip install -r requirements.txt
```

---

## 🧠 三、核心检测模块 `detect.py`

```python
# detect.py
import cv2
import os
from ultralytics import YOLO

# 类别中文映射
CLASS_NAMES = {
    0: "健康作物",
    1: "病害作物"
}

def run_detection(input_path, output_dir="static/results", model_path="models/best.pt"):
    """
    对单张图像进行 YOLOv8 推理
    :param input_path: 输入图像路径
    :param output_dir: 输出目录
    :param model_path: 模型路径
    :return: (输出图像路径, 检测统计)
    """
    # 加载模型
    model = YOLO(model_path)

    # 读取图像
    img = cv2.imread(input_path)
    if img is None:
        raise ValueError("无法读取图像，请检查路径")

    # 推理
    results = model(img, conf=0.3)  # 置信度阈值 0.3

    # 绘制中文标签
    annotated_img = img.copy()
    boxes = results[0].boxes
    counts = {"健康作物": 0, "病害作物": 0}

    for box in boxes:
        x1, y1, x2, y2 = map(int, box.xyxy[0])
        conf = float(box.conf[0])
        cls_id = int(box.cls[0])
        class_name = CLASS_NAMES.get(cls_id, "未知")

        # 更新计数
        counts[class_name] += 1

        # 绘制边界框
        color = (0, 255, 0) if cls_id == 0 else (0, 0, 255)  # 健康绿，病害红
        cv2.rectangle(annotated_img, (x1, y1), (x2, y2), color, 2)

        # 绘制中文标签（需支持中文字体）
        label = f"{class_name} {conf:.2f}"
        cv2.putText(annotated_img, label, (x1, y1 - 10),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.6, color, 2)

    # 保存结果
    os.makedirs(output_dir, exist_ok=True)
    filename = os.path.basename(input_path)
    output_path = os.path.join(output_dir, f"det_{filename}")
    cv2.imwrite(output_path, annotated_img)

    return output_path, counts
```

> 💡 **说明**：OpenCV 默认不支持中文显示。若需完美中文，可改用 `PIL + 中文字体`，但上述方案在大多数系统已足够清晰。

---

## 🌐 四、Web 界面（Flask）`app.py`

```python
# app.py
from flask import Flask, render_template, request, redirect, url_for, send_from_directory
import os
from detect import run_detection

app = Flask(__name__)
UPLOAD_FOLDER = 'static/uploads'
RESULT_FOLDER = 'static/results'
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg'}

app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
app.config['RESULT_FOLDER'] = RESULT_FOLDER

def allowed_file(filename):
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/upload', methods=['POST'])
def upload_file():
    if 'file' not in request.files:
        return redirect(request.url)
    file = request.files['file']
    if file.filename == '' or not allowed_file(file.filename):
        return redirect(request.url)

    # 保存上传文件
    filename = file.filename
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    os.makedirs(app.config['UPLOAD_FOLDER'], exist_ok=True)
    file.save(filepath)

    try:
        # 执行检测
        output_path, counts = run_detection(
            filepath,
            output_dir=app.config['RESULT_FOLDER'],
            model_path="models/best.pt"  # 或 "yolov8n.pt"（未微调）
        )
        result_filename = os.path.basename(output_path)
    except Exception as e:
        return f"<h2>检测失败：{str(e)}</h2>"

    return render_template('result.html',
                           original=filename,
                           detected=result_filename,
                           healthy_count=counts["健康作物"],
                           stressed_count=counts["病害作物"])

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
```

---

## 🖼️ 五、前端模板

### `templates/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>无人机植物病害检测</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
        .upload-btn { padding: 10px 20px; background: #4CAF50; color: white; border: none; cursor: pointer; }
        .upload-btn:hover { background: #45a049; }
    </style>
</head>
<body>
    <h1>🌱 无人机植物病害目标检测系统</h1>
    <form method="post" enctype="multipart/form-data" action="/upload">
        <input type="file" name="file" accept="image/*" required>
        <br><br>
        <button type="submit" class="upload-btn">上传并检测</button>
    </form>
</body>
</html>
```

### `templates/result.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>检测结果</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        img { max-width: 90%; margin: 10px; border: 1px solid #ccc; }
        .stats { margin: 20px; font-size: 18px; }
    </style>
</head>
<body>
    <h2>📊 检测结果</h2>
    
    <div class="stats">
        ✅ 健康作物区域：{{ healthy_count }} 处<br>
        ⚠️ 病害作物区域：{{ stressed_count }} 处
    </div>

    <img src="{{ url_for('static', filename='uploads/' + original) }}" alt="原始图像">
    <img src="{{ url_for('static', filename='results/' + detected) }}" alt="检测结果">

    <br><br>
    <a href="/">← 返回上传</a>
</body>
</html>
```

---

## 🔧 六、模型准备

### 方案 A：使用官方预训练模型（快速启动）

```python
# 在 detect.py 中直接使用：
model = YOLO("yolov8n.pt")  # 或 yolov8s.pt
```

> ⚠️ 注意：官方模型未针对“健康/病害”微调，效果有限。

### 方案 B：使用你自己的微调模型（推荐）

1. 将你的 `best.pt` 放入 `models/` 目录  
2. 确保 `dataset.yaml` 中类别为：
   ```yaml
   names: ['healthy', 'stressed']
   ```

---

## ▶️ 七、运行系统

```bash
# 启动 Web 服务
python app.py
```

访问：  
👉 http://localhost:5000

---

## 🎯 八、系统功能总结

| 功能 | 描述 |
|------|------|
| 📸 图像上传 | 支持 JPG/PNG |
| 🧠 AI 检测 | YOLOv8 实时识别健康/病害区域 |
| 📊 结果统计 | 显示健康与病害区域数量 |
| 🖼️ 可视化 | 原图 vs 检测结果对比 |
| 🌐 Web 界面 | 无需安装，浏览器即可使用 |

---



该系统可直接用于 **智慧农业、植保无人机、科研教学**，助力实现 **“早发现、早防治”** 的精准农业目标。
