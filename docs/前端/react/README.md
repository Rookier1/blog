# React全家桶

## 基础
### 介绍

* 由Facebook开发开源的用于构建用于界面的JavaScript库(<span style='color:red'>只关注界面</span>)，将数据渲染为HTML视图。
* 因为原生js操作dom效率低、步骤繁琐、并且导致浏览器出现大量的重绘重排，代码复用率低，所以要使用框架来开发。框架的组件化模式，声明式编码，能够大大提高开发效率和复用率。使用虚拟dom，不直接操作真实dom，diffing算法能够最小化页面重绘，非常高效。

* 前提：this，class，es6，原型和原型链，数组方法，模块化，npm

### 开始

- 外部引入js：act一共需要引入三个文件，分别是：
    - react.js核心库（先引入这个）
    - react-dom.js 操作dom的react拓展库（在引入这个）
    - babel.min.js 解析jsx语法的库（最后引入这个）
        1. 浏览器不能直接解析JSX代码, 需要babel转译为纯JS的代码才能运行
        2. <span style='color:red'>只要用了JSX，都要加上type="text/babel"</span>, 声明需要babel来处理

![image](https://api2.mubu.com/v3/document_image/defa3162-4483-41ae-9795-7848a52928ca-9596755.jpg)

* JSX

    * 全称JavaScript Xml ,是react定义的一种类似于XML的JS扩展语法: JS + XML , 本质是React.createElement(component, props, ...children)方法的语法糖

    - 作用：简化创建虚拟DOM
        1.  写法:```var ele =<h1>Hello JSX!</h1>```
        2. 它不是字符串, 也不是HTML/XML标签,最终产生的就是一个js对象

     - 标签
        1. 标签名任意HTML标签或者其他标签；标签属性任意HTML标签属性或其他

     - 语法规则
        1. 遇到 <开头的代码, 以标签的语法解析:html同名标签转换为html同名元素, 其它标签需要特别解析
        2. 遇到以 { 开头的代码，以JS语法解析: 标签中的js表达式必须用{ }包含
        3. 样式类名要用className
        4. 内联样式要用style={{key:value}}的形式写
        5. 只能有一个跟标签
        6. 标签必须闭合
        7. 标签首字母
            - 如果小写字母开头,将标签转换为HTML同名元素,如果HTML没有此元素则报错
            - 如果大写字母开头,react就渲染对应的组件,若组件没有定义则报错
* 示例 : 
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <div id="root">
    
    </div>
    <div id="root1"></div>
    <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
    <script type="text/babel">
        const name = 'jone';
        const elements = <h5>hello,{name}</h5> //这是被称为jsx的语法拓展
        console.log(elements)
        ReactDOM.render(
            elements,
            document.getElementById('root1')
        );
        /*function format(user){
            return user.firstname + ' ' + user.lastname;
        }
        const user = {
            firstname : '123',
            lastname : '456'
        }
        const element = (
            <h2>
                hello,{format(user)}!
            </h2>
        ); */ //建议把jsx分为多行，并将jsx包括在括号中，可以避免自动插入分号的坑
        //函数也可以放入大括号内
        /*ReactDOM.render(
            element,
            document.getElementById('root')
        )*/
    
        //编译后jsx会被转换为普通js，可以在if，for循环中使用jsx
        //将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX
        /*function getGreeting(user) {
            if (user) {
                return <h1>Hello, {format(user)}!</h1>;
            }
            return <h1>Hello, Stranger.</h1>;
        }
    
        ReactDOM.render(
            getGreeting(user),
            document.getElementById('root')
        )*/
        //你可以通过使用引号，来将属性值指定为字符串字面量：
        //const element1 = <div tabIndex="0"></div>;
        //也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：
        //const element2 = <img src={user.avatarUrl}></img>;
    
        /*在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。
        你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，
         对于同一属性不能同时使用这两种符号。
         因为 JSX 语法上更接近 JavaScript 而不是 HTML，
         所以 React DOM 使用 camelCase（小驼峰命名）来定义属性的名称，
         而不使用 HTML 属性名称的命名约定。
        例如，JSX 里的 class 变成了 className，而 tabindex 则变为 tabIndex。
         */
    
        /*
            Babel 会把 JSX 转译成一个名为 React.createElement() 函数调用。
    
            以下两种示例代码完全等效：
    
            const element = (
              <h1 className="greeting">
                Hello, world!
              </h1>
            );
    
            const element = React.createElement(
              'h1',
              {className: 'greeting'},
              'Hello, world!'
            );
    
            React.createElement() 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：
    
            // 注意：这是简化过的结构
            const element = {
              type: 'h1',
              props: {
                className: 'greeting',
                children: 'Hello, world!'
              }
            };
    
            这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。
    React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。
        * */
    </script>
    </body>
    </html>
    ```
* 虚拟DOM
    1. 就是js中的对象
    2. 虚拟DOM比较轻量,真实dom比较重量级
    3. 最终被react转化为真实dom
    4. 渲染虚拟dom
        1. 语法 ReactDOM.render(virtualDOM, containerDOM)
            - 参数1 纯js或jsx创建的虚拟dom对象
            - 参数2  用来包含虚拟DOM元素的真实dom元素对象(一般是一个div)
        2. 作用: 将虚拟DOM元素渲染到页面中的真实容器DOM中显示
### 组件化编程

* 模块
    1. 理解：向外提供特定功能的js程序, 一般就是一个js文件
    2. 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂。
    3. 作用：复用js, 简化js的编写, 提高js运行效率
    4. 模块化：当应用的js都以模块来编写的, 这个应用就是一个模块化的应用

* 组件

    1. 理解：用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)
    2. 为什么要用组件： 界面的功能更复杂
    3. 作用：复用编码, 简化项目编码, 提高运行效率
    4. 注意：
        * 组件名必须大写
        * 虚拟dom元素只能有一个根标签
        * 虚拟dom元素必须有结束标签
    5. 组件化当应用是以多组件的方式实现, 这个应用就是一个组件化的应用
    6. 函数式组件：适用于简单组件，无状态；创建一个函数，要有返回值
    7. 类组件：适用于复杂组件，有状态；创建一个类，必须继承react内置类React.Compoent ，不需要写构造器，类中必须写render方法，方法要有返回值

* 渲染类组件标签的基本流程

    1. React内部会创建组件实例对象

    2. 调用render()得到虚拟DOM, 并解析为真实DOM

    3. 插入到指定的页面元素内部

* 组件三大属性[<span style='color:red'>重要</span>]

    * state

        1. state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)
        2. 组件被称为"状态机", 通过更新组件的state来更新对应的页面显示(重新渲染组件)
        3. 注意
            * 组件中render方法中的this为组件实例对象
            * 组件自定义的方法中this为undefined : 一种方法是强制绑定this,通过函数对象的bind(this),另一种方法是使用赋值语句+箭头函数
            *  状态不能直接修改或更新

    * props

        1.  每个组件对象都会有props(properties的简写)属性
        2. 通过标签属性从组件外向组件内传递变化的数据
        3. 组件标签的所有属性都保存在props中
        4. props是只读的
        5. 注意
            * 组件内部不要修改props的数据

    * refs

        1. 组件内的标签可以定义ref属性来标识自己,相当于id

        2. 三种形式

            * 字符串形式(<span style='color:red'>尽量不用,官方已经不建议使用</span>)

                ```javascript
                <input ref="input1"/>
                ```

                

            * 回调形式的ref

                ```javascript
                <input ref={(c)=>{this.input1 = c}}/>
                ```

            * createRef创建ref容器

                ```javascript
                myRef = React.createRef()     <input ref={this.myRef}/>
                //调用后返回一个容器,存储被ref标识的节点,该容器只能存一个
                ```
        ```
        
        ```
    
* 事件处理

    1. 通过onXxx属性指定事件处理函数(注意大小写)
        * React使用的是自定义(合成)事件, 而不是使用的原生DOM事件----为了更好的兼容性
        * React中的事件是通过事件委托方式处理的(委托给组件最外层的元素)----为了高效 
    2. 通过event.target得到发生事件的DOM元素对象----不要过度使用ref

* 表单

    1. 受控组件
    2. 非受控组件

* 组件间通信

    1. 消息订阅-发布机制

        * 工具: PubSubJS

        * 下载: npm install pubsub-js --save

        * 使用: 

            ```javascript
            import PubSub from 'pubsub-js' //引入
            PubSub.subscribe('delete', function(data){ }); //订阅 ---->在接收消息的组件中订阅消息
            PubSub.publish('delete', data) //发布消息
            ```

## 进阶
## 高级