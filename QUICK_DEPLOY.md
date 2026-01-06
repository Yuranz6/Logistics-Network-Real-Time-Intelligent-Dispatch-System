# 快速部署指南

## 🎯 部署目标

- **前端**: GitHub Pages → `https://yuranz6.github.io/Logistics-Network-Real-Time-Intelligent-Dispatch-System/`
- **后端**: Render → `https://logistics-dispatch-center.onrender.com`

## ⚡ 快速步骤

### 1. 部署后端到 Render（5分钟）

1. 访问 https://dashboard.render.com/ 并使用 `yuranzhang6@gmail.com` 登录
2. 点击 "New +" → "Web Service"
3. 连接仓库：`Yuranz6/Logistics-Network-Real-Time-Intelligent-Dispatch-System`
4. 配置：
   - Name: `logistics-dispatch-center`
   - Root Directory: `applications/scheduler`
   - Build Command: `pip install --upgrade pip && pip install -r requirements.txt`
   - Start Command: `python dispatch_center.py`
5. 点击 "Create Web Service"
6. 等待部署完成，记下服务 URL

### 2. 配置 GitHub Pages（3分钟）

1. 在 GitHub 仓库设置 GitHub Secrets：
   - Settings → Secrets and variables → Actions
   - New repository secret
   - Name: `REACT_APP_API_URL`
   - Value: `https://logistics-dispatch-center.onrender.com`（使用您的 Render URL）

2. 启用 GitHub Pages：
   - Settings → Pages
   - Source: 选择 "GitHub Actions"

### 3. 推送代码（1分钟）

```bash
git add .
git commit -m "配置 GitHub Pages 和 Render 部署"
git push origin main
```

### 4. 等待部署

- GitHub Actions 会自动构建和部署前端（约 2-3 分钟）
- 访问：`https://yuranz6.github.io/Logistics-Network-Real-Time-Intelligent-Dispatch-System/`

## ✅ 验证

- 后端：访问 `https://logistics-dispatch-center.onrender.com/docs`
- 前端：访问 GitHub Pages URL，检查是否能正常加载数据

## 📚 详细文档

查看 `DEPLOYMENT_GUIDE.md` 获取完整部署说明和故障排除指南。

