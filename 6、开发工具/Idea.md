## 快捷键

### 常用的快捷键：

| 快捷键组合                  | 实现效果                                            |
| --------------------------- | --------------------------------------------------- |
| psvm + Tab键 / main + Tab键 | public static void main(String[] args)              |
| sout + Tab键                | System.out.println()                                |
| Ctrl + X                    | 删除当前行                                          |
| Ctrl +D                     | 复制当前行                                          |
| Alt+Insert(或右键Generate)  | 生成代码(如get,set方法,构造函数等)                  |
| Ctrl+Alt+T                  | 生成try catch （或者 Alt+enter选择）                |
| CTRL+ALT+T                  | 把选中的代码放在 TRY{} IF{} ELSE{} 里               |
| Ctr+shift+U                 | 实现大小写之间的转化                                |
| ALT+回车                    | 导入包,自动修正                                     |
| CTRL+ALT+L                  | 格式化代码                                          |
| CTRL+ALT+I                  | 自动缩进                                            |
| CTRL+E                      | 最近更改的代码                                      |
| fori                        | 生成for (int i = 0; i < ; i++) {}                   |
| Alt + <–左右–>键            | 实现窗口左右更换（多窗口）                          |
| Ctrl + 鼠标点击             | 快速找到成员变量的出处                              |
| Shift+F6                    | 重构/重命名 (包、类、方法、变量、甚至注释等)        |
| CTRL+Q                      | 查看当前方法的声明                                  |
| Ctrl+Alt+V                  | 自动创建变量（new 对象();之后选择按快捷键）         |
| Ctrl+O                      | 重写方法                                            |
| Ctrl+I                      | 实现方法                                            |
| ALT+/                       | 代码提示                                            |
| Ctrl+Shift+R                | 在当前项目中替换指定内容                            |
| Ctrl+E                      | 最近编辑的文件列表                                  |
| Ctrl+P                      | 显示方法参数信息                                    |
| Ctrl+Shift+Insert           | 查看历史复制记录，idea可以保留历史复制的 100 条记录 |

### sout相关：

| 生成控制台的相关快捷键 | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| sout + Tab键           | 生成System.out.println();，输出到控制台语句并换行。          |
| souf + Tab键           | 生成System.out.printf("");,输出一个格式化字符串到控制台。    |
| soutm + Tab键          | 生成System.out.println("类名.方法名");，输出当前 类和方法名 到控制台。 |
| soutp + Tab键          | 生成System.out.println(所有方法参数名+值);，输出当前 方法的参数名和值 到控制台。 |

### 查找

| 快捷键                 | 介绍                         |
| ---------------------- | ---------------------------- |
| Ctrl + F               | 在当前文件进行文本查找       |
| Ctrl + R               | 在当前文件进行文本替换       |
| Shift + Ctrl + F       | 在项目进行文本查找           |
| Shift + Ctrl + R       | 在项目进行文本替换           |
| Shift + Shift          | 快速搜索                     |
| Ctrl + N               | 查找class                    |
| Ctrl + Shift + N       | 查找文件                     |
| Ctrl + Shift + Alt + N | 查找symbol（查找某个方法名） |

### 跳转切换

| 快捷键           | 介绍                  |
| ---------------- | --------------------- |
| Ctrl + E         | 最近文件              |
| Ctrl + Tab       | 切换文件              |
| Ctrl + Alt + ←/→ | 跳转历史光标所在处    |
| Alt + ←/→ 方向键 | 切换子tab             |
| Ctrl + G         | go to（跳转指定行号） |

### 编码相关

| 快捷键                      | 介绍                                                         |
| --------------------------- | ------------------------------------------------------------ |
| Ctrl + W                    | 快速选中                                                     |
| (Shift + Ctrl) + Alt + J    | 快速选中同文本                                               |
| Ctrl + C/Ctrl + X/Ctrl + D  | 快速复制或剪切                                               |
| 多行选中 Tab / Shift + Tab  | tab                                                          |
| Ctrl + Y                    | 删除整行                                                     |
| 滚轮点击变量/方法/类        | 快速进入变量/方法/类的定义处                                 |
| Shift + 点击Tab             | 快速关闭tab                                                  |
| Ctrl + Z 、Ctrl + Shift + Z | 后悔药，撤销/取消撤销                                        |
| Ctrl + Shift + enter        | 自动收尾，代码自动补全                                       |
| Alt + enter                 | IntelliJ IDEA 根据光标所在问题，提供快速修复选择，光标放在的位置不同提示的结果也不同 |
| Alt + ↑/↓                   | 方法快速跳转                                                 |
| F2                          | 跳转到下一个高亮错误 或 警告位置                             |
| Alt + Insert                | 代码自动生成，如生成对象的 set / get 方法，构造函数，toString() 等 |
| Ctrl + Shift + L            | 格式化代码                                                   |
| Shift + F6                  | 快速修改方法名、变量名、文件名、类名等                       |
| Ctrl + F6                   | 快速修改方法签名                                             |

### 代码阅读相关

| 快捷键                     | 介绍                               |
| -------------------------- | ---------------------------------- |
| Ctrl + P                   | 方法参数提示显示                   |
| Ctrl + Shift + i           | 就可以在当前类里再弹出一个窗口出来 |
| Alt + F7                   | 可以列出变量在哪些地方被使用了     |
| 光标在子类接口名，Ctrl + u | 跳到父类接口                       |
| Alt + F1 + 1， esc         |                                    |
| (Shift) + Ctrl + +/-       | 代码块折叠                         |
| Ctrl + Shift + ←/→         | 移动窗口分割线                     |
| Ctrl + (Alt) + B           | 跳转方法定义/实现                  |
| Ctrl + H                   | 类的层级关系                       |
| Ctrl + F12                 | Show Members 类成员快速显示        |

### 版本管理相关

| 快捷键       | 介绍             |
| ------------ | ---------------- |
| Ctrl + D     | Show Diff        |
| (Shift) + F7 | （上）下一处修改 |