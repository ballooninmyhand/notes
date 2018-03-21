### bootstrap 模态框找不到焦点问题

**解决方法：**

在 js 文件开头加入下面代码，重写 bootstrap 的 focus 方法

> $.fn.modal.Constructor.prototype.enforceFocus = function() {}

