> * 原文地址：[Modules vs. microservices](https://www.oreilly.com/ideas/modules-vs-microservices)
> * 原文作者：[Sander Mak](https://www.oreilly.com/people/sander_mak)
> * 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
> * 译者：[lsvih](https://github.com/lsvih)
> * 校对者：[steinliber](https://github.com/steinliber),[DeadLion](https://github.com/DeadLion)

# 模块化 vs. 微服务

使用模块化系统设计原则来避免微服务的复杂性。

![](https://d3tdunqjn7n0wj.cloudfront.net/360x240/container-227877_1920-0db52b796e6b80d98f6df2d01a6ee4fb.jpg)

从单体式应用向微服务架构迁移已经是老生常谈的话题了。除了过过嘴瘾，似乎真的动手将单体式应用拆分成微服务也不是什么很困难的事。但是这种做法真的是你们团队的最佳选择吗？维护一个凌乱的单体式应用的确很伤脑筋，但是还有另一种优秀但常常被人忽视的替代方案：模块化应用开发。本文将探讨这种替代方案，并展现其与构建微服务的关系。

## 模块化微服务

“通过微服务，我们终于能够让团队独立工作了”或者“我们的单体式应用实在太复杂了，它降低了我们的工作效率”之类的话，只是让团队改用微服务架构的诸多原因中的一小部分；还有一种说法是需要可拓展性与弹性。所有开发人员似乎都渴望系统设计和开发的模块化。软件开发中的模块化可以总结为以下三个原则：

- **强大的封装性**：隐藏了各个组件内部实现的细节，减少了不同组件之间的耦合性。团队可以在系统的各个非耦合部分中独立地工作。
- **定义良好的接口**：你不可能隐藏组件内的所有东西（否则你的系统将毫无意义），因此有必要在组件之间定义良好且可靠的 API。任意一个组件都可以被符合接口规范的其它组件替换。
- **显式依赖**：模块化系统意味着不同的组件需要在一起工作。因此你最好能有一种途径来表达（与验证）它们之间关系。

这些原则都可以用微服务架构来实现。只要做到对其它服务暴露定义明确的接口（通常是一个 REST API），就能以任意方式来实现一个微服务。它的实现细节是这个服务内部的事情，你可以改变这些实现细节而不影响整个系统。微服务之间的依赖关系通常在开发时是不明确的，这可能会导致在运行时服务编排失败。只能说在大多数微服务架构中，实现最后一条模块化原则还需要再接再厉。

因此，微服务架构实现了重要的模块化原则，并带来了以下三点实实在在的好处：

- 团队能够独立地工作与扩张。
- 微服务小巧、专一，降低了复杂度。
- 服务可以在不会影响全局的情况下内部进行更改或者替换。

那么微服务架构的缺点是什么呢？当你从一个单体式（虽然有点臃肿）应用切换成微服务分布式系统的时候，给表操作带来了巨大的复杂性。突然间，你发现你要不断地部署各种不同的（可能是由容器包装的）服务。这时，服务发现、分布式日志记录、跟踪等新的问题出现了。现在，你更加容易出现[分布式计算的谬论](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)造成的错误。接口的版本管理与配置管理成为你面对的主要问题。各种问题将数不胜数向你涌来。

事实证明，由于所有的微服务个体都需要联合起来实现业务逻辑，微服务之间的连接将变得无比复杂。看到这里，你应该意识到不能简单地将单体式应用拆分成微服务了。单体式应用中的“意大利面条式代码”问题重重，在其中再加上网络边界会将这些纠缠在一起的问题升级成彻头彻尾的痛苦。

* 译注：[什么是意大利面式代码](http://www.ituring.com.cn/article/10311)

## 模块化的替代方案

这是否意味着我们要么沉没在混乱的单体式应用中，要么淹没在令人抓狂的微服务复杂性中呢？其实，模块化也可以通过其它方式实现。在开发时最重要的是正确地规划项目边界并实施方案，我们也可以通过创建一个结构良好的单体式应用来实现这一点。当然，这意味着我们将尽可能利用编程语言与开发工具的协助来实现模块化原则。

例如在 Java 中，有几个可以帮助你构建应用的模块系统。OSGi 是其中最著名的一个，不过随着 Java 9 的发布，Java 平台将加入一个原生的模块系统。现在模块作为一等结构（first-class construct），成为了语言和平台的一部分。Java 模块可以表明对其它模块的依赖，以及在强封装实现类的时候公开暴露接口。甚至 Java 平台本身（一个庞大的代码库）已经使用了新的 Java 模块系统进行模块化。你可以在我即将出版的书[Java 9 Modularity](https://www.safaribooksonline.com/library/view/java-9-modularity/9781491954157/?utm_source=newsite&amp;utm_medium=content&amp;utm_campaign=lgen&amp;utm_content=modules-vs-microservices-inline)中了解有关 Java 9 模块化开发的更多信息。（现早期版本已经发布）

其它的语言也提供了类似的机制。例如，JavaScript 在 ES2015 规范中提供了一个[模块系统](http://exploringjs.com/es6/ch_modules.html)。在此之前，Node.js 也为 JavaScript 后端提供了一个非标准的模块系统。然而 JavaScript 作为一种动态语言，对于强制接口（类型）与模块封装的支持还是较弱。你可以考虑在 JavaScript 的基础上使用 TypeScript 来重新获得这些优点。微软的 .Net 框架与 Java 一样都有着强类型，但就强封装以及程序集（Assemblies）间的显式依赖而言，它与 Java 即将推出的模块系统并不相同。尽管如此，你可以通过使用 [.Net Core](https://msdn.microsoft.com/en-us/magazine/mt707534.aspx) 中标准化的反转控制模式（IOC）以及创建逻辑相关的程序集来实现良好的模块化架构。即使是 C++ 也在以后的版本中[考虑添加](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4610.pdf)模块系统。许多语言都在向模块化靠近，这本身就是一个显著的进步。

当你有意识地使用你的开发平台的模块化特性时，你就可以实现之前提及的微服务的模块化优势。基本上模块系统越好，你在开发过程中获得的帮助就越多。只要在不同团队间的接触点定义好明确的接口，不同的团队也可以独立进行不同部分的工作。当然，在部署时还是要将模块在一个单独的部署单元中组合起来。这样可以防止过于复杂，以及减少迁移到微服务所需要的开发与管理成本。诚然，这也意味着你不能使用不同的技术栈来构建不同的模块，但你的团队应该不会真的这么做吧？

## 模块设计

创建好的模块和创建好的微服务一样，都需要严谨的设计。一个模块应该基于其域的有界上下文建模（DDD）。选择微服务的边界是架构上重要的决策，一旦出错就可能要付出沉重的代价。相较而言，模块化应用程序模块的边界更容易修改一些。模块间的重构通常由类型系统和编译器支持。微服务边界的重新划分则涉及大量的进程间通信（IPC），以确保运行时稳定性。老实说，你真的只用一次两次就能正确的划分好边界？

在许多方面，静态语言的模块为了定义明确的接口而提供了更好的结构。通过调用另一个模块暴露的接口提供的方法，比去调用另一个微服务的 REST 端点健壮性要强的多。REST+JSON 现在无处不在，但在没有编译器检查的情况下，它并没有”类型良好的互通性“这个特点。而事实上，通过网络序列化（或者反序列化）数据并不是无开销的，甚至这种传输方式更加逊色。此外，许多模块化系统允许你表明此模块对于其它模块的依赖关系，模块系统将不允许违背这些依赖关系的情况出现。而微服务之间的依赖关系只在运行时实现，导致系统难以调试。

模块也是代码所有权中的自然单位。一个团队可以负责系统中的一个或者多个模块，而只需要给其它团队提供模块的公共 API。在运行时，模块之间的隔离比微服务少，毕竟模块化单体式应用的所有模块都运行在同一个进程中。

* 译注：[什么是代码所有权](http://blog.csdn.net/mfowler/article/details/974251)

毫无疑问，单体式应用的模块不可能像微服务一样有自己的数据。模块化应用内部的数据交流是通过定义良好的接口或者模块间的消息进行的，而不是通过共享数据存储进行。它与微服务最大的差别就是它的一切都发生在同一个进程中，因此同样不能低估最终的数据一致性问题。对于模块来说，最终的一致性问题可以是一个策略问题，或者你也可以仅将数据”逻辑地“分开存储在同一数据库内并仍然使用跨域事务。而对于微服务来说，这个问题别无选择：必须保证最终的一致性。

## 何时微服务才适用于你的团队？

那么何时迁移到微服务架构才合适呢？到目前为止，我们主要关注的是如何通过模块化来解决复杂性问题。对于这一点，微服务与模块化应用都可以做到，只不过各有所难。

当你的团队有如同 Google 或者 Netflix 般的规模的时候，拥抱微服务是毋庸置疑的。你有能力去建立你自己的平台与工具库，并且工程师的数量排除了任何使用单体式解决方案的可能。但是大多数的组织都达不到这个规模。即使你认为你的公司有朝一日将成为一个市值十亿美元的独角兽，在刚起步时使用模块化的单体式应用也无伤大雅。

另一个拆分微服务的理由是：不同的服务在实现上更适合使用不同的技术栈。那么，你必须有足够的规模来吸引人才以解决这些迥然不同的技术栈，并支持这些平台的运行。

微服务还可以做到独立部署系统的不同部分，这在大多数模块化平台中很难（甚至不可能）实现。隔离部署增加了系统的弹性与容错能力。此外，每个微服务的缩放特性可以是不同的，可以部署不同的微服务以匹配硬件。模块化的单体式应用可以进行水平缩放，但是只能将所有模块捆绑在一起同时进行拓展。虽然你可以通过这种方法得到很多好处，但这可能并不是最好的解决方案。

## 总结
 
总之，最好的方案就是找到一个折中的点。这两种方案都有可取之处，需要根据实际环境、组织和应用本身进行选择。既然你可以在之后迁移成微服务架构，那为什么最开始不直接使用模块化应用呢？如果你之前就已经划分好了模块边界，那也就不需要再去拆分你的单体式应用了。甚至你还可以在模块内部搭建微服务架构。那么问题就变成了：为什么微服务一定要是“微”的呢？

即使你的应用刚从模块化应用转成微服务架构，服务也不必非得很“微”才具备可维护性。在服务中应用模块化原则能让它们在复杂度的可扩展性上超越通常的微服务。现在这份蓝图中既有微服务也有模块，减少架构中的服务的数量可以节约成本；而其中的模块可以像构建单体式应用一样，构建和扩展服务。

如果你追求模块化的好处，请确保自己不要自嗨进入一种“非微服务不可”的心态。探索你喜爱的技术栈中的同进程模块化功能或框架，你将会得到支持去真正的执行模块化设计，而不是仅靠着约定来避免“意大利面条式代码”。最后，请深思熟虑后再选择：你是否愿意接受引入微服务造成的复杂度成本。有的时候你别无选择，但更多的时候其实你可以找到更好的解决方案。

---

> [掘金翻译计划](https://github.com/xitu/gold-miner) 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.im) 上的英文分享文章。内容覆盖 [Android](https://github.com/xitu/gold-miner#android)、[iOS](https://github.com/xitu/gold-miner#ios)、[React](https://github.com/xitu/gold-miner#react)、[前端](https://github.com/xitu/gold-miner#前端)、[后端](https://github.com/xitu/gold-miner#后端)、[产品](https://github.com/xitu/gold-miner#产品)、[设计](https://github.com/xitu/gold-miner#设计) 等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://github.com/xitu/gold-miner)。
