# 狮山云瞳

> 基于 uni-app + Vue 3 的农业数据采集微信小程序，用于玉米表型采集与拷种分析。

## 项目简介

狮山云瞳是一款面向农业科研场景的微信小程序，主要服务于田间玉米种植研究中的**表型采集**与**玉米拷种**两大核心业务。通过拍照采集作物影像，结合地理位置信息和计算机视觉技术，实现对玉米植株表型指标（株高、茎粗、叶夹角、穗位高等）以及玉米果穗拷种指标（长度、籽粒数量、行数、列数、患病概率等）的数据记录与分析。

## 功能模块

| 模块 | 说明 |
|------|------|
| **用户登录** | 用户名+密码登录，JWT Token 鉴权 |
| **任务管理** | 按分类（表型采集 / 玉米拷种 / 其他）展示任务列表，支持展开/折叠 |
| **任务详情** | 查看任务信息、管理任务状态（新建→开始→结束）、影像列表展示 |
| **影像采集** | 调用设备摄像头拍照，表型采集支持双目相机左右视图预览，拷种支持九宫格辅助线 |
| **数据录入** | 录入经纬度、高程、小区ID等地理信息 |
| **结果查看** | 展示分析结果：表型指标（株高、茎粗等）和拷种指标（长度、粒数等） |
| **个人信息** | 查看账户信息、退出登录 |

## 技术栈

### 前端（小程序）

- **框架**: [uni-app](https://uniapp.dcloud.net.cn/) (Vue 3 + Composition API)
- **UI 组件库**: [uview-plus](https://uiadmin.net/uview-plus/) 3.x
- **图像处理**: OpenCV.js（灰度化等预处理）
- **样式预处理**: Sass/SCSS
- **工具库**: dayjs（日期处理）、clipboard（剪贴板）

### 桌面端辅助工具

- **Python 3** + **OpenCV**：双目 USB 摄像头图像采集脚本

```
usb_camera.py   —— 双目摄像头实时预览与拍照（左右双画面）
opencv_test.py  —— 单目摄像头基础测试
```

## 项目结构

```
狮山云瞳/
├── App.vue                    # 应用入口
├── main.js                    # Vue 初始化（注册 uview-plus）
├── pages.json                 # uni-app 页面路由配置
├── manifest.json              # 应用配置（微信小程序 appid 等）
├── pages/
│   ├── login/login.vue        # 登录页
│   ├── index/index.vue        # 首页（任务分类列表）
│   ├── message/message.vue    # 个人信息页
│   ├── photo/photo.vue        # 拍照页（OpenCV 测试）
│   ├── task-detail/           # 任务详情页
│   ├── collect/collect.vue    # 采集页（拍照+数据录入）
│   └── result/result.vue      # 结果查看页
├── components/
│   ├── request.js             # 网络请求封装
│   └── tabBar/tabBar.vue      # 自定义底部导航栏
├── static/                    # 静态资源（图标、占位图）
├── uni_modules/               # uni-app 模块（含 OpenCV SDK）
├── usb_camera.py              # 双目摄像头采集脚本
├── opencv_test.py             # 摄像头基础测试脚本
└── package.json
```

## 快速开始

### 环境要求

- [HBuilderX](https://www.dcloud.io/hbuilderx.html)（推荐）或 Node.js 16+
- 微信开发者工具
- Python 3.7+（仅桌面端摄像头脚本需要）

### 安装与运行

```bash
# 1. 安装依赖
npm install

# 2. 使用 HBuilderX 打开项目，点击「运行 → 运行到小程序模拟器 → 微信开发者工具」
```

或直接通过命令行运行：

```bash
# 运行到微信小程序
npx @dcloudio/uvm
npm run dev:mp-weixin
```

### 后端服务

本项目依赖后端 API 服务（地址配置在 `components/request.js`）：

```
https://leaf.yuntong.work:1314
```

主要接口：

| 接口 | 方法 | 说明 |
|------|------|------|
| `/user/login` | POST | 用户登录 |
| `/user/getUserInfo` | GET | 获取用户信息 |
| `/task/getTask` | GET | 获取任务列表 |

### 桌面端摄像头脚本

```bash
# 安装 Python 依赖
pip install opencv-python

# 双目摄像头实时预览+拍照
python usb_camera.py

# 单目摄像头测试
python opencv_test.py
```

## 微信小程序配置

- **AppID**: `wx9de41d7fe8498bf7`
- 已申请的权限：摄像头（`camera`）、用户位置（`userLocation`）
- 位置权限用途说明：图片地理信息标注