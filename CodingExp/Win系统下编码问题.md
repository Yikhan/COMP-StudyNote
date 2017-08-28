## 如何让Win系统下编码的文件顺利在Linux下运行
之前已经遇到过几次这种问题，这次记录下来方便以后查阅
> 原因是CR/LF问题，在dos/window下按一次回车键实际上输入的是回车(CR)和换行(LF)：\r\n
而linux/unix下按一次回车键只输入换行(LF):\n，所以修改的sh文件在每行都会多了一个CR，所以linux下运行时就会报错找不到命令。

### 方法1
使用Linux里的vim编辑器，改变文件的编码方式。
用vim打开文件后，一句命令就搞定了：

`:set ff=unix 或 :set fileformat=unix`

注意vim的一些基本操作，不熟悉的话会觉得很奇怪，但熟练之后就非常快捷了。

[vim模式切换教程](https://zh.wikibooks.org/zh-hans/Vim/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%BC%8F)

### 方法2
其实Notepad++等编辑工具已经提供了解决方法了，可以在把文件传到Linux系统之前就在Win系统里转好。

`Notepad++ --> 编辑 --> 文档格式转换 --> Unix`

直接搞定。
