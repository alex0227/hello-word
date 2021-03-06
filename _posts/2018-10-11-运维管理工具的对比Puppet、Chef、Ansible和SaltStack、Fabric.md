---
layout: post
title:
categories:
description:
keywords:
---
我们发现分布式是一个发展的趋势，无论是大型网站的负载均衡架构还是大数据框架部署，以及云存储计算系统搭建都离不开多台服务器的连续部署和环境搭建。

当我们的基础架构是分散式或者基于云的，并且我们经常需要处理在大部分相同的服务器上频繁部署大致相同的服务时，我们就应该考虑自动化配置和维护了。

让用户极容易配置和维护数十台、数百台、乃至数千台服务器。
幸运的是，现在有很多运维管理工具是为此目的而设计的，它们能够让我们使用配方，模板，剧本（或者其他描述）来简化环境搭建中的自动化和编排，以提供标准，一致的部署。

我们现在面临的问题不是没有选择，而是选择太多。

所以在选择工具时，需要明确的是我们的需求，需要集中型的管理还是分布式的管理，还有环境的组成，一些工具以不同的语言编写，并且对特定操作系统或设置的支持可以不同。 最后确保我们选择的工具与我们的环境和我们的团队的特殊技能很好地融合在一起可以为我们节省很多精力。

下面我们分别来简单了解下这几种工具并且做对比。

Ansible

简介

官网https://www.ansible.com/
Ansible介绍视频： https://www.youtube.com/watch?v=iVWmbStE1MM



Ansible是用于在可重复的方式将应用程序部署到远程节点和配置服务器的开源工具。 它为您提供了使用推送模型设置推送多层应用程序和应用程序工件的通用框架，但如果愿意，您可以将其设置为主客户端。 Ansible是建立在playbooks，你可以应用于各种各样的系统部署你的应用程序。


这是Ansible Tower仪表板。

Ansible极其类似Salt，而不太类似Puppet或Chef。Ansible关注的重点是力求精简和快速，而且不需要在节点上安装代理软件。因此，Ansible通过SSH执行所有功能。Ansible基于Python;相比之下，Puppet和Chef基于Ruby。

Ansible可以通过Git软件库克隆，安装到Ansible主服务器上。安装完毕后，需要管理的节点被添加到Ansible配置环境，SSH授权密钥被附加到每个节点上，这与运行Ansible的用户有关。一旦完成了这步，Ansible主服务器可以通过SSH与节点进行通信，执行所有必要的任务。为了与默认情况下不允许根SSH访问的操作系统或发行版协同运行，Ansible接受sudo登录信息，以便在那些系统上以根用户的身份运行命令。

Ansible可以使用Paramiko(基于SSH2协议的Python实现)或标准SSH用于通信，不过还有一种加速模式，允许更快速、更大规模的通信。

针对确保服务在运行，或者触发更新和重新启动之类的简单任务，Ansible可以从命令行来运行，不需要使用配置文件。至于比较复杂的任务，Ansible配置通过名为Playbook的配置文件中的YAML语法来加以处理。Playbook还可以使用模板来扩展其功能。

Ansible有一大批模块，可用于管理各种系统以及亚马逊弹性计算云(EC2)和OpenStack等云计算基础设施。可以用几乎任何一种语言来编写自定义Ansible模块，只要模块输出是有效的JSON。

Ansible的Web用户界面以AnsibleWorks AWX的形式出现，但AWX与CLI并不直接联系在一起。这意味着，除非进行了同步过程，否则CLI里面的配置元素不会出现在Web用户界面中。你可以使用那个内置的同步工具，让两者保持一致，但需要按照预定计划运行同步工具。

何时使用

如果快速运行，方便对你很重要，我们不想把运维工具安装在远程节点或托管服务器代理商那里，那可以考虑Ansible。 Ansible专注于精简和快速，所以如果这些是我们的关键问题，可以考虑一下Ansible。

ansible比较适合做“一次性”的工作，例如，系统部署、应用发布、打补丁等等。
在企业中使用ansible，要注意以下几点：
1. 安全控制，简单来说就是避免用root用户来执行。
2. 控制好依赖 在写playbook的时候，控制好先后顺序和依赖关系。
3. 结果的收集和分析 因为一下子几百台机器一起干活，所以，就要自己写外置脚本，更好地收集ansible的操作结果，并且进行直观的汇总和展现。

价格

有免费的开源版本支持10个节点以内的集群，付费的Ansible Tower在$ 5,000每年（它给你多达100个节点）支付计划。

优点

基于SSH，因此不需要在远程节点上安装任何代理。
使用YAML语法易于学习。
Playbook结构简单，结构清晰。
具有可变注册功能，可使任务为以后的任务注册变量
比一些其他工具更加精简的代码库

缺点

不如基于其他编程语言的工具强大。
它的逻辑通过它的DSL，这意味着需要经常检查文档
即使是基本功能，也需要变量注册，这使得更简单的任务更复杂
内省很差。 很难看到playbooks里的变量的价值
输入，输出和配置文件的格式之间不一致
性能和速度有待加强

chef

简介

官网https://www.chef.io/
Chef介绍视频：https://www.youtube.com/watch?v=kDeRHgnuDzc


Chef是配置管理的开源工具，专注于开发方为它的用户群。 厨师作为主客户端模型运行，具有控制主人所需的单独的工作站。 它基于Ruby，用纯Ruby编写的大多数元素。 厨师的设计是透明的，并根据它给出的说明，这意味着你必须确保你的说明是清楚的。

Chef DashBoard

Chef的总体概念类似Puppet，因为在被管理的节点上安装有主服务器和代理软件，但实际部署又不一样。除了主服务器外，安装的Chef环境还需要工作站来控制主服务器。代理软件可以借助使用SSH来部署的knife工具从工作站加以安装，减轻了安装负担。之后，被管理的节点通过使用证书，完成与主服务器之间的验证。

Chef的配置离不开Git，所以对Chef运作而言，了解Git如何工作是先决条件。与Puppet一样，Chef同样基于Ruby，所以还需要了解Ruby。与Puppet一样，模块可以下载，也可以从头开始编写，可以在所需配置之后部署到被管理的节点。

与Puppet不一样，Chef还没有一项完善的推送功能，不过提供了测试版代码。这意味着需要配置代理软件，以便与主服务器进行联系，实际上不可能立即应用变更的内容。

企业版Chef的Web用户界面很实用，但不提供更改配置的功能。这个Web用户界面不如Puppet企业版来得全面，缺少报告及其他功能，但允许库存控制和节点组织。

与Puppet一样，Chef得益于一大批的模块和配置菜谱，那些模块和配置菜谱又高度依赖Ruby。由于这个原因，Chef非常适合注重开发的基础设施。

何时使用

考虑使用Chef的时候需要保证你会使用Git/Ruby，因为书写配置的时候你会用到的。 Chef非常适合以开发为中心的团队和环境。 它对于寻找一个更加成熟的解决方案的企业非常适合异构环境。

价格

有免费的开源版本，超出限制数量的节点价格为6/节点/月或6/节点/月或 6.75 /节点/月。

优点

丰富的模块和配置配方集合。
代码驱动的方法为您的配置提供更多的控制和灵活性。
围绕Git提供强大的版本控制功能。
“Knife”工具（使用SSH从工作站部署代理）减轻了安装负担。

缺点

如果你还不熟悉Ruby和过程编码，学习曲线是很陡峭的。
这不是一个简单的工具，这可能导致大的代码库和复杂的环境。
不支持推送功能。

Fabric

简介

官网http://www.fabfile.org/
Fabric介绍视频：https://www.youtube.com/watch?v=VmcGuKPpWH8  

Fabric是在应用程序部署精简SSH一个基于Python的工具。 它主要用于跨多个远程系统运行任务，但也可以使用插件扩展以提供更高级的功能。 Fabric将配置您的系统，执行系统/服务器管理，并自动部署您的应用程序。

何时使用

如果你只是刚刚接触部署自动化领域，Fabric是一个很好的起点。 如果你的环境涉及至少一点点Python，它是有帮助的。

价格

免费

优点

擅长部署以任何语言编写的应用程序。 它不依赖于系统架构，而是OS和包管
理器。
比这个领域的其他工具更简单，更容易部署
与SSH进行广泛集成，实现基于脚本的精简

缺点

Fabric是单点故障设置（通常是您运行部署的机器）
虽然它是用于在大多数语言中部署应用程序的一个很好的工具，但它需要Python运行，因此您的Fabric环境中必须至少有一个Python。

Puppet

简介

官网https://puppet.com/
Puppet介绍视频：https://www.youtube.com/watch?v=j8ImF23jZAg


Puppet是在全面配置管理空间长期工具之一。 它是一个开源工具，但考虑到它已经存在多久，它已经被良好的审查和部署在一些最大和最苛刻的环境中。 Puppet基于Ruby，但是使用更接近JSON的定制的域脚本语言（DSL）来在其中工作。 它作为主客户端设置运行，并使用模型驱动方法。 Puppet代码设计作为依赖关系列表，这可以使事情更容易或更混乱，这取决于您的设置。

Puppet企业仪表板

Puppet也许是四款工具中最深入人心的。就可用操作、模块和用户界面而言，它是最全面的。Puppet呈现了数据中心协调的全貌，几乎涵盖每一个运行系统，为各大操作系统提供了深入的工具。初始设置比较简单，只需要在需要加以管理的每个系统上安装主服务器和客户端代理软件。

命令行接口(CLI)简单直观，允许通过puppet命令下载和安装模块。然后，需要对配置文件进行更改，好让模块适合所需的任务;应接到指令的客户端与主服务器联系时，会更改配置文件，或者客户端通过立即触发更改配置文件的推送(push)来进行更改。

还有一些模块可以提供和配置云服务器实例和虚拟服务器实例。所有模块和配置都使用基于Ruby的Puppet专属语言或者Ruby本身构建而成，因而除了系统管理技能外，还需要编程专业知识。

Puppet企业版拥有最全面的Web用户界面，允许使用主服务器上的预制模块和菜谱(cookbook)，实时控制被管理的节点。Web用户界面很适合用于管理，但是不允许对模块进行诸多配置。报告工具非常完善，提供了详细信息，以便了解代理软件运行如何、已做出什么样的变更。

何时使用

Puppet是一个不错的选择，稳定性和成熟是你选择的关键因素。 它对于DevOps团队具有异构环境和技能范围的大型企业很有好处。

价格

Puppet可以使用免费开源版本，同时也提供收费企业版每年每节点$ 112与批量折扣付费商业企业版本。

优点

通过Puppet Labs建立良好的支持社区。
它有最成熟的接口，几乎在每个操作系统上运行。
简单的安装和初始设置。
在这个空间中最完整的Web UI。
强大的报告功能。

缺点

对于更高级的任务，您将需要使用基于Ruby的CLI（这意味着您必须理解Ruby）。
支持纯Ruby版本（而不是使用Puppet的定制DSL）正在缩减。
由于DSL和一个不专注于简单性的设计，Puppet代码库可能会变得庞大，笨重，难以在更高规模的组织中接纳新用户。
与代码驱动方法相比，模型驱动方法意味着更少的控制。

SaltStack

简介

官网https://saltstack.com/

SaltStack（或Salt）是一个基于命令行的工具，可以设置一个主客户端模式还是非集中模式。 Salt基于Python，提供了一种推送方法和一种与客户端通信的SSH方法。 Salt允许对客户端和配置模板进行分组，以简化对环境的控制。

Salt类似Ansible，因为它也是基于CLI的工具，采用了推送方法实现客户端通信。它可以通过Git或通过程序包管理系统安装到主服务器和客户端上。客户端会向主服务器提出请求，请求在主服务器上得到接受后，就可以控制该客户端了。

Salt可以通过普通的SSH与客户端进行通信，但如果使用名为minion的客户端代理软件，可以大大增强可扩展性。此外，Salt含有一个异步文件服务器，可以为客户端加快文件服务速度，这完全是Salt注重高扩展性的一个体现。

与Ansible一样，你可以直接通过CLI，向客户端发出命令，比如启动服务或安装程序包;你也可以使用名为state的YAML配置文件，处理比较复杂的任务。还有“pillar”，这些是放在集中地方的数据集，YAML配置文件可以在运行期间访问它们。

你可以直接通过CLI，向客户端请求配置信息，比如内核版本或网络接口方面的详细信息。只要使用名为“grain”的库存元素，就可以描述客户端;这样一来，管理员可以轻松向某一种类型的服务器发出命令，不需要依赖已配置群组。比如说，只要使用一个CLI命令，你就可以向运行某个内核版本的每个客户端发送命令。

与Puppet、Chef和Ansible一样，Salt也提供了大量的模块，以处理特定的软件、操作系统和云服务。自定义模块可以用Python或PyDSL来编写。除了Unix管理外，Salt的确提供Windows管理功能，但它还是更擅长管理Unix和Linux系统。

Salt的Web用户界面Halite非常新，功能不如其他系统的Web用户界面来得全面。它提供了事件日志和客户端状态的视图，能够在客户端上运行命令，但除此之外乏善可陈。

Salt的最大优点在于可扩展性和弹性。你可以有多个级别的主服务器。上游主服务器可以控制下游主服务器及其客户端。另一个优点在于对等系统，让客户端可以向主服务器提出问题，然后主服务器从其他服务器得到答案，提供全面信息。如果需要在实时数据库中查询数据，以便完成客户端的配置，这个优点就很方便。

何时使用

Salt是一种不错的选择，它对系统管理员很有好处，因为它的可用性比较好。

价格

免费的开源版本，这是基于每年每节点订阅的基础上SaltStack Enterprise版本。 具体定价不在他们的网站上列出（只有“联系我们”链接），但其他人报告每个节点每年150美元起。

优点

一旦您完成设置阶段，即可轻松组织和使用。
他们的DSL是功能丰富，并不是逻辑和状态所必需的。
输入，输出和配置非常一致 - 所有YAML。
内省是非常好的。 很容易看到Salt内发生了什么。
强大的社区。
在主模型中具有minions和层级层次的高可扩展性和弹性。

缺点

很难设置和挑选新用户。
文档在介绍层面很难理解。
Web UI比空间中的其他工具的Web UI更新，更不完整。
对非Linux操作系统不是很好的支持。
SaltStack介绍视频：https://www.youtube.com/watch?v=TQjE2I8CrzQ  
Ansible vs. Chef vs. Fabric vs. Puppet vs. SaltStack

选用Puppet、Chef、Ansible还是Salt，Fabric




  工具|  语言|  架构|  协议
:---:|:---:
  Puppet|  Ruby|  C/S|  HTTP
  Chef| Ruby|  C/S|  HTTP
Ansible|  Python|  无Client|  SSH
  Saltstack|  Python|  C/S(可无Client)|  SSH/ZMQ/RAET




使用哪种配置管理或部署自动化工具将取决于您的环境的需求和首选项。 Chef和Puppet是一些较老的，更成熟的选择，使它们适合于那些重视成熟度和稳定性而不是简单性的大型企业和环境。
Ansible和SaltStack是那些寻求快速和简单的解决方案，而在不需要支持quirky功能或大量的操作系统的环境中工作的人的好选择。
Fabric是小型环境和那些寻求更低人力和入门级解决方案的好工具。

Puppet和Chef会吸引广大开发人员和注重开发的公司，而Salt和Ansible极其适合系统管理员的要求。Ansible的简洁界面和可用性非常迎合系统管理员的想法;而在拥有许多Linux和Unix系统的公司，Ansible运行起来一开始就快速又轻松。

Salt是四款工具中最漂亮最稳健的;与Ansible一样，它也会博得系统管理员的芳心。Salt拥有高扩展性和强大功能，唯一的软肋就是Web用户界面。

Puppet是这四款工具中最成熟的，因为web管理界面不错从可用性的角度来看恐怕也最容易上手，不过竭力建议你对Ruby要有深入了解。Puppet不如Ansible或Salt来得精简，配置起来有时会变得错综复杂。对异构环境来说，Puppet是最稳妥的选择，但是你可能会发觉Ansible或Salt比较适合更庞大或更一致的基础设施。

Chef拥有稳定的、精心设计的布局，虽然它在原始功能方面远未达到Puppet的水平，但这是款功能非常强大的解决方案。要是管理员缺乏丰富的编程经验，Chef学起来可能最困难，但它也许最适合注重开发的管理员和开发部门。

Ansible或者Puppet

哪个最好

存在即是合理，起码是存在3年以上的；没有最好的，只有合适的，你说白菜和青菜哪个最好？

一般来说，有两种配置管理：
1. 推模式
2. 拉模式

两种模式有不同的擅长点，有不同的使用场景。

拉模式 (puppet)

这种模式主张去中心化的设计思路，典型代表 puppet。一般实现多为在每个节点上部署 agent，定时获取该节点的配置信息，根据配置信息配置本节点。如果一次配置失败了，那么下次继续尝试，直到地老天荒。这个节点完全不管其他节点的执行情况，一心只顾做好自己的事情。

所以它比较适合这种场景：
对配置何时生效不敏感，不关心的。你知道它总是会生效的，可能是下一分钟，也可能是下个小时，但是对你没什么影响。
节点和节点之间不需要协作的。比如这种 场景就不合适： A 先升级，然后 B 在升级。
即使某一次拉取信息失败了，下一次还能补上，所以比较适合跨地域的大规模部署。

推模式 (ansible)

推模式有一个中心节点，用于将最新的配置信息推到各个节点上，典型代表 ansible。很明显，推模式的瓶颈就在中心节点，如果同一时间有 10000 个节点需要更新配置，那么中心节点如何稳定的工作就比较有学问。

它比较适合这种场景：
对配置生效的时间敏感，十分关心。必须让他们即可生效，如果不生效，立马要采取行动让他们生效。
配置生效的顺序十分关心和敏感。比如需要这10个节点一起生效，或者按照依次生效。

执行顺序的区别

配置生效顺序在 puppet 和 ansible 之间有很大的不同，理解这些区别对于如何选择配置管理至关重要。下面举例说明两者的区别：

假设现在有三个节点 host1，2，3，它们需要执行配置更新的三个步骤：1，2，3（圆圈表示），我们用图来表示三个节点上的三个步骤执行顺序是如何的。

puppet 的执行顺序如图所示，因为拉模式不管不顾其他节点的执行情况，专心致志完成自己的任务，所以节点之间的更新步骤完全随机。这种更新方法很“分布式”，跑得快的节点可以很快地跑完全程，不用等其他慢的小伙伴，没有短板，没有瓶颈。

ansible 的执行顺序如上图所示。由于有一个中心节点控制所有机器的配置更新。只有一个步骤在 所有 节点上完成后，才会继续下一个步骤，所以三个节点的执行顺序会很整齐。这种更新方法目的很明确，就是要“整齐”，跑的快的节点需要等待其他小伙伴完成这个步骤后，大家在进行下一个步，谁都不允许自己先执行下一步。

不过 ansible 2.0 中新增加了一种 playbook 的执行策略：free strategy，它允许某个 playbook 不再“整齐”的执行，而是像 puppet 一样，每个节点 try best 的跑到终点，而不用管其他节点执行的情况如何。

如何选择

虽然 puppet 和 ansible 有一定的功能重叠，比如都支持配置文件模版，装包，启动服务，等等，但是仍然有区别。如何选择？只能说“视情况而定”。

如果你的团队已经有合适的配置管理，并且已经能否覆盖 90% 的场景，那么很幸运，不需要换配置管理工具。
如果如 puppet 顺序图所示的情况你不能接受，那么最好选择 ansible。
如果一开始选择了 ansible，并且对“整齐”不敏感，不关心，那么可以使用 free strategy，或者用 ansible-pull [3]。
如果已经使用 puppet 好多年，并且对于“不整齐”执行没什么顾虑，不关心，不敏感，那么继续使用 puppet。
如果已经使用 puppet 好多年，并且对于“不整齐”有顾虑，很敏感，但是又不想抛弃已有的 puppet modules，因为它们已经很稳定了，那么可以选择使用 Mcollective 改造，或者用 ansible 调用 puppet，或者两者一起用。

Atlassian 公司（做 Jira 的公司）已经给我们介绍了经验
我们用 Ansible 创建 AWS 虚拟机，将它们放进 DNS，和负载均衡后面，然后用 Puppet 管理这些虚拟机的操作系统的配置。当它们完成后，再使用 Ansible 管理应用层软件的部署和升级。

SaltStack或Ansible

SaltStack与Ansible都是Python写的而且较新，网上评论也很好。

1、是否需要每台机器部署agent（客户端）  
很多选用ansible的朋友，都是因为agentless这个原因，觉得要维护agent很麻烦。
而一些使用saltstack比较顺的朋友，觉得这个问题无所谓，agent出问题的概率有，但不高。
其实ansible也支持agent的方式，即所谓的“pull”的模式，就是通过一个客户端去拉取要执行的任务。

2、大规模并发的能力
这方面的对比已经比较多了，因为实现机制的差异，也导致saltstack在这方面是占优的。不过对于几十台-200台规模的兄弟来讲，ansible的性能也可接受。
注：前期调研的大多数都是中下企业，服务器规模一般不超过200台，所以对这个问题不算太看重。如果一次操作的机器过千台，可能还是用saltstack效率更高一些。

ansible的执行架构已经有所优化，采用基于MQ的agent机制，已支持比较大规模（1000-10000台）的服务器的批量自动化运维。这样，在这种存在大规模运维的需求的客户这里，也可以应用丰富的ansible的Playbook了。

3、二次开发扩展的能力
ansible和saltstack都是基于python的，而python在运维开发这个圈子里接受度还是非常高的，二次开发的人员相对也好招。
这也是这两个工具相对于puppet和chef更容易被接受度原因，这两个曾经的主流工具都是基于ruby，而现在ruby的活跃度越来越低了，要招人也不容易。
ansible和saltstack都具备很好的二次开发扩展能力，可以写YAML编排。

4、开源社区的对接
在github上，ansbile有18300多颗星，salt有6700多颗星。
直接按关键字搜索，ansible的相关项目也更多一些。
这些指标虽然不能直接说明什么，但很多技术人员会关注自己所使用的技术的活跃度。
一般来说，越活跃的开源项目，得到的关注会更高些，功能完善和问题解决的效率也会更高。

5、学习的门槛
从第一次使用来讲，ansible的部署配置会更简单一些。
从官方文档的质量来看，saltstack就比ansible要好一些。
从国内的中文资料来说，ansbile和saltstack好像各有2-3本中文书。   这两家的国内用户组也分别在做一些技术资料翻译的工作。

6、操作界面的友好程度
试用过Ansible的Tower，但实在是不喜欢这种操作习惯，只能说勉强可用。 saltstack的没仔细用过，但看过朋友搭建的环境，感觉官方的UI还可以，基本够用了。Ansible的最初设计定位就不是一个完整的运维管理系统，因此官方UI粗糙些也在预料之中。

7、第三方工具的丰富程度
ansibe有一个galaxy站点：Ansible Galaxy
这个站点集合了3000多个第三方开发的Role/Playbook。
salt也有一些预先写好的Formulas（Formulas are pre-written Salt States）
官方地址：Salt Formulas
github地址：Salt Stack Formulas · GitHub

目前已有的Formulas大概在200个左右，比ansible galaxy少了一个数量级，不过大部分常用软件也覆盖到了。

8、现有用户使用的规模
根据rightscale的调研报告：
Ansible在2015年有10%的用户选用，而2016年有20%的用户选用。     Saltstack在2015年有6%的用户选用，而2016年有9%的用户选用。

9、对Windows支持的友好程度
ansible对windows的支持简直不忍直视，agentless只是对于linux的，windows要安装bug修复补丁，powershell还要3.0，还要安装python。还不如salt方便。salt有对windows的支持，但是也不是很好。

我们自己目前是用的Ansible，但正在结合我们自己的DevOps平台，重新给Ansible开发一套WEB UI。

前一段时间用了saltstack，免不得要谈一下他们的优缺点。两者都是安装和使用都非常方便的批量管理软件。
1、salt要安装agent；ansible不需要，通过ssh连接，省掉装agent的事。
2、salt在server端要启进程；ansible不需要，但这都无所谓差不多。
3、salt与ansible都有模块，可使用任意语言开发模块。
4、salt与ansible都使用yaml语言格式编写剧本。
ansible由于走的是ssh,所以它有认证的过程，以及加密码的过程，这使得ansible非常慢，不适用于大规模环境（指上千台）。
为什么我放弃salt呢，首先服务器不多（百台左右），其次，salt的master端与minion端TCP连接经常断开，导致有时执行命令时会漏机器，这简直让我忍无可忍。听说最新版的salt好了很多，但由于公司系统是定制的，安装软件特别麻烦（15M的系统，解决依赖就是个大问题），我还是选择了ansible。

参考链接:
http://blog.takipi.com/deployment-management-tools-chef-vs-puppet-vs-ansible-vs-saltstack-vs-fabric/  
http://blog.51cto.com/liqilong2010/1850617
https://www.gaott.info/which-one-ansible-or-puppet/
https://www.zhihu.com/question/22707761
---------------------
作者：张小凡vip
来源：CSDN
原文：https://blog.csdn.net/zzq900503/article/details/80143740
版权声明：本文为博主原创文章，转载请附上博文链接！
