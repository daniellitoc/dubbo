# 源码解析

* https://github.com/daniellitoc/dubbo-Research/tree/readme/dubbo-config/dubbo-config-spring
* https://github.com/daniellitoc/dubbo-Research/tree/readme/dubbo-config/dubbo-config-api
* https://github.com/daniellitoc/dubbo-Research/tree/readme/dubbo-common
* https://github.com/daniellitoc/dubbo-Research/tree/readme/dubbo-rpc/dubbo-rpc-api

## dubbo-config-spring

* Spring解析配置源码分析

    * dubbo:application
    * dubbo:module
    * dubbo:registry
    * dubbo:monitor
    * dubbo:provider
    * dubbo:consumer
    * dubbo:protocol
    * dubbo:service
    * dubbo:method
    * dubbo:argument
    * dubbo:reference
    * dubbo:annotation

* Spring Bean源码分析

    * ServiceBean
    * ReferenceBean
    * AnnotationBean

## dubbo-config-api

* Dubbo Config相关类setter/getter源码分析

    * ApplicationConfig
    * ModuleConfig
    * RegistryConfig
    * MonitorConfig
    * ProviderConfig
    * ConsumerConfig
    * ProtocolConfig
    * MethodConfig
    * ArgumentConfig
    * AnnotationBean
    * ServiceConfig
    * ReferenceConfig

* ServiceConfig源码分析

    * public synchronized void export()
    * public synchronized void unexport()

* ReferenceConfig源码分析

    * public synchronized T get()
    * public synchronized void destroy()

## dubbo-common

* ExtensionLoader源码分析

    * ExtensionLoader（ExtensionFactory）
    * ExtensionLoader（非ExtensionFactory）

* Wrapper源码分析

## dubbo-rpc-api

### Dubbo SPI和对应的实现分析（provider/consumer启动、调用）

* com.alibaba.dubbo.common.compiler.Compiler

    * com.alibaba.dubbo.common.compiler.support.AdaptiveCompiler
    * com.alibaba.dubbo.common.compiler.support.JavassistCompiler（未）
    * com.alibaba.dubbo.common.compiler.support.JdkCompiler（未）

* com.alibaba.dubbo.rpc.ProxyFactory

    * com.alibaba.dubbo.rpc.proxy.javassist.JavassistProxyFactory
    * com.alibaba.dubbo.rpc.proxy.AbstractProxyInvoker
    * com.alibaba.dubbo.rpc.proxy.InvokerInvocationHandler
    * com.alibaba.dubbo.rpc.proxy.wrapper.StubProxyFactoryWrapper

* com.alibaba.dubbo.common.threadpool.ThreadPool

    * com.alibaba.dubbo.common.threadpool.support.limited.LimitedThreadPool

* com.alibaba.dubbo.common.serialize.Serialization

    * com.alibaba.dubbo.common.serialize.support.hessian.Hessian2Serialization

* com.alibaba.dubbo.rpc.cluster.ConfiguratorFactory

    * com.alibaba.dubbo.rpc.cluster.configurator.override.OverrideConfiguratorFactory
    * com.alibaba.dubbo.rpc.cluster.configurator.override.OverrideConfigurator
    * com.alibaba.dubbo.rpc.cluster.configurator.absent.AbsentConfiguratorFactory
    * com.alibaba.dubbo.rpc.cluster.configurator.absent.AbsentConfigurator

* com.alibaba.dubbo.rpc.Protocol

* com.alibaba.dubbo.rpc.protocol.ProtocolFilterWrapper

    * com.alibaba.dubbo.rpc.Filter
    * com.alibaba.dubbo.rpc.filter.ActiveLimitFilter（未）
    * com.alibaba.dubbo.rpc.filter.ExecuteLimitFilter（未）
    * com.alibaba.dubbo.rpc.filter.TokenFilter（未）
    * com.alibaba.dubbo.rpc.filter.GenericFilter（未）
    * com.alibaba.dubbo.rpc.filter.GenericImplFilter（未）

* com.alibaba.dubbo.rpc.protocol.ProtocolListenerWrapper

    * com.alibaba.dubbo.rpc.ExporterListener
    * com.alibaba.dubbo.rpc.InvokerListener
    * com.alibaba.dubbo.rpc.listener.ListenerExporterWrapper
    * com.alibaba.dubbo.rpc.listener.ListenerInvokerWrapper

* com.alibaba.dubbo.rpc.support.MockProtocol

    * com.alibaba.dubbo.rpc.support.MockInvoker

* com.alibaba.dubbo.rpc.protocol.injvm.InjvmProtocol

    * com.alibaba.dubbo.rpc.protocol.AbstractExporter
    * com.alibaba.dubbo.rpc.protocol.injvm.InjvmExporter
    * com.alibaba.dubbo.rpc.protocol.AbstractInvoker
    * com.alibaba.dubbo.rpc.protocol.injvm.InjvmInvoker

* com.alibaba.dubbo.rpc.protocol.http.HttpProtocol

    * com.alibaba.dubbo.rpc.protocol.http.HttpProtocol.InternalHandler

* com.alibaba.dubbo.rpc.protocol.dubbo.DubboProtocol

    * com.alibaba.dubbo.rpc.protocol.dubbo.DubboProtocol.ExchangeHandler
    * com.alibaba.dubbo.rpc.protocol.dubbo.DubboExporter
    * com.alibaba.dubbo.remoting.exchange.Exchangers
    * com.alibaba.dubbo.rpc.protocol.dubbo.DubboInvoker
    * com.alibaba.dubbo.rpc.protocol.dubbo.ReferenceCountExchangeClient
    * com.alibaba.dubbo.rpc.protocol.dubbo.LazyConnectExchangeClient

* com.alibaba.dubbo.remoting.exchange.Exchanger

    * com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchanger
    * com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeServer
    * com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeClient
    * com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeHandler
    * com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeChannel
    * com.alibaba.dubbo.remoting.Transporters

* com.alibaba.dubbo.remoting.Transporter

    * com.alibaba.dubbo.remoting.transport.netty.NettyTransporter
    * com.alibaba.dubbo.remoting.transport.netty.NettyServer
    * com.alibaba.dubbo.remoting.transport.netty.NettyClient
    * com.alibaba.dubbo.remoting.exchange.support.header.HeartbeatHandler

* com.alibaba.dubbo.remoting.Dispatcher

    * com.alibaba.dubbo.remoting.transport.dispatcher.all.AllDispatcher

* com.alibaba.dubbo.remoting.Codec2

    * com.alibaba.dubbo.rpc.protocol.dubbo.DubboCountCodec（未）
    * com.alibaba.dubbo.rpc.protocol.dubbo.DubboCodec（未）

* com.alibaba.dubbo.rpc.cluster.Cluster

    * com.alibaba.dubbo.rpc.cluster.support.wrapper.MockClusterWrapper
    * com.alibaba.dubbo.rpc.cluster.support.FailoverCluster
    * com.alibaba.dubbo.rpc.cluster.support.FailfastCluster
    * com.alibaba.dubbo.rpc.cluster.support.AvailableCluster
    * com.alibaba.dubbo.rpc.cluster.support.MergeableCluster
    * com.alibaba.dubbo.rpc.cluster.directory.AbstractDirectory
    * com.alibaba.dubbo.rpc.cluster.directory.StaticDirectory
    * com.alibaba.dubbo.registry.integration.RegistryDirectory
    * com.alibaba.dubbo.rpc.cluster.support.wrapper.MockClusterInvoker
    * com.alibaba.dubbo.rpc.cluster.support.AbstractClusterInvoker
    * com.alibaba.dubbo.rpc.cluster.support.FailoverClusterInvoker
    * com.alibaba.dubbo.rpc.cluster.support.FailfastClusterInvoker
    * com.alibaba.dubbo.rpc.cluster.support.MergeableClusterInvoker（未）

* com.alibaba.dubbo.rpc.cluster.Router

* com.alibaba.dubbo.rpc.cluster.LoadBalance

    * com.alibaba.dubbo.rpc.cluster.loadbalance.LeastActiveLoadBalance（未）
    * com.alibaba.dubbo.rpc.cluster.loadbalance.RoundRobinLoadBalance（未）

* com.alibaba.dubbo.registry.integration.RegistryProtocol
* com.alibaba.dubbo.registry.RegistryFactory

    * com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistryFactory
    * com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry

* com.alibaba.dubbo.remoting.zookeeper.ZookeeperTransporter

    * com.alibaba.dubbo.remoting.zookeeper.curator.CuratorZookeeperTransporter（未）

* com.alibaba.dubbo.remoting.telnet.TelnetHandler（未）
* com.alibaba.dubbo.common.status.StatusChecker（未）

# 简述

    支持多协议和多注册中心
    内部对象的生命周期及各个组件之间的交互主要通过ExtensionLoader + URL完成（包括自适应扩展）
    通过Spring的Handler解析配置成BeanDefinition，添加到Spring的上下文的Register中，主要的逻辑是set相关属性和进行配置校验
    对provider暴漏的服务（不包含范化引用），会使用javassist实现反射机制（分析类方法字段信息，最终提供invokerMethod方法，内部使用if else机制进行直接调用）代替java的动态代理
    provider的启动流程（dubbo协议、zk注册中心、指定服务场景）
        启动nettyserver
            接收客户端请求并解序列化为Response对象并传递给业务线程，执行ProxyFactory创建的ProxyInvoker对象
                对于oneway的请求方式，不需要返回值，所以直接不需要等待服务方法执行业务方法完毕
                对于twoway请求，在业务线程中，需要等待服务方法执行完毕然后写入到对方的Channel中
            接收心跳事件，保持长连接
        注册到zk
            注册当前服务方法信息到providers节点下，主要包括ip、port、interface、methods、version、group相关信息
            对configuration节点添加监听事件，用于实现动态配置，注册到providers节点下的服务是baseUrl，后续的每次改变，会整合configuration下的一类信息，根据协议override/absent整合baseUrl实现动态配置变更
        注册到zk和追加到nettyserver中，都是通过在Protocol中，通过ExtensionLoader提供的Wrapper机制，解析一个Url完成
        默认情况还会暴漏一个injvm协议，就是将可调用对象加入到一个进程级别的map属性中，这样，如果是相同进程内的consumer调用，不走RPC方式
    consumer的启动流程
        注册到zk
            注册当前信息到consumers节点下，主要包括ip、port、interface、methods、version、group相关信息
            对route、providers、configuration节点添加监听事件，实现路由转发和动态provider
        对每个provider调用封装为一个的Invoker，对一组Invoker封装为Cluster，提供failfast、failover机制
        对远程的Invoker创建nettyclient
            发送心跳事件，保持长连接
            超时参数provider可以提供，不过是提供给consumer的默认参数，provider本身无超时逻辑验证
            consumer整合timeout参数，每次调用的时候，对调用信息处理成Request参数，并生成一个requestId，然后创建一个Future实例，并加入到map属性中，序列化Request对象并写入到provider的Channel中，然后后台启动一个线程，定时扫描所有map元素，进行超时检测
        同步等待方式就是依赖Future.get逻辑，因此提供了超时重试机制，对于异步调用，超时重试机制不起作用
        Callback的实现机制是，将consumer端的callback注册成provider（非服务），然后provider执行完逻辑后，在调用consumer的callback完成，是通过两次RPC完成的请求
        mock和injvm，都是Invoker的一种实现
