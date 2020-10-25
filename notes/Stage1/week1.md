## Week1-万丈高楼，地基首要

#### [1-2 大型网站的特点与设计宗旨](./PDF/1-2.pdf)

#### 1.3 大型网站架构演变历程

![image-20201009204742499](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009204742.png)

早期是单体架构模式，网站没什么用户访问，一台服务器就够，文件服务器与数据库也部署在这个服务器内，页面是打包成war包部署在服务器上（即上线）

![image-20201009210034864](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009210035.png)随着项目规模变大，**开始将文件服务器与数据库从服务器分离**，这样即便服务器挂了，文件服务器和数据库可以正常访问

![image-20201009210259930](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009210300.png)

随着数据库规模不断增大，出现了**缓存中间件**的概念（如redis），这样用户首先是从缓存中查询，然后再从数据库查询，访问网站速度更快了。

![image-20201009210526667](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009210526.png)

随后又出现了**负载均衡**和**集群（cluster）**的概念，避免宕机（某台服务器挂掉）对于整个项目产生太大影响，但这时候数据库依然只有一条。

![image-20201009210727304](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009210727.png)

到了千万级项目，就出现了**数据库读写分离**的技术，因为用户写读遵循**20/80**原则，而读相对快，大量写入则对数据库要求比较高，所以就设置**主从两个数据库**，这样给用户的感觉是读取速度（访问网页）速度非常快。同时主从服务器之间也要不断进行**同步**。

![image-20201009211354403](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009211354.png)随后又出现了**数据库集群（分布式数据库）**的技术，可以把很大的一张表拆成N张更小的表**散列**到不同的子数据库中，查询的时候通过散列技术找到对应数据库，也就能找到对应的那一截表。



通常当表的规模达到700W条数据时就要采用数据库集群了，否则数据库性能会急剧下降，此时主键就不能用自增长了，而要用**分布式主键**

![image-20201009212448679](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009212448.png)

**搜索引擎（如ElasticSearch）**技术是为了解决**用户多样化的查询**（比如模糊查询）需求而产生的的，它**对数据库也做了保护**，用户的查询不会轻易动数据库

![image-20201009212805503](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009212805.png)

项目更大之后，开始出现**业务分离**，每个业务对应于一个团队，都有一个tech lead去带，也可以用到主从数据库以及服务集群的技术，当然这对于DevOps和QA都是较大的挑战

![image-20201009213058801](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201009213115.png)

微服务架构中存在某些**公共服务资源**，比如这里的短信邮件推送微服务，这个服务可能是被订单服务以及商品服务这两个业务共用的。就如同操作系统对于进程有一套同步/异步的机制，**异步队列MQ**也是用来解决不同业务之间的异步问题的。在此过程中又涉及到**分布式**和**调优**两大技术。

#### 1.4 JAVA架构师核心技术栈

> 关注前沿技术

他说有一个带他的架构师每天花2小时了解最前沿的技术

> 会管人

架构师要知道**排兵布阵**

除此之外，还要能带新人，会沟通（PM的能力），

> soft skills

比如会跟老板搞好关系，公司中不止你一个架构师，架构师之间可能有分歧，这个时候你跟老板关系好，你就可能更有impact。

#### 2.1 单体架构阶段概述与项目演示

项目上线属于Devops做的事情，但是小公司也希望你会，所以学这个对后面去爱尔兰找实习很有帮助

#### 2.2 前后端技术选型讲解

> 技术选型

![image-20201019153143946](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019153145.png)

SpringMVC是Spring的一个子集，它是一个框架，而SpringBoot是简化配置的工具，把很多jar包都集成了，简化了配置，maven中一个*-starter即可完成所有的配置，Tomcat也是内置，无需配置

> 前端技术选型

![image-20201019153746777](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019153747.png)

**一个后端可以不用写那么多前端代码，但一定要懂前端的前沿技术**，尤其是架构师或者senior software engineer。

**VUE.JS是渐进式开发**，比如一个纯粹由Jquery写的前端，**你可以用VUE.JS完成重构**，但是又不会破坏原有的前端项目。

![image-20201019154218718](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019154220.png)

**社区活跃度**，比如为什么这么多人用React和Augular，就是因为社区活跃度大

**团队技术水平**如果不够，就需要培训

**安全性**，比如有些框架有漏洞，容易被攻击

**开源**，有些老板是舍不得花钱买框架来用的

#### 2.3 前后端分离开发模式

> 传统MVC架构模式

![image-20201019152002624](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019152004.png)

早起所有页面的渲染操作都是由后端服务器来执行（servlet），这对服务器压力很大，当有几百万请求的时候，**服务器扛不住**

> 前后端分离MVVM

![image-20201019152336089](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019152337.png)

- 首先多了一个Nginx服务器，Nginx无法处理JSP这种动态页面，但是处理静态页面（HTML,JS,CSS）效率很高。
- **所有的静态页面都在Nginx服务器中**， 这个服务器归前端人员管
- Nginx通过Ajax**向服务器(back-end)发出RESTful API来进行request**，数据都是JSON格式
- **这样后端人员只需要维护一套Restful接口即可**，他不用管前端，即便是Mobile，只要它支持HTML5，也可以用为Web开发的Restful API接口（手机打开Youtube页面一样好使）
- 这样就做到了前后端分离，JSP页面那种页面里写JAVA代码的不伦不类就消失了。团队分工更明确
- 项目部署，**上线的时候也是不同团队各自部署自己的前/后端项目**，不会出现踢皮球的现象（明明是前端的问题，却说是后端团队的问题） 

#### 2.4 项目分层设计原则讲解

![image-20201019154718782](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019154720.png)

**一款产品就像一辆车**，车有很多零部件，比如轮子，发动机，窗户，空调，**互联网公司不会自己去造所有的组件，**正如汽车厂商不会自己造所有的零件一样，能外包的业务尽量外包了，这就是外包公司存在的价值之一。

> MAVEN聚合

![image-20201019155110248](https://raw.githubusercontent.com/ZhouMeng1998/IMG/image-upload/20201019155112.png)

一些公共的模块比如common.jar可以直接抽离出来，无需进行重复开发。几个不同的项目，一份common.jar代码即可

#### 2.5 构建聚合工程

顶级项目foodie-dev打包方式为<packaging>pom</packaging>

 ```
- jar：默认的打包方式，打包成jar用作jar包使用。打包成jar用作jar包使用。
- war：将会打包成war，发布在服务器上，如网站或服务。一般是java web项目打包。
- pom：用在父级工程或聚合工程中，用来做jar包的版本控制，必须指明这个聚合工程的打包方式为pom。
 ```

#### 2.6 PDMan数据库建模工具使用

PDMan启动记得用管理员模式，不然可能报权限不足

