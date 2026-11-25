# Mission: 在 VS Code 里独立地跑和调 Python

## Why

【待确认，先按已知的写】刚装好 VS Code，目标是在这台机器上把 Python 脚本跑起来，并且能在终端里定位问题——不是"学 Python 语法"，而是**让自己写的脚本能跑、跑错了能查**。

旁证：这个桌面文件夹里已经有一批「递归调用次数分析」的材料（docx/pdf），所以更具体的猜想是——要把自己写的算法/分析脚本跑通并验证结论，而不是从零学语言。

## Success looks like

- 新建一个 `.py`，选对解释器，终端里 `python xxx.py` 一次跑通
- 遇到报错时，能下断点、单步、在变量面板里读出那一刻的真实值，而不是靠 print 猜
- 能用 `python -m pdb` 或 `debugpy --wait-for-client` + attach 在终端里调试
- 遇到「装了包却 import 不到」这类问题，能自己判断是解释器/环境问题而不是代码问题

## Constraints

- Windows 11，PowerShell 5.1，Python 3.14.6，VS Code 1.147（中文界面）
- 已装 C/C++ 扩展与 Code Runner；Python 扩展由本次会话补装
- 中文界面 + 中文文档，术语对照（Select Interpreter ↔ 选择解释器）要写出来

## Out of scope

- 打包发布（PyInstaller）、Web 框架、单元测试框架——等"跑通 + 能调"稳定了再谈
- 虚拟环境之外的依赖管理工具（poetry / uv / conda）——现在只用 `venv` + `pip`
