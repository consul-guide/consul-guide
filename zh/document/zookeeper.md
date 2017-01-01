# 1.2.1 ZooKeeper,doozerd,etcd

ZooKeeper、doozerd和etcd在架构上都是类似的。它们三个都需要quorum数量的节点来运行（通常只是简单的使用大多数作为quorum数量）。它们都是强一致的，并暴露出多种可在程序内部通过客户端库使用的原语，以用来构建复杂的分布式系统。

在一个数据中心内部，Consul也使用server节点。在每个数据中心内，也需要quorum数量的Consul server来运行并提供强一致性。但是Consul拥有天生对多数据中心的支持，以及一个更多特性的gossip系统来连接server和client节点。

就K/V存储方面而言，所有这些系统都有大致相同的语义：读是强一致的，当出现网络分隔时，可用性被牺牲以换取一致性。但当这些系统用于更高级的用途时，它们的区别变得更加明显。

这些系统的原语对于构建服务发现系统是很有吸引力的，但需要强调的是这些特性是需要进一步构建的，。ZooKeeper及其它系统只提供了一个原始的K/V存储，需要应用开发者利用它自己去开发系统提供服务发现。而相反Consul则提供了一个自带的服务发现框架，从而减轻了开发工作。client只需要简单的注册服务，然后通过DNS或HTTP接口执行发现。其它的系统则需要一个自行开发的方案。

一个令人信服的服务发现系统应该具有健康检查并考虑失效的可能。如果节点失效了或服务挂掉了，那知道节点A提供了Foo服务是没用的。初级的系统设计使用心跳、间断的更新和TTL。但这些方案需要和节点的数量成线性关系的工作量，将命令在固定数量的server上执行。另外，失效检测的窗口至少为TTL的长度。

ZooKeeper提供了临时的节点作为K/V存储的一条记录，当client连接断开时记录被删除。

Consul使用了非常不同的架构用于健康检查。

Consul提供了对服务发现、健康检查、K/V存储和跨数据中心极好的支持。
