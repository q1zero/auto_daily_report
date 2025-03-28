# 自动日报生成器

一个自动获取、处理并发送每日学习报告的Python工具。

## 功能特点

- 从GitHub获取每日学习内容
- 使用Gemini AI处理和格式化内容
- 自动发送格式化的HTML邮件
- 支持多个收件人
- 详细的日志记录
- 工作日自动执行（周一至周五）
- 使用北京时间

## 安装步骤

1. 克隆仓库：
```bash
git clone https://github.com/zhaojinyang117/auto_daily_report.git
cd auto_daily_report
```

2. 安装依赖：
```bash
# 安装Python依赖
pip install -r requirements.txt

# 安装系统依赖（如果需要）
sudo apt-get update
sudo apt-get install -y at python3-dotenv
```

3. 配置环境变量：
手动创建`.env`文件并填写以下配置：
```env
# Gemini API配置
GEMINI_API_KEY=your_api_key

# 个人信息配置
USER_NAME=你的姓名

# 邮件配置
EMAIL_FROM=your_email@example.com
EMAIL_PASSWORD=your_email_password
EMAIL_TO=recipient1@example.com,recipient2@example.com

# SMTP配置
SMTP_SERVER=your_smtp_server
SMTP_PORT=465

# 日志配置
LOG_LEVEL=INFO
```

## 使用方法

1. fork仓库,在GitHub上更新`study_today.txt`文件内容
(修改`scraper.py`中的`url`变量，指向新的`study_today.txt`文件)

2. 运行程序：
```bash
python main.py
```

3. 设置定时执行：

   本地执行：

   使用传统方式（crontab）：
   ```bash
   # 给脚本添加执行权限
   chmod +x schedule_tasks.sh

   # 运行定时任务设置脚本
   ./schedule_tasks.sh
   ```

   使用uv环境设置定时任务：
   
   Linux/macOS:
   ```bash
   # 给脚本添加执行权限
   chmod +x schedule_tasks_uv.sh

   # 运行定时任务设置脚本（使用uv虚拟环境）
   ./schedule_tasks_uv.sh
   ```
   
   Windows:
   ```bash
   # 运行Windows下的定时任务设置脚本
   schedule_tasks_uv.bat
   ```

   GitHub Actions执行（推荐）：

   1. 在GitHub仓库设置中添加以下Secrets:
      - `GEMINI_API_KEY`: 你的Gemini API密钥
      - `USER_NAME`: 你的姓名
      - `EMAIL_FROM`: 发件人邮箱
      - `EMAIL_PASSWORD`: 邮箱密码或应用专用密码
      - `EMAIL_TO`: 收件人邮箱（多个邮箱用逗号分隔）
      - `SMTP_SERVER`: SMTP服务器地址
      - `SMTP_PORT`: SMTP服务器端口

   2. GitHub Actions会自动按计划运行（工作日每晚8点），或者你可以手动触发工作流程

脚本会：
- 自动安装at命令（如果需要）
- 检查已有任务，避免重复设置
- 设置工作日（周一至周五）的执行计划
- 每天北京时间20:00自动运行程序
- 显示所有已设置的任务

## 文件结构

- `main.py`: 主程序入口
- `scraper.py`: 内容获取模块
- `gemini_processor.py`: AI内容处理模块
- `email_generator.py`: 邮件内容生成器
- `email_sender.py`: 邮件发送模块
- `logger.py`: 日志记录模块
- `config.py`: 配置加载模块
- `schedule_tasks.sh`: 定时任务设置脚本（传统方式）
- `setup_uv.sh`: Linux/macOS下使用uv的安装脚本
- `setup_uv.bat`: Windows下使用uv的安装脚本
- `schedule_tasks_uv.sh`: Linux/macOS下基于uv环境的定时任务脚本
- `schedule_tasks_uv.bat`: Windows下基于uv环境的定时任务脚本
- `github_action_runner.py`: GitHub Actions运行脚本
- `.github/workflows/`: GitHub Actions工作流配置文件

## 注意事项

- 确保已正确配置所有环境变量
- 检查SMTP服务器设置是否正确
- 确保有足够的权限访问日志文件
- 所有定时任务使用北京时间
- 只在工作日（周一至周五）执行
- 需要安装python-dotenv和google-generativeai包

