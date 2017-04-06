# com.alibaba.dubbo.common.compiler.Compiler
## 默认值：javassist
## 实现
    adaptive=com.alibaba.dubbo.common.compiler.support.AdaptiveCompiler         // 无法自动生成
    jdk=com.alibaba.dubbo.common.compiler.support.JdkCompiler
    javassist=com.alibaba.dubbo.common.compiler.support.JavassistCompiler
### com.alibaba.dubbo.common.compiler.support.AdaptiveCompiler
    public static void setDefaultCompiler(String compiler)                  // 设置默认编译器，由ApplicationConfig指派
### com.alibaba.dubbo.common.compiler.support.JavassistCompiler
### com.alibaba.dubbo.common.compiler.support.JdkCompiler

# com.alibaba.dubbo.rpc.ProxyFactory
## 自适应参数名称：proxy，默认值：javassist
## 实现
    stub=com.alibaba.dubbo.rpc.proxy.wrapper.StubProxyFactoryWrapper
    javassist=com.alibaba.dubbo.rpc.proxy.javassist.JavassistProxyFactory
### com.alibaba.dubbo.rpc.proxy.javassist.JavassistProxyFactory
    public <T> Invoker<T> getInvoker(T proxy, Class<T> type, URL url)       // 服务端调用、客户端mock调用
        调用Wrapper.getWrapper(proxy.getClass().getName().indexOf('$') < 0 ? proxy.getClass() : type)获取Wrapper实例
        创建new AbstractProxyInvoker<T>(proxy, type, url)实例，重写方法doInvoke(T proxy, String methodName, Class<?>[] parameterTypes, Object[] arguments)，调用wrapper.invokeMethod
    public <T> T getProxy(Invoker<T> invoker, Class<?>[] interfaces)        // 客户端调用
        返回Proxy.getProxy(interfaces).newInstance(new InvokerInvocationHandler(invoker))实例
#### com.alibaba.dubbo.rpc.proxy.AbstractProxyInvoker
    public AbstractProxyInvoker(T proxy, Class<T> type, URL url)                             // 服务端
        如果proxy等于null，抛异常
        如果type等于null，抛异常
        如果proxy不是type的实例，抛异常
        赋值到对应的成员变量
    public Result invoke(Invocation invocation) throws RpcException
        调用doInvoke(proxy, invocation.getMethodName(), invocation.getParameterTypes(), invocation.getArguments())方法，使用RpcResult包装并返回，如果产生InvocationTargetException异常，返回new RpcResult(e.getTargetException())，其他异常抛出RpcException(Failed to invoke remote proxy method + invocation.getMethodName()..., e)
#### com.alibaba.dubbo.rpc.proxy.InvokerInvocationHandler
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable        // 客户端
        如果当时声明method方法的class等于Object
            返回method.invoke(invoker, args)
        如果methodName等于toString，方法参数类型的个数等于0
            返回invoker.toString()
        如果methodName等于hashCode，方法参数类型的个数等于0
            返回invoker.hashCode()
        如果methodName等于equals，方法参数类型的个数等于1
            返回invoker.equals(args[0])
        返回invoker.invoke(new RpcInvocation(method, args)).recreate()      // recreate方法的逻辑是，如果exception属性不等于null，抛异常，否则返回result属性
### com.alibaba.dubbo.rpc.proxy.wrapper.StubProxyFactoryWrapper         // 包装类：无论proxy选择javassist还是jdk，都会使用StubProxyFactoryWrapper对实际实例进行包装
    public <T> Invoker<T> getInvoker(T proxy, Class<T> type, URL url) throws RpcException
        调用proxyFactory的getInvoker(proxy, type, url)
    public <T> T getProxy(Invoker<T> invoker) throws RpcException           // stub参数生效的地方，只针对客户端
        设置proxy变量等于proxyFactory.getProxy(invoker)
        如果invoker.getInterface()不等于GenericService.class
            设置stub变量等于invoker.getUrl().getParameter("stub")
            如果stub变量不等于null、"false"、"0"、"null"、"N/A"
                设置serviceType变量等于invoker.getInterface()
                如果stub变量等于"true"或者"default"
                    设置stub变量等于serviceType + "Stub"
                加载stub变量类，设置给stubClass变量
                校验stubClass变量值必须是serviceType变量值的子类，如果不是，抛异常IllegalStateException(The stub implemention class...)
                使用存在interfaceClass属性参数的构造方法创建实例，方法参数是proxy，返回包装后的实例设置给proxy
                如果invoker.getUrl()的dubbo.stub.event参数等于true             // 暴漏client端服务给server端
                    根据invoker.getUrl()创建新的实例设置给url变量
                    添加dubbo.stub.event.methods和StringUtils.join(Wrapper.getWrapper(proxy.getClass()).getDeclaredMethodNames(), ",")到url变量的参数中
                    添加isserver和false到url变量的参数中
                    执行protocol.export(proxyFactory.getInvoker(proxy, (Class)invoker.getInterface(), url))
        返回proxy变量

# com.alibaba.dubbo.common.threadpool.ThreadPool
## 自适应参数名称：threadpool，默认值：fixed
## 实现
    limited=com.alibaba.dubbo.common.threadpool.support.limited.LimitedThreadPool
### com.alibaba.dubbo.common.threadpool.support.limited.LimitedThreadPool
    public Executor getExecutor(URL url)                            // 没有keepAliveTime参数控制
        url参数中的threadname参数，默认值Dubbo，控制线程名称
        url参数中的corethreads参数，默认值0，控制最小线程数
        url参数中的threads参数，默认值200，控制最大线程数
        url参数中的queues参数，默认为0，控制队列大小，等于0，使用无界队列，否则使用有界队列

# com.alibaba.dubbo.common.serialize.Serialization
## 自适应参数名称：serialization，默认值：hessian2
## 实现
    hessian2=com.alibaba.dubbo.common.serialize.support.hessian.Hessian2Serialization
    fastjson=com.alibaba.dubbo.common.serialize.support.json.FastJsonSerialization
### com.alibaba.dubbo.common.serialize.support.hessian.Hessian2Serialization
    public ObjectOutput serialize(URL url, OutputStream out) throws IOException
        返回new Hessian2ObjectOutput(out)
    public ObjectInput deserialize(URL url, InputStream is) throws IOException
        返回new Hessian2ObjectInput(is)

# com.alibaba.dubbo.rpc.cluster.ConfiguratorFactory
## 自适应参数名称：protocol
## 实现
    override=com.alibaba.dubbo.rpc.cluster.configurator.override.OverrideConfiguratorFactory
    absent=com.alibaba.dubbo.rpc.cluster.configurator.absent.AbsentConfiguratorFactory
### com.alibaba.dubbo.rpc.cluster.configurator.override.OverrideConfiguratorFactory
    public Configurator getConfigurator(URL url)
        返回new OverrideConfigurator(url)实例
#### com.alibaba.dubbo.rpc.cluster.configurator.override.OverrideConfigurator
    public URL configure(URL url)           // 删除baseUrl的category、check、dynamic、enabled、application、side、~开头的参数，合并到url中，按照baseUrl的参数为准，相对于url是override模式
        如果configuratorUrl属性等于null或者configuratorUrl.getHost()等于null或者url参数等于null或者url.getHost()等于null，返回url参数
        否则
            如果configuratorUrl.getHost()等于0.0.0.0或者configuratorUrl.getHost()等于url.getHost()
                分别获取configurationUrl属性和url参数中的application参数，默认值为其username属性
                如果configApplication变量等于null或者等于*或者等于currentApplication变量
                    如果configuratorUrl属性的port属性等于0或者和url参数的port属性相同
                        设置condtionKeys变量等于new HashSet<String>()实例，添加category、check、dynamic、enabled
                        遍历configuratorUrl属性的参数列表
                            如果参数key以~开头或者等于application或者等于side
                                加入参数key到condtionKeys变量中
                                如果value不等于null并且不等于*并且不等于url参数中的指定key的value值
                                    返回url参数
                        基于configurationUrl，移除condtionKeys中的参数，生成一个新的url
                        添加新url变量的参数到url参数中，并返回url参数

        返回url参数
### com.alibaba.dubbo.rpc.cluster.configurator.absent.AbsentConfiguratorFactory
    public Configurator getConfigurator(URL url)
        返回new AbsentConfigurator(url)实例
#### com.alibaba.dubbo.rpc.cluster.configurator.absent.AbsentConfigurator
    public URL configure(URL url)           // 删除baseUrl的category、check、dynamic、enabled、application、side、~开头的参数，合并到url中，按照url的参数为准，相对于url是absent模式
        如果configuratorUrl属性等于null或者configuratorUrl.getHost()等于null或者url参数等于null或者url.getHost()等于null，返回url参数
        否则
            如果configuratorUrl.getHost()等于0.0.0.0或者configuratorUrl.getHost()等于url.getHost()
                分别获取configurationUrl属性和url参数中的application参数，默认值为其username属性
                如果configApplication变量等于null或者等于*或者等于currentApplication变量
                    如果configuratorUrl属性的port属性等于0或者和url参数的port属性相同
                        设置condtionKeys变量等于new HashSet<String>()实例，添加category、check、dynamic、enabled
                        遍历configuratorUrl属性的参数列表
                            如果参数key以~开头或者等于application或者等于side
                                加入参数key到condtionKeys变量中
                                如果value不等于null并且不等于*并且不等于url参数中的指定key的value值
                                    返回url参数
                        基于configurationUrl，移除condtionKeys中的参数，生成一个新的url
                        判断url参数中是否存在新url变量的参数，添加不存在的，并返回url参数

        返回url参数

# com.alibaba.dubbo.rpc.Protocol
## 自适应默认值：dubbo                    // 不包含自适应参数，唯一特殊的一个，通过url.getProtocol()或url.getProtocol() == pre ? (%s) : url.getProtocol()或url.getProtocol() == null ? dubbo : url.getProtocol()获取
## 实现
    filter=com.alibaba.dubbo.rpc.protocol.ProtocolFilterWrapper             // 1（包装）
    listener=com.alibaba.dubbo.rpc.protocol.ProtocolListenerWrapper         // 2（包装）
    injvm=com.alibaba.dubbo.rpc.protocol.injvm.InjvmProtocol                // 3
    registry=com.alibaba.dubbo.registry.integration.RegistryProtocol        // 3（包装）
    http=com.alibaba.dubbo.rpc.protocol.http.HttpProtocol                   // 3
    dubbo=com.alibaba.dubbo.rpc.protocol.dubbo.DubboProtocol                // 3
    mock=com.alibaba.dubbo.rpc.support.MockProtocol
### com.alibaba.dubbo.rpc.protocol.ProtocolFilterWrapper
    public <T> Exporter<T> export(Invoker<T> invoker) throws RpcException
        如果invoker.getUrl()的protocol属性等于registry，调用protocol.export(invoker)并返回           // 是注册中心url
        否则                                                                                     // 非注册中心url
            调用buildInvokerChain(invoker, "service.filter", "provider")返回wrapperInvoker，然后调用protocol.export(wrapperInvoker)
    public <T> Invoker<T> refer(Class<T> type, URL url) throws RpcException
        如果url参数的protocol属性等于registry，调用protocol.refer(type, url)并返回                   // 是注册中心url
        否则                                                                                     // 非注册中心url
            调用protocol.refer(type, url)返回invoker，然后调用buildInvokerChain(invoker, "reference.filter", "consumer")
    private static <T> Invoker<T> buildInvokerChain(final Invoker<T> invoker, String key, String group)
        调用ExtensionLoader.getExtensionLoader(Filter.class).getActivateExtension(invoker.getUrl(), key, group)获取所有匹配Filter
        返回一个Invoker，当调用其invoke方法，会执行所有匹配Filter的invoke方法
#### com.alibaba.dubbo.rpc.Filter
##### com.alibaba.dubbo.rpc.filter.ActiveLimitFilter
##### com.alibaba.dubbo.rpc.filter.ExecuteLimitFilter
##### com.alibaba.dubbo.rpc.filter.TokenFilter
##### com.alibaba.dubbo.rpc.filter.GenericFilter
##### com.alibaba.dubbo.rpc.filter.GenericImplFilter
### com.alibaba.dubbo.rpc.protocol.ProtocolListenerWrapper
    public <T> Exporter<T> export(Invoker<T> invoker) throws RpcException
        如果invoker.getUrl()的protocol属性等于registry，调用protocol.export(invoker)并返回           // 是注册中心url
        否则                                                                                     // 非注册中心url
            调用ExtensionLoader.getExtensionLoader(ExporterListener.class).getActivateExtension(invoker.getUrl(), "exporter.listener"))返回所有匹配ExporterListener
            调用protocol.export(invoker)，返回exporter
            返回new ListenerExporterWrapper<T>(exporter, listeners);
    public <T> Invoker<T> refer(Class<T> type, URL url) throws RpcException
        如果url参数的protocol属性等于registry，调用protocol.refer(type, url)并返回                    // 是注册中心url
        否则                                                                                     // 非注册中心url
            调用ExtensionLoader.getExtensionLoader(InvokerListener.class).getActivateExtension(url, "invoker.listener"))返回所有匹配InvokerListener
            调用protocol.refer(type, url)，返回invoker
            返回new ListenerInvokerWrapper<T>(invoker, listeners);
#### com.alibaba.dubbo.rpc.ExporterListener
#### com.alibaba.dubbo.rpc.InvokerListener
#### com.alibaba.dubbo.rpc.listener.ListenerExporterWrapper
    public ListenerExporterWrapper(Exporter<T> exporter, List<ExporterListener> listeners)  // 服务端
        赋值到成员变量
        遍历listeners参数，调用其listener.exported(this)方法
    public Invoker<T> getInvoker()
        返回exporter.getInvoker()
    public void unexport()
        调用exporter属性的unexport()
        遍历listeners属性，调用其listener.unexported(this)方法
#### com.alibaba.dubbo.rpc.listener.ListenerInvokerWrapper
    public ListenerInvokerWrapper(Invoker<T> invoker, List<InvokerListener> listeners)      // 客户端
        赋值到成员变量
        遍历listeners参数，调用其listener.referred(invoker)方法
    public Result invoke(Invocation invocation) throws RpcException
        返回invoker.invoke(invocation)
    public void destroy()
        调用invoker属性的destroy()
        遍历listeners属性，调用其listener.destroyed(invoker)方法
### com.alibaba.dubbo.rpc.support.MockProtocol
    public <T> Exporter<T> export(Invoker<T> invoker) throws RpcException
        抛出UnsupportedOperationException异常                                                 // 服务端
    public <T> Invoker<T> refer(Class<T> type, URL url) throws RpcException
        返回new MockInvoker<T>(url)实例                                                       // 客户端
#### com.alibaba.dubbo.rpc.support.MockInvoker
    public Result invoke(Invocation invocation) throws RpcException                          // 客户端
        设置mock变量等于url属性的参数invocation.getMethodName()+".mock"值
        如果mock变量等于null
            设置mock变量等于url属性的参数mock值
        如果mock变量等于null
            抛异常RpcException(new IllegalAccessException(mock can not be null. url))
        解码mock值
        如果mock等于return ，创建new Result()实例，设置value属性为null并返回
        如果mock以return 开头，截取return 后边的部分，按照invocation需要的结果，创建new Result()实例，设置value属性并返回
        如果mock以throw开头，截取throw后边部分，如果内容为空，返回RpcException(" mocked exception for Service degradation. ")，否则创建对应的异常实例，使用RpcException包装并返回
        否则
            判断mocks属性中是否存在指定mock的Invoker实例，如果存在，调用invoker.invoke(invocation)
            否则
                如果mock等于"true"或者"default"，加载url.getServiceInterface() + "Mock"类，否则加载mock类
                调用其无参构造方法创建实例，执行proxyFactory.getInvoker(mockObject, (Class<T>)serviceType, url)返回结果，添加mock和实例到mocks属性中，返回invoker.invoke(invocation)
### com.alibaba.dubbo.rpc.protocol.injvm.InjvmProtocol
    public <T> Exporter<T> export(Invoker<T> invoker) throws RpcException
        返回new InjvmExporter<T>(invoker, invoker.getUrl().getServiceKey(), exporterMap)
    public <T> Invoker<T> refer(Class<T> serviceType, URL url) throws RpcException
        返回new InjvmInvoker<T>(serviceType, url, url.getServiceKey(), exporterMap)
#### com.alibaba.dubbo.rpc.protocol.AbstractExporter
    public void unexport()                                                      // 服务端
        如果unexported属性等于true，退出
        设置unexported属性等于true
        调用invoker属性的destroy()
#### com.alibaba.dubbo.rpc.protocol.injvm.InjvmExporter
    InjvmExporter(Invoker<T> invoker, String key, Map<String, Exporter<?>> exporterMap)     // 服务端
        赋值到成员变量
        调用exporterMap.put(key, this)
    public void unexport()
        调用super.unexport()                      // getInvoker().destroy()
        调用exporterMap.remove(key)
#### com.alibaba.dubbo.rpc.protocol.AbstractInvoker
    public AbstractInvoker(Class<T> type, URL url)                              // 客户端
        赋值到成员变量
    public Result invoke(Invocation inv) throws RpcException
        如果destroyed属性等于true，抛异常RpcException(Rpc invoker for service...)
        设置inv参数中的invoker属性等于this
        如果attachment属性不等于null并且个数大于0，追到到inv参数的attachments属性中
        如果RpcContext.getContext().getAttachments()不等于null，追到到inv参数的attachments属性中
        判断url属性的参数中inv.getMethodName() + ".async"是否等于true，默认为false，如果为true，追加async和true到inv参数的attachments属性中
        如果是异步请求(url参数中inv.getMethodName() + ".invocationid.autoattach"等于true或attachments属性中async等于true或url参数中inv.getMethodName() + ".async"等于true或)
            如果inv参数的attachments属性中不存在id，追加id和全局自增id到inv参数的attachments属性中
        返回doInvoke(invocation)，如果产生InvocationTargetException，返回new ResultSet(e)，如果产生RpcException，判断是否是业务产生的，如果是返回new ResultSet(e)，否则抛上去，如果是其他异常，返回new RpcResult(e)
    public void destroy()
        如果destroyed属性等于true，则退出
        设置destroyed属性等于true
#### com.alibaba.dubbo.rpc.protocol.injvm.InjvmInvoker
    InjvmInvoker(Class<T> type, URL url, String key, Map<String, Exporter<?>> exporterMap)  // 客户端
        赋值到成员变量
    public Result doInvoke(Invocation invocation) throws Throwable
        调用InjvmProtocol.getExporter(exporterMap, getUrl())返回exporter变量              // exporterMap是进程级别的共享，存储了exporter
        如果exporter变量等于null，返回new RpcException(Service [ + key + ] not found.)
        否则返回exporter.getInvoker().invoke(invocation)                                 // 本地执行
### com.alibaba.dubbo.rpc.protocol.http.HttpProtocol                // 实际是Spring HttpInvoker
    public <T> Exporter<T> export(final Invoker<T> invoker) throws RpcException
        判断exporterMap属性中是否包含指定uri，如果包含，返回其value
        否则
            调用doExport(proxyFactory.getProxy(invoker), invoker.getInterface(), invoker.getUrl())返回Runnable实例
            创建new AbstractExporter<T>(invoker)实例，添加uri和实例到exporterMap中并返回，重写unexport方法，额外从exporterMap中移除指定uri，同时如果Runnable不等于null，调用其run方法
    protected <T> Runnable doExport(final T impl, Class<T> type, URL url) throws RpcException
        如果serverMap中不包含指定url的ip:port实例，如果不包含，调用httpBinder.bind(url, new InternalHandler())返回HttpServer并添加到serverMap中
        创建HttpInvokerServiceExporter实例，设置接口和实现，调用其初始化方法，添加path和实例到skeletonMap中，返回Runnable实例，run方法内执行skeletonMap.remove(path)
    public <T> Invoker<T> refer(final Class<T> type, final URL url) throws RpcException
        执行proxyFactory.getInvoker(doRefer(type, url), type, url)返回实例，设置给target变量
        创建new AbstractInvoker<T>(type, url)实例，添加到invokers属性中并返回，重写的doInvoke(Invocation invocation)方法内部，方法内部调用tagert.invoke(invocation)并得到返回值，然后一些识别处理
    protected <T> T doRefer(final Class<T> serviceType, final URL url) throws RpcException
        创建new HttpInvokerProxyFactoryBean()实例，设置给httpProxyFactoryBean变量
        设置httpProxyFactoryBean变量的serviceUrl和interface
        根据url参数的client参数值选择客户端，根据url的timeout和connection.timeout设置读取超时和连接超时
        调用httpProxyFactoryBean.afterPropertiesSet()
        返回httpProxyFactoryBean.getObject()
#### com.alibaba.dubbo.rpc.protocol.http.HttpProtocol.InternalHandler
    public void handle(HttpServletRequest request, HttpServletResponse response)
        通过request参数中的requestURI属性从skeletonMap属性中获取对应的HttpInvokerServiceExporter实例，设置给exporter变量
        如果request方法不等于POST，返回500
        调用RpcContext.getContext().setRemoteAddress(request.getRemoteAddr(), request.getRemotePort())
        调用exporter.handleRequest(request, response)
### com.alibaba.dubbo.rpc.protocol.dubbo.DubboProtocol
    private ExchangeHandler requestHandler = new ExchangeHandlerAdapter()
        public Object reply(ExchangeChannel channel, Object message) throws RemotingException
            如果message参数不是Invocation类型，抛异常RemotingException(channel, Unsupported request...)
            否则
                设置path变量等于inv.getAttachments().get("path")
                设置port变量等于channel.getLocalAddress().getPort()
                如果inv参数的attachments属性中dubbo.stub.event值等于true，设置port变量等于channel.getRemoteAddress().getPort()
                如果channel.getRemoteAddress()和channel.getUrl()中的port和ip相等，并且inv参数的attachments属性中dubbo.stub.event值不等于true
                    设置path变量等于path + "." + inv.getAttachments().get("callback.service.instid")
                    添加_isCallBackServiceInvoke和true到inv变量的attachments属性中
                通过path、port、inv.getAttachments().get("version")、inv.getAttachments().get("group")生成serviceKey
                如果exporterMap属性中不包含serviceKey，抛异常RemotingException(channel, Not found exported service...)
                设置invoker变量等于exporter.getInvoker()
                如果inv参数的attachments属性中_isCallBackServiceInvoke值等于true
                    获取invoker.getUrl()中methods参数值，使用,分隔，判断inv.getMethodName()是否在其中，如果不在，返回null
                调用RpcContext.getContext().setRemoteAddress(channel.getRemoteAddress())
                返回invoker.invoke(inv)
        public void received(Channel channel, Object message) throws RemotingException
            如果message参数是Invocation类型实例，调用reply((ExchangeChannel) channel, message)
        public void connected(Channel channel) throws RemotingException
            执行invoke(channel, "onconnect")
        public void disconnected(Channel channel) throws RemotingException
            执行invoke(channel, "ondisconnect")
        private void invoke(Channel channel, String methodKey)
            调用createInvocation(channel, channel.getUrl(), methodKey)返回实例，设置给invocation变量，如果invocation变量不等于null，调用received(channel, invocation)
        private Invocation createInvocation(Channel channel, URL url, String methodKey)
            获取url参数中的methodKey参数值，如果参数值等于null，返回null
            否则
                创建new RpcInvocation(method, new Class<?>[0], new Object[0])实例，设置给invocation变量
                添加path和url.getPath()到invocation变量的attachments属性中
                添加group和url.getParameter("group")到invocation变量的attachments属性中
                添加interface和url.getParameter("interface")到invocation变量的attachments属性中
                添加version和url.getParameter("version")到invocation变量的attachments属性中
                如果url参数的dubbo.stub.event参数值等于true（默认为false），添加添加dubbo.stub.event和true到invocation变量的attachments属性中
                返回invocation
    public <T> Exporter<T> export(Invoker<T> invoker) throws RpcException
        根据invoker.getUrl()生成serviceKey      // 如：com.alibaba.dubbo.demo.DemoService:20880
        创建new DubboExporter<T>(invoker, serviceKey, exporterMap)实例，添加serviceKey和实例到exporterMap属性中
        如果invoker.getUrl()的参数中的dubbo.stub.event参数值等于true，默认为false，并且is_callback_service参数值等于false，默认为false
            如果invoker.getUrl()的参数中的dubbo.stub.event.methods参数值不等于null
                添加到serviceKey和dubbo.stub.event.methods参数值到stubServiceMethodsMap属性中
        // 如：dubbo://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=32862&sayHello.executes=2&side=provider&timestamp=1457251126597
        //      protocol属性等于dubbo
        //      host属性等于192.168.31.246
        //      port属性等于20880
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        如果invoker.getUrl()的参数中的isserver参数值等于true（默认值为true）
            判断serverMap属性中是否存在指定invoker.getUrl().getAddress()的value值，
            如果不存在，调用createServer(url)返回实例，添加address和实例到serviceMap属性中
            如果存在，调用server.reset(invoker.getUrl)
        返回exporter
    private ExchangeServer createServer(URL url)
        如果url参数的参数中不存在channel.readonly.sent，添加channel.readonly.sent和true到url参数的参数中
        如果url参数的参数中不存在heartbeat，添加heartbeat和60 * 1000到url参数的参数中
        获取url参数的server参数，如果等于null，使用netty，如果参数值不等于null，并且ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(str)等于false，抛异常RpcException(Unsupported server type...)
        添加codec和dubbo到url参数的参数中
        获取url参数的client参数，验证在ExtensionLoader.getExtensionLoader(Transporter.class).getSupportedExtensions()中是否存在，如果不存在，抛异常RpcException(Unsupported client type...)
        // 如：dubbo://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&channel.readonly.sent=true&codec=dubbo&dubbo=2.0.0&generic=false&heartbeat=60000&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=32862&sayHello.executes=2&side=provider&timestamp=1457251126597
        //      protocol属性等于dubbo
        //      host属性等于192.168.31.246
        //      port属性等于20880
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        调用Exchangers.bind(url, requestHandler)得到ExchangeServer实例，并返回
    public <T> Invoker<T> refer(Class<T> serviceType, URL url) throws RpcException
        获取url参数中的connections参数值，默认为0，设置给connections变量
        如果connections等于0，设置connections等于1，设置service_share_connect变量等于true，否则设置为false
        创建new ExchangeClient[connections]数组并遍历
            如果service_share_connect等于true
                设置key变量等于url.getAddress()
                获取referenceClientMap属性中对应key的实例，设置给client变量
                如果client变量不等于null，并且不是关闭的，增加其refenceCount属性个数，设置数组元素为client，如果是关闭的，从referenceClientMap中移除指定key
                添加codec和dubbo到url参数的参数中
                添加heartbeat和60 * 1000到url参数的参数中
                根据url参数中的client、server参数值，默认值为netty，验证ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(str)是否存在，如果不存在，抛异常RpcException(Unsupported client type...)
                如果url参数的lazy参数等于true（默认为false），创建new LazyConnectExchangeClient(url ,requestHandler)实例，否则创建Exchangers.connect(url ,requestHandler)实例，设置给exchagneclient变量
                创建new ReferenceCountExchangeClient(exchagneclient, ghostClientMap)实例，设置给client变量
                添加key和client到referenceClientMap属性中
                从ghostClientMap属性中移除key
                设置数组元素等于client变量
            否则
                添加codec和dubbo到url参数的参数中
                添加heartbeat和60 * 1000到url参数的参数中
                根据url参数中的client、server参数值，默认值为netty，验证ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(str)是否存在，如果不存在，抛异常RpcException(Unsupported client type...)
                如果url参数的lazy参数等于true（默认为false），创建new LazyConnectExchangeClient(url ,requestHandler)实例，否则创建Exchangers.connect(url ,requestHandler)实例，设置给exchagneclient变量
                设置数组元素等于exchagneclient变量
        创建new DubboInvoker<T>(serviceType, url, exchangeClients, invokers)实例，设置给invoker变量
        添加invoker变量到invokers属性中，并返回invoker变量
#### com.alibaba.dubbo.rpc.protocol.dubbo.DubboExporter
    public DubboExporter(Invoker<T> invoker, String key, Map<String, Exporter<?>> exporterMap)                  // 服务端
        赋值到成员变量
    public void unexport()
        super.unexport()
        exporterMap.remove(key)
#### com.alibaba.dubbo.remoting.exchange.Exchangers
    public static ExchangeServer bind(URL url, ExchangeHandler handler) throws RemotingException                // 服务端
        如果url参数的参数中不存在codec参数，添加codec和exchange到url参数的参数中
        获取url参数中的exchanger参数值，默认值header，设置给type变量，调用ExtensionLoader.getExtensionLoader(Exchanger.class).getExtension(type).bind(url, handler)并返回
    public static ExchangeClient connect(URL url, ExchangeHandler handler) throws RemotingException             // 客户端
        如果url参数的参数中不存在codec参数，添加codec和exchange到url参数的参数中
        获取url参数中的exchanger参数值，默认值header，设置给type变量，调用ExtensionLoader.getExtensionLoader(Exchanger.class).getExtension(type).connect(url, handler)并返回
#### com.alibaba.dubbo.rpc.protocol.dubbo.DubboInvoker
    public DubboInvoker(Class<T> serviceType, URL url, ExchangeClient[] clients, Set<Invoker<?>> invokers)      // 客户端
        赋值到成员变量
        设置version属性等于url参数中的version参数值，默认为0.0.0
        获取url参数中的interface参数值，添加interface和参数值到attachment属性中
        获取url参数中的group参数值，添加interface和参数值到attachment属性中
        获取url参数中的token参数值，添加interface和参数值到attachment属性中
        获取url参数中的timeout参数值，添加interface和参数值到attachment属性中
    protected Result doInvoke(final Invocation invocation) throws Throwable                                     // invocation转换为rpc字节流
        获取invocation参数的methodName设置给methodName变量
        添加path和url.getPath()到invocation参数的attachments属性中
        添加version和version属性到invocation参数的attachments属性中
        如果clients属性的长度等于1，获取第一个元素，否则通过index递增取摸方式，获取clients中的某个元素，设置给currentClient变量
        如果invocation参数的attachments属性中存在async并且等于true，设置给isAsync变量
        否则
            从url属性的参数中获取methodName + async和async的结果，默认为false，设置给isAsync变量
        如果invocation参数的attachments属性中存在return并且等于false，设置true给isOneway变量
        否则
            从url属性的参数中获取methodName + return和return的结果，默认为true，如果最后结果为true，设置isOneway变量等于false，否则等于true
        获取url属性的参数中timeout参数，默认为1000，设置给timeout变量
        如果isOneway变量等于true
            从url属性的参数中获取methodName + sent和sent的结果，默认为false
            执行currentClient.send(inv, isSent)
            执行RpcContext.getContext().setFuture(null)
            返回new RpcResult()实例
        如果isAsync变量等于true
            执行currentClient.request(inv, timeout)方法返回future变量
            执行RpcContext.getContext().setFuture(new FutureAdapter<Object>(future))
            返回new RpcResult()实例
        否则
            执行RpcContext.getContext().setFuture(null)
            返回currentClient.request(inv, timeout).get()
        如果上述send、request产生TimeoutException，返回RpcException(2, ...)，如果产生RemotingException，返回RpcException(1, ...)
#### com.alibaba.dubbo.rpc.protocol.dubbo.ReferenceCountExchangeClient      // 计数作用，以及关闭情况（幽灵连接）的预防措施
#### com.alibaba.dubbo.rpc.protocol.dubbo.LazyConnectExchangeClient         // url的connect.lazy.initial.state控制初始状态，默认为true，特点是在发送请求时执行Exchangers.connect(url, requestHandler)初始化client

# com.alibaba.dubbo.remoting.exchange.Exchanger
## 自适应参数名称：exchanger，默认值：header
## 实现
    header=com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchanger
### com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchanger
    public ExchangeServer bind(URL url, ExchangeHandler handler) throws RemotingException
        返回new HeaderExchangeServer(Transporters.bind(url, new DecodeHandler(new HeaderExchangeHandler(handler))))
    public ExchangeClient connect(URL url, ExchangeHandler handler) throws RemotingException
        返回new HeaderExchangeClient(Transporters.connect(url, new DecodeHandler(new HeaderExchangeHandler(handler))))
#### com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeServer
    添加heartbeat功能
        心跳检测周期使用url的heartbeat参数，默认为0，表示不启动heartbeat，非0会启动一个定时任务
            根据heartbeat和Channel中的READ_TIMESTAMP和WRITE_TIMESTAMP与当前时间的时间差比对，判断是否发送Request请求
        心跳超时使用url的heartbeat.timeout参数，默认为heartbeat * 3，
            根据heartbeat.timeout和Channel中的READ_TIMESTAMP和WRITE_TIMESTAMP与当前时间的时间差比对，判断是否关闭连接（如果是Client端，尝试重连）
    服务端关闭的时候，向所有持有的Channel发送请求，设置request.setData("R")
    重置heartbeat功能
        重新设置heartbeat和heartbeat.timeout，先停止之前执行的任务，然后启动一个新任务
    和所有和当前server连接的客户端进行心跳检测
#### com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeClient
    添加heartbeat功能
        心跳检测周期使用url的heartbeat参数，默认为0，表示不启动heartbeat，非0会启动一个定时任务
            根据heartbeat和Channel中的READ_TIMESTAMP和WRITE_TIMESTAMP与当前时间的时间差比对，判断是否发送Request请求
        心跳超时使用url的heartbeat.timeout参数，默认为heartbeat * 3，
            根据heartbeat.timeout和Channel中的READ_TIMESTAMP和WRITE_TIMESTAMP与当前时间的时间差比对，判断是否关闭连接（如果是Client端，尝试重连）
    服务端关闭的时候，向所有持有的Channel发送请求，设置request.setData("R")
    重置heartbeat功能
        重新设置heartbeat和heartbeat.timeout，先停止之前执行的任务，然后启动一个新任务
    和server进行心跳检测
#### com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeHandler
    public void connected(Channel channel) throws RemotingException
        追加READ_TIMESTAMP或WRITE_TIMESTAMP
        使用com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeChannel包装channel参数，调用handler.connected(exchangeChannel)
    public void disconnected(Channel channel) throws RemotingException
        追加READ_TIMESTAMP或WRITE_TIMESTAMP
        使用com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeChannel包装channel参数，调用handler.disconnected(exchangeChannel)
    public void sent(Channel channel, Object message) throws RemotingException
        追加WRITE_TIMESTAMP，使用com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeChannel包装channel参数，调用handler.sent(exchangeChannel, message)
        如果message参数是Request类型
            调用DefaultFuture.sent(channel, (Request) message)            // 如果FUTURES存在request.getId()，设置value的sent属性等于当前时间戳
        如果handler.sent发送产生了异常
            如果异常是RuntimeException类型
                抛出
            如果异常是RemotingException类型
                抛出
            否则
                使用RemotingException包装，并抛出
    public void received(Channel channel, Object message) throws RemotingException
        追加READ_TIMESTAMP，使用com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeChannel包装channel
        如果message参数是Request类型
            如果request.isEvent()等于true
                如果request.getData()等于R，设置channel参数的channel.readonly属性值等于true
            否则
                如果request.isTwoWay()等于false
                    调用handler.received(exchangeChannel, request.getData())
                否则
                    创建new Response(request.getId(), request.getVersion())实例
                    如果request.isBroken()等于true
                        如果response.getData()等于null，msg局部变量为null，如果msg是Throwable类型，msg局部变量为异常消息，否则msg为toString()方法值
                        设置response.setErrorMessage("Fail to decode request due to: " + msg)
                        设置response.setStatus(40)                                                             // 40: BAD_REQUEST
                    否则
                        调用handler.reply(channel, msg)返回结果，设置res.setResult(result)和res.setStatus(20)     // 20: OK
                        如果handler.reply产生异常，设置res.setErrorMessage(exceptionMessage)和res.setStatus(70)   // 70: SERVICE_ERROR
                    channel.send(response)
        否则如果是Response类型
            DefaultFuture.received(channel, response);                  // 从FUTURES中移除response.getId()，设置response，释放条件，如果需要执行callback，执行callback
        否则如果是String类型
            如果channel.getRemoteAddress()和channel.getUrl()中的port和ip相等
                记录异常日志Dubbo client can not supported string message...
            否则
                获取handler.telnet(channel, (String) message)返回值，如果不等于null并且长度大于0
                    执行channel.send(echo)
        否则
            执行handler.received(exchangeChannel, message)
#### com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeChannel
    public void send(Object message, boolean sent) throws RemotingException
        如果message是Request或者Response或者String类型
            执行channel.send(message, sent)
        否则
            创建new Request()实例，设置version属性等于2.0.0，twoWay属性等于false，date属性等于message
            执行channel.send(request, sent)
    public ResponseFuture request(Object request, int timeout) throws RemotingException
        创建new Request()实例，设置version属性等于2.0.0，twoWay属性等于false，date属性等于request
        创建new DefaultFuture(channel, req, timeout)实例，设置给future变量            // 内部有个静态map存储请求id和future的对应关系，同时启动一个扫描线程，30秒循环一次，扫描出超时的future，创建timeoutResponse，设置给future并从map重移除，正常接收response后也会移除
        执行channel.send(req)，如果产生异常，执行future.cancel()
        返回future
#### com.alibaba.dubbo.remoting.Transporters
    public static Server bind(URL url, ChannelHandler... handlers) throws RemotingException                     // 服务端
        如果handlers长度等于1
            ExtensionLoader.getExtensionLoader(Transporter.class).getAdaptiveExtension().bind(url, handlers[0])
        否则
            ExtensionLoader.getExtensionLoader(Transporter.class).getAdaptiveExtension().bind(url, new ChannelHandlerDispatcher(handlers))
    public static Client connect(URL url, ChannelHandler... handlers) throws RemotingException                  // 客户端
        如果handlers长度等于0
            ExtensionLoader.getExtensionLoader(Transporter.class).getAdaptiveExtension().connect(url, new ChannelHandlerAdapter())
        如果handlers长度等于1
            ExtensionLoader.getExtensionLoader(Transporter.class).getAdaptiveExtension().connect(url, handlers[0])
        否则
            ExtensionLoader.getExtensionLoader(Transporter.class).getAdaptiveExtension().connect(url, new ChannelHandlerDispatcher(handlers))

# com.alibaba.dubbo.remoting.Transporter
## 自适应参数名称：transporter、server/client，默认值：netty                         // 按优先级获取
## 实现
    netty=com.alibaba.dubbo.remoting.transport.netty.NettyTransporter
### com.alibaba.dubbo.remoting.transport.netty.NettyTransporter
    public Server bind(URL url, ChannelHandler listener) throws RemotingException                               // 服务端
        调用new NettyServer(url, listener)并返回
    public Client connect(URL url, ChannelHandler listener) throws RemotingException                            // 客户端
        调用new NettyClient(url, listener)并返回
#### com.alibaba.dubbo.remoting.transport.netty.NettyServer
    配置
        设置timeout属性等于url参数的timeout参数，默认为1000                        // 写入超时，发送response时，如果url参数中的send参数等于true，同时等待timeout，如果超过，抛异常RemotingException(this, Failed to send message...)
        设置connectTimeout属性等于url参数的connect.timeout参数，默认为3000         // 未使用
        设置codec属性等于url参数的codec参数，默认值为telnet，执行ExtensionLoader.getExtensionLoader(Codec2.class).getExtension(codecName)获取        // 编码解码器
        设置accepts属性等于url参数的accepts参数，默认值为0                         // 连接限制个数
        设置idleTimeout属性等于url参数的idle.timeout参数，默认为600000             // 未使用
        设置threads属性根据ExtensionLoader.getExtensionLoader(ThreadPool.class).getAdaptiveExtension().getExecutor(url)获取                      // 业务线程池
        根据url参数的host和port参数值启动server
        根据url参数的iothreads参数值控制worker线程池个数，默认为cpu核数 + 1
        根据url参数的buffer参数值控制编码解码器的buffer大小，默认值为1024 * 8
    Handler顺序
        com.alibaba.dubbo.remoting.transport.netty.NettyCodecAdapter.InternalEncoder                // Codec2，payload参数生效处
        com.alibaba.dubbo.remoting.transport.netty.NettyCodecAdapter.InternalDecoder                // Codec2，payload参数生效处
        com.alibaba.dubbo.remoting.transport.netty.NettyHandler                                     // 适配类，用于netty事件到dubbo事件
            com.alibaba.dubbo.remoting.transport.netty.NettyServer
                com.alibaba.dubbo.remoting.transport.MultiMessageHandler
                    com.alibaba.dubbo.remoting.exchange.support.header.HeartbeatHandler
                        com.alibaba.dubbo.remoting.transport.dispatcher.all.AllChannelHandler           // 使用业务线程池（ThreadPool）转发所有事件
                            com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeHandler
                                com.alibaba.dubbo.rpc.protocol.dubbo.DubboProtocol.requestHandler
    public void connected(Channel ch) throws RemotingException
        如果accepts大于0并且当前server持有的所有Channel个数大于accepts
            关闭channel，并返回
        否则
            执行super.connected(ch)
    public void reset(URL url)
        重置codec属性
        重置timeout属性               // 这里有个bug
        重置connection.time属性
        重置accepts属性
        重置idle.timeout属性
        重置threads属性
#### com.alibaba.dubbo.remoting.transport.netty.NettyClient
    配置
        设置timeout属性等于url参数的timeout参数，默认为1000
        设置connectTimeout属性等于url参数的connect.timeout参数，默认为3000
        设置codec属性等于url参数的codec参数，默认值为telnet，执行ExtensionLoader.getExtensionLoader(Codec2.class).getExtension(codecName)获取        // 编码解码器
        设置send_reconnect属性等于url参数的send.reconnect参数，默认值为false
        设置shutdown_timeout属性等于url参数的shutdown.timeout参数，默认值为1000 * 60 * 15
        设置reconnect_warning_period属性等于url参数的reconnect.waring.period参数，默认值为1800
        设置客户端keepAlive等于true，tcpNoDelay等于true，connectTimeoutMillis等于timeout属性，worker线程池个数等于cpu核数 + 1
        设置客户端线程池，默认等于cached，线程池名称根据url参数的threadname参数值（默认等于DubboClientHandler）+ "-" + url.getAddress()                  // 在channel中事先添加好，连接成功后在获取和删除
    Handler顺序
        com.alibaba.dubbo.remoting.transport.netty.NettyCodecAdapter.InternalEncoder                // Codec2，payload参数生效处
        com.alibaba.dubbo.remoting.transport.netty.NettyCodecAdapter.InternalDecoder                // Codec2，payload参数生效处
        com.alibaba.dubbo.remoting.transport.netty.NettyHandler                                     // 适配类，用于netty事件到dubbo事件
            com.alibaba.dubbo.remoting.transport.netty.NettyClient
                com.alibaba.dubbo.remoting.transport.MultiMessageHandler
                    com.alibaba.dubbo.remoting.exchange.support.header.HeartbeatHandler
                        com.alibaba.dubbo.remoting.transport.dispatcher.all.AllChannelHandler           // 使用业务线程池（ThreadPool）转发所有事件
                            com.alibaba.dubbo.remoting.exchange.support.header.HeaderExchangeHandler
                                com.alibaba.dubbo.rpc.protocol.dubbo.DubboProtocol.requestHandler
    protected void connect() throws RemotingException
        如果channel属性的isConnected()等于true，则退出
        否则
            如果url属性的reconnect参数如果等于null或者等于true，设置reconnect变量等于2000，如果等于false，设置等于0，否则转成int类型赋值给reconnect变量
            如果reconnect变量大于0，并且reconnectExecutorFuture属性等于null或者处于取消状态
                使用reconnectExecutorService属性指定一个定时任务，时间间隔为reconnect毫秒
                    如果channel属性的isConnected()等于false
                        调用connect()方法
                    否则
                        设置lastConnectedTime属性等于当前时间戳
                    如果调用connect()方法产生异常
                        判断当前时间和lastConnectedTime时间间隔是否大于shutdown_timeout，如果大于，判断reconnect_error_log_flag属性如果等于false,设置等于true，并退出当前方法
                        递增reconnect_count属性，如果取摸reconnect_warning_period等于0，打印错误日志
            连接url.getHost()和url.getPort()，最长等待connectTimeout属性毫秒，如果返回false，抛异常RemotingException(client(url) failed to connect to server)
            否则
                设置新channel的interestOps属性等于5，关闭旧的channel，设置新的channel给channel属性
                设置reconnect_count属性等于0，reconnect_error_log_flag属性等于false
#### com.alibaba.dubbo.remoting.exchange.support.header.HeartbeatHandler
    连接操作追加READ_TIMESTAMP或WRITE_TIMESTAMP
    关闭操作清空READ_TIMESTAMP或WRITE_TIMESTAMP
    发送操作追加WRITE_TIMESTAMP
    接收操作追加READ_TIMESTAMP，处理heartbeat请求，通过ping/pong方式实现心跳

# com.alibaba.dubbo.remoting.Dispatcher
## 自适应参数名称：dispatcher、dispather、channel.handler，默认值：all
## 实现
    all=com.alibaba.dubbo.remoting.transport.dispatcher.all.AllDispatcher
### com.alibaba.dubbo.remoting.transport.dispatcher.all.AllDispatcher
    public ChannelHandler dispatch(ChannelHandler handler, URL url)
        创建new AllChannelHandler(handler, url)实例返回

# com.alibaba.dubbo.remoting.Codec2
## 自适应参数名称：codec
## 实现
    dubbo=com.alibaba.dubbo.rpc.protocol.dubbo.DubboCountCodec
### com.alibaba.dubbo.rpc.protocol.dubbo.DubboCodec

# com.alibaba.dubbo.rpc.cluster.Cluster
## 默认值：failover
## 实现
    mock=com.alibaba.dubbo.rpc.cluster.support.wrapper.MockClusterWrapper
    failover=com.alibaba.dubbo.rpc.cluster.support.FailoverCluster
    failfast=com.alibaba.dubbo.rpc.cluster.support.FailfastCluster
    available=com.alibaba.dubbo.rpc.cluster.support.AvailableCluster
    mergeable=com.alibaba.dubbo.rpc.cluster.support.MergeableCluster
### com.alibaba.dubbo.rpc.cluster.support.wrapper.MockClusterWrapper
    public <T> Invoker<T> join(Directory<T> directory) throws RpcException
        返回new MockClusterInvoker<T>(directory, cluster.join(directory))
### com.alibaba.dubbo.rpc.cluster.support.FailoverCluster
    public <T> Invoker<T> join(Directory<T> directory) throws RpcException
        返回new FailoverClusterInvoker<T>(directory)实例
### com.alibaba.dubbo.rpc.cluster.support.FailfastCluster
    public <T> Invoker<T> join(Directory<T> directory) throws RpcException
        返回new FailfastClusterInvoker<T>(directory)实例
### com.alibaba.dubbo.rpc.cluster.support.AvailableCluster
    public <T> Invoker<T> join(Directory<T> directory) throws RpcException
        返回new AbstractClusterInvoker<T>(directory)实例，重写doInvoke方法
            遍历invokers，如果invoker.isAvailable()等于true，返回invoker.invoke(invocation)，如果为空，抛异常RpcException(No provider available in
### com.alibaba.dubbo.rpc.cluster.support.MergeableCluster
    public <T> Invoker<T> join(Directory<T> directory) throws RpcException
        返回new MergeableClusterInvoker<T>( directory )实例
#### com.alibaba.dubbo.rpc.cluster.directory.AbstractDirectory
    public AbstractDirectory(URL url)
        赋值到成员变量
        创建new  ArrayList<Router>()实例，设置给成员变量
        根据url参数中router参数值，设置给routerkey变量
        如果routerkey变量不等于null
            执行ExtensionLoader.getExtensionLoader(RouterFactory.class).getExtension(routerkey).getRouter(url)，添加到routers属性中
            创建new MockInvokersSelector()实例，添加到routers属性中
        排序routers属性
    public List<Invoker<T>> list(Invocation invocation) throws RpcException
        执行doList(invocation)返回设置给invokers变量
        遍历routers属性
            如果router.getUrl()等于null或者router.getUrl()中的runtime参数等于true
                执行router.route(invokers, getConsumerUrl(), invocation)设置给invokers变量
        返回invokers
#### com.alibaba.dubbo.rpc.cluster.directory.StaticDirectory
    public StaticDirectory(URL url, List<Invoker<T>> invokers)
        super(url == null && invokers != null && invokers.size() > 0 ? invokers.get(0).getUrl() : url)
        设置invokers属性等于invokers参数
    protected List<Invoker<T>> doList(Invocation invocation) throws RpcException
        返回invokers属性
#### com.alibaba.dubbo.registry.integration.RegistryDirectory
    public RegistryDirectory(Class<T> serviceType, URL url)
        // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=34193&refer=application%3Ddemo-consumer%26dubbo%3D2.0.0%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D34193%26side%3Dconsumer%26timestamp%3D1457270729249&timestamp=1457270769633
        //      protocol属性等于zookeeper
        //      host属性等于127.0.0.1
        //      port属性等于2181
        //      path属性等于com.alibaba.dubbo.registry.RegistryService
        //      其他是parameters属性
        super(url)
        如果serviceType参数等于null，抛异常IllegalArgumentException(service type is null.)
        如果url.getServiceKey()等于null，抛异常IllegalArgumentException(registry serviceKey is null.)
        设置serviceType属性等于serviceType参数
        设置serviceKey属性等于url.getServiceKey()         // 如：com.alibaba.dubbo.registry.RegistryService
        设置queryMap属性等于url参数的refer参数使用&分隔，使用=分隔出k/v
        // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
        //      protocol属性等于zookeeper
        //      host属性等于127.0.0.1
        //      port属性等于2181
        //      path属性等于com.alibaba.dubbo.registry.RegistryService
        //      其他是parameters属性
        // 将注册中心url处理为半消费者url（参数转了，协议/主机/端口/路径没转）
        设置overrideDirectoryUrl属性和directoryUrl属性等于url.setPath(url.getServiceInterface()).clearParameters().addParameters(queryMap).removeParameter("monitor")
        如果url参数的group参数值不等于null并且等于*或者包含,，设置multiGroup属性等于true
        如果queryMap属性的methods值不等于null，使用,分隔后，设置给serviceMethods属性，否则设置serviceMethods属性等于null      // 如：[ "sayHello" ]
    public URL getUrl()
        返回overrideDirectoryUrl属性
    public void subscribe(URL url)
        // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=providers,configurators,routers&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
        //      protocol属性等于consumer
        //      host属性等于192.168.31.246
        //      port属性等于0
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        设置consumerUrl属性等于url参数
        执行registry.subscribe(url, this)方法           // 添加对providers、configurators、routers三个节点的子节点变化监听器，变化后收到通知，调用notify方法
    public synchronized void notify(List<URL> urls)
        遍历urls参数
            如果url变量的category参数值（默认等于providers）等于routers并且url变量的protocol属性等于route
                添加url到routerUrls变量中
            如果url变量的category参数值（默认等于providers）等于configurators并且url变量的protocol属性等于override
                添加url到configuratorUrls变量中
            如果url变量的category参数值（默认等于providers）等于providers
                添加url到invokerUrls变量中
        如果configuratorUrls属性不等于null并且个数大于0
            设置configurators属性等于toConfigurators(configuratorUrls)
        如果routerUrls属性不等于null并且个数大于0
            根据url转换为Router类型，将整理后的列表设置给routers属性
        设置overrideDirectoryUrl属性等于directoryUrl属性            // 设置到基准url（第一次初始化的时候的），基于此进行override/absent
        遍历configurators属性
            设置overrideDirectoryUrl属性等于configurator.configure(overrideDirectoryUrl)
        执行refreshInvoker(invokerUrls)
    private void refreshInvoker(List<URL> invokerUrls)
        如果invokerUrls不等于null并且个数等于1，且第1个元素不等于null且第一个元素的protocol属性等于empty
            设置forbidden属性等于true
            设置methodInvokerMap属性等于null
            遍历urlInvokerMap属性，关闭所有invoker，并清空urlInvokerMap属性
        否则
            设置forbidden属性等于false
            如果invokerUrls参数个数等于0并且cachedInvokerUrls属性不等于null            // 这里cachedInvokerUrls的作用是处理routers和configurators节点时，invokerUrls参数等于空的场景
                添加cachedInvokerUrls属性到invokerUrls参数中
            否则
                创建HashSet实例，设置给cachedInvokerUrls属性并添加invokerUrls参数到cachedInvokerUrls属性中
            如果invokerUrls参数个数等于0，退出
            // 此处如果之前已存在，则不需要重新refer，此外，providerUrl会合并消费端的queryMap属性，以及使用configurators属性动态处理一遍，最后根据url的disabled和enabled判断是否执行refer
            // 此处会根据协议进行真正的refer，算上之前的过程为：filter > listener > registry > filter > listener > dubbo
            根据queryMap属性的protocol参数拦截invokerUrl；过滤掉invokerUrl的protocol属性等于emtpy的url或不存在invokerUrl的protocol属性名称的Protocol实例；进行合并后，执行new InvokerDelegete<T>(protocol.refer(serviceType, url), url, providerUrl)，处理成新的urlInvokerMap
            根据newUrlInvokerMap中每个invoker的url属性的methods参数值进行重组，处理成新的methodInvokerMap
            关闭所有不在使用的invoker                                                                                // 只要是不使用的，就关闭
    public List<Invoker<T>> doList(Invocation invocation)
        如果forbidden等于true，抛异常RpcException(4, Forbid consumer...)
        从methodInvokerMap匹配出满足invocation的Invoker列表并返回
    public static List<Configurator> toConfigurators(List<URL> urls)        // 存在空协议直接返回空列表，否则根据url的protocol协议(override/absent)添加Configurator
        遍历urls
            如果url的protocol等于empty，退出返回空列表
            如果urls的参数列表中移除anyhost参数后，如果不存在参数，跳过，否则调用configuratorFactory.getConfigurator(url)添加到configurators变量中
        返回configurators变量
#### com.alibaba.dubbo.rpc.cluster.support.wrapper.MockClusterInvoker
    public Result invoke(Invocation invocation) throws RpcException
        获取directory.getUrl()参数中的invocation.getMethodName() + mock或mock参数值(默认值等于false)，设置给value
        如果value等于null或者等于false
            返回invoker.invoke(invocation)
        否则
            如果value以force开头
                返回doMockInvoke(invocation, null)
            否则
                返回invoker.invoke(invocation)，如果出现异常，返回doMockInvoke(invocation, null)
    private Result doMockInvoke(Invocation invocation,RpcException e)
        设置mockInvokers变量等于selectMockInvoker(invocation)
        如果mockInvokers变量等于null或者个数等于0
            设置minvoker变量等于new MockInvoker(directory.getUrl())
        否则
            设置minvoker变量等于mockInvokers变量的第0个元素
        返回minvoker.invoke(invocation)
    private List<Invoker<T>> selectMockInvoker(Invocation invocation)
        如果invocation参数是RpcInvocation类型
            添加invocation.need.mock和true到invocation参数的attachments属性中
            返回directory.list(invocation)
        返回null
#### com.alibaba.dubbo.rpc.cluster.support.AbstractClusterInvoker
    public AbstractClusterInvoker(Directory<T> directory, URL url)
        设置directory属性等于directory参数
        设置availablecheck属性等于url参数中的cluster.availablecheck参数，默认值为true
    public Result invoke(final Invocation invocation) throws RpcException
        检测destroyed必须不等于true，否则抛异常RpcException(Rpc cluster invoker for...)
        执行directory.list(invocation)返回设置给invokers变量
        如果invokers变量不等于null并且个数大于0
            获取第一个invoker的url参数中invocation.getMethodName() + loadbalance或loadbalance参数值，默认为random，设置给value变量
            执行ExtensionLoader.getExtensionLoader(LoadBalance.class).getExtension(value)设置给loadbalance变量
        否则
            执行ExtensionLoader.getExtensionLoader(LoadBalance.class).getExtension(random)设置给loadbalance变量
        如果是异步请求(async等于true或invocationid.autoattach等于true)
            如果invocation的attachments属性中不存在id，追加id和全局自增id到invocation的attachments属性中
        执行doInvoke(invocation, invokers, loadbalance)并返回
#### com.alibaba.dubbo.rpc.cluster.support.FailoverClusterInvoker
    public FailoverClusterInvoker(Directory<T> directory)
        super(directory, directory.getUrl())
    public Result doInvoke(Invocation invocation, final List<Invoker<T>> invokers, LoadBalance loadbalance)
        检测invokers个数必须大于0，否则抛异常RpcException(Failed to invoke the method...)
        根据url属性的invocation.getMethodName() + retries或retries获取重试次数
        一共执行retries + 1次
            根据loadbalance选择一个invoker.isAvailable()等于true的invoker，设置给invoker变量
            执行invoker.invoke(invocation)，出现异常，记录最后一个，最后如果执行成功，忽略，否则，包装最后一个异常往上抛
#### com.alibaba.dubbo.rpc.cluster.support.FailfastClusterInvoker
    public Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException
        检测invokers个数必须大于0，否则抛异常RpcException(Failed to invoke the method...)
        根据loadbalance选择一个invoker.isAvailable()等于true的invoker，设置给invoker变量
        执行invoker.invoke(invocation)，出现异常往上抛RpcException
#### com.alibaba.dubbo.rpc.cluster.support.MergeableClusterInvoker
    public Result invoke(final Invocation invocation) throws RpcException
        不考虑

# com.alibaba.dubbo.rpc.cluster.Router

# com.alibaba.dubbo.rpc.cluster.LoadBalance
## 自适应参数名称：loadbalance，默认值：random
## 实现
    leastactive=com.alibaba.dubbo.rpc.cluster.loadbalance.LeastActiveLoadBalance
    roundrobin=com.alibaba.dubbo.rpc.cluster.loadbalance.RoundRobinLoadBalance
### com.alibaba.dubbo.rpc.cluster.loadbalance.LeastActiveLoadBalance
### com.alibaba.dubbo.rpc.cluster.loadbalance.RoundRobinLoadBalance

------------------------------------------------------------------------------------------------------------------------

# com.alibaba.dubbo.registry.integration.RegistryProtocol
    public <T> Exporter<T> export(final Invoker<T> originInvoker) throws RpcException
        获取originInvoker.getUrl()中export参数值并转换为URL类型，设置给providerUrl变量
        将providerUrl变量复制为一个新的实例并移除dynamic参数和enabled参数，调用其toFullString()设置给key变量
        如果bounds属性中存在指定key变量的实例，设置给exporter变量
        否则
            // 此处传递providerUrl调用export，实际上又重头来了，因为protocol还是自适应类实现，因此整个形式为 filter > listener > registry > filter > listener > dubbo
            设置exporter变量等于new ExporterChangeableWrapper<T>((Exporter<T>)protocol.export(new InvokerDelegete<T>(originInvoker, providerUrl)), originInvoker)
            添加key变量和exporter变量到bounds属性中
        设置registry变量等于getRegistry(originInvoker)
        // 如：dubbo://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=33250&sayHello.executes=2&side=provider&timestamp=1457257046274
        //      protocol属性等于dubbo
        //      host属性等于192.168.31.246
        //      port属性等于20880
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        复制providerUrl变量，移除monitor参数和.开头的参数，将处理后的实例，设置给registedProviderUrl变量
        执行registry.register(registedProviderUrl)
        // 如：provider://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=33250&sayHello.executes=2&side=provider&timestamp=1457257046274
        //      protocol属性等于provider
        //      host属性等于192.168.31.246
        //      port属性等于20880
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        设置overrideSubscribeUrl变量等于getSubscribedOverrideUrl(registedProviderUrl)
        创建new OverrideListener(overrideSubscribeUrl)实例，设置给overrideSubscribeListener变量
        添加overrideSubscribeUrl和overrideSubscribeListener到overrideListeners属性中
        // 对服务节点的configurators节点添加监听器，当configurators节点下的子元素发生变化时，收到通知，调用overridelistener.notify方法
        执行registry.subscribe(overrideSubscribeUrl, overrideSubscribeListener)
        返回Exporter实例            // 依然是个包装
            重写getInvoker方法，返回exporter.getInvoker()实例
            重写unexport()方法，
                执行exporter.unexport()
                执行registry.unregister(registedProviderUrl)
                执行overrideListeners.remove(overrideSubscribeUrl)
                执行registry.unsubscribe(overrideSubscribeUrl, overrideSubscribeListener)
    private URL getSubscribedOverrideUrl(URL registedProviderUrl)
        复制registedProviderUrl变量，设置protocol属性等于provider，添加category和configurators到url中，添加check和false到url中，并返回url
    com.alibaba.dubbo.registry.integration.RegistryProtocol.OverrideListener
        public void notify(List<URL> urls)                                     // 接收服务级别的协议url列表
            复制urls参数，设置给result变量
            遍历result变量
                如果url变量的category参数等于null并且url的protocol属性等于override
                    添加category和configurators到url变量的参数中
                如果UrlUtils.isMatch(subscribeUrl, url)等于false，则从result中移除url         // 删除不匹配的
            执行RegistryDirectory.toConfigurators(result.size() == 0 ? urls : result)返回设置给configurators属性    // 存在空协议直接返回空列表，否则根据url的protocol协议(override/absent)添加Configurator
            创建new ArrayList<ExporterChangeableWrapper<?>>(bounds.values())实例，设置给exporters，并进行遍历         // bounds存储所有服务服务级别的带有协议url的Invoker
                // 如：registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.0.0&export=dubbo%3A%2F%2F192.168.31.246%3A20880%2Fcom.alibaba.dubbo.demo.DemoService%3Fanyhost%3Dtrue%26application%3Ddemo-provider%26dubbo%3D2.0.0%26generic%3Dfalse%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26loadbalance%3Droundrobin%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D33250%26sayHello.executes%3D2%26side%3Dprovider%26timestamp%3D1457257046274&owner=danielli&pid=33250&registry=zookeeper&timestamp=1457257046217
                //      protocol属性等于registry
                //      host属性等于127.0.0.1
                //      port属性等于2181
                //      path属性等于com.alibaba.dubbo.registry.RegistryService
                //      其他是parameters属性
                如果exporter.getOriginInvoker()是InvokerDelegete类型，设置originInvoker变量等于exporter.getOriginInvoker().getInvoker()，否则设置originInvoker变量等于exporter.getOriginInvoker()
                // 如：dubbo://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=33250&sayHello.executes=2&side=provider&timestamp=1457257046274
                //      protocol属性等于dubbo
                //      host属性等于192.168.31.246
                //      port属性等于20880
                //      path属性等于com.alibaba.dubbo.demo.DemoService
                //      其他是parameters属性
                获取originInvoker.getUrl()中export参数值并转换为URL类型，设置给originUrl变量
                执行getNewInvokerUrl(originUrl)，设置给newUrl变量           // 根据Configuration配置的baseUrl选择override还是absent，前提是匹配
                如果originUrl和newUrl不等                                  // 如果不等，就说明configurators下的子节点起作用了
                    执行RegistryProtocol.this.doChangeLocalExport(originInvoker, newUrl)
        private URL getNewInvokerUrl(URL url)
            如果configurators属性不等于null并且个数大于0，进行遍历
                设置url参数等于configurator.configure(url)
            返回url参数
    private <T> void doChangeLocalExport(final Invoker<T> originInvoker, URL newInvokerUrl)     // 基于已存在的originInvoker和newInvokerUrl重新export一次（只变化注册中心下边的部分，注册中心本身不变）
        获取originInvoker.getUrl()中export参数值并转换为URL类型，设置给providerUrl变量
        将providerUrl变量复制为一个新的实例并移除dynamic参数和enabled参数，调用其toFullString()设置给key变量
        如果bounds属性中存在指定key变量的实例
            exporter.setExporter(protocol.export(new InvokerDelegete<T>(originInvoker, newInvokerUrl)))
    private Registry getRegistry(final Invoker<?> originInvoker)
        // 如：registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.0.0&export=dubbo%3A%2F%2F192.168.31.246%3A20880%2Fcom.alibaba.dubbo.demo.DemoService%3Fanyhost%3Dtrue%26application%3Ddemo-provider%26dubbo%3D2.0.0%26generic%3Dfalse%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26loadbalance%3Droundrobin%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D32862%26sayHello.executes%3D2%26side%3Dprovider%26timestamp%3D1457251126597&owner=danielli&pid=32862&registry=zookeeper&timestamp=1457251122641
        //      protocol属性等于registry
        //      host属性等于127.0.0.1
        //      port属性等于2181
        //      path属性等于com.alibaba.dubbo.registry.RegistryService
        //      其他是parameters属性
        设置registryUrl变量等于originInvoker.getUrl()
        如果registryUrl.getProtocol()等于registry
            获取registryUrl的registry参数，默认值等于dubbo，设置给protocol变量
            设置registryUrl变量的protocol属性等于protocol变量，移除registry参数，将处理后的实例重新设置给registryUrl变量
        // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.0.0&export=dubbo%3A%2F%2F192.168.31.246%3A20880%2Fcom.alibaba.dubbo.demo.DemoService%3Fanyhost%3Dtrue%26application%3Ddemo-provider%26dubbo%3D2.0.0%26generic%3Dfalse%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26loadbalance%3Droundrobin%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D32862%26sayHello.executes%3D2%26side%3Dprovider%26timestamp%3D1457251126597&owner=danielli&pid=32862&timestamp=1457251122641
        //      protocol属性等于zookeeper
        //      host属性等于127.0.0.1
        //      port属性等于2181
        //      path属性等于com.alibaba.dubbo.registry.RegistryService
        //      其他是parameters属性
        返回registryFactory.getRegistry(registryUrl)
    public <T> Invoker<T> refer(Class<T> type, URL url) throws RpcException
        // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=34193&refer=application%3Ddemo-consumer%26dubbo%3D2.0.0%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D34193%26side%3Dconsumer%26timestamp%3D1457270729249&timestamp=1457270769633
        //      protocol属性等于zookeeper
        //      host属性等于127.0.0.1
        //      port属性等于2181
        //      path属性等于com.alibaba.dubbo.registry.RegistryService
        //      其他是parameters属性
        获取url参数中的registry参数值，默认值为dubbo，设置给registry变量，设置url参数的protocol属性等于registry变量，并移除registry参数，重新设置给url参数
        设置registry变量等于registryFactory.getRegistry(url)
        如果type参数等于RegistryService，返回proxyFactory.getInvoker((T) registry, type, url)
        获取url的refer参数，整理成Map实例设置给qs变量
        如果qs变量中存在group参数                                          // 不考虑了
            如果参数值存在,可分割或者等于*
                返回doRefer(ExtensionLoader.getExtensionLoader(Cluster.class).getExtension("mergeable"), registry, type, url )
        返回doRefer(cluster, registry, type, url)
    private <T> Invoker<T> doRefer(Cluster cluster, Registry registry, Class<T> type, URL url)
        // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=34193&refer=application%3Ddemo-consumer%26dubbo%3D2.0.0%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D34193%26side%3Dconsumer%26timestamp%3D1457270729249&timestamp=1457270769633
        //      protocol属性等于zookeeper
        //      host属性等于127.0.0.1
        //      port属性等于2181
        //      path属性等于com.alibaba.dubbo.registry.RegistryService
        //      其他是parameters属性
        设置directory变量等于new RegistryDirectory<T>(type, url)实例
        设置directory变量的registry属性等于registry
        设置directory变量的protocol属性等于protocol属性
        // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
        //      protocol属性等于consumer
        //      host属性等于192.168.31.246
        //      port属性等于0
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        设置subscribeUrl变量等于new URL(Constants.CONSUMER_PROTOCOL, NetUtils.getLocalHost(), 0, type.getName(), directory.getUrl().getParameters())
        如果url.getServiceInterface()不等于*并且url参数中register参数等于true（默认为true）
            // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=consumers&check=false&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
            //      protocol属性等于consumer
            //      host属性等于192.168.31.246
            //      port属性等于0
            //      path属性等于com.alibaba.dubbo.demo.DemoService
            //      其他是parameters属性
            复制subscribeUrl变量，添加category和consumers到变量中，添加check和false到变量中，执行registry.register(url1)       // 添加到注册中心
        // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=providers,configurators,routers&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
        //      protocol属性等于consumer
        //      host属性等于192.168.31.246
        //      port属性等于0
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        复制subscribeUrl变量，添加category和providers,configurators,routers到变量中，执行directory.subscribe(url2)
        返回cluster.join(directory)

# com.alibaba.dubbo.registry.RegistryFactory
## 自适应参数名称：protocol，默认值：dubbo
## 实现
    dubbo=com.alibaba.dubbo.registry.dubbo.DubboRegistryFactory
    zookeeper=com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistryFactory
### com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistryFactory
    public Registry getRegistry(URL url)
        设置url参数的path属性等于com.alibaba.dubbo.registry.RegistryService，添加interface和com.alibaba.dubbo.registry.RegistryService到url的参数中，移除export和refer参数
        执行url.toServiceString()生成serviceKey，判断在REGISTRIES是否存在，如果存在，直接返回
        否则
            // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.0.0&interface=com.alibaba.dubbo.registry.RegistryService&owner=danielli&pid=32862&timestamp=1457251122641
            //      protocol属性等于zookeeper
            //      host属性等于127.0.0.1
            //      port属性等于2181
            //      path属性等于com.alibaba.dubbo.registry.RegistryService
            //      其他是parameters属性
            // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&interface=com.alibaba.dubbo.registry.RegistryService&owner=danielli&pid=34193&timestamp=1457270769633
            //      protocol属性等于zookeeper
            //      host属性等于127.0.0.1
            //      port属性等于2181
            //      path属性等于com.alibaba.dubbo.registry.RegistryService
            //      其他是parameters属性
            创建new ZookeeperRegistry(url, zookeeperTransporter)实例，设置给registry变量
            添加serviceKey和registry到REGISTRIES属性中
            返回registry变量
#### com.alibaba.dubbo.registry.zookeeper.ZookeeperRegistry
    public ZookeeperRegistry(URL url, ZookeeperTransporter zookeeperTransporter)
        设置url参数到给url属性
        获取url的save.file参数（默认为false），设置给syncSaveFile属性
        获取url的file参数，默认为${user.home}/.dubbo/dubbo-registry-${host}.cache，设置给filename变量
        如果filename变量不等于null、"false"、"0"、"null"、"N/A"
            创建new File(filename)实例，如果文件不存在且父目录不等于null且父目录不存在
                如果无法创建父目录，抛异常
        设置file属性等于file变量
        如果file存在，读取内容到properties属性
        执行notify(url.getBackupUrls())
        根据url的retry.period参数，默认值为5 * 1000，启动一个定时任务，周期为retry.period参数值
            任务
                如果failedRegistered不等于null，遍历
                    调用主注册逻辑，执行成功从failedRegistered中移除url
                如果failedUnregistered不等于null，遍历
                    调用主解注册逻辑，执行成功从failedUnregistered中移除url
                如果failedSubscribed不等于null，遍历
                    调用主订阅逻辑，执行成功从failedSubscribed中移除该listener
                如果failedUnsubscribed不等于null，遍历
                    调用主解订阅逻辑，执行成功从failedUnsubscribed中移除该listener
                如果failedNotified不等于null，遍历
                    调用listener.notify(urls);，执行成功从failedNotified中移除该listener
        如果url的host属性等于0.0.0.0或者包含url的anyhost等于true
            抛异常IllegalStateException(registry address == null)
        获取url的group参数，默认为dubbo，如果不以/开头，补加/，设置给root属性
        创建zk客户端，添加状态监听事件
            如果状态等于重连
                添加registered中的URL实例，到failedRegistered中
                添加subscribed中的Entry<URL, Set<NotifyListener>>到failedSubscribed中
    protected void notify(List<URL> urls)
        如果urls不等于null且个数大于0
            遍历subscribed属性
                如果UrlUtils.isMatch(entry.getKey(), urls.get(0))等于false
                    则继续遍历
                遍历entry.getValue()
                    执行notify(url, listener, filterEmpty(url, urls))         filterEmpty表示如果urls为空，补加一个url.setProtocol(Constants.EMPTY_PROTOCOL)实例进去
    private String toServicePath(URL url)
        设置name等于url.getServiceInterface()
        如果name等于*
            返回root
        如果root等于/
            返回/ + name
        否则返回root + / + name
    public void register(URL url)
        // 如：dubbo://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=33250&sayHello.executes=2&side=provider&timestamp=1457257046274
        //      protocol属性等于dubbo
        //      host属性等于192.168.31.246
        //      port属性等于20880
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=consumers&check=false&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
        //      protocol属性等于consumer
        //      host属性等于192.168.31.246
        //      port属性等于0
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        加入url到registered中
        从failedRegistered中移除url
        从failedUnregistered中移除url
        创建zk节点：toServicePath(url) + / + url的category参数（默认值providers） + / + url.toFullString()，如果url的dynamic参数等于true（默认为true），则创建临时节点，否则创建永久节点
        如果创建失败，添加url到failedRegistered中
    public void subscribe(URL url, NotifyListener listener)
        添加url和listener到subscribed中
        从failedSubscribed中移除url对应的listener
        从failedUnsubscribed中移除url对应的listener
        从failedNotified中移除url对应的listener
        如果url.getServiceInterface()等于*          // 获取根节点下的所有子节点进行订阅，并且对root节点添加监听器，当子节点发生变动后收到通知，对于所有新增子节点进行订阅
            从zkListeners中获取指定url对应listeners映射，从映射中得到listener对应的映射ChildListener实例是否存在
            如果不存在
                添加ChildListener实例
                    public void childChanged(String parentPath, List<String> services)
                        遍历services
                            如果anyServices不包含service
                                添加service到anyServices中
                                执行subscribe(url.setPath(service).addParameters(Constants.INTERFACE_KEY, service, "check", String.valueOf(false)), listener)
                添加到listeners映射中
                设置zkListener变量等于ChildListener实例
            否则设置给zkListener变量
            创建root节点，设置为临时节点
            执行addChildListener(root, zkListener)，返回所有root下面所有子节点，进行遍历
                添加service到anyServices中
                执行subscribe(url.setPath(service).addParameters(Constants.INTERFACE_KEY, service, "check", String.valueOf(false)), listener)
        否则                                      // 单一服务订阅，收到通知后，调用notify
            创建new ArrayList<URL>()实例，设置给urls变量
            // 如：provider://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=33250&sayHello.executes=2&side=provider&timestamp=1457257046274
            //      protocol属性等于provider
            //      host属性等于192.168.31.246
            //      port属性等于20880
            //      path属性等于com.alibaba.dubbo.demo.DemoService
            //      其他是parameters属性
            // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=providers,configurators,routers&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
            //      protocol属性等于consumer
            //      host属性等于192.168.31.246
            //      port属性等于0
            //      path属性等于com.alibaba.dubbo.demo.DemoService
            //      其他是parameters属性
            如果url的category参数等于*，遍历toServicePath(url) + / + providers、toServicePath(url) + / + consumers、toServicePath(url) + / + routers、toServicePath(url) + / + configurators
            否则遍历目录的toServicePath(url) + / + url的category参数（默认值providers），设置给path变量
                从zkListeners中获取指定url对应listeners映射，从映射中得到listener对应的映射ChildListener实例是否存在
                如果不存在
                    添加ChildListener实例
                        public void childChanged(String interface, List<String> urlList)
                            执行ZookeeperRegistry.this.notify(url, listener, toUrlsWithEmpty(url, interface, urlList))
                    添加到listeners映射中
                    设置zkListener变量等于ChildListener实例
                否则设置给zkListener变量
                创建path节点，设置为临时节点
                执行addChildListener(path, zkListener)，返回所有path下面所有子节点，如果子节点不等于null，执行toUrlsWithEmpty(url, path, children)添加到urls变量中
            执行notify(url, listener, urls)
        如果以上逻辑执行失败
            遍历properties属性，获取url.serviceKey对应的URL列表，设置给urls变量
            如果urls变量不等于null并且元素个数大于0
                执行notify(url, listener, urls)
            添加url和listener到failedSubscribed中
    private List<URL> toUrlsWithEmpty(URL subscribe, String path, List<String> childNodes)          // 过滤 + 处理元素空场景添加空协议
        遍历childNodes，处理成URL，调用UrlUtils.isMatch(subscribe, url)，如果为true，加入url到urls中
        如果urls等于null或者个数等于0
            获取path中的最后/后边的部分，设置给category变量，如果不包含/，设置category变量等于path
            执行consumer.setProtocol("empty").addParameter("category", category)添加到urls中
    protected void notify(URL url, NotifyListener listener, List<URL> urls)                         // urls可能包含空协议，否则，都是最终的服务URL
        创建new HashMap<String, List<URL>>()实例，设置给result
        遍历urls
            如果UrlUtils.isMatch(url, u)等于true
                获取u的category参数，默认为providers
                添加category参数值和u到result中
        如果result个数等于0，退出
        否则
            遍历result
                添加url、entry.key、entry.value到notified中
                // 如：provider://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&category=configurators&check=false&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=33250&sayHello.executes=2&side=provider&timestamp=1457257046274
                //      protocol属性等于provider
                //      host属性等于192.168.31.246
                //      port属性等于20880
                //      path属性等于com.alibaba.dubbo.demo.DemoService
                //      其他是parameters属性
                // 如：consumer://192.168.31.246/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&category=providers,configurators,routers&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=34193&side=consumer&timestamp=1457270729249
                //      protocol属性等于consumer
                //      host属性等于192.168.31.246
                //      port属性等于0
                //      path属性等于com.alibaba.dubbo.demo.DemoService
                //      其他是parameters属性
                执行saveProperties(url)
                执行listener.notify(entry.value)                                                    // 对urls进行分类通知
        如果通知失败，添加url和listener和urls到failedNotified中
    private void saveProperties(URL url)
        如果file属性等于null，则退出
        获取notified属性汇总，当前url对应的Map<String, List<URL>>类型的实例，进行遍历                     // 进程内需要的category和urls
            使用空额追加所有URL实例到一个字符串
        添加url.getServiceKey()和拼接后的字符串到properties中
        设置lastCacheChanged属性自身加1，并返回给version变量
        如果syncSaveFile等于true
            执行doSaveProperties(version)
        否则
            异步执行doSaveProperties(version)
    public void doSaveProperties(long version)
        如果lastCacheChanged大于version，退出
        如果file等于null，退出
        保存properties内容到file
        如果保存错误，判断lastCacheChanged是否大于version，如果是，退出，否则异步执行doSaveProperties(lastCacheChanged++)
    public void unregister(URL url)
        从registered中移除
        从failedRegistered中移除url
        从failedUnregistered中移除url
        删除zk节点：toServicePath(url) + / + url的category参数（默认值providers） + / + url.toFullString()
        如果创建失败，添加url到failedUnregistered中
    public void unsubscribe(URL url, NotifyListener listener)
        从subscribed中移除url对应的listener
        从failedSubscribed中移除url对应的listener
        从failedUnsubscribed中移除url对应的listener
        从failedNotified中移除url对应的listener
        从zkListeners中获取指定url对应listeners映射，从映射中得到listener对应的ChildListener实例，移除子监听器：removeChildListener(toServicePath(url) + / + url的category参数（默认值providers） + / + url.toFullString(), childListener)
        如果取消订阅失败，添加url和listener到failedUnsubscribed中

# com.alibaba.dubbo.remoting.zookeeper.ZookeeperTransporter
## 自适应参数名称：client、transporter，默认值：zkclient
## 实现
    curator=com.alibaba.dubbo.remoting.zookeeper.curator.CuratorZookeeperTransporter
## com.alibaba.dubbo.remoting.zookeeper.curator.CuratorZookeeperTransporter

------------------------------------------------------------------------------------------------------------------------

### com.alibaba.dubbo.registry.dubbo.DubboRegistryFactory
    public Registry getRegistry(URL url)
        设置url参数的path属性等于com.alibaba.dubbo.registry.RegistryService，添加interface和com.alibaba.dubbo.registry.RegistryService到url的参数中，移除export和refer参数
        执行url.toServiceString()生成serviceKey，判断在REGISTRIES是否存在，如果存在，直接返回
        否则
            添加sticky和true到url的参数中
            添加lazy和true到url的参数中
            添加reconnect和false到url的参数中
            如果url参数中不包含timeout，添加timeout和10000到url的参数中
            如果url参数中不包含callbacks，添加callbacks和10000到url的参数中
            如果url参数中不包含connect.timeout，添加connect.timeout和10000到url的参数中
            添加methods和RegistryService接口定义的公有方法名称列表（非继承方法）按照,分隔加入到url的参数中
            添加subscribe.1.callback和true到url的参数中
            添加unsubscribe.1.callback和false到url的参数中
            创建new ArrayList<URL>()实例，设置给urls变量
            添加移除backup参数的url到urls变量中
            用,分隔backup参数值并进行遍历，分别设置url的address并加入到urls变量中
            复制url变量，添加refer和url.toParameterString()到参数中，创建new RegistryDirectory<RegistryService>(RegistryService.class, newUrl)实例设置给directory变量
            执行cluster.join(directory)设置给registryInvoker变量
            执行proxyFactory.getProxy(registryInvoker)设置给registryService变量
            创建new DubboRegistry(registryInvoker, registryService)设置给registry变量
            设置directory变量registry属性等于registry变量
            设置directory变量protocol属性等于protocol属性
            执行directory.notify(urls)
            执行directory.subscribe(new URL("consumer", NetUtils.getLocalHost(), 0, RegistryService.class.getName(), url.getParameters()))
            添加serviceKey和registry到REGISTRIES属性中
            返回registry
#### com.alibaba.dubbo.registry.dubbo.DubboRegistry
    public DubboRegistry(Invoker<RegistryService> registryInvoker, RegistryService registryService)
        整个操作还是依赖于registryService

------------------------------------------------------------------------------------------------------------------------

# com.alibaba.dubbo.remoting.telnet.TelnetHandler

# com.alibaba.dubbo.common.status.StatusChecker

# com.alibaba.dubbo.rpc.protocol.dubbo.CallbackServiceCodec

# com.alibaba.dubbo.rpc.protocol.dubbo.filter.FutureFilter

# 其他
* 自适应

















