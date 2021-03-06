Operator 能力阶段：

最近这一两年，对中间件的更高级的玩法，社区也在不断思考和发展。其中很有代表性的一个技术就是 Operator。那 Operator 是什么？这里简单的总结一下，Operator 是一种基于 K8s 的申明式的能力，是扩展 K8s 生态和能力的方式。Operator 可以理解成是一个框架，或一个 SDK。具体使用这个技术完成什么样的业务能力，由使用者和需求来决定。

举例，基于 Operator 可以完成自己产品的安装和管理、产品能力的开发、服务的统一管理和上下架能力等等。那在有了 Operator 之后，中间件的方案会有怎样的变化呢？首先，最大的特点是程序化、自动化能力可以更加的灵活，可以工程化的设计方案、研发方案、发布方案。每一个中间件都是非常复杂的一套系统，解决特殊领域的问题。既能发挥出中间件复杂业务能力，还能更加自动化、可自治化、可迭代化，可以说 Operator 是一个很好的选择。

前面几个阶段，都是基于手动、模版、编排的能力，完成方案设计和部署能力。在使用 Operator 之后，整体的工作会有很大的区别。首先需要在之前的基础上，熟悉中间件本身的能力，熟悉基本的编排。其次需要精通 Operator 开发的能力，来支持开发出一个健壮稳定的 Operator，这是非常重要的。

大致的玩法可以总结为以下几个步骤：

1. 构建基础镜像，既然有开发能力，那推荐的玩法是可以基于配置文件，或者中间件的 API/SDK 来扩展其能力，所以更加倾向于选择基础镜像，这样可以借助社区的能力；

2. 设计 Operator 实现的目标和范围，也就是具体的需求是怎样的。不同的需求，实现的方案是不同的。社区有一个 Operator 的成熟度模型，不同的Level 的 Operator 要完成的目标是不一样的。

3. 根据需求设计实现的方案；

4. 定义 Operator 的业务模型 CRD；

5. 开发 Operator 的核心 Controller 来完成业务能力；

6. 最终以部署文档的方式和运维文档交付，也可以是一套标准的基于 Operator 开发的中间件产品为交付物。

02
常见玩法
YAML 方式：

这种玩法是熟悉 K8s YAML 的运维人员，很喜欢的一种方式。特点是直观，部署方式固定，而且简单。常见的操作是，能够以传统运维的方式来运维，因为运维人员关注和操作的单元，还是以物理机/虚拟机为运维点。正常使用交付物的方式是，先学习文档，然后规划机器，以及每个机器的角色，然后根据文档进行安装，最后交付出一套一套的中间件集群给到最终客户。这个玩法适合不需要太多自动化、智能化的诉求的方案，快速、简单、交付难度小、交付周期小。

helm 方式：

这种玩法只是在 YAML 的基础上，做了一个封装，以及可以稍微标准化一点的交付方案。这个看习惯。但是和 YAML 不同的地方是，helm 还提供了一套管理机制、打包机制、发布版本机制，能够以制品的方式，将制作好的 chart 包，上架到一些仓库中，例如 harbor 是有 chart 的仓库。还有一些产品和平台，也支持基于 helm charts 的服务上下架能力。

Operator 方式：

这种玩法一般是大规模的，对中间件部署有很高的要求，特别是中间件能力中的可开发能力、可自愈能力、可封装能力、可系统化能力、可云原生能力。随着这部分的需求和方案越来越普及，这种玩法也将逐步成为主流方式。让中间件本身成为云原生的一部分，让企业的这部分基础设施能力可 IT 化、可兑现化。