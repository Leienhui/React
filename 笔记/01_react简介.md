#  react简介

* 是什么？

  * 用于构建用户**界面**的JavaScript库

  * 是一个将数据渲染为html视图的开源JavaScript库

* 谁开发的？

  * Facebook

* 为什么要学？

  * 原生JavaScript操作dom频繁、效率低
  * 原生JavaScript直接操作dom，浏览器会进行大量的重绘重排
  * 原生JavaScript没有组件化编码方案，代码复用率低

##  react特点

*  采用**组件化模式、声明式编码、提高开发效率以及组件的复用率**
* 再react Native中可以使用React语法进行**移动端开发**
* 使用**虚拟dom+优秀的diff算法，尽量检查与真实dom的交互**

## 相关js库

* react.js：React核心库。
* react-dom.js：提供操作DOM的react扩展库。
* babel.min.js：解析JSX语法代码转为JS代码的库。

##  创建虚拟DOM的两种方式

* 纯JS方式(一般不用)

  ```javascript
      <div id="test"></div>
      <!-- 最先引入react核心库 -->
     <script type="text/javascript" src="./js/react.development.js"></script>
     <script type="text/javascript" src="./js/react-dom.development.js"></script>
      <script>
      // 创建虚拟dom,createElement(标签名，标签属性，标签内容)
      const vDom=React.createElement('h1',{id:'title'},'hello world')
      // 渲染虚拟dom到页面
      // ReactDOM.render(虚拟dom,渲染到的容器)
      ReactDOM.render(vDom,document.getElementById('test'))
      </script>
  ```

  

* JSX方式

  ```javascript
      <div id="test"></div>
      <!-- 最先引入react核心库 -->
     <script type="text/javascript" src="./js/react.development.js"></script>
     <script type="text/javascript" src="./js/react-dom.development.js"></script>
     <script type="text/javascript" src="./js/babel.min.js"></script>
      <!-- type必须写 -->
      <script type="text/babel">
      // 创建虚拟dom
      const vDom=<h1>hello world</h1> /*这里不要写引号，因为不是字符串，时虚拟dom*/
      // 渲染虚拟dom到页面
      // ReactDOM.render(虚拟dom,渲染到的容器)
      ReactDOM.render(vDom,document.getElementById('test'))
      </script>
  ```