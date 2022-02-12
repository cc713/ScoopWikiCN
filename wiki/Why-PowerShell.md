无论是从 PowerShell 还是 cmd.exe 运行, Scoop 都会尝试 "正常工作", 但 **我建议改用 PowerShell**. 以下是原因.

### 是的, Powershell 有问题

* `Verb-Noun`(`动词-名词`) 结构冗长, 似乎不是为打字而设计的命令
* ISE—命令行界面的 GUI. 我知道这些命令很难输入 — 但是单击是解决方案吗?
* 名字 PowerShell, 非官方缩写 PoSH(时髦的). 奉承.
* "一切都是对象!" 最终感觉笨拙. 有时文本更容易使用. 对 primitives/原语, arrays/数组 和 hashes/散列 的支持就足够了.
* Modules/模块. 谁知道它们怎么工作?
* 看起来不像 Windows 中的一流 shell
* 内置参数解析不好
* 沉重的 'sysadmin' 感觉让开发人员/devops(操作员) 感到难过

### 尽管如此..

你仍应使用 PowerShell. 为什么? 因为你可以忽略这些问题中的大部分, 而仍可以使用一种出色, 灵活, 动态, 功能强大的脚本语言.

你不用写 `Verb-Noun` 'cmdlets', 只需写脚本. 如果你愿意, 可以从 PowerShell 脚本中返回文本 —— 因为文本是通用接口. 解析你自己的参数(或 dot-source getopt). 如果有人提到 PoSH, 请公开嘲笑他们(开玩笑).

所以一旦你忽略了缺点, 还剩下什么?

* **一个非常强大的编程环境**, 远胜于 cmd.exe.
* **快速 REPL/Read–eval–print loop/读取-计算-打印 循环** (像 ScriptCS, 但更简单, 更动态)
* **安装在 Windows 上你可依赖的唯一脚本语言**
* **对原始类型, 数组和散列的强大语言支持**
* **使用不起眼的 shell 带来的自豪感**. Zsh? Fish? Pfft(语气词, 表示中止). 几乎 *没人* 使用 PowerShell(不确定实际数字).

是的, 你一直在使用古老且看似已被遗忘的 Windows 控制台, 但你可以通过 [一点自定义](https://github.com/ScoopInstaller/scoop/wiki/Theming-Powershell) 很好地使用它.
