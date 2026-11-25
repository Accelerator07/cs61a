# python-lab — VS Code 跑 Python + 终端调试

这个文件夹已经配好了，打开就能跑、能调。里面只有一件东西要做：**把解释器选对**。

```
python-lab/
├── main.py                    # 示例脚本（递归调用次数），断点都标在注释里
├── hello.py                   # 你自己的三行测试代码（导入已修正）
├── .venv/                     # 虚拟环境，已建好，debugpy 已装
├── .vscode/
│   ├── launch.json            # F5 用哪套配置
│   └── settings.json          # 固定用 .venv，新终端自动激活
└── README.md                  # 就是本文件
```

---

## 0. 已经替你做好的部分

| 事项 | 状态 |
| --- | --- |
| 装 Python 扩展（ms-python.python + debugpy + Pylance） | 已装 |
| 建虚拟环境 `.venv` | 已建 |
| 写 `launch.json` / `settings.json` | 已写 |
| 验证 `main.py` 能跑通 | 已跑，输出 `fib(5) = 5`、调用 15 次、自检通过 |

唯一没替你点的是 VS Code 界面里的那一下选择（第 1 步），因为那是你以后换项目也要自己会做的动作。

---

## 1. 选解释器（每个新项目都要做一次）

打开这个文件夹（`文件 → 打开文件夹`，选 `python-lab`），然后二选一：

- **点右下角状态栏**上的 Python 版本号 → 选带 `.venv` 的那一项；
- 或按 `Ctrl+Shift+P`，输入 `Python: Select Interpreter`（中文界面：`Python: 选择解释器`）→ 选 `.venv`。

选对之后，状态栏显示类似 `Python 3.14.6 ('.venv': venv)`。**选错的表现**：终端里 `pip install` 装完，代码里却 `ModuleNotFoundError`。

---

## 2. 跑起来

| 方式 | 操作 | 说明 |
| --- | --- | --- |
| 集成终端 | `Ctrl+`` 打开终端，输入 `python main.py` | 最接近真实使用，报错原样显示 |
| 不调试直接跑 | `Ctrl+F5` | 走 launch.json，输出在终端 |
| 右键运行 | 编辑器右上角 ▷ 按钮 | 同样走 launch.json |

终端里第一行应该是 `(.venv) PS C:\...\python-lab>`。没有 `(.venv)` 前缀，就手动激活一次：

```powershell
.\.venv\Scripts\Activate.ps1
```

---

## 3. 调试（图形界面那套）

1. 打开 `main.py`，在行号左边点一下，出现**红点**（`F9` 也可以切换断点）；
2. 在 `CALLS += 1`（第 16 行）和 `result = fib(...)`（第 24 行）这两行各下一个断点；
3. 按 `F5`，顶部下拉框选 `Python: 当前文件（终端）`；
4. 程序停在红点那一行（还没执行）。此时看左侧四个面板：

| 面板 | 看什么 |
| --- | --- |
| 变量 VARIABLES | `n`、`depth`、`CALLS`、`result` 的当前值 |
| 监视 WATCH | 手动加表达式，比如 `CALLS` 或 `n * 2` |
| 调用堆栈 CALL STACK | 已经压了几层 `fib` —— 递归深度一眼可见 |
| 断点 BREAKPOINTS | 所有断点，可以临时取消勾选 |

5. 单步：

| 按键 | 动作 | 什么时候用 |
| --- | --- | --- |
| `F10` | 单步跳过（不进函数） | 只想往下走一行 |
| `F11` | 单步进入（进函数） | 想看 `fib` 里面发生了什么 |
| `Shift+F11` | 单步跳出 | 这一层看够了，回上一层 |
| `F5` | 继续到下一个断点 | 跳过中间一堆循环 |
| `Shift+F5` | 停止调试 | |

6. 在**调试控制台**（`Ctrl+Shift+Y`）里可以现场敲表达式，比如 `CALLS`、`fib(3)`，不影响程序继续跑。

---

## 4. 调试（终端那套）

### 4.1 纯命令行：pdb

不用图形界面，直接在终端里下断点：

```powershell
python -m pdb main.py
```

进去之后：

| 命令 | 作用 |
| --- | --- |
| `l` | 列出当前位置前后代码 |
| `n` | 下一行（不进函数） |
| `s` | 进入函数 |
| `b main.py:16` | 在第 16 行下断点（`CALLS += 1` 那行） |
| `c` | 继续运行到下一个断点 |
| `p CALLS` | 打印变量 |
| `w` | 看调用栈（递归时特别好用） |
| `q` | 退出 |

### 4.2 终端启动 + VS Code 界面调试（推荐）

这是"脚本在终端跑，但断点、变量面板照用"的姿势。两个终端窗口，或一个终端 + 一个 F5：

```powershell
# 终端 A：先让程序挂起，等你连上来
python -m debugpy --listen 5678 --wait-for-client main.py
```

然后回到 VS Code，按 `F5`，选 **`Python: 附加到 5678 端口`**。程序立刻从第一行开始跑，断点照常命中，变量面板照常显示。

好处：可以调试**已经在跑的东西**（比如被别的程序调用、带一堆命令行参数、或者你只想在它崩掉的那一刻接上去）。

---

## 5. 报错对照表

| 现象 | 原因 | 处理 |
| --- | --- | --- |
| `python3` 提示「Python was not found; run without arguments to install from the Microsoft Store」 | **Windows 上没有真正的 `python3.exe`**，那个文件是微软商店的占位符（0 字节） | 改用 `python main.py` 或 `py main.py`。`python3` 是 Linux/macOS 的写法，别在 Windows 上用 |
| 终端里 `python` 不是 3.14，或提示找不到 | 解释器没选对 / 终端没激活 venv | 回到第 1 步；或 `.\.venv\Scripts\Activate.ps1`（cmd 里是 `.venv\Scripts\activate.bat`） |
| `无法加载文件 Activate.ps1，因为在此系统上禁止运行脚本` | PowerShell 执行策略 | `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` |
| `ModuleNotFoundError: No module named 'xxx'` | 包装到别的解释器里去了 | 先 `where python` 看是不是 `.venv` 那个 |
| `pip` 不是内部或外部命令 | venv 里没 pip | `python -m ensurepip --upgrade`，再 `python -m pip install -U pip` |
| 按 F5 没反应 / 弹出"选择调试配置" | 没打开 .py 文件，或 launch.json 不在 `.vscode/` 下 | 确认文件在 `python-lab/.vscode/launch.json` |
| 断点是灰色空心圈 | 解释器路径不对，或文件被改过 | 重新选解释器，`Ctrl+Shift+P` → `Developer: Reload Window` |
| 中文输出乱码 | 终端代码页 | 终端里先跑 `chcp 65001`，或设环境变量 `PYTHONUTF8=1` |

---

## 6. 练习（做完就算真的会了）

1. 把 `main.py` 里 `n = 5` 改成 `8`，先在终端跑一遍看输出长度，再用断点单步，体会"指数爆炸"；
2. 在 `fib` 里下一个**条件断点**（右键红点 → `编辑断点` → 表达式填 `n == 2`），让程序只在 `n` 等于 2 时停下；
3. 用 `python -m pdb main.py`，只用 `b`、`c`、`w`、`p CALLS` 四个命令，把 `CALLS` 停在 10；
4. 用 4.2 的方式，在 `--wait-for-client` 挂起期间把 `main.py` 里某行改成会报错的代码，看断点是否仍然先命中。
