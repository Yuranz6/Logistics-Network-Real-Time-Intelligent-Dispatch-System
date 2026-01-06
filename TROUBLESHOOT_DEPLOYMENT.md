# 部署故障排除指南

## 🔧 常见问题

### 问题：依赖安装卡住（"Installing build dependencies: started" 停在这里）

#### 解决方案 1：取消并重新运行
1. 进入 GitHub Actions 页面
2. 点击正在运行的 workflow
3. 点击 "Cancel workflow"
4. 等待取消完成
5. 重新推送代码或手动触发 workflow

#### 解决方案 2：检查网络问题
- GitHub Actions 有时会因为网络问题卡住
- 等待 10-15 分钟，如果仍然卡住，使用解决方案 1

#### 解决方案 3：使用手动触发
1. 进入 Actions 标签页
2. 选择 "Deploy to GitHub Pages" workflow
3. 点击 "Run workflow"
4. 选择 main 分支
5. 点击 "Run workflow" 按钮

### 问题：npm install 失败

如果看到 npm 错误，可以尝试：

1. **清除 GitHub Actions 缓存**
   - Settings → Actions → Caches
   - 删除所有缓存
   - 重新运行 workflow

2. **检查 package-lock.json**
   ```bash
   cd applications/dashboard
   rm package-lock.json
   npm install
   git add package-lock.json
   git commit -m "更新 package-lock.json"
   git push
   ```

### 问题：构建失败

1. **检查环境变量**
   - 确认 `REACT_APP_API_URL` secret 已设置
   - 或者使用默认值（代码中已设置）

2. **查看详细日志**
   - 在 GitHub Actions 中展开失败的步骤
   - 查看错误信息

### 问题：Render 部署卡住

1. **检查 Render 日志**
   - 进入 Render Dashboard
   - 查看服务日志

2. **优化构建命令**
   - 已更新 `render.yaml` 使用 `--no-cache-dir` 选项
   - 这可以避免缓存问题

3. **重新部署**
   - 在 Render Dashboard 中点击 "Manual Deploy"
   - 或推送新的代码

## 🚀 快速修复步骤

如果部署卡住，按以下步骤操作：

```bash
# 1. 取消当前的 GitHub Actions 运行（在网页上操作）

# 2. 确保所有更改已提交
git add .
git commit -m "优化部署配置"

# 3. 推送代码
git push origin main

# 4. 或者手动触发 workflow
# 在 GitHub Actions 页面点击 "Run workflow"
```

## 📊 监控部署状态

- **GitHub Actions**: https://github.com/Yuranz6/Logistics-Network-Real-Time-Intelligent-Dispatch-System/actions
- **Render Dashboard**: https://dashboard.render.com/

## ⏱️ 预期时间

- **GitHub Actions 构建**: 通常 3-5 分钟
- **依赖安装**: 通常 1-2 分钟
- **构建应用**: 通常 1-2 分钟
- **部署到 Pages**: 通常 30 秒-1 分钟

如果超过 10 分钟仍在 "Installing build dependencies"，建议取消并重新运行。

