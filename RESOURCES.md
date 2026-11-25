# VS Code + Python 资源

## Knowledge

- [Docs: VS Code · Python tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
  官方入门路径，从装扩展到跑第一个脚本。用于：第一次配置、界面元素找不到时的对照。
- [Docs: VS Code · Python debugging](https://code.visualstudio.com/docs/python/debugging)
  断点、条件断点、`launch.json` 全字段、远程调试。用于：本课程没覆盖的调试场景。**首选主源。**
- [Docs: VS Code · Python environments](https://code.visualstudio.com/docs/python/environments)
  解释器选择、虚拟环境、`python.defaultInterpreterPath`。用于：环境类问题（import 不到、版本不对）。
- [Docs: Python · pdb — The Python Debugger](https://docs.python.org/3/library/pdb.html)
  命令行调试器官方文档，命令全集。用于：纯终端调试、服务器上没有图形界面时。
- [Repo: microsoft/debugpy](https://github.com/microsoft/debugpy)
  VS Code 调试器背后的实现，`--listen`／`--wait-for-client` 参数的权威说明。用于：attach 模式。
- [Docs: VS Code · Terminal basics](https://code.visualstudio.com/docs/terminal/basics)
  集成终端、多终端、shell 集成。用于：终端行为不符合预期时（激活环境、默认 shell）。

## Wisdom (Communities)

- [Stack Overflow · `python` + `visual-studio-code` 标签](https://stackoverflow.com/questions/tagged/visual-studio-code+python)
  提问前先搜报错原文。用 `where python` 的输出一起贴，能省掉一半来回。
- [VS Code GitHub Issues](https://github.com/microsoft/vscode-python/issues)
  扩展本身的 bug（断点不生效、解释器不刷新）在这里查，比论坛准。

## Gaps

- 缺一个中文的、按"报错原文 → 原因 → 处理"组织的故障库；本项目 `reference/vscode-python-cheatsheet.html` 第 6 节是雏形，后续遇到新坑往里加。
- 缺 `debugpy` 远程调试（WSL / 容器 / 服务器）的实操材料——等有真实需求再补。
