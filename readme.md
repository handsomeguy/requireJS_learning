### require JS 源码解析

- 1、调用require方法，先进行参数的判断。之后调用context下的require方法 
- 2、context的require方法来源于makeRequire里封装的localRequire方法，里面进行了deps参数的判断，如果是正常的依赖加载，通过调用nextTick，异步操作，获取模块的map，之后封装成module对象，（module对象有事件对象、依赖项、依赖计数还有原型链上封装的一些方法），调用module的init操作初始化模块。 
- 3、init方法做了错误监听，还有数据初始化，之后调用模块的enable方法。 
- 4、enable方法中缓存模块对象进enabledRegistry 中，之后遍历依赖项，遍历过程中，依赖计数depCount++，把依赖项封装成对应的模块的map，之后对子模块设定define事件监听，回调函数是调用父模块的check方法，检查是否可执行。方法最后执行了一次check方法。 
- 5、如果一个模块进入check方法调用，如果已被定义且依赖计数小于1，即加载完依赖的情况下，执行该模块的回调函数，同时清除对应id的registry，触发模块上的define事件。（即变相通知了依赖本模块的父模块）


### 调试和查看方式： 
new Error 查看函数调用栈 
断点调试 
consolelog