# Render 部署修复指南 - metadata-generation-failed 错误

## 🔧 问题描述

在 Render 部署时遇到 `metadata-generation-failed` 错误，通常是因为：
1. 某些 Python 包需要编译，但构建环境缺少工具
2. 包版本不兼容
3. pip/setuptools 版本过旧

## ✅ 已应用的修复

### 1. 优化构建命令
- 升级 pip、setuptools 和 wheel
- 分步安装依赖，便于定位问题
- 为 pydantic 添加备选安装方案

### 2. 系统依赖
确保 `apt.txt` 包含必要的编译工具：
```
librdkafka-dev
gcc
g++
python3-dev
```

## 🚀 部署步骤

### 方法 1：使用优化后的配置（推荐）

1. **在 Render Dashboard 中更新服务**
   - 进入您的服务设置
   - 确保使用最新的 `render.yaml` 配置
   - 或者手动设置构建命令

2. **手动设置构建命令**（如果 render.yaml 不工作）
   在 Render Dashboard 的 Build Command 中粘贴：
   ```bash
   python -m pip install --upgrade pip setuptools wheel && pip install --no-cache-dir python-dotenv==1.0.0 && pip install --no-cache-dir websockets==12.0 && pip install --no-cache-dir 'pydantic==1.10.13' && pip install --no-cache-dir 'fastapi==0.104.1' && pip install --no-cache-dir 'uvicorn[standard]==0.24.0' && pip install --no-cache-dir 'kafka-python-ng==2.2.3'
   ```

### 方法 2：使用简化版 requirements

如果仍然失败，可以尝试使用 `requirements-render.txt`：

1. 在 Render Dashboard 中修改构建命令：
   ```bash
   pip install --upgrade pip setuptools wheel && pip install --no-cache-dir -r requirements-render.txt
   ```

### 方法 3：使用更兼容的包版本

如果 pydantic 1.10.13 仍然失败，可以尝试：

1. 修改 `requirements.txt`，使用更新的 pydantic：
   ```
   pydantic>=1.10.0,<2.0
   ```

2. 或者使用 pydantic 2.x（需要更新代码）：
   ```
   pydantic>=2.0.0
   ```

## 🔍 调试步骤

1. **查看 Render 日志**
   - 进入 Render Dashboard
   - 点击服务 → Logs
   - 查看详细的错误信息

2. **检查 Python 版本**
   - 确保使用 Python 3.11.0（在 render.yaml 中已设置）
   - 如果问题持续，可以尝试 Python 3.10

3. **测试本地安装**
   ```bash
   cd applications/scheduler
   python -m pip install --upgrade pip setuptools wheel
   pip install --no-cache-dir -r requirements.txt
   ```

## 📝 常见错误和解决方案

### 错误：pydantic metadata-generation-failed

**解决方案**：
```bash
# 在构建命令中先单独安装 pydantic
pip install --no-cache-dir 'pydantic>=1.10.0,<2.0'
```

### 错误：kafka-python-ng 安装失败

**解决方案**：
- 确保 `apt.txt` 包含 `gcc` 和 `g++`
- 或者使用纯 Python 的 kafka-python（性能较差）

### 错误：uvicorn[standard] 安装失败

**解决方案**：
```bash
# 使用基础版本
pip install --no-cache-dir uvicorn==0.24.0
```

## 🎯 快速修复命令

如果所有方法都失败，使用这个最简化的构建命令：

```bash
pip install --upgrade pip && pip install fastapi uvicorn python-dotenv websockets kafka-python-ng pydantic
```

这会安装最新兼容版本，虽然不是精确版本，但通常可以工作。

## 📞 需要帮助？

如果问题持续存在：
1. 检查 Render 日志中的完整错误信息
2. 尝试在本地 Docker 容器中重现问题
3. 考虑使用 Render 的 Python 3.10 而不是 3.11

