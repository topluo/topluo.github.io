title: 【系统架构师修炼之道】（7）：绪论——FEA框架
date: 2015-10-14 13:38:41
categories:
tags:
---

**FEA**全称联邦企业架构，在1999年由预算管理办公室(OMB)根据“联邦政府组织架构框架”的基本精神，提出了“联邦政府组织架构”(Federal Enterprise Architecture,FEA),并此成立了“FEA项目管理办公室(FEAPMO)”。FEA的目的是将整个联邦政府的所有机构的错综复杂的关系当成一个大型的组织系统，根据信息化和电子政务的基本规律，大胆地规划网络环境下的全新的联邦政府行政管理体系。


<!-- more -->

**FEA**是一种基于业务与绩效，用于某级政府的跨部门的绩效改进框架。它分为5个参考模型，共同提供了联邦政府的业务，绩效与技术的通用定义和架构。

## FEA组织架构


![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-24/66167724.jpg)

### 绩效参考模型(FEA-PRM)
**PRM**是为整个联邦政府提供一般结果与产出指标的绩效评测框架。它为政府提供了一种对照理想的FEA并缩短现实与理想之间差距的方法。同时它能够让政府机构从战略高度更高地管理政府业务。PRM提供了政府机构用于实现其业务规划目标的通用的绩效结果与方法集合。
**PRM**是联邦政府提供衡量跨部门行为计划的各服务构建的效率必备的工具。

PRM 框架图

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/58772773.jpg)

PRM 内容组织结构

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/95431168.jpg)

1. 测量域（Measurement Area）：是PRM框架针对各测量指标在最高层次进行组织的概念，与在机构和方案层面制定的性能目标直接相关。PRM中包含如下六种测量域：

 - 任务与业务结果（Mission and Business Results）
 - 客户结果（Custom Results）
 - 流程与活动（Processes and Activities）
 - 人力资本（Human Capital）
 -  技术（Technology）
 - 其他固定资产（Other Fixed Assets）

2. 测量分类（Measurement Category）：在每个测量域中根据不同的属性而归纳出的不同组合。例如，在任务与业务结果（Mission and Business Results）测量域中就包含了市民服务（Services for Citizens）、服务交付支持（Support Delivery of Services）和政府资源管理（Management of Government Resources）这三个测量分类。
3. 测量分组（Measurement Grouping）：在测量分类中根据测量指标的类型而进行的进一步分组。
4. 测量指标（Measurement Indicator）：具体的测量标准，例如用户满意度百分比等。

### 业务参考模型(FEA-BRM)

BRM是描述联邦政府机构所实施的但与具体的政府机构无关的业务框架。它构成FEA的基础内容。该模型描述了联邦政府内部运行与对外向公民提供服务的业务流程，而这些业务流程与联邦政府的某个具体的委，办，局没有任何关系。由于它避开了政府部门具体部门的定义，所有它可以有效地促进政府各机构之间的协作。BRM包含四个业务区，39条业务线和153项子功能。


组织结构图

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/55842414.jpg)


业务参考模型中定义了如下四种业务领域

 - 市民服务（Services for Citizens）业务领域：此服务是政府的最终目标，是其对外部公民所能提供的各种服务。
 - 交付模式（Mode of Delivery）业务领域：包含了政府为了达成目标所采用的机制。
 - 服务交付支持（Support Delivery of Services）业务领域：包含了用于支持政府运行的各种关键政策和计划与管理基础。
 - 政府资源管理（Management of Government Resources）业务领域：包含为了支持联邦政府的有效率运行而针对所有领域资源的管理功能。


业务参考模型

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/54775775.jpg)


### 服务构件参考模型(FEA_SRM)

构件前面也有提到，就是一个可以被复用的业务过程或者功能。SRM是一种业务驱动的功能架构，它根据业务目标改进方式进而对服务架构进行分类。SRM基于横向业务领域，与具体的业务职能无关。因此，它能够实现业务重用，提高以及优化业务服务。

组织结构

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/29782565.jpg)

1.服务领域（Service Domain）

服务领域为用于支持机构流程和应用的各种服务和能力提供了一份高层次视图。根据所面向的业务的不同，服务组件参考模型中将服务领域定义为如下几种

  - 客户服务（Custom Services）
  - 流程自动化（Process Automation）
  - 业务管理服务（Business Management Services）
  - 数字资产服务（Digital Asset Services）
  - 业务分析服务（Business Analytical Services）
  - 后台服务（Back Office Services）
  - 支持服务（Support Services）


SRM内容


![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/7380308.jpg)


2.服务类型（Service Type）
服务类型对服务领域进行了进一步的细化，它为具体的服务组件提供了更为详细的分类上下文。

3.组件（Component）
为业务提供信息管理能力的构建块，即自包含的业务流程或服务，其预定功能是通过业务或技术接口来对外提供的。


### 数据参考模型(FEA-DRM)


**DRM**用来描述项目与业务运行过程中的数据和信息，描述政府与客户，选民和业务伙伴之间信息交换和作用的类型。DRM会按照大家容易接受的方式对联邦信息加以分类，从而比较容易确定那些重复建设的数据资源，并由此实现政府机构之间的信息共享。

内容组织结构

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/49290014.jpg)

- 数据描述（Data Description）：提供对于数据的统一描述方法，从而支持数据的发现和共享。
- 数据上下文（Data Context）：采用某种分类法对数据进行归类，从而便于数据的发现。此外，数据上下文还使得定义一个利益共同体的权威数据资产（authoritative data assets）成为可能。
- 数据共享（Data Sharing）：支持数据的访问和交换，其中数据访问是指单次性的特定请求（例如对于数据的查询），而数据交换指的是在不同团体之间经常性发生的针对于固定模式或需求的数据的往来交互事务（例如库存部门和审核部门之间经常需要对库存中的货物信息进行核对，虽然每次交互的货物信息的内容有所不同，但是其对于用于描述货物信息的数据模型却是早已确定好了的)。

抽象模型

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/44067409.jpg)

### 技术参考模型(FEA-TRM)

**TRM**是一种分级的技术构架，用于描述传输服务构建与提高服务性能的技术支持方式。它规定了一套技术方案(如：[firstGov](https://www.usa.gov/),[payGov](https://pay.gov/public/home)以及24个总统有限的电子政务计划所采用的成熟技术与工具)

内容组织

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/99280109.jpg)


 - 服务领域（Service Area）：代表用于支持服务组件的安全构建、交互和交付的一个技术层。
 - 服务分类（Service Category）：将技术和标准按照其所服务的业务或技术功能进行进一步的细分。每个服务分类包含若干服务标准。
 - 服务标准（Service Standard）：定义了支持某一服务分类的具体技术和标准。为了帮助各机构将自身技术情况映射到技术参考模型之上，在OMB的这份参考模型文档中，很多标准除了详细定义外还采用了说明性的应用或技术作为实例。


具体内容

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-26/85069857.jpg)




## FEA的特点

- 业务规划是核心

在FEA模型体系中，业务参考模型(BRM)是其基础，决定后面的绩效参考模型(RPM),服务构件参考模型(SRM),数据参考模型(DRM),技术参考模型(TRM)的具体评估内容。

- 注意流程整合，淡化部门概念，强调业务协调统一

FEA不是部门概念，它是大政府的概念。

- 系统关联性

在FEA中，业务，绩效，数据，技术是相互分离，同时又通过服务构件相联系。

- 应用信息管理系统的理论思路和技术方法

BRM对业务区，业务线和子功能的划分；SRM对服务域，服务类型和服务构件的划分，应用BSP对公共业务内容和数据库建设项目的划分。

FEA一直在更新中。。。

---

感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/14/the-pragmatic-sa-7/

创作时间：2015-9-1

更新时间：2015-10-16
