---
title: 打造性感好用的VS Code编辑器
date: 2018-10-28 17:16:49
tags: [Tools, IDE]
categories: ToolsDev
---

![](https://ws2.sinaimg.cn/large/006tNbRwly1fwo22d17edj30m809n0uf.jpg)

官网： https://code.visualstudio.com/

<!-- more -->

## 主命令框

`F1 `或` Ctrl+Shift+P`: 打开命令面板。在打开的输入框内，可以输入任何命令，例如：

- 按一下 `Backspace`会进入到 `Ctrl+P` 模式
- 在`Ctrl+P`下输入`>`可以进入`Ctrl+Shift+P`模式

在 `Ctrl+P` 窗口下还可以:

- 直接输入文件名，跳转到文件
- `?`列出当前可执行的动作
- `!`显示 Errors或 Warnings，也可以`Ctrl+Shift+M`
- `:`跳转到行数，也可以 Ctrl+G 直接进入
- `@`跳转到`symbol`（搜索变量或者函数），也可以 `Ctrl+Shift+O` 直接进入
- `@`根据分类跳转`symbol`，查找属性或函数，也可以 `Ctrl+Shift+O` 后输入:进入
- `#`根据名字查找 `symbol`，也可以 `Ctrl+T`

## 常用快捷键

### 编辑器与窗口管理

- 打开一个新窗口： `Ctrl+Shift+N`
- 关闭窗口： `Ctrl+Shift+W`
- 同时打开多个编辑器（查看多个文件）
- 切出一个新的编辑器（最多 3 个） `Ctrl+\`，也可以按住 `Ctrl` 鼠标点击 `Explorer` 里的文件名
- 左中右 3 个编辑器的快捷键 `Ctrl+1` `Ctrl+2` `Ctrl+3`
- 编辑器换位置， Ctrl+k然后按 Left或 Right

## 代码编辑

### 格式调整

> 对python文件进行代码格式化操作时，会提示安装`autopep8`
- **代码格式化**： `Shift+Alt+F`，或 `Ctrl+Shift+P` 后输入 `format code`
- **代码行缩进**: `Ctrl+[` 、 `Ctrl+]`

---

- 在当前行下边插入一行 `Ctrl+Enter`
- 在当前行上方插入一行 `Ctrl+Shift+Enter`

---

- 上下移动一行： `Alt+Up` 或 `Alt+Down`
- 向上向下复制一行： `Shift+Alt+Up` 或 `Shift+Alt+Down`


### 光标相关

- 移动到定**义处**： `F12`
- **定义**处缩略图：只看一眼而不跳转过去:`Alt+F12`

---

- 移动到文件结尾： `Ctrl+End`
- 移动到文件开头： `Ctrl+Home`

> 下面两个功能和`alt+↑/↓`配合，很方便的移动代码块：
- 选择从光标到行尾：` Shift+End`
- 选择从行首到光标处： `Shift+Home`

---
> 这两个功能很爽，可以同时编辑一些变量名：
- 多行编辑(列编辑)：`Ctrl+Alt+Down/Up`或者`Alt+Shift+鼠标左键`，
- 同时选中所有匹配： `Ctrl+Shift+L`
- Ctrl+D 下一个匹配的也被选中 (在 sublime 中是删除当前行，后面自定义快键键中，设置与 Ctrl+Shift+K 互换了)

---

- 回退上一个光标操作： Ctrl+U
- 删除光标右侧的所有字： Ctrl+Delete
- 扩展/缩小选取范围： Shift+Alt+Left 和 Shift+Alt+Right

- 移动到后半个括号： Ctrl+Shift+]

### 重构代码
> 查看函数引用和批量休修改函数名，好用：
- 重命名：比如要修改一个方法名，可以选中后按 F2，输入新的名字，回车，会发现所有的文件都修改了
- 找到所有的引用： `Shift+F12`
- 同时修改本文件中所有匹配的： `Ctrl+F12`
- 跳转到下一个 Error 或 Warning：当有多个错误时可以按 `F8 `逐个跳转
- 查看 diff： 在 explorer 里选择文件右键 Set file to compare，然后需要对比的文件上右键选择 Compare with file_name_you_chose

### 查找替换

- 查找:`Ctrl+F`
- 查找替换:`Ctrl+H`
- 整个文件夹中查找:`Ctrl+Shift+F`

## 显示相关

- 全屏：`F11`
- zoomIn/zoomOut：`Ctrl +/-`
- 侧边栏显/隐：`Ctrl+B`
- 显示资源管理器:`Ctrl+Shift+E`
- 显示搜索:`Ctrl+Shift+F`
- 显示 Git:`Ctrl+Shift+G`
- 显示 Debug:`Ctrl+Shift+D`
- 显示 Output:`Ctrl+Shift+U`

## 设置

### 修改 VSCode 默认 terminal/终端

在 vscode 设置中，搜索 `terminal.integrated.shell.windows`，点击在 `settings.json` 中编辑，将 git bash 终端路径设置进去即可

```
D:\\Program Files\\Git\\bin\\bash.exe
```

## 其他

自动保存：File -> AutoSave ，或者 Ctrl+Shift+P，输入 auto

### VS Code 中文注释显示乱码怎么办？

https://www.zhihu.com/question/34415763/answer/60444047
将设置中的"files.autoGuessEncoding"项的值改为true即可。

## 我的配置

```
{
    "workbench.iconTheme": "vscode-icons",
    "files.autoSave": "onWindowChange",
    // 设置保存时，自动将Python代码的一些空格给trim掉；
    "files.trimTrailingWhitespace": true,
    // 将一些编译后而不想在编辑器里看到的文件隐藏；
    "files.exclude": {
        ".vs*": true,
        "*.*~": true,
        "*.pyc": true,
        "*/*.pyc": true
    },
    "terminal.integrated.rendererType": "dom",
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
    "python.formatting.autopep8Args": [
        "--max-line-length=100"
    ],
    "editor.formatOnSave": true,
    "python.linting.pylintEnabled": true,
    "python.linting.pylintArgs": [
        "--include-naming-hint=n",
        "--disable=W0311",
        "--disable=C0103",
        "--disable=E1101"
    ],
    "files.eol": "\n",
    "editor.wordWrap": "on"
}
```

## 常用扩展

VS Code扩展商店：https://marketplace.visualstudio.com/vscode
![](https://ws4.sinaimg.cn/large/006tNbRwly1fwo21q6y14j30m808fjs8.jpg)

### 主题

可以来[主题商城](https://marketplace.visualstudio.com/search?target=VSCode&category=Themes&sortBy=Downloads)选择：

![](https://ws3.sinaimg.cn/large/006tNbRwly1fwo2220cgtj30m80anta6.jpg)

> 切换主题的快捷键：`ctrl+k`,`ctrl+t`

- One Dark Pro
- Atom One Dark
- FlatUI
- Material Icon Theme: 图标主题
- vscode-icons
- VSCode Great Icons
- Dracula Official：主题，推荐
- [bookmarks](https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks)：`ctrl+alt+l`快速跳转，[图标设置](http://www.iconfont.cn/search/index?searchType=icon&q=bookmark)

### 开发扩展

- AutoFileNmae:auto complete file name，自动补全文件的名字
- [code runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner):`ctrl+alt+n`
- Remote - SSH：远端连接 VM 编辑的利器！
- Guides:显示代码对其辅助线
- gitlens:显示代码每一行的最新修改人
- Beautify:显示js/json/css美化，按F1
- Rainbow Brackets：有颜色的显示括号匹配
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- Path Intellisense: 路径匹配
- Auto Rename Tag：自动修改标签
- [leetcode](https://github.com/jdneo/vscode-leetcode/blob/master/docs/README_zh-CN.md) 在 VS Code 中刷算法的插键，强烈安利，为此还录制了一期 [B 站视频](https://www.bilibili.com/video/av66300578/)
- Color Highlight:写csss时，颜色值会增加对应的颜色背景显示
- Vetur：VUE扩展
- open in browser:alt+b选择浏览器预览文件
- Settings Sync: 同步配置

> 下面介绍一下怎么设置同步：
`ctrl+shift+p`，输入`sync`，会看到相关选项，选择`update/upload settings`，然后会输入https://github.com/settings/tokens中设置的key。具体的用法，Settings Sync的扩展主页介绍的很详细了：https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync

## FAQ

### 如何设置中文界面

- [设置中文界面](https://code.visualstudio.com/docs/getstarted/locales)
- [Microsoft/vscode-tips-and-tricks](https://github.com/Microsoft/vscode-tips-and-tricks)
- [language-pack](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)

## 参考

- [Visual Studio Code 简明使用教程-w3cschool](https://www.w3cschool.cn/visualstudiocode/visualstudiocode-iy3422zb.html)
- [Microsoft Visual Studio Code 中文手册](https://legacy.gitbook.com/book/jeasonstudio/vscode-cn-doc/details)
- [VS Code 配置](https://segmentfault.com/a/1190000005986197)
- [VSCode配置备忘](http://www.yueye.org/2016/my-vscode-user-settings.html)
- [Getting Started with Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial)
- [Visual Studio Code初探](http://www.cnblogs.com/greyzeng/p/4669625.html)
- [使用Python virtualenv时如何设置VS code](http://www.cashqian.net/blog/00148782264567542a619e58913497eb7f15af7426517ba000)
- [学会用好 Visual Studio Code](http://nshen.net/article/2015-11-20/vscode/)