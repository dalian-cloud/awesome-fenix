# 服务网格

:::tip 服务网格（Service Mesh）

A service mesh is a dedicated infrastructure layer for handling service-to-service communication. It’s responsible for the reliable delivery of requests through the complex topology of services that comprise a modern, cloud native application. In practice, the service mesh is typically implemented as an array of lightweight network proxies that are deployed alongside application code, without the application needing to be aware.

服务网格是一种用于管控服务间通信的的基础设施，职责是为现代云原生应用支持网络请求在复杂的拓扑环境中可靠地传递。在实践中，服务网格通常会以轻量化网络代理的形式来体现，这些代理与应用程序代码会部署在一起，对应用程序来说，它完全不会感知到代理的存在。

:::right

—— [What's A Service Mesh? And Why Do I Need One?](https://buoyant.io/2017/04/25/whats-a-service-mesh-and-why-do-i-need-one/)，Willian Morgan，Buoyant CEO，2017

:::

容器编排系统管理的最细粒度只能到达容器层次，在此粒度之下的技术细节，仍然只能依赖程序员自己来管理，编排系统很难提供有效的支持。2016 年，原 Twitter 基础设施工程师 William Morgan 和 Oliver Gould 在 GitHub 上发布了第一代的服务网格产品 Linkerd，并在很短的时间内围绕着 Linkerd 组建了 Buoyant 公司，担任 CEO 的 William Morgan 发表的文章《[What's A Service Mesh? And Why Do I Need One?](https://buoyant.io/2017/04/25/whats-a-service-mesh-and-why-do-i-need-one/)》中首次正式地定义了“服务网格”（Service Mesh）一词，此后，服务网格作为一种新兴通信理念开始迅速传播，越来越频繁地出现在各个公司以及技术社区的视野中。之所以服务网格能够获得企业与社区的重视，就是因为它很好地弥补了容器编排系统对分布式应用细粒度管控能力高不足的缺憾。

服务网格并不是什么神秘难以理解的黑科技，它只是一种处理程序间通信的基础设施，典型的存在形式是部署在应用旁边，一对一为应用提供服务的边车代理，及管理这些边车代理的控制程序。“[边车](https://en.wikipedia.org/wiki/Sidecar)”（Sidecar）本来就是一种常见的[容器设计模式](https://www.usenix.org/sites/default/files/conference/protected-files/hotcloud16_slides_burns.pdf)，用来形容外挂在容器身上的辅助程序。早在容器盛行以前，边车代理就已有了成功的应用案例，譬如 2014 年开始的[Netflix Prana 项目](https://github.com/Netflix/Prana)，由于 Netflix OSS 套件是用 Java 语言开发的，为了让非 JVM 语言的微服务，譬如以 Python、Node.js 编写的程序也同样能接入 Netflix OSS 生态，享受到 Eureka、Ribbon、Hystrix 等框架的支持，Netflix 建立了 Prana 项目，它的作用是为每个服务都提供一个专门的 HTTP Endpoint，使得非 JVM 语言的程序能通过访问该 Endpoint 来获取系统中所有服务的实例、相关路由节点、系统配置参数等在 Netflix 组件中管理的信息。

Netflix Prana 的代理需要由应用程序主动去访问才能发挥作用，但在容器的刻意支持下，服务网格无需应用程序的任何配合，就能强制性地对应用通信进行管理。它使用了类似网络攻击里中间人流量劫持的手段，完全透明（既无需程序主动访问，也不会被程序感知到）地接管掉容器与外界的通信，将管理的粒度从容器级别细化到了每个单独的远程服务级别，使得基础设施干涉应用程序、介入程序行为的能力大为增强。如此一来，云原生希望用基础设施接管应用程序非功能性需求的目标就能更进一步，从容器粒度延伸到远程访问，分布式系统继容器和容器编排之后，又发掘到另一块更广袤的舞台空间。
