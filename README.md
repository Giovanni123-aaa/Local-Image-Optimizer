# Local-Image-Optimizer
Compress your AI images locally.

# ImgTool - 智能图片压缩与 GitHub 图床上传工具链

这是一个高度自动化的本地图片处理 SOP (标准作业程序) 工具套件。它可以自动优化、等比缩放、有损/无损流式压缩目标文件夹中的所有图片，并以严格的本地时间戳及随机哈希防重名机制重命名文件。同时，它支持一键将压缩后的资产分发部署到自定义的 GitHub 仓储，并自动输出汇总的 Markdown 图床账本。

## 📁 目录结构

建议将这 4 个脚本文件及配置文件放置在同一个工程目录下：
```text
├── imgTool.py                  # 总控入口脚本 (CLI 调度核心)
├── compress_images.py          # 核心图片批量压缩模块
├── upload_github_imgBed.py     # GitHub 自动化图床分发模块
└── gitConfig.ini               # GitHub 独立认证与仓储信息配置文件 (需手动创建并填写)

⚙️ 环境依赖
确保本地已安装 Python 3.6+ 以及以下第三方依赖库：
pip install Pillow requests

🛠️ 配置说明 (gitConfig.ini)
在首次执行带有上传功能的命令前，请务必在 gitConfig.ini 文件中填入你的 GitHub 配置信息，工具会自动动态解析该配置：
[github]
# 填入你的 GitHub Personal Access Token (需具备 repo 读写权限)
token = ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
# 你的 GitHub 用户名
owner = your_github_username
# 你的图床公开/私有仓库名称
repo = image-hosting-bed
# 默认分发的分支
branch = main

🚀 使用方法
入口文件为 imgTool.py，支持两种独立的工作模式：

1. 组合模式：压缩图片并同步上传至 GitHub 图床

python imgTool.py compresswithupload <需要压缩图片的文件夹> <压缩图片存放的文件夹>

执行逻辑：

🚀 扫描资产：扫描输入文件夹中的所有图片资产（支持 .jpg, .jpeg, .png, .webp, .bmp, .tiff）。

🗜️ 智能压缩：按照 YY-MM-DD-H:M:S-随机哈希 (例如: 26-06-13-14-05-30-a1b2.jpg) 进行防重名智能压缩，自动处理 PNG 量化或 WebP 转换，并输出到指定目录。

🌐 云端分发：自动读取 gitConfig.ini 配置文件，并在 GitHub 仓储中自动按月份创建归档目录（如 images/2026-06/）进行文件推送。

📝 账本输出：推送完毕后，在压缩输出目录中自动创建一个以当前时间命名的 Markdown 汇总账本 YY-MM-DD-HH-MM-SS-IMGBED.md，内含精美的 Markdown 表格、图片预览及加速后的 Raw 原始链接。

2. 纯净模式：仅在本地执行批量图片压缩
python imgTool.py compress <需要压缩图片的文件夹> <压缩图片存放的文件夹>

执行逻辑：

仅在本地执行上述的时间戳重命名与图片优化压缩，不触发任何网络请求。非常适合纯本地的素材整理。

⚠️ 参数错误提示
若输入的命令参数、子命令或文件夹路径缺失/不正确，总控系统会自动捕捉异常并打印标准的使用指南：

[错误] 参数输入不正确！请输入正确的命令与路径参数。
