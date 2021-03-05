# React面向组件编程

## 基本理解和使用

* ### 使用React开发者工具调试

  * ![](../images\调试工具.png)

##  组件

* 组件名必须首字母大写
* 虚拟DOM元素只能有一个根元素
* 虚拟DOM元素必须有结束标签

* # 函数式组件：

  * 用函数定义的组件(简单组件)

  * ```javascript
        // 创建函数式组件
        function FunComponent(){
            //babel编译后开启了严格模式，所以这里的this式undefined
            return <h2>我是函数定义的组件</h2>
        }
        // 渲染组件到页面
       ReactDOM.render(<MyComponent/>,document.getElementById('test'))  
    ```

  * 定义的函数时react内部自己调用

  * ## 执行了 ` ReactDOM.render(<MyComponent/>,document.getElementById('test'))  `后发生了什么？

    * react解析组件标签，找到组件（FunComponent）

    * 发现组件式函数定义的，随后调用该函数，将返回的虚拟dom转为真实dom，最后呈现再页面中

* ##  类的基本知识

  * ```javascript
            // 定义类
            class Person {
                //构造器（里面的属性都在Person这个类上）
                constructor(name,age){
                    // 构造器中的this指向类的实例对象
                    this.name=name
                    this.age=age
                }
                // 一般方法(这个方法不在Person这个类上，但是它再Person这个类的原型对象上；是给实例用的)
                say(){
                    //this指向调用他的实例
                    console.log(`我叫${this.name},今年${this.age}岁了`)
                }
            }
            //创建实例对象
            const p1 =new Person('tom',18)
            const p2 =new Person('honghong',18)
            // 调用一般方法
            p1.say()
            p2.say()
    ```

* ## 类的继承

  ```javascript
          // 定义类
          class Person {
              //构造器（里面的属性都在Person这个类上）
              constructor(name,age){
                  // 构造器中的this指向类的实例对象
                  this.name=name
                  this.age=age
              }
              // 一般方法(这个方法不在Person这个类上，但是它再Person这个类的原型对象上；是给实例用的)
              say(){
                  //this指向调用他的实例
                  console.log(`我叫${this.name},今年${this.age}岁了`)
              }
          }
          //创建一个Student类继承Person
          class Student extends Person(){
              //它继承所有Person的属性方法
              constructor(name,age,grade){
                  // 如果有constructor，继承者必须写super给父级构造器递过去操作
                  // super必须写在最顶部
                  super(name,age)
                  this.grade=grade
              } 
  
          }
          const s1 = new Student('huahua',12,'六年级')
          //学生先查原型自己有没有这个方法，没有再去父类原型查（子>父）
          s1.say()
  ```

  * 类中的**构造器不是必须要写的**，要对实例进行一些初始化操作，如添加指定属性时才写
  * 如果**A类继承了B类，且A类中写了构造器**，那么A类构造器中的**super**是必须要调用的，且写在构造器内的**最顶部**
  * 类中定义的方法都是定义在类的原型对象上

* ##  类的直接赋值

  ```javascript
  class Weather {
      // 类中可以直接写赋值语句
      // 往类的实例上添加一个a属性，值为白天
      a="白天"
  }
  const aa = new Weather()
  console.log(aa)//Weather {a: "白天"}
  ```

  

* # 类式组件：

  * 用类定义的组件(复杂组件)

    * 复杂组件局势有状态的那种

    ```javascript
       // React.Component是react中内置的类
        // 创建类式组件
        class MyComponent extends React.Component {
            // render函数是放在MyComponent这个类的原型对象上，供实例使用
            render(){ 
                //this指向MyComponent这个类的实例对象
                return <h2>我是类定义的组件</h2>
            }
    
        }
        // 渲染组件到页面
      ReactDOM.render(<MyComponent/>,document.getElementById('test'))  
    ```

  * ## 执行了 ` ReactDOM.render(<MyComponent/>,document.getElementById('test'))  `后发生了什么？

    * react解析组件标签，找到组件（MyComponent）

    * 发现组件是类定义的，随后new出该类（MyComponent）的实例，并通过该实例调用原型上的render()方法
    * 将返回的虚拟dom转为真实dom，最后呈现再页面中

##    渲染类组件标签的基本流程

* React内部会创建组件实例对象
* 调用render()得到虚拟DOM, 并解析为真实DOM
* 插入到指定的页面元素内部

##   组件实例的三大核心属性1: state

* state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)
* 组件被称为"状态机", 通过更新组件的state来更新对应的页面显示(重新渲染组件)

* ### 注意

  *  组件中render方法中的**this为组件实例对象**

  * 组件自定义的方法中this为undefined，如何解决？

    ```javascript
        <script type="text/babel">
        // React.Component是react中内置的类
        // 创建类式组件
        class Weather extends React.Component {
            // 初始化就要constructor
            constructor(props){
                super(props)
                this.state={
                    isHot:true
                }
    
            }
            // render函数是放在MyComponent这个类的原型对象上，供实例使用
            // 实例是什么呢？
            render(){
                // 读取状态  this.state.isHot
                const {isHot} =this.state
                // 原生是onclick,react是onClick 
                // demo不能用demo()，否则在返回的时候就直接调用（还没触发事件就调用了）
                return <h2 onClick={demo}>今天天气很{isHot?'炎热':'寒冷'}</h2>
            }
    
        }
        // 渲染组件到页面
      ReactDOM.render(<Weather/>,document.getElementById('test'))  
      function demo(){
        //这里的this指向undefined
          console.log(this)//undefined
          console.log('标题被点击了')
      }
        </script>  
    ```

    ```javascript
        <script type="text/babel">
            // React.Component是react中内置的类
        // 创建类式组件
        class Weather extends React.Component {
            // 初始化就要constructor
            constructor(props){
                super(props)
                this.state={
                    isHot:true
                }
    
            }
            // render函数是放在MyComponent这个类的原型对象上，供实例使用
            // 实例是什么呢？
            render(){
                // 读取状态  this.state.isHot
                const {isHot} =this.state
                return <h2 onClick={this.demo}>今天天气很{isHot?'炎热':'寒冷'}</h2>
            }
            demo(){
                //注意：类中定义的方法默认会开启严格模式，所以类中自定义的方法的this一般是undefined
                 //由于demo是作为onClick的回调，所以不是通过实例调用的，是直接调用
                console.log(this) 
                this.state.isHot=!this.state.isHot//报错
            }
    
        }
        // 渲染组件到页面
      	ReactDOM.render(<Weather/>,document.getElementById('test'))  
        </script>
    ```

    

    * 强制绑定this: 通过函数对象的bind()

      * ```javascript
            <script>
                function demo(){
                    console.log(this)
                }
                demo() //window
                demo.bind({a:1,b:2})//不输出，因为bind返回一个函数，这个函数还没调用
               const a = demo.bind({a:1,b:2})
               a()//Object  {a:1,b:2}
            </script>
        ```

      * ```javascript
        <script type="text/babel">
        // React.Component是react中内置的类
        // 创建类式组件
        class Weather extends React.Component {
            // 初始化状态就要constructor
            constructor(props){
                // 构造器只调用1次
                console.log(constructor)
                super(props)
                this.state={
                    isHot:true
                }
                this.Weather=this.demo.bind(this)
        
            }
            // render函数是放在MyComponent这个类的原型对象上，供实例使用
            //   render调用1+n次，1是初始化那一次，n是状态更新的次数 
            render(){
                // 读取状态  this.state.isHot
                const {isHot} = this.state
                return <h2 onClick={this.Weather}>今天天气很{isHot?'炎热':'寒冷'}</h2>
            }
            //demo调用几次，事件触发几次，就调用几次
            demo(){
            // 上面已经用bind指定this是实例对象
            /* 状态的数据不可以直接更改，以下就是直接更改
            this.state.isHot=!this.state.isHot
            */
           //状态的数据必须通过setState这个内置API来更改
            this.setState({
                isHot:!this.state.isHot
            })
            }
        
        }
        // 渲染组件到页面
        ReactDOM.render(<Weather/>,document.getElementById('test'))  
        </script>
        ```

    * 箭头函数

      * ```javascrupt
            <script type="text/babel">
            // React.Component是react中内置的类
            // 创建类式组件
            class Weather extends React.Component {
                // 初始化Weather就要constructor，否则就可以不要
                constructor(props){
                    // 构造器只调用1次
                    console.log(constructor)
                    super(props)
                    // this.state={
                    //     isHot:true
                    // }
                    this.Weather=this.demo.bind(this)
        
                }
                state={
                        isHot:true
                    }
                render(){
                    // 读取状态  this.state.isHot
                    const {isHot} = this.state
                    return <h2 onClick={this.Weather}>今天天气很{isHot?'炎热':'寒冷'}</h2>
                }
                // 自定义方法=赋值语句+箭头函数
                demo=()=>{
                // this是Weather的实例对象
                this.setState({
                    isHot:!this.state.isHot
                })
                }
        
            }
            // 渲染组件到页面
            ReactDOM.render(<Weather/>,document.getElementById('test'))  
            </script>
        ```

      * 

    * 状态数据，不能直接修改或更新

      

##  组件实例的三大核心属性2: props

* 每个组件对象都会有props(properties的简写)属性

* ##  props基本使用

  * ```javascript
        <script type="text/babel">
        class Person extends React.Component{
            constructor(props){
                super(props)
            }
            render(){
                return (
                    <ul>
                        <li>{this.props.name}</li>
                        <li>{this.props.sex}</li>
                        <li>{this.props.age+1}</li>
                    </ul>
                )
                
            }
        }
        // 这里传递的属性被存储到props中,
        // 传递props就是传递标签属性
        // 这里的age是字符串类型
        ReactDOM.render(<Person name="tom"  sex="男" age="18"/>,document.getElementById('test1'))
        // 需要写数字类型的19
        ReactDOM.render(<Person name="小红" sex="女" age={19} />,document.getElementById('test2'))
        const p1 ={name:"小华",age:18,sex:"男", }
        // babel可以通过{...对象名称}来展开一个对象进行传参，只能用于标签属性
        ReactDOM.render(<Person {...p1}/>,document.getElementById('test3'))
        </script>
    ```

  * 原生的展开运算符

    * ```javascript
              <script>
                  let arr1 = [1,2,34,5]
                  let arr2 = [34,12,64,90]
                  // 展开一个数组
                  console.log(...arr1)//1 2 34 5
                  // 连接两个数组
                  let arr3=[...arr1,...arr2]
                  console.log(arr3)//[1, 2, 34, 5, 34, 12, 64, 90]
                  // 用于传参
                  function sum(...num){
                      console.log(num)//[1,2,3,4,5,6]
                  }
                  sum(1,2,3,4,5,6)
                  // 展开运算符不能展开对象
                  let person1 ={name:'tom','age':18}
                  // 复制一个对象{...}
                  let person2 ={...person1}
                  console.log(person2)//{name:'tom','age':18}
                  person2.name="小红"
                  console.log(person2)//{name:'小红','age':18}
                  console.log(person1)//{name:'tom','age':18}
                  // 复制的时候直接改属性值（合并）
                  let person3 ={...person1,name:'花花',address:'南开区'}
                  console.log(person3)//{name: "花花", age: 18,address:'南开区'}
      
              </script>
      ```

* **组件标签的所有属性都保存在props中**

* ### 作用

  * 通过标签属性从**组件外向组件内传递变化的数据**

  * 注意: 组件内部不要修改props数据

*  内部读取某个属性值

  * `this.props.name`

* 对props中的属性值进行类型限制和必要性限制

  * 第一种方式（React v15.5 开始已弃用）：

    * ```javascript
      Person.propTypes = {
       name: React.PropTypes.string.isRequired,
       age: React.PropTypes.number
      }
      ```

  * 第二种方式（新）：使用prop-types库进限制（需要引入prop-types库）

    * ```javascript
      Person.propTypes = {
        name: PropTypes.string.isRequired,
        age: PropTypes.number. 
      }
      ```

*   扩展属性: 将对象的所有属性通过props传递
  * `<Person {...person}/>`

*  默认属性值：

  * ```javascript
    Person.defaultProps = {
      age: 18,
      sex:'男'
    }
    ```

* ##  props类型限制优化

  * ```javascript
        <div id="test1"></div>
        <div id="test2"></div>
        <div id="test3"></div>
        <!-- 全局拥有React -->
        <script src="./js/react.development.js"></script> 
        <!--全局拥有ReactDOM  -->
        <script src="./js/react-dom.development.js"></script>
        <script src="./js/babel.min.js"></script>
        <!-- 用于对组件标签属性进行限制 -->
        <!-- 全局拥有PropTypes -->
        <script src="./js/prop-types.js"></script>
        <script type="text/babel">
        class Person extends React.Component{
            constructor(props){
                super(props)
            }
            render(){
                return (
                    <ul>
                        <li>{this.props.name}</li>
                        <li>{this.props.sex}</li>
                        <li>我是计算不是修改：{this.props.age+1}</li>
                    </ul>
                )
                
            }
            // props的验证规则
            // 给类自身添加属性
            //propTypes必须要这么些
            static propTypes={
            name:PropTypes.string.isRequired,
            sex:PropTypes.string,
            age:PropTypes.number,
            // 给函数设置规则func,指定是函数
            speak:PropTypes.func
    
            }
            // 设置标签默认值
            static defualtProps={
            sex:'男',
            age:18
            }
        }
    
        // 传递props就是传递标签属性，是只读的，不能修改props中的属性值
        ReactDOM.render(<Person name="小红" sex="女" age={19} />,document.getElementById('test1'))
        const p1 ={name:"小华",age:18,sex:"男", }
        // babel可以通过{...对象名称}来展开一个对象进行传参，只能用于标签属性
        ReactDOM.render(<Person {...p1}/>,document.getElementById('test2'))
        // 传函数
        ReactDOM.render(<Person name="小芳" sex="女" age={19}  speak={speak}/>,document.getElementById('test3'))
        function speak(){
    
        }
        
        </script>
    ```

  * 

* 组件类的构造函数:          

  *  ```javascript
    constructor(props){
      super(props)
      console.log(props)//打印所有属性
    }
     ```

  * ```javascript
        <script type="text/babel">
        class Person extends React.Component{
            
            constructor(props){
                super(props)
                console.log(this.props)//{name="小红",sex="女",age=19}
            }
            // constructor(){
            //     super()
            //     console.log(this.props)//undefined
            // }
            render(){
                return (
                    <ul>
                        <li>{this.props.name}</li>
                        <li>{this.props.sex}</li>
                        <li>我是计算不是修改：{this.props.age+1}</li>
                    </ul>
                )
                
            }
            static propTypes={
            name:PropTypes.string.isRequired,
            sex:PropTypes.string,
            age:PropTypes.number,
            speak:PropTypes.func
    
            }
            static defualtProps={
            sex:'男',
            age:18
            }
        }
        ReactDOM.render(<Person name="小红" sex="女" age={19} />,document.getElementById('test1'))
        </script>
    ```

  * 如果没有构造器不影响组件的使用，如果有构造器必须传props,super()接收,否则this.props就是undefined

* ##   函数式组件使用props(不能使用state，refs,除非使用最新版的hooks)

  * ```javascript
        <!-- 全局拥有React -->
        <script src="./js/react.development.js"></script> 
        <!--全局拥有ReactDOM  -->
        <script src="./js/react-dom.development.js"></script>
        <script src="./js/babel.min.js"></script>
        <!-- 用于对组件标签属性进行限制 -->
        <!-- 全局拥有PropTypes -->
        <script src="./js/prop-types.js"></script>
        <script type="text/babel">
        function Person(props){
            console.log(this)//undefined
            return (
                    <ul>
                        <li>{props.name}</li>
                        <li>{props.sex}</li>
                        <li>我是计算不是修改：{props.age+1}</li>
                    </ul>
                )
        }
            // 对标签属性进行限制
        // props的验证规则
        Person.propTypes={
            name:PropTypes.string.isRequired,
            sex:PropTypes.string,
            age:PropTypes.number,
            // 给函数设置规则func,指定是函数
            speak:PropTypes.func
    
        }
        // 设置默认值
        Person.defualtProps={
            sex:'男',
            age:18
        }
        ReactDOM.render(<Person name="小红" sex="女" age={19} />,document.getElementById('test1'))
        </script>
    ```

##   组件实例的三大核心属性3: refs与事件处理

*  组件内的标签可以定义ref属性来标识自己

* 字符串形式的ref

  * `<input ref="input1"/>`

* 回调形式的ref

  * `<input ref={(c)=>{this.input1 = c}}`

*  createRef创建ref容器·

  * ```javascript
    myRef = React.createRef() 
    <input ref={this.myRef}/>
    ```

    