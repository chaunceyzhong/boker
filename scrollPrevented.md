---
title: 阻止子窗口滚动条影响父窗口滚动条
date: 2017-09-09 14:01:36
tags:
	- FrontEnd
	- scroll
---
### 起因
最近在工作上遇到一个问题，就是在子窗口上/移动滚动条至末端时候继续上/移动滚动条,结果父窗口的滚动条也上/下移动了。
### 解决方案
1. 利用浏览器滚动事件.（FF使用DOMMouseScroll，其他浏览器都是用mousewheel.）
2. 解决思路.
FF下有个特殊属性event.detail，表示滚动的值
event.detail
正数：向下滚动，负数：向上滚动
滚动一次值3，向上滚动一页值为-32768，向下滚动一页值为+32768，其他值代表滚动的行数, 方向代表了数值的正负号
受信任的事件是不会给detail赋值0
其他浏览器，通过event.wheelDelta获取滚动值
正数：向上滚动，负数：向下滚动(滚动一次值120).

> | \ 		           |      FF        |  	Others   	|
> | :---                 |     :---:      |          ---: |
> | event.detail         |        Y   	|		 N      |
> | event.wheelDelta     | 		N       | 		 Y      |

3. 实现方案.

```javascript
/*禁止触发父元素的滚动事件-只能绑定当前对象 chauncey*/
    $.fn.scrollUnique = function () {
        return $(this).each(function () {
            $(this).on("mousewheel DOMMouseScroll", function (e) {
                var e0 = e.originalEvent;
                var delta = e0.wheelDelta || -e0.detail;
                this.scrollTop += ( delta < 0 ? 1 : -1 ) * 30;
                e0.preventDefault();
            });
        });
    }

//调用 sonObject.scrollUnique();其中 object 为子窗口对象.

```
4. 参考资料
https://developer.mozilla.org/en-US/docs/Web/Events/DOMMouseScroll
https://developer.mozilla.org/en-US/docs/Web/Events/mousewheel