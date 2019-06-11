# nodeJS
nodeJS学习

**nodeJS和其他后台语言不同**
1. nodeJS的对象、语法跟JavaScript一模一样；利于前端人员用
2. 性能还不错，还可以
3. 在C语言面前，所有语言的性能都是渣
4. 与PHP比，性能方面有优势，提升八十多倍
5. 前后台配合方便
6. 依赖模块
**缺点：**
1. Java有及其丰富库支持
2. nodeJS可以做服务器--小型后台系统
**运行nodeJS程序**
* node filename
- 引入http模块
- createServer
- listen
**默认端口号**
> http：80
  ftp: 21
  mysql: 3306
**response有两个东西，一个是write，一个是end**
> nodeJS和JS一样，单线程、单进程
  nodeJS用的是非阻塞异步交互
**爬虫--抓取别人网站的数据**
**request**
> method
  url
**assert模块**
> 断言
  assert(arguments.length == 2, '必须传递两个参数')
**Buffer模块**
> 帮助我们处理二进制数据
**fs模块**
> 二进制数据不要转成字符串，否则文件会被破坏
**child processes**
**Crypto**
> 签名，加密
  createHash
  digest
  有md5和sha1，单向散列，这些都是签名算法，RSA非对称加密
**os**
> os.cpus()
**path**
> path.parse()，dirname，basename，extname
**一个程序由多个进程组成，一个进程由多个线程组成，纤程比线程还小**
> - 进程拥有独立的执行空间、存储  
  - 同一个进程内的所有线程共享一套空间、代码
  - 多进程--成本高--慢--安全--进程间隔离--进程间通信麻烦--写代码简单
  - 多线程--成本低--快--不安全--要死一块死--通信较简单--写代码复杂
**repl 交互式环境**
**Events 事件队列**
> 解除耦合
**Query Strings**
> parse
**url**
> parse
  parse, true
**dns 域名解析**
*流--连续的数据*
### 程序员内功
- 算法
- 设计模式
- 架构
writeHeader
**url.parse()//解析的是整个url，querystring.parse()//解析的是数据那一段**
**数据库**
- 文件型数据库--sqlite
  - 简单，数据量少
- 关系型--MySQL-Oracle
  - 数据之间是有关系的，性能高，速度快
- 文档型--mongodb
  - 直接存储异构数据方便
*大型系统主数据库--用关系型
* MySQL是免费的，适用于绝大多数普通应用，容灾能力比Oracle弱一点
* Oracle适用于金融、医疗，容灾能力强
<mark>SQL，性能略差，nosql，没有复杂的关系，对性能有极高的要求</mark>
**表单没有跨域的说法**
* uuid，.net一般称为guid
**表单的三种post**
* text/plain // 用的很少，纯文本
* application/x-www-form-urlencoded 默认，url编码方式，xxx=xxx&xxx=xxx，只能上传文件名
* multipart/form-data // 上传文件内容
**对Buffer进行操作**
* 查找-indexOf()
* 切分-slice() // 包左不包右
* 截取-split() // 目前buffer还不支持split
* new Buffer().toString()
*正则只能处理字符串，不能处理二进制数据*
**uuid**
* v1用的时间戳
* v4用的随机数

**npm**
* node package manager
* cnpm 就是npm 安装一个taobao镜像

*上传一部分就解析一部分，极大地节约内存*
**readFile先把所有数据全部读到内存中，然后回调**
* 极其占用内存
* 资源利用极其不充分

**流**
* 读一点、发一点
* 三种流
    * 读取流-读文件
    * 写入流-写文件
    * 读写流 // 压缩、加密
* let rs = fs.createReadStream
* let ws = fs.createWriteStream
* rs.pipe(ws)
* req、res其实也是流，req是读取流，res是写入流
* 流.on('error', (err) => {})
* 流.on('finish', () => {}) // 写入完成
* writeHeader

**zlib**
* 服务端开启gzip压缩
* let zlib = require('zlib')
* zlib.createGzip()
* setHeader('content-Encoding', 'gzip')

*流的底层是生产者/消费者模型*
*一般来说读比写快*

**http端点续传，可以用content-range**

**服务端如何判断文件类型**
* 通过文件扩展名
* 分析文件结构(没有广义通用的方法)

**缓存**
* 需要设置缓存
* 减少对服务器资源的浪费
* stat
* toMGTString
* toUTCString
* on 监听读着读着可能出错
* 服务器的文件时间大于客户端的文件时间，需要给客户端最新的
* 流程
    * 第一次S->C Last-modified
    * 第二次C-S if-modified-since
    * 第三次S-C 200 || 304
* 浏览器的缓存一般不限制大小
* 隐私的文件不需要缓存
* cache-control
* writeHeader只能设置一次，可以用setHeader
* 获取客户端时间通过```if-modified-since```
* 获取服务端时间通过```stat.mtime```
* cookie是不受控的

**多进程**
* 多进程：性能高、复杂
* 多线程：性能略低、简单
* nodeJS：默认单进程单线程
* 主进程：负责派生(创建)子进程
* 子进程：负责干活
* nodeJS可以变为多进程
* 一个程序只有一个主进程
* cluster、process
* 普通进程不能创建进程，只有系统进程才能创建进程，只有主进程可以分裂
* 进程是分裂出来的
* 分裂出来的进程执行的是同一套代码
* 父子进程之间可以共享句柄
* cluster模块
* fork()
* isMaster()
* os 模块
* os.cpus() //获取几个cpu
* 主进程是不工作的，工作的是子进程
* 主进程也叫守护进程
* 子进程也叫工作进程
* process 模块
* process.pid //获取进程id
* 进程调度--有开销
* 多个进程，第一个满了--去启用第二个
* 进程死了，系统会把它销毁

**数据库**
* C/S
* nodeJS是客户端、Java、PHP、Navicat for MySQL
* 库：文件夹，不能存数据，只能管理表
* 表：文件，可以存数据

**使用Navicat**
* 新建表，表里包含的是字段
* 列-字段(域)
* 字段类型
    * 数字
        * 整数：typeint、smallint、mediumint、int、bigint
        * 小数：float、double
    * 字符串
        * varchar
        * text
* 主键
    * 性能高
    * 唯一
**表情和图片如何存在数据库**
* 存成文件
