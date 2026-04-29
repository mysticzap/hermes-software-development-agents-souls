# Ubuntu 系统下 HermesAgent 安装指南

## 一、系统准备与依赖安装

### 1. 更新系统包
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. 安装必要系统依赖
```bash
sudo apt install -y curl wget git unzip build-essential ffmpeg
```

## 二、Python 环境配置（使用 conda）

### 1. 安装 Miniconda
```bash
# 下载最新版 Miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# 添加执行权限
chmod +x Miniconda3-latest-Linux-x86_64.sh

# 运行安装脚本
./Miniconda3-latest-Linux-x86_64.sh
```

安装过程中注意：
- 按 Enter 阅读许可协议
- 输入 `yes` 接受协议
- 使用默认安装路径（`/home/用户名/miniconda3`）
- 输入 `yes` 初始化 conda

### 2. 激活 conda 环境
```bash
source ~/.bashrc
# 或重新打开终端
```

### 3. 创建 Python 3.11 环境
```bash
# 创建名为 hermes 的 Python 3.11 环境
conda create -n hermes python=3.11 -y

# 激活环境
conda activate hermes
```

### 4. 验证 Python 版本
```bash
python --version
# 应显示 Python 3.11.x
```

## 三、Node.js 环境配置（使用 n）

### 1. 安装 n（Node.js 版本管理器）
```bash
# 安装 n
curl -L https://raw.githubusercontent.com/tj/n/master/bin/n | bash

# 或使用 npm 安装（如果已安装 npm）
# npm install -g n
```

### 2. 安装 Node.js 22
```bash
# 安装 Node.js 22 LTS 版本
n 22

# 或安装最新稳定版
# n latest
```

### 3. 验证 Node.js 安装
```bash
node --version
# 应显示 v22.x.x
npm --version
# 应显示对应的 npm 版本
```

## 四、安装 uv（Python 包管理器）

### 1. 安装 uv
```bash
# 使用官方脚本安装
curl -LsSf https://astral.sh/uv/install.sh | sh

# 添加 uv 到 PATH
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 2. 验证 uv 安装
```bash
uv --version
# 应显示 uv 版本信息
```

## 五、安装 HermesAgent

### 1. 克隆 HermesAgent 仓库
```bash
# 创建目录并克隆
mkdir -p ~/.hermes && cd ~/.hermes
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
```

### 2. 使用 uv 创建虚拟环境并安装依赖
```bash
# 创建虚拟环境（使用 conda 环境中的 Python）
uv venv --python $(which python)

# 激活虚拟环境
source .venv/bin/activate

# 安装 HermesAgent 依赖
uv pip install -e .
```

### 3. 安装前端依赖（Node.js 部分）
```bash
# 进入前端目录
cd hermes_agent/frontend

# 安装 Node.js 依赖
npm install

# 返回项目根目录
cd ../..
```

## 六、配置与验证

### 1. 添加环境变量
```bash
# 将 hermes 命令添加到 PATH
echo 'export PATH="$HOME/.hermes/hermes-agent/.venv/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 2. 验证安装
```bash
# 检查 hermes 命令是否可用
hermes --version

# 运行健康检查
hermes doctor
```

预期输出应显示：
- ✅ Python 3.11.x
- ✅ Hermes CLI installed
- ✅ Core dependencies OK

### 3. 初始化配置
```bash
# 运行配置向导
hermes config init

# 或手动创建配置文件
cp config.example.yaml config.yaml
# 编辑 config.yaml 配置模型 API 密钥等
```

## 七、国内用户加速方案

### 1. conda 镜像源配置
```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

### 2. npm 镜像源配置
```bash
npm config set registry https://registry.npmmirror.com
```

### 3. uv 镜像源配置
```bash
# 临时使用
export UV_INDEX_URL="https://pypi.tuna.tsinghua.edu.cn/simple"

# 永久配置
echo 'export UV_INDEX_URL="https://pypi.tuna.tsinghua.edu.cn/simple"' >> ~/.bashrc
```

### 4. Git 克隆加速
```bash
# 使用 GitHub 镜像
git clone --recurse-submodules https://ghproxy.com/https://github.com/NousResearch/hermes-agent.git
```

## 八、常见问题解决

### 1. conda 命令找不到
```bash
# 重新初始化 conda
source ~/miniconda3/etc/profile.d/conda.sh
conda init
```

### 2. n 命令找不到
```bash
# 重新加载 bashrc
source ~/.bashrc

# 或手动添加 n 到 PATH
export N_PREFIX="$HOME/.n"
export PATH="$N_PREFIX/bin:$PATH"
```

### 3. uv 安装依赖失败
```bash
# 清理缓存重试
uv cache clean
UV_INDEX_URL="https://pypi.tuna.tsinghua.edu.cn/simple" uv pip install -e .
```

### 4. Node.js 版本不匹配
```bash
# 切换到正确的 Node.js 版本
n 22
node --version  # 确认版本
```

## 九、启动 HermesAgent

### 1. 命令行模式
```bash
# 确保在 hermes-agent 目录下
cd ~/.hermes/hermes-agent

# 激活虚拟环境
source .venv/bin/activate

# 启动 hermes
hermes chat
```

### 2. 后台运行
```bash
# 使用 nohup 后台运行
nohup hermes chat > hermes.log 2>&1 &
```

## 十、环境管理命令速查

### conda 常用命令
```bash
# 列出所有环境
conda env list

# 激活环境
conda activate hermes

# 退出环境
conda deactivate

# 更新 conda
conda update conda
```

### n 常用命令
```bash
# 列出已安装版本
n ls

# 切换版本
n 22

# 设置默认版本
n alias default 22

# 删除版本
n rm 18
```

### uv 常用命令
```bash
# 同步依赖
uv sync

# 添加包
uv add package_name

# 移除包
uv remove package_name

# 更新所有包
uv update
```

## 注意事项

1. **环境隔离**：conda 环境与 uv 虚拟环境是独立的，确保在正确的环境中操作
2. **版本兼容**：HermesAgent 要求 Python >3.11 和 Node.js >22，请务必满足
3. **网络问题**：国内用户建议配置所有镜像源以加速下载
4. **权限问题**：避免使用 sudo 安装 Python/Node.js 包，以免权限混乱
5. **存储空间**：安装过程需要约 2-3GB 磁盘空间

按照以上步骤操作，你将在 Ubuntu 系统上成功安装 HermesAgent，并使用 conda、n 和 uv 分别管理 Python 版本、Node.js 版本和 Python 包。
