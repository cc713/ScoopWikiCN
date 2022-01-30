关于这两个项目最详细的描述是 Mike Zick 在 [这个线程](http://sourceforge.net/mailarchive/forum.php?thread_name=200506130821.11185.mszick%40morethan.org&forum_name=mingw-msys) 的回答.

> Cygwin is an operating system wrapper/Cygwin 是一个操作系统包装器<br>
> The goal of Cygwin is to provide a Linux Programming API./Cygwin 的目标是提供 Linux 编程 API.
>
>
> Msys is a command shell substitute/Msys 是一个命令壳层替换<br>
> The goal of Msys is to provide a POSIX scripting environment./Msys 的目标是提供 POSIX/Portable Operating System Interface(可移植操作系统接口)脚本环境

它可能不是一个完全准确或全面的描述, 但它很容易掌握.

类似的, 对于 Scoop:

**Scoop is an installer/Scoop 是一个安装程序**<br>
**The goal of Scoop is to let you use Unix-y programs in a normal Windows environment/Scoop 的目标是让你在通常 Windows 环境中使用 Unix 样式程序**

使用 Scoop 可以让你实现与 Cygwin 和 MSYS 类似的事情, 但无需了解和使用单独的环境. 你可以继续做已经在做的事情, 但可以轻松访问需要的跨平台工具.

碰巧的是, Scoop 安装的许多程序要么直接来自 MinGW/MSYS 项目, 要么是使用他们的工具构建的. 正因为 15 年来在 MinGW/MSYS 上的惊人工作, Scoop 才有望实现其目标, 而它本身又是基于 Cygwin 的.