# jsx

* 全称: JavaScript XML
* react定义的一种类似于XML的JS扩展语法: JS + XML本质是`React.createElement(component, props, ...children)`方法的语法糖
* 标签名任意: HTML标签或其它标签
* 标签属性任意: HTML标签属性或其它

##  jsx作用

* 用来简化创建虚拟DOM
  * 写法：`var ele =<h1>Hello JSX!</h1>`
  *  注意1：它不是字符串, 也不是HTML/XML标签
  * 注意2：它最终产生的就是一个JS对象

##  jsx基本语法规则

* 遇到 <开头的代码, 以标签的语法解析: html同名标签转换为html同名元素, 其它标签需要特别解析

*  遇到以 { 开头的代码，以JS语法解析: 标签中的js表达式必须用{ }包含

* 定义虚拟dom不要引号

* 样式的类名要用className,不是用class

* 行内样式不能这样写`style="color:red"`,必须写成对象的形式`style={{color:'red',fontSize:'29px'}}`

  * 注意，是在{}中写对象

* 根标签只有一个

* 标签必须闭合

* 他的标签名字不要说时html标签，他是jsx标签

* 标签首字母

  * 若小写字母开头，则将该标签转为html中同名元素，若html中无同名，则报错
  * 如果以大写字母开头，react就去渲染对应的组件，若组件没有，就报错未定义

* react能遍历数组，但是不能遍历对象

* `{}`里面可以写表达式(有返回值)，不能写js代码（语句）

  * 表达式：变量名(a)、a+b、函数调用（fun()）,arr.map(),方法、定义一个函数`function test(){}`
  * 语句：if、for、switch

* ```javascript
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
      <style>
          .h3{
              background-color: aqua;
          }
      </style>
  </head>
  
  <body>
      <div id="test"></div>
      <!-- 最先引入react核心库 -->
      <script type="text/javascript" src="./js/react.development.js"></script>
      <script type="text/javascript" src="./js/react-dom.development.js"></script>
      <script type="text/javascript" src="./js/babel.min.js"></script>
      <!-- type必须写 -->
      <script type="text/babel">
          const h1Id='h1'
      const arr = ["angular", "vue", "react"];
      // 创建虚拟dom
      const vDom=(
          <div id={h1Id}>
              {arr.map((e)=> <h3 className="h3" key={e}>{e}</h3>)}
          </div>
          
      )
      // 渲染虚拟dom到页面
      // ReactDOM.render(虚拟dom,渲染到的容器)
      ReactDOM.render(vDom,document.getElementById('test'))
      </script>
  </body>
  
  </html>
  ```

##  模块与组件

* 模块：

  * 向外提供特定功能的js程序, 一般就是一个js文件

  * 为什么要拆成模块？
    * 随着业务逻辑增加，代码越来越多且复杂
  * 作用：
    * 复用js, 简化js的编写, 提高js运行效率

* 组件：

  * 用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)
  * 为什么要用组件？
    *  一个界面的功能更复杂
  * 作用：
    * 复用编码, 简化项目编码, 提高运行效率

## 模块化与组件化

* 模块化：
  * 当应用的js都以模块来编写的, 这个应用就是一个模块化的应用
* 组件化：
  * 当应用是以多组件的方式实现, 这个应用就是一个组件化的应用





































