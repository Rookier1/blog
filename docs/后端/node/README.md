# Node知识
### 基础 ( 一 ) 三大基础模块

#### Process模块

介绍：提供当前Node进程相关的全局环境信息。

``````javascript
const process = require('process')//内置模块，直接使用
``````

process.cwd()

这是一个函数返回当前 Node 进程执行的目录，举例一个常见的场景：

一个 Node 模块 `A` 通过 NPM 发布，项目 `B` 中使用了模块 `A`。在 `A` 中需要操作 `B` 项目下的文件时，就可以用 `process.cwd()` 来获取 `B` 项目的路径。

```javascript
const cwd = process.cwd(); 
console.log(cwd);//输出:C:\Users\Qi
```

process.argv

在终端通过 Node 执行命令的时候，通过 `process.argv` 可以获取传入的命令行参数，返回值是一个数组：

	- 0: Node 路径（一般用不到，直接忽略）
	- 1: 被执行的 JS 文件路径（一般用不到，直接忽略）
	- 2~n: 真实传入命令的参数**

所以，我们只要从 `process.argv[2]` 开始获取就好了。一般都是这样用：

```javascript
const args = process.argv.slice(2);
```

process.env

返回一个对象，存储当前环境相关的所有信息，一般很少直接用到。

一般我们会在 `process.env` 上挂载一些变量标识当前的环境。比如最常见的用 `process.env.NODE_ENV` 区分 `development` 和 `production`。在 `vue-cli` 的源码中也经常会看到 `process.env.VUE_CLI_DEBUG` 标识当前是不是一 `DEBUG` 模式。

这里提一个 webpack 的插件 **DefinePlugin**[2]，在日常的构建流程中，我们经常会通过这个插件来注入不同的全局变量，从而执行不同的构建流程，并且代码中的 `process.env.xxx` 会被替换成具体的值，在 Terser 压缩阶段会将 deadCode 移除，优化代码体积。

process.platform (不常用)

返回当前系统信息(所有情况):

```javascript
console.log(process.platform);

// 'aix'
// 'darwin'  - macOS
// 'freebsd'
// 'linux' - linux
// 'openbsd'
// 'sunos'
// 'win32' - windows
```

#### Path模块

Node中路径相关的操纵都会用到这个模块

```javascript
const path = require('path');// 内置模块，直接使用
```

path.join(...paths)

`path.join` 作用是将传入的多个路径拼成一个完整的路径。

```javascript
const dPath = path.join('template', 'aaa', 'bbb', 'ccc', 'd.js');
// 输出: template/aaa/bbb/ccc/d.js
```

来看一个非常常见的场景，我们需要获取当前项目的 package.json 文件，就可以这样获取它的路径：

```javascript
const pkgPath = path.join(process.cwd(), './package.json');
// 输出: /Users/xiaolian/Code/node-api-test/package.json
```

`path.join` 可以传入任意个路径，比如：

```javascript
['package.json', 'README.md'].forEach(fileName => {
  const templateFilePath = path.join(process.cwd(), 'template', fileName);
  console.log(templateFilePath);
});
// 输出: /Users/xiaolian/Code/node-api-test/template/package.json
// 输出: /Users/xiaolian/Code/node-api-test/template/README.md
```

path.resolve(...paths)

`path.resovle` 和 `path.join` 的区别在于它的作用是将传入的多个路径和当前执行路径拼接成一个完整的绝对路径。

假设我现在 `index.js` 在 `scripts` 目录下，然后我在根目录下执行 `node scripts/index.js`，它的代码如下：

```javascript
const dPath = path.resolve('aaa', 'bbb', 'ccc', 'd.js');
// 输出:  /Users/xiaolian/Code/node-api-test/aaa/bbb/ccc/d.js
```

一般情况下，当 `path.resolve` 的第一个参数为 `./` 时，可以直接理解和 `path.join(processs.cwd(), '')` 表现一致。

path.basename(path[,ext])

`path.basename` 返回指定 `path` 最后一个路径名，其中第二个参数 `ext` 可选，表示文件扩展名。比如：

```javascript
console.log(path.basename('scripts/index.js'));  // index.js
console.log(path.basename('scripts/index.js', '.js'));  // 匹配到 .js，返回 index
console.log(path.basename('scripts/index.js', '.json'));  // 没匹配到，返回 index.js
```

path.dirname(path)

和 `path.basename` 对应，返回指定 `path` 最后一个路径名之前的路径。比如：

```javascript
console.log(path.basename('scripts/index.js'));  // scripts
console.log(path.basename('scripts/hook/index.js'));  // scripts/hook
```

path.extname(path)

和 `path.basename` 对应，返回指定 `path` 最后一个路径名的文件扩展名（含小数点 `.`）。比如：

```javascript
console.log(path.basename('scripts/index.js'));  // .js
console.log(path.basename('README.md'));  // .md
```

#### fs(File System)模块

文件系统相关操作都会用到fs模块,还有另外一个也会经常用到fs-extra,之后再详谈。关于node学习还是看[官方文档](http://api.nodejs.cn/)

```javascript
const fs = require('fs');// 内置模块，直接使用
```
fs 模块的 API 默认都是异步回调的形式，如果你想使用同步的方法，有两种解决方法：

	1. 使用 Node 提供的同步 API：xxxSync，也就是在 API 的后面加一个 Sync 后缀，它就是一个同步方法了（具体还是需要查文档，是否有提供同步 API）
	2. 包装成一个 Promise 使用

fs.stat(path[,options],callback)

返回一个文件或目录信息

```javascript
const fs = require('fs');

fs.stat('a.js', function(err, stats) {
  console.log(stats);
});
```
其中包含的参数有很多，介绍几个比较常用的：
```javascript
export interface StatsBase<T> {
  isFile(): boolean;                 // 判断是否是一个文件
  isDirectory(): boolean;            // 判断是否一个目录

  size: T;                           // 大小（字节数）
  atime: Date;                       // 访问时间
  mtime: Date;                       // 上次文件内容修改时间
  ctime: Date;                       // 上次文件状态改变时间
  birthtime: Date;                   // 创建时间
}
```
一般我们会使用 fs.stat 来取文件的大小，做一些判断逻辑，比如发布的时候可以检测文件大小是否符合规范。在 CLI 中，经常需要获取一个路径下的所有文件，这时候也需要使用 fs.stat 来判断是目录还是文件，如果是目录则继续递归。当然，现在也有更方便的 API 可以完成这个工作。

同步方法
```javascript
const fs = require('fs');

try {
  const stats = fs.statSync('a.js');
} catch(e) {}
```
fs.readdir(path)

获取 `path` 目录下的文件和目录，返回值为一个包含 `file` 和 `directory` 的数组。[只获取当前目录,不会深入遍历]

假设目录为:

```javascript
├── a
│   ├── a.js
│   └── b
│       └── b.js
├── index.js
└── package.json
```
执行下面代码:
```javascript
const fs = require('fs');

fs.readdir(process.cwd(), function (error, files) {
  if (!error) {
    console.log(files);
  }
});
//返回['a','index.js','package.json']
```
同步方法
```javascript
const fs = require('fs');

try {
  const dirs = fs.readdirSync(process.cwd());
} catch(e) {}
```
fs.readFile(path[,options],callback)

文件读取的 API，通过 `fs.readFile` 可以获取指定 `path` 的文件内容。

参数如下:

1. 第一个参数: 文件路径
2. 第二个参数: 配置对象，包括 `encoding` 和 `flag`，也可以直接传如 `encoding` 字符串
3. 第三个参数: 回调函数

使用:

```javascript
const fs = require('fs');
const path = require('path');

fs.readFile(path.join(process.cwd(), 'package.json'), 'utf-8', function (
  error,
  content
) {
  if (!error) {
    console.log(content);
  }
});
```
如果没传 `encoding`，则其默认值为 `null`，此时返回的文件内容为 `Buffer` 格式。



同步方法

```javascript
const fs = require('fs');

try {
  fs.readFileSync(path.join(process.cwd(), 'package.json'), 'utf-8');
} catch(e) {}
```
fs.writeFile(file,data[,options],callback)

对应着读文件 `readFile`，`fs` 也提供了写文件的 API `writeFile`，接收四个参数：

	1. 第一个参数: 待写入的文件路径
	2. 第二个参数: 待写入的文件内容
	3. 第三个参数: 配置对象，包括 `encoding` 和 `flag`，也可以直接传如 `encoding` 字符串
	4. 第三个参数: 回调函数

使用:

```javascript
const fs = require('fs');
const path = require('path');

fs.writeFile(
  path.join(process.cwd(), 'result.js'),
  'console.log("Hello World")',
  function (error, content) {
    console.log(error);
  }
);
```
同步方法
```javascript
const fs = require('fs');
const path = require('path');

try {
  fs.writeFileSync(
    path.join(process.cwd(), 'result.js'),
    'console.log("Hello World")',
    'utf-8'
  );
} catch (e) {}
```
***以上内容摘自公众号[nodejs技术栈],用于自用复习***



