# Render Python 版本修复指南

## 🔴 问题

Render 正在使用 Python 3.13，但 pandas 等包还不支持 Python 3.13，导致编译错误。

## ✅ 解决方案

### 方法 1：在 Render Dashboard 中手动设置 Python 版本（推荐）

1. **进入 Render Dashboard**
   - 访问 https://dashboard.render.com/
   - 登录您的账户

2. **进入服务设置**
   - 找到 `logistics-dispatch-center` 服务
   - 点击进入服务详情页

3. **设置 Python 版本**
   - 在 "Environment" 部分
   - 找到 "Python Version" 或 "Runtime Version"
   - **手动设置为 `3.11.0`** 或 `python-3.11.0`
   - 保存更改

4. **重新部署**
   - 点击 "Manual Deploy" → "Deploy latest commit"
   - 或等待自动部署触发

### 方法 2：确保 runtime.txt 文件正确

`applications/scheduler/runtime.txt` 文件应该包含：
```
python-3.11.0
```

### 方法 3：在 render.yaml 中明确指定

`render.yaml` 中已经设置了：
```yaml
envVars:
  - key: PYTHON_VERSION
    value: 3.11.0
  - key: RUNTIME_VERSION
    value: python-3.11.0
```

## 🔍 验证 Python 版本

构建命令中已经添加了版本检查：
```bash
python --version
if ! python --version | grep -q "3.11"; then
  echo "错误: 需要使用 Python 3.11，但检测到其他版本"
  exit 1
fi
```

如果版本不正确，构建会失败并显示错误信息。

## 📝 注意事项

1. **Render Dashboard 设置优先**
   - 即使在 `runtime.txt` 和 `render.yaml` 中设置了版本
   - Render Dashboard 中的手动设置可能会覆盖它们
   - **建议在 Dashboard 中明确设置 Python 3.11.0**

2. **清除构建缓存**
   - 如果问题持续，尝试在 Render Dashboard 中清除构建缓存
   - 或创建一个新的服务实例

3. **检查构建日志**
   - 查看构建日志中的 `python --version` 输出
   - 确认使用的是 Python 3.11 而不是 3.13

## 🚀 快速修复步骤

1. 进入 Render Dashboard
2. 找到 `logistics-dispatch-center` 服务
3. 进入 Settings → Environment
4. 设置 Python Version 为 `3.11.0`
5. 保存并重新部署

## ✅ 预期结果

部署成功后：
- Python 版本应该是 3.11.0
- 不会尝试编译 pandas
- 所有依赖安装成功
- 服务正常启动

