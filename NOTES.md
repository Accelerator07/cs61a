# 工作笔记

## 环境事实（2026-11-25 核对）

| 项 | 值 |
| --- | --- |
| VS Code | 1.147.0，x64，装在 `%LOCALAPPDATA%\Programs\Microsoft VS Code`，`code` 已在 PATH |
| 界面语言 | 中文（ms-ceintl.vscode-language-pack-zh-hans）——写步骤时给中英双写 |
| 已有扩展 | C/C++ 全家桶、CMake Tools、Code Runner、壁纸插件 |
| 本次补装 | `ms-python.python` 2026.10.0（连带 debugpy 2026.6.0、Pylance 2026.4.1、python-envs 1.44.0） |
| Python | 3.14.6，`%LOCALAPPDATA%\Programs\Python\Python314`，pip 26.3 |
| 终端 | PowerShell 5.1（`$PSVersionTable` 报 5.1），`ComSpec` 是 cmd |
| 执行策略 | CurrentUser = RemoteSigned（venv 的 `Activate.ps1` 可以跑） |
| 虚拟环境 | `python-lab/.venv`，含 pip 26.3 + debugpy 1.8.22 |

## 坑（已踩过，别再踩）

- **沙箱里 `python -m venv` 会失败**：ensurepip 复制文件时 `shutil.rmtree` 对临时目录 chmod 被拒（WinError 5）。解决办法是把 `TMP`/`TEMP` 指到工作区内，并且需要放开文件权限。用户自己手动跑不受影响。
- `code --install-extension` 要写 `%APPDATA%\Code` 和 `~/.vscode/extensions`，沙箱默认模式下会被 EPERM 拦掉。
- `main.py` 的输出在工具管道里显示为乱码（cp936/utf-8 错配），但用户在 VS Code 终端里看是正常的——不要因为这个误判脚本有问题。
- **用户敲了 `python3 main.py`**，跳出去 Microsoft Store 的提示。`where python3` 只找到 `%LOCALAPPDATA%\Microsoft\WindowsApps\python3.exe`（0 字节，应用执行别名）。真正的 Python 只提供 `python.exe` 和 `py.exe`。→ 已写进 README 与速查表第 6 节。用户是 Linux 习惯，以后示例命令一律用 `python`。

## 用户偏好

- 用中文，术语给中英对照（因为界面是中文的）。
- 要能直接复制粘贴的命令，不要"你去某菜单里点一下"这种含糊描述。
- 讲原理要短，重点在"点哪里、按什么、看到什么算对"。
