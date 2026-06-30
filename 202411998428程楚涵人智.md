# PyTorch3D 可微3D模型变形实验
基于PyTorch3D实现的可微渲染3D模型变形任务，将基础球体网格通过梯度下降优化，变形为目标奶牛3D模型。

---

## 📋 实验简介
本实验通过可微剪影渲染技术，以`cow.obj`为目标模型，将初始球体网格迭代优化变形为奶牛模型，核心技术基于PyTorch3D的软剪影光栅化与梯度反向传播，实现无监督3D形状重建。

---

## 🛠️ 环境配置
### 依赖安装
```bash
# 升级基础工具
pip install --upgrade pip setuptools wheel -i https://pypi.tuna.tsinghua.edu.cn/simple

# 安装前置依赖
pip install fvcore iopath matplotlib ninja -i https://pypi.tuna.tsinghua.edu.cn/simple

# 源码安装PyTorch3D（适配云平台）
pip install "git+https://gitee.com/hongwenzhang/pytorch3d.git" --no-build-isolation

```
## 项目文件结构
pytorch3d-deformation/
├── test_pytorch3d.ipynb   # 完整实验代码
├── cow.obj                # 目标奶牛模型文件
├── output_dir/            # 优化过程生成的中间模型文件
│   ├── step_000.obj
│   ├── step_500.obj
│   └── final_cow.obj
└── README.md              # 项目说明文档

## 快速运行
准备文件：将cow.obj与test_pytorch3d.ipynb放在同一目录
启动环境：在 ModelScope / 本地 JupyterLab 中打开test_pytorch3d.ipynb
执行代码：按顺序运行所有单元格，等待优化完成（约 5-10 分钟）
查看结果：优化后的模型文件将保存在output_dir目录中，可使用 MeshLab 打开查看

## 实验核心流程
目标模型加载：加载cow.obj并通过多视角渲染生成目标剪影图
初始模型初始化：创建高细分球体网格作为变形起点
可微渲染管线构建：使用SoftSilhouetteShader构建可微剪影渲染器
梯度优化循环：
以顶点偏移量为可微参数，通过 Adam 优化器更新
损失函数：剪影 MSE 损失 + 拉普拉斯正则化 + 边长正则化
结果输出：保存优化过程中的中间模型与最终模型
## 结果展示
优化过程：球体网格逐步变形为奶牛模型，剪影与目标剪影的误差持续降低
输出文件：output_dir中保存了每轮迭代的中间.obj模型文件，可下载至本地用 MeshLab 查看 3D 效果


