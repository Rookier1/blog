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

* 虚拟DOM
    1. 就是js中的对象
    2. 虚拟DOM比较轻量,真实dom比较重量级
    3. 最终被react转化为真实dom
    4. 渲染虚拟dom
        1. 语法 ReactDOM.render(virtualDOM, containerDOM)
            - 参数1 纯js或jsx创建的虚拟dom对象
            - 参数2  用来包含虚拟DOM元素的真实dom元素对象(一般是一个div)
        2. 作用: 将虚拟DOM元素渲染到页面中的真实容器DOM中显示


## 进阶
## 高级