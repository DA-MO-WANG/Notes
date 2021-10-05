2021.10.1-因为s要与特定的c进行一对一交互，所以要有一个识别机制。这个识别机制具体技术实现先有cookie. cookie本质就是一种变量，样子长得像 name=value这个样子。服务器可以设置name\value具体是什么。
		cookie本身要占网络资源，所以这就制约了它不可能存储很多信息。但有些信息又得必要和用户挂钩。这里就有了session，这玩意实际是以小换大，用一个id来指向一大坨屎。

​		session的设计，样子是key-value这种。具体是设计不是很简单的哈希表，而是基于哈希表的加工。
​		比如manager\provider\session的接口设计----》
​				handler函数会把请求中的cookie放到manager中，这玩意负责管理最上层的session的各种操作，它里面还有一个provider的设计，provider从manager中拿到sid，然后找到对应的session，这里session还是一个接口，它f