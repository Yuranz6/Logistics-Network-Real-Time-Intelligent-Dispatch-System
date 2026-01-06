# 部署指南 - GitHub Pages (前端) + Render (后端)

本指南将帮助您将前端部署到 GitHub Pages，后端部署到 Render。

## 📋 部署架构

- **前端**: GitHub Pages - `https://yuranz6.github.io/Logistics-Network-Real-Time-Intelligent-Dispatch-System/`
- **后端**: Render - `https://logistics-dispatch-center.onrender.com`

## 🚀 部署步骤

### 第一部分：部署后端到 Render

1. **登录 Render**
   - 访问 [Render Dashboard](https://dashboard.render.com/)
   - 使用 `yuranzhang6@gmail.com` 登录

2. **创建新 Web Service**
   - 点击 "New +" → "Web Service"
   - 连接您的 GitHub 仓库：`Yuranz6/Logistics-Network-Real-Time-Intelligent-Dispatch-System`
   - 或者使用 "Public Git repository" 并输入仓库 URL

3. **配置服务**
   - **Name**: `logistics-dispatch-center`
   - **Region**: `Oregon (US West)`
   - **Branch**: `main`
   - **Root Directory**: `applications/scheduler`
   - **Environment**: `Python 3`
   - **Build Command**: `pip install --upgrade pip && pip install -r requirements.txt`
   - **Start Command**: `python dispatch_center.py`
   - **Plan**: `Free`

4. **设置环境变量**
   在 Render Dashboard 的 Environment 部分添加：
   - `PYTHON_VERSION` = `3.11.0`
   - `CONFLUENT_BOOTSTRAP_SERVERS` = (您的 Kafka 服务器地址，可选)
   - `CONFLUENT_API_KEY` = (如果使用 Confluent Cloud，可选)
   - `CONFLUENT_API_SECRET` = (如果使用 Confluent Cloud，可选)

5. **部署**
   - 点击 "Create Web Service"
   - Render 会自动开始构建和部署
   - 等待构建完成，记下您的服务 URL（例如：`https://logistics-dispatch-center.onrender.com`）

### 第二部分：部署前端到 GitHub Pages

1. **配置 GitHub Pages**
   - 进入 GitHub 仓库：`https://github.com/Yuranz6/Logistics-Network-Real-Time-Intelligent-Dispatch-System`
   - 点击 **Settings** → **Pages**
   - 在 **Source** 下选择 **GitHub Actions**

2. **设置 GitHub Secrets**
   - 进入 **Settings** → **Secrets and variables** → **Actions**
   - 点击 **New repository secret**
   - 添加以下 secret：
     - **Name**: `REACT_APP_API_URL`
     - **Value**: `https://logistics-dispatch-center.onrender.com`（使用您的 Render 服务 URL）

3. **推送代码触发部署**
   ```bash
   git add .
   git commit -m "配置 GitHub Pages 和 Render 部署"
   git push origin main
   ```

4. **等待部署完成**
   - GitHub Actions 会自动构建并部署前端
   - 查看 Actions 标签页确认部署状态
   - 部署完成后，访问：`https://yuranz6.github.io/Logistics-Network-Real-Time-Intelligent-Dispatch-System/`

## 🔧 配置说明

### 前端配置

前端使用环境变量 `REACT_APP_API_URL` 来连接后端。在构建时，GitHub Actions 会使用您在 Secrets 中设置的值。

如果需要在本地开发时使用不同的后端 URL，可以在 `applications/dashboard` 目录下创建 `.env` 文件：

```bash
cd applications/dashboard
echo "REACT_APP_API_URL=http://localhost:8001" > .env
```

### 后端配置

后端已经配置了 CORS，允许所有来源的请求（`allow_origins=["*"]`），所以可以正常接收来自 GitHub Pages 的请求。

后端使用 `PORT` 环境变量（Render 会自动设置），如果没有设置则默认使用 8001。

## 🔍 验证部署

### 检查后端

1. 访问您的 Render 服务 URL
2. 访问 `/docs` 端点查看 API 文档（例如：`https://logistics-dispatch-center.onrender.com/docs`）
3. 测试 API 端点：
   - `GET /api/v1/statistics`
   - `GET /api/v1/orders`
   - `GET /api/v1/vehicles`

### 检查前端

1. 访问 GitHub Pages URL
2. 打开浏览器开发者工具（F12）
3. 检查 Console 是否有错误
4. 检查 Network 标签，确认 API 请求是否成功

## 🐛 故障排除

### 前端问题

**问题：页面显示空白**
- 检查浏览器控制台是否有错误
- 确认 `package.json` 中的 `homepage` 字段正确
- 检查 GitHub Actions 构建是否成功

**问题：API 连接失败**
- 确认 Render 后端服务正在运行
- 检查 `REACT_APP_API_URL` secret 是否正确设置
- 确认后端 URL 可以访问（在浏览器中打开）

**问题：WebSocket 连接失败**
- 确认 Render 后端支持 WebSocket
- 检查 WebSocket URL 是否正确（代码会自动转换 HTTP/HTTPS 到 WS/WSS）

### 后端问题

**问题：服务无法启动**
- 检查 Render 日志
- 确认所有依赖都已安装
- 检查环境变量是否正确设置

**问题：CORS 错误**
- 后端已配置允许所有来源，如果仍有问题，检查 Render 日志

## 📝 注意事项

1. **GitHub Pages 限制**
   - 只支持静态网站
   - 所有后端逻辑必须在 Render 上运行
   - 如果仓库是私有的，需要 GitHub Pro/Team 账户

2. **Render 免费计划限制**
   - 服务在 15 分钟无活动后会休眠
   - 首次请求可能需要几秒钟来唤醒服务
   - 每月有使用时间限制

3. **环境变量**
   - 敏感信息（如 API keys）应该存储在 GitHub Secrets 和 Render Environment Variables 中
   - 不要将敏感信息提交到代码仓库

## 🔗 相关链接

- [GitHub Pages 文档](https://docs.github.com/en/pages)
- [Render 文档](https://render.com/docs)
- [React 环境变量](https://create-react-app.dev/docs/adding-custom-environment-variables/)
- [FastAPI CORS](https://fastapi.tiangolo.com/tutorial/cors/)

## 📞 支持

如果遇到问题，请检查：
1. GitHub Actions 日志
2. Render 服务日志
3. 浏览器控制台错误
4. 网络请求状态

