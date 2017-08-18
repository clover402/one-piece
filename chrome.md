## 调试js代码
F12打开开发者工具，选择Sources面板，会发现有一排按钮【暂停/继续】、【单步执行】、【单步跳入】、【单步跳出】、【启用/禁用所有断点】、【异常时断点】
### 设置断点
在左边内容区域鼠标点击行号就可以设置和删除断点。添加的每个断点都会出现在右侧调试区的Breakpoints列表中，点击列表中断点就会定位过去，
每个断点都有两种状态：激活和禁用。
### 条件断点
断点处点击右键选择编辑断点，可以写一个表达式，表达式为true时才触发断点
### 调用栈
在断点停下来时，右侧的Call Stack区会显示当前的方法调用栈
### 查看变量
查看右侧Scope Variables列表，或者用鼠标选择想要查看的变量或表达式，然后右键菜单执行“Evaluate in Console”
### 临时修改js代码
在chrome中直接修改Ctr+S保存，结合console就可以立即重新调试
### ![](http://img0.ph.126.net/Lc7mnvcr4FwE0HeC4laSbA==/6597601930285221144.png)异常时断点
3种状态：
* 异常不中断（默认）
* 遇到所以异常都会中断，包括已捕获的情况
* 仅在遇到未捕获的异常时才中断
### 在DOM元素上设置断点
进入Element Panel中在某元素上点击右键，可以设置3中不同情况的断点
* 子节点修改
* 自身属性修改
* 自身节点被删除
选中之后Sources Panel中的DOM Breakpoints列表中就会出现该DOM断点。
### XHR断点
右侧调试区有一个 XHR Breakpoints，点击+ 并输入 URL 包含的字符串即可监听该 URL 的 Ajax 请求，输入内容就相当于 URL 的过滤器。如果什么都不填，那么就监听所有 XHR 请求。一旦 XHR 调用触发时就会在 request.send() 的地方中断。
### 按事件类型触发断点
右侧调试区的 Event Listener 列表，这里列出了各种可能的事件类型。勾选对应的事件类型，当触发了该类型的事件的 JavaScript 代码时就会自动中断。
### //@ sourceURL 给 eval 出来的代码命名
有时候一些非常动态的代码是以字符串的形式通过 eval() 函数在当前 Javascript context 中创建出来，而不是作为一个独立的 js 文件加载的。这样你在左边的内容区就找不到这个文件，因此很难调试。其实我们只要在 eval 创建的代码末尾添加一行 “//@ sourceURL=name“ 就可以给这段代码命名（浏览器会特殊对待这种特殊形式的注释），这样它就会出现在左侧的内容区了，就好像你加载了一个指定名字的 js 文件一样，可以设置断点和调试了。下图中，我们通过 eval 执行了一段代码，并利用 sourceURL 将它命名为 dynamicScript.js ，执行后左侧内容区就出现了这个“文件”，而它的内容就是 eval 的中的内容。
![](http://img0.ph.126.net/LMfR9cctSwFeT-LB8jKzWg==/6597570044448017583.png)
### 格式化代码（Pretty Print）
Sources Panel选中js文件左下方会有{}按钮，它用于把杂乱的代码重新格式化为漂亮的代码，比如一些已被压缩的 js 文件基本没法看、更没法调试。点一下格式化。

参考资料：
* [chrome浏览器调试JS代码](http://blog.csdn.net/luckyyulin/article/details/20477695)
* [Chrome Developer Tools官方文档](https://developers.google.com/chrome-developer-tools/docs/scripts)
