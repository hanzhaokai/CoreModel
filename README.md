![image](https://github.com/CharlinFeng/Resource/blob/master/CoreModel/logo.jpg)<br/><br/>


##更新特性太多，文档重制中！陆续为您呈现！
##创业初期精力有限，本人最后一个开源OC框架：因您而精彩！


<br/><br/><br/>
一、CoreModel使用前言
==========

<br/>
#### 1.为什么要重制？
首先感谢大量朋友对我的框架的喜欢，同时也提出了各种问题和要求，最重要的有以下：<br/>
> (1). 不支持NSData。<br/>
> (2). 不支持NSArray。<br/>
> (3). 全部主线程操作，对性能有一定的影响。<br/>

<br/>
#### 2.框架依赖
> (1). CoreFMDB 数据库操作。<br/>
> (2). CoreHttp 网络请求：第四季及第五季用到。<br/>
> (3). CoreStatus 网络状态检测：第四季及第五季用到。<br/>
> (4). MJExtension 整个框架仅仅用了他的遍历成员属性这唯一的一个功能，别无他用。<br/>



<br/>
#### 3.最终申明
在开始之前，请您注意以下几点：<br/>
>(1). 导入了sqlite.lib 动态库。<br/>
>(2). 模型继承自CoreModel。<br/>
>(3). 为了更好的说明问题并突出重点，我可能在以下每个例子中使用的模型是不一样的。<br/>



