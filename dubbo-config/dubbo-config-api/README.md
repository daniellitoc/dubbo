# ApplicationConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setName(String name)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setOwner(String owner)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
    public void setOrganization(String organization)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setArchitecture(String architecture)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setEnvironment(String environment)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        必须是develop或test或product
    public void setCompiler(String compiler)
        调用AdaptiveCompiler.setDefaultCompiler(compiler);        // 设置静态变量用于设置默认编译器
    public void setLogger(String logger)
        LoggerFactory.setLoggerAdapter(logger)                   // 一层适配器，设置默认logger，初始化是按照log4j/slf4j/jcl/jul识别一遍

# ModuleConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setName(String name)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setOwner(String owner)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
    public void setOrganization(String organization)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z

# RegistryConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setProtocol(String protocol)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setUsername(String username)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setPassword(String password)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setFile(String file)
        检测最长200个字符
    public void setTransporter(String transporter)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setServer(String server)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setClient(String client)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public static void destroyAll()
        调用AbstractRegistryFactory.destroyAll()              // 获取所有Registry实例，并调用其destroy方法

# MonitorConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z

# ProviderConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setLoadbalance(String loadbalance)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果loadbalance不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(LoadBalance.class).hasExtension(loadbalance)是否存在，若不存在，抛出IllegalStateException
    public void setMock(String mock)
        如果mock不等于null并且以return开头，检测最长100个字符，
        否则，检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setStub(String stub)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setCluster(String cluster)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果cluster不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Cluster.class).hasExtension(cluster)是否存在，若不存在，抛出IllegalStateException
    public void setProxy(String proxy)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果proxy不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(ProxyFactory.class).hasExtension(proxy)是否存在，若不存在，抛出IllegalStateException
    public void setFilter(String filter)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果client不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Filter.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public String getListener()
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(InvokerListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setListener(String listener)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(ExporterListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setLayer(String layer)
        检测最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setOwner(String owner)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
    public void setVersion(String version)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setGroup(String group)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setToken(String token)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setRegister(Boolean register)
        如果register等于false，创建new RegistryConfig(RegistryConfig.NO_AVAILABLE)实例赋值
    public void setContextpath(String contextpath)
        检测最长200个字符，格式只能包含/\-$._0-9a-zA-Z
    public void setThreadpool(String threadpool)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果threadpool不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(ThreadPool.class).hasExtension(threadpool)是否存在，若不存在，抛出IllegalStateException
    public void setTelnet(String telnet)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果telnet不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(TelnetHandler.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setStatus(String status)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果status不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(StatusChecker.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setTransporter(String transporter)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果transporter不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(transporter)是否存在，若不存在，抛出IllegalStateException
    public void setExchanger(String exchanger)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果exchanger不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Exchanger.class).hasExtension(exchanger)是否存在，若不存在，抛出IllegalStateException
    public void setDispatcher(String dispatcher)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果dispatcher不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Dispatcher.class).hasExtension(dispatcher)是否存在，若不存在，抛出IllegalStateException

# ConsumerConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setLoadbalance(String loadbalance)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果loadbalance不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(LoadBalance.class).hasExtension(loadbalance)是否存在，若不存在，抛出IllegalStateException
    public void setMock(String mock)
        如果mock不等于null并且以return开头，检测最长100个字符，
        否则，检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setStub(String stub)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setCluster(String cluster)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果cluster不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Cluster.class).hasExtension(cluster)是否存在，若不存在，抛出IllegalStateException
    public void setProxy(String proxy)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果proxy不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(ProxyFactory.class).hasExtension(proxy)是否存在，若不存在，抛出IllegalStateException
    public void setFilter(String filter)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果client不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Filter.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public String getListener()
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(InvokerListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setListener(String listener)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(InvokerListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setLayer(String layer)
        检测最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setOwner(String owner)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
    public void setVersion(String version)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setGroup(String group)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setTimeout(Integer timeout)
        如果系统属性sun.rmi.transport.tcp.responseTimeout等于null或者等于0，设置timeout到系统属性sun.rmi.transport.tcp.responseTimeout

# ProtocolConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setName(String name)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setHost(String host)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setContextpath(String contextpath)
        检测最长200个字符，格式只能包含/\-$._0-9a-zA-Z
    public void setThreadpool(String threadpool)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果threadpool不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(ThreadPool.class).hasExtension(threadpool)是否存在，若不存在，抛出IllegalStateException
    public void setCodec(String codec)
        如果name等于dubbo
            检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
            如果codec不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Codec.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setSerialization(String serialization)
        如果name等于dubbo
            检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
            如果serialization不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Serialization.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setServer(String server)
         如果name等于dubbo
            检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
            如果server不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setClient(String client)
        如果name等于dubbo
            检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
            如果client不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setTelnet(String telnet)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果telnet不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(TelnetHandler.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setStatus(String status)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果status不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(StatusChecker.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setTransporter(String transporter)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果transporter不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Transporter.class).hasExtension(transporter)是否存在，若不存在，抛出IllegalStateException
    public void setExchanger(String exchanger)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果exchanger不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Exchanger.class).hasExtension(exchanger)是否存在，若不存在，抛出IllegalStateException
    public void setDispatcher(String dispatcher)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果dispatcher不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Dispatcher.class).hasExtension(dispatcher)是否存在，若不存在，抛出IllegalStateException
    public void destory()
        如果name不等于null
            调用ExtensionLoader.getExtensionLoader(Protocol.class).getExtension(name).destroy()
    public static void destroyAll()
        调用AbstractRegistryFactory.destroyAll()              // 获取所有Registry实例，并调用其destroy方法
        调用ExtensionLoader.getExtensionLoader(Protocol.class)获取所有Protocol实例，并调用他们的destroy()方法

# MethodConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setLoadbalance(String loadbalance)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果loadbalance不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(LoadBalance.class).hasExtension(loadbalance)是否存在，若不存在，抛出IllegalStateException
    public void setMock(String mock)
        如果mock不等于null并且以return开头，检测最长100个字符，
        否则，检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setName(String name)
        检测最长100个字符，格式以a-zA-Z开头，后面满足0-9a-zA-Z

# ArgumentConfig

# AnnotationBean

# ServiceConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setLoadbalance(String loadbalance)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果loadbalance不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(LoadBalance.class).hasExtension(loadbalance)是否存在，若不存在，抛出IllegalStateException
    public void setMock(String mock)
        如果mock不等于null并且以return开头，检测最长100个字符，
        否则，检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setStub(String stub)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setCluster(String cluster)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果cluster不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Cluster.class).hasExtension(cluster)是否存在，若不存在，抛出IllegalStateException
    public void setProxy(String proxy)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果proxy不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(ProxyFactory.class).hasExtension(proxy)是否存在，若不存在，抛出IllegalStateException
    public void setFilter(String filter)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果client不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Filter.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public String getListener()
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(InvokerListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setListener(String listener)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(ExporterListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setLayer(String layer)
        检测最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setOwner(String owner)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
    public void setVersion(String version)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setGroup(String group)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setToken(String token)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setRegister(Boolean register)
        如果register等于false，创建new RegistryConfig(RegistryConfig.NO_AVAILABLE)实例赋值
    public Class<?> getInterfaceClass()
        如果interfaceClass不等于null，返回interfaceClass
        如果ref是GenericService类型，返回GenericService.class
        如果interfaceName不等于null并且长度大于0，加载并返回，如果加载失败，抛出IllegalStateException
    public void setPath(String path)
        检测最长200个字符，格式只能包含/\-$._0-9a-zA-Z
    public void setGeneric(String generic)
        generic如果不等于null，必须等于true、nativejava、bean

# ReferenceConfig
    static {                    // 属于AbstractConfig
        添加关闭钩子
            调用ProtocolConfig.destroyAll()
    }
    public void setLoadbalance(String loadbalance)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果loadbalance不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(LoadBalance.class).hasExtension(loadbalance)是否存在，若不存在，抛出IllegalStateException
    public void setMock(String mock)
        如果mock不等于null并且以return开头，检测最长100个字符，
        否则，检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setParameters(Map<String, String> parameters)
        遍历检测value最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setStub(String stub)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public void setCluster(String cluster)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果cluster不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(Cluster.class).hasExtension(cluster)是否存在，若不存在，抛出IllegalStateException
    public void setProxy(String proxy)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
        如果proxy不等于null并且长度大于0，调用ExtensionLoader.getExtensionLoader(ProxyFactory.class).hasExtension(proxy)是否存在，若不存在，抛出IllegalStateException
    public void setFilter(String filter)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果client不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(Filter.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public String getListener()
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(InvokerListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setListener(String listener)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
        如果listener不等于null并且长度大于0，按,分隔并遍历，如果元素以-开头，截取，判断是否等于default，如果等于，跳过当前元素，否则调用ExtensionLoader.getExtensionLoader(InvokerListener.class).hasExtension(v)是否存在，若不存在，抛出IllegalStateException
    public void setLayer(String layer)
        检测最长100个字符，格式只能包含:*,/\-._0-9a-zA-Z
    public void setOwner(String owner)
        检测最长100个字符，格式只能包含,\-._0-9a-zA-Z
    public void setVersion(String version)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public void setGroup(String group)
        检测最长100个字符，格式只能包含*,\-._0-9a-zA-Z
    public Class<?> getInterfaceClass()
        如果interfaceClass不等于null，返回interfaceClass
        如果isGeneric()为true或者getConsumer().isGeneric()为true，返回GenericService.class
        如果interfaceName不等于null并且长度大于0，加载并返回，如果加载失败，抛出IllegalStateException
    public void setClient(String client)
        检测最长100个字符，格式只能包含\-._0-9a-zA-Z
    public Boolean isGeneric()
        ProtocolUtils.isGeneric(generic)        // generic等于true、nativejava、bean

# 启动
## ServiceConfig
    public synchronized void export()
        如果export属性等于null，并且provider不等于null，设置export属性等于provider.getExport()
        如果export属性不等于null并且不等于true
            退出
        如果delay属性等于null，并且provider不等于null，设置delay属性等于provider.getDelay()
        如果delay属性不等于null并且大于0，启动一个新的线程，内部睡眠delay，调用doExport()，否则直接调用doExport()
    protected synchronized void doExport()
        如果unexported属性等于true，抛异常IllegalStateException(Already unexported!)
        如果exported属性等于true，退出
        设置exported属性等于true
        如果interfaceName属性为null，抛异常IllegalStateException(<dubbo:service interface="" /> interface not allow null!)
        如果provider属性等于null，创建new ProviderConfig()实例并设置给provider
        遍历provider属性的所有setter方法，通过系统属性进行填充，填充规则为：
            dubbo.provider. + id + property
            dubbo.provider + property
            如果存在getter方法，并且对应getter方法返回的是null
                根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                读取文件作为Properties，按照dubbo.provider. + id + property、dubbo.provider + property规则调用setter方法
        如果application属性等于null，设置application属性等于provider.getApplication()
        如果module属性等于null，设置module属性等于provider.getModule()
        如果registries属性等于null，设置registries属性等于provider.getRegistries()
        如果monitor属性等于null，设置monitor属性等于provider.getMonitor()
        如果protocols属性等于null，设置protocols属性等于provider.getProtocols()
        如果module属性不等于null且registries属性等于null，设置registries属性等于module.getRegistries()
        如果module属性不等于null且monitor属性等于null，设置monitor属性等于module.getMonitor()
        如果application属性不等于null且registries属性等于null，设置registries属性等于application.getRegistries()
        如果application属性不等于null且monitor属性等于null，设置monitor属性等于application.getMonitor()
        如果ref属性是GenericService类型，设置interfaceClass属性等于GenericService.class，如果generic属性等于null，设置generic属性等于true
        否则
            加载类名为interfaceName属性，赋值给interfaceClass属性，如果不存在，抛异常IllegalStateException
            校验interfaceClass属性必须不等于null并且是接口类型，检测methods属性中的所有MethodConfig实例的name属性都是interfaceClass中的方法名
            校验ref属性必须不等于null，且ref属性是interfaceClass类型的实例
            设置generic属性等于false
        如果stub属性不等于null                                             // stub主要用于getProxy场景，和服务端没有关系，这里确需要进行校验
            如果stub属性等于true，设置stub属性等于interfaceName + "Stub"
            通过加载stub属性类验证必须存在，同时必须是interfaceClass属性的实现类
        如果application属性等于null
            如果dubbo属性文件中dubbo.application.name不等于null，创建new ApplicationConfig()实例并设置给application属性
        如果application属性等于null，抛异常IllegalStateException(No such application config! Please add <dubbo:application name="" /> to your spring config.)
        遍历application属性的所有setter方法，通过系统属性进行填充，填充规则为：
            dubbo.application. + id + property
            dubbo.application. + property
            如果存在getter方法，并且对应getter方法返回的是null
                根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                读取文件作为Properties，按照dubbo.application. + id + property、dubbo.application. + property规则调用setter方法
        如果dubbo配置文件中存在属性dubbo.service.shutdown.wait，添加到系统属性中
        否则
            如果dubbo配置文件中存在属性dubbo.service.shutdown.wait.seconds，添加到系统属性中
        如果registries属性等于null或者个数等于0
            如果dubbo属性文件中dubbo.registry.address不等于null，创建new ArrayList<RegistryConfig>()实例并设置给registries属性
            使用|分隔dubbo.registry.address属性值进行遍历，创建new RegistryConfig()实例并设置address属性等于属性值，添加实例到registries属性中
        如果registries属性等于null或者个数等于0，抛异常IllegalStateException(No such any registry to...)
        遍历registries属性
            遍历registryConfig变量的所有setter方法，通过系统属性进行填充，填充规则为：
                dubbo.registry. + id + property
                dubbo.registry. + property
                如果存在getter方法，并且对应getter方法返回的是null
                    根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                    读取文件作为Properties，按照dubbo.registry. + id + property、dubbo.registry. + property规则调用setter方法
        如果protocols属性等于null或者个数等于0，设置protocols属性等于provider.getProtocols()
        如果protocols属性等于null或者个数等于0，创建Arrays.asList(new ProtocolConfig[] {new ProtocolConfig()})实例设置给protocols属性
        遍历protocols属性
            如果protocolConfig变量的name属性等于null，设置name属性等于dubbo
            遍历protocolConfig变量的所有setter方法，通过系统属性进行填充，填充规则为：
                dubbo.protocol. + id + property
                dubbo.protocol. + property
                如果存在getter方法，并且对应getter方法返回的是null
                    根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                    读取文件作为Properties，按照dubbo.protocol. + id + property、dubbo.protocol. + property规则调用setter方法
        遍历当前实例的所有setter方法，通过系统属性进行填充，填充规则为：
            dubbo.service. + id + property
            dubbo.service. + property
            如果存在getter方法，并且对应getter方法返回的是null
                根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                读取文件作为Properties，按照dubbo.service. + id + property、dubbo.service. + property规则调用setter方法
        如果stub属性不等于null、"false"、"0"、"null"、"N/A"                // stub主要用于getProxy场景，和服务端没有关系，这里确需要进行校验
            如果stub属性等于"true"或者"default"，加载interfaceClass + "Stub"，否则加载stub类
            验证加载的类必须是interfaceClass子类且存在interfaceClass属性参数的构造方法
        如果mock属性不等于null、"false"、"0"、"null"、"N/A"
            如果mock属性以return 为前缀，截取return 以后的部分，通过调用MockInvoker.parseMockValue(value)检测是否合理
            否则
                如果mock属性等于"true"或者"default"，加载interfaceClass + "Mock"，否则加载mock类
                验证加载的类必须是interfaceClass子类且存在无参的构造方法
        如果path属性等于null，设置path属性等于interfaceName属性
        调用doExportUrls()
    private void doExportUrls()
        执行loadRegistries(true)设置给registryURLs变量         // 多注册中心
        遍历protocols属性                                     // 多协议
            调用doExportUrlsFor1Protocol(protocolConfig, registryURLs)        // 单个协议 * 多注册中心
    protected List<URL> loadRegistries(boolean provider)
        如果registries属性等于null或者个数等于0
            如果dubbo属性文件中dubbo.registry.address不等于null，创建new ArrayList<RegistryConfig>()实例并设置给registries属性
            使用|分隔dubbo.registry.address属性值进行遍历，创建new RegistryConfig()实例并设置address属性等于属性值，添加实例到registries属性中
        如果registries属性等于null或者个数等于0，抛异常IllegalStateException(No such any registry to...)
        遍历registries属性
            遍历registryConfig变量的所有setter方法，通过系统属性进行填充，填充规则为：
                dubbo.registry. + id + property
                dubbo.registry. + property
                如果存在getter方法，并且对应getter方法返回的是null
                    根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                    读取文件作为Properties，按照dubbo.registry. + id + property、dubbo.registry. + property规则调用setter方法
        创建new ArrayList<URL>()实例，设置给registryList变量
        如果registries不等于null并且个数大于0，进行遍历
            设置address变量等于registryConfig.getAddress()
            如果address变量等于null
                设置address变量等于0.0.0.0
            如果系统属性dubbo.registry.address存在，设置address变量等于其值
            如果address不等于null并且不等于N/A
                创建new HashMap<String, String>()实例，设置给map变量
                将application属性和registryConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中
                添加path和RegistryService.class到map变量中
                添加version和Version类对应的jar版本到map变量中
                添加timestamp和当前系统时间戳到map变量中
                如果可以获取到系统pid且大于0
                    添加pid和系统pid到map变量中
                如果map变量中不包含key名称等于protocol的元素
                    如果ExtensionLoader.getExtensionLoader(RegistryFactory.class).hasExtension("remote")等于true    // 默认没有
                        添加protocol和remote到map变量中
                    否则
                        添加protocol和dubbo到map变量中     // 如果registryConfig.protocol没设置，默认值用dubbo，最终address设置了，还是要参考address
                // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.0.0&owner=danielli&pid=23031&timestamp=1457196712687
                //      protocol属性等于zookeeper
                //      host属性等于127.0.0.1
                //      port属性等于2181
                //      path属性等于com.alibaba.dubbo.registry.RegistryService
                //      其他是parameters属性
                使用;分隔address，调用UrlUtils.parseURL(address, map)方法返回URL实例，添加到urls变量中
                遍历urls
                    添加registry和url变量的protocol属性到url变量参数中
                    // 如：registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-provider&dubbo=2.0.0&owner=danielli&pid=23031&registry=zookeeper&timestamp=1457196712687
                    设置url变量的protocol属性等于属性等于registry
                    如果provider参数等于true并且url的register参数等于true（默认为true）或者provider参数等于false并且url的subscribe参数等于true（默认为true）
                        添加url到registryList变量中
        返回registryList变量
    private void doExportUrlsFor1Protocol(ProtocolConfig protocolConfig, List<URL> registryURLs)
        设置name变量等于protocolConfig.getName()
        如果name变量等于null
            设置name变量等于dubbo
        设置host变量等于protocolConfig.getHost()，如果host变量等于null，设置host变量等于provider.getHost()
        设置anyhost变量等于false
        如果host变量等于null或者等于localhost或者等于0.0.0.0或者匹配127断开头
            设置anyhost变量等于true
            设置host变量等于本机ip
        设置port变量等于protocolConfig.getPort()，如果port变量等于null，设置post变量等于provider.getPort()
        设置defaultPort变量等于ExtensionLoader.getExtensionLoader(Protocol.class).getExtension(name).getDefaultPort()
        如果port变量等于null，设置port变量等于defaultPort变量
        如果port变量等于null，设置port变量等于指定name变量对应的随机端口
        创建new HashMap<String, String>()实例，设置给map变量
        如果anyhost等于true
            添加anyhost和true到map变量中
        添加side和provider到map变量中
        添加dubbo和Version类对应的jar版本号到map变量中
        添加timestamp和当前系统时间戳到map变量中
        如果可以获取到系统pid且大于0
            添加pid和系统pid到map变量中
        将application属性、module属性中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中
        将provider属性中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用default.field）
        将protocolConfig属性、this中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中
        如果methods属性不等于null且个数大于0，进行遍历
            将methodConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用methodConfigName.field）
            如果map变量中包含methodConfigName.retry指定key，移除指定key，如果对应的value等于false，添加methodConfigName.retries和0到map变量中
            如果method.getArguments()不等于null并且个数大于0，进行遍历
                如果argumentConfig.getType()不等于null
                    找到interfaceClass属性中和methodConfig.getName()相同的方法
                        如果argumentConfig.getIndex()不等于-1
                            验证匹配方法的参数类型中，指定index的类型是否等于argumentConfig.getType()，如果不等，抛异常IllegalArgumentException(argument config error : the index attribute and type attirbute not match)
                            否则将argumentConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用methodConfigName.index.field）
                        遍历匹配方法的所有参数类型，如果存在和argumentConfig.getType相同的
                            将argumentConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用methodConfigName.parameterTypeIndex.field）
                如果argumentConfig.getIndex()不等于-1
                    将argumentConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用methodConfigName.index.field）
                否则抛异常IllegalArgumentException(argument config must set index or type attribute.eg)
        如果isGeneric()等于true
            添加generic和generic属性到map变量中
            添加methods和*到map变量中
        否则
            获取interfaceClass所在的jar的版本（如果找不到，使用version属性）设置给revision变量
            如果revision变量不等于null
                添加revision和revision变量到map变量中
            设置methods变量等于Wrapper.getWrapper(interfaceClass).getMethodNames()
            如果methods长度为0
                添加methods和*到map变量中
            否则
                添加methods和使用,进行合并的methods到map变量中
        如果token属性不等于null、"false"、"0"、"null"、"N/A"
            如果token属性等于"true"或者"default"，添加token和UUID到map中
            否则，添加token和token属性到map变量中
        如果protocolConfig.getName()等于injvm
            设置protocolConfig参数的register属性等于false，添加notify和false到map中
        设置contextPath变量等于protocolConfig.getContextpath()，如果contextPath变量等于null，设置contextPath变量等于provider.getContextpath()
        // 如：dubbo://192.168.31.246:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=23193&sayHello.executes=2&side=provider&timestamp=1457199105180
        //      protocol属性等于dubbo
        //      host属性等于192.168.31.246
        //      port属性等于20080
        //      path属性等于com.alibaba.dubbo.demo.DemoService
        //      其他是parameters属性
        创建new URL(name, host, port, (contextPath == null || contextPath.length() == 0 ? "" : contextPath + "/") + path, map)实例设置给url变量
        // 如果是override，此处起到的作用是删除某些参数
        如果ExtensionLoader.getExtensionLoader(ConfiguratorFactory.class).hasExtension(url.getProtocol())等于true
            设置url变量等于ExtensionLoader.getExtensionLoader(ConfiguratorFactory.class).getExtension(url.getProtocol()).getConfigurator(url).configure(url)
        // 至此，协议对应的URL已经初步得出，即协议和注册中心都是通过URL表示，接下来的流程都是通过ExtensionLoader（工厂） + SPI（实现） + URL（动态参数）来处理
        设置scope变量等于url.getParameter("scope")
        如果scope变量不等于none
            // 默认情况下（不设置scope），会做本地暴漏
            如果scope变量不等于remove且url参数的protocol属性不等于injvm
                // 如：injvm://127.0.0.1/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&dubbo=2.0.0&generic=false&interface=com.alibaba.dubbo.demo.DemoService&loadbalance=roundrobin&methods=sayHello&owner=danielli&pid=23193&sayHello.executes=2&side=provider&timestamp=1457199105180
                //      protocol属性等于injvm
                //      host属性等于127.0.0.1
                //      port属性等于0
                //      path属性等于com.alibaba.dubbo.demo.DemoService
                //      其他是parameters属性
                设置url变量的protocol属性等于injvm，host属性等于127.0.0.1，port属性等于0
                执行ExtensionLoader.getExtensionLoader(ProxyFactory.class).getAdaptiveExtension().getInvoker(ref, (Class) interfaceClass, url))方法返回invoker变量
                执行ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension().export(invoker)方法并返回exporter变量添加到exporters属性中
            // 默认情况下（不设置scope），会做远程暴漏
            如果scope变量不等于local
                // 是否注册到注册中心，是通过添加提供者协议url到注册中心url的参数，让接下来的流程处理，即Invoker中的URL可能是服务端提供者协议url，也可能是注册中心url内嵌提供者协议url
                如果registryURLs参数不等于null并且个数大于0，且url.getParameter("register", true)等于true
                    遍历registryURLs参数      // 暴漏服务，且注册到注册中心
                        如果url变量中不包含dynamic指定key，添加dynamic和registryURL.getParameter("dynamic")到url中
                        调用loadMonitor(registryURL)返回monitorUrl变量，如果monitorUrl变量不等于null，添加monitor和monitorUrl变量的字符串形式到url的参数中
                        添加export和url变量的字符串形式到registryUrl的参数中
                        执行ExtensionLoader.getExtensionLoader(ProxyFactory.class).getAdaptiveExtension().getInvoker(ref, (Class) interfaceClass, registryURL))方法返回invoker变量
                        执行ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension().export(invoker)方法并返回exporter变量添加到exporters属性中
                否则                         // 暴漏服务，不注册到注册中心
                    执行ExtensionLoader.getExtensionLoader(ProxyFactory.class).getAdaptiveExtension().getInvoker(ref, (Class) interfaceClass, url))方法返回invoker变量
                    执行ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension().export(invoker)方法并返回exporter变量添加到exporters属性中
        加入url变量到urls属性中
    protected URL loadMonitor(URL registryURL)
        此部分主要用于拼接得到监控url，主要的大方法执行逻辑，export中都有，不再做分析
    public synchronized void unexport()
        如果exported属性等于false，则退出
        如果unexported属性等于true，则退出
        如果exporters属性不等于null并且个数大于0，进行遍历
            执行exporter.unexport()
        清空exporters
        设置unexported为true

## ReferenceConfig
    public synchronized T get()
        如果destroyed属性等于true，抛异常IllegalStateException(Already destroyed!)
        如果ref等于null，调用init()
        返回ref
    private void init()
        如果initialized属性等于true，退出
        设置initialized属性等于true
        如果interfaceName属性等于null
            抛异常IllegalStateException(<dubbo:reference interface="" /> interface not allow null!)
        如果consumer属性等于null，创建new ConsumerConfig()实例并设置给consumer
        遍历consumer属性的所有setter方法，通过系统属性进行填充，填充规则为：
            dubbo.consumer. + id + property
            dubbo.consumer + property
            如果存在getter方法，并且对应getter方法返回的是null
                根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                读取文件作为Properties，按照dubbo.consumer. + id + property、dubbo.consumer + property规则调用setter方法
        遍历当前实例的所有setter方法，通过系统属性进行填充，填充规则为：
            dubbo.reference. + id + property
            dubbo.reference. + property
            如果存在getter方法，并且对应getter方法返回的是null
                根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                读取文件作为Properties，按照dubbo.reference. + id + property、dubbo.reference. + property规则调用setter方法
        如果generic属性等于null，设置generic属性等于consumer.getGeneric()
        如果isGeneric()等于true
            设置interfaceClass属性等于GenericService.class
        否则
            加载类名为interfaceName属性，赋值给interfaceClass属性，如果不存在，抛异常IllegalStateException
            校验interfaceClass属性必须不等于null并且是接口类型，检测methods属性中的所有MethodConfig实例的name属性都是interfaceClass中的方法名
        获取系统属性interfaceName值，如果值等于null
            获取系统属性dubbo.resolve.file，如果属性值等于null            // 暂无其他逻辑生成此文件，只能手动创建
                创建new File(new File(System.getProperty("user.home")), "dubbo-resolve.properties")实例，如果文件存在
                    创建实例new Properties()实例，并加载文件内容，复制给properties变量
                    获取properties变量中interfaceName属性值，如果属性值不等于null
                        设置属性值给url属性
        如果application属性等于null，设置application属性等于consumer.getApplication()
        如果module属性等于null，设置module属性等于consumer.getModule()
        如果registries属性等于null，设置registries属性等于consumer.getRegistries()
        如果monitor属性等于null，设置monitor属性等于consumer.getMonitor()
        如果module属性不等于null且registries属性等于null，设置registries属性等于module.getRegistries()
        如果module属性不等于null且monitor属性等于null，设置monitor属性等于module.getMonitor()
        如果application属性不等于null且registries属性等于null，设置registries属性等于application.getRegistries()
        如果application属性不等于null且monitor属性等于null，设置monitor属性等于application.getMonitor()
        如果application属性等于null
            如果dubbo属性文件中dubbo.application.name不等于null，创建new ApplicationConfig()实例并设置给application属性
        如果application属性等于null，抛异常IllegalStateException(No such application config! Please add <dubbo:application name="..." /> to your spring config.)
        遍历application属性的所有setter方法，通过系统属性进行填充，填充规则为：
            dubbo.application. + id + property
            dubbo.application. + property
            如果存在getter方法，并且对应getter方法返回的是null
                根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                读取文件作为Properties，按照dubbo.application. + id + property、dubbo.application. + property规则调用setter方法
        如果dubbo配置文件中存在属性dubbo.service.shutdown.wait，添加到系统属性中
        否则
            如果dubbo配置文件中存在属性dubbo.service.shutdown.wait.seconds，添加到系统属性中
        如果stub属性不等于null、"false"、"0"、"null"、"N/A"
            如果stub属性等于"true"或者"default"，加载interfaceClass + "Stub"，否则加载stub类
            验证加载的类必须是interfaceClass子类且存在interfaceClass属性参数的构造方法
        如果mock属性不等于null、"false"、"0"、"null"、"N/A"
            如果mock属性以return 为前缀，截取return 以后的部分，通过调用MockInvoker.parseMockValue(value)检测是否合理
            否则
                如果mock属性等于"true"或者"default"，加载interfaceClass + "Mock"，否则加载mock类
                验证加载的类必须是interfaceClass子类且存在无参的构造方法
        创建new HashMap<String, String>()实例，设置给map变量
        添加side和consumer到map变量中
        添加dubbo和Version类对应的jar版本号到map变量中
        添加timestamp和当前系统时间戳到map变量中
        如果可以获取到系统pid且大于0
            添加pid和系统pid到map变量中
        如果isGeneric()等于false
            获取interfaceClass所在的jar的版本（如果找不到，使用version属性）设置给revision变量
            如果revision变量不等于null
                添加revision和revision变量到map变量中
            设置methods变量等于Wrapper.getWrapper(interfaceClass).getMethodNames()
            如果methods长度为0
                添加methods和*到map变量中
            否则
                添加methods和使用,进行合并的methods到map变量中
        添加interface和interfaceName属性到map变量中
        将application属性、module属性中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中
        将consumer属性中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用default.field）
        将this中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中
        通过map变量中的指定keys（group、interface、version）生成serviceKey变量
        创建new HashMap<String, String>()实例，设置给attribute变量
        如果methods属性不等于null且个数大于0，进行遍历
            将methodConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中（key使用methodConfigName.field）
            如果map变量中包含methodConfigName.retry指定key，移除指定key，如果对应的value等于false，添加methodConfigName.retries和0到map变量中
            将methodConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，如果注解的attribute()方法返回true，添加其field和value到attribute变量中（key使用methodConfigName.field）
            如果methodConfig.isReturn()等于false并且（methodConfig.getOnreturn()不等于null或者methodConfig.getOnthrow()不等于null），抛异常IllegalStateException(method config error : return attribute must be set true when onreturn or onthrow has been setted.)
            使用serviceKey、methodConfigName、"onreturn.method"字符串生成onReturnMethodKey变量，获取attribute中onReturnMethodKey变量的value值
            如果onReturnMethodKey变量的value值不等于null并且是String类型
                添加onReturnMethodKey变量和methodConfig.getOnreturn().getClass()中onReturnMethodKey变量值对应的Method实例到attributes变量中
            使用serviceKey、methodConfigName、"onthrow.method"字符串生成onThrowMethodKey变量，获取attribute中onThrowMethodKey变量的value值
            如果onThrowMethodKey变量的value值不等于null并且是String类型
                添加onThrowMethodKey变量和methodConfig.getOnthrow().getClass()中onThrowMethodKey变量值对应的Method实例到attributes变量中
            使用serviceKey、methodConfigName、"oninvoke.method"字符串生成onInvokeMethodKey变量，获取attribute中onInvokeMethodKey变量的value值
                如果onInvokeMethodKey变量值不等于null并且是String类型
                    添加onInvokeMethodKey变量和methodConfig.getOninvoke().getClass()中onInvokeMethodKey变量值对应的Method实例到attributes变量中
        执行StaticContext.getSystemContext().putAll(attributes)       // 进程级别的ConcurrentHashMap
        设置ref属性等于createProxy(map)
    private T createProxy(Map<String, String> map)
        // 如：temp://localhost?application=demo-consumer&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=23627&side=consumer&timestamp=1457206162748
        //      protocol属性等于temp
        //      host属性等于localhost
        //      port属性等于0
        //      path属性等于null
        //      其他是parameters属性
        创建new URL("temp", "localhost", 0, map)实例设置给tmpUrl变量
        如果injvm属性等于null
            如果url属性不等于null              // 直连情况
                设置isJvmRefer变量为false
            // 默认情况下，都能获取到（interface、group、version）
            否则如果ExtensionLoader.getExtensionLoader(Protocol.class).getExtension("injvm").isInjvmRefer(tmpUrl)等于true     // protocol等于injvm或者scope参数等于remote或者generic参数等于true，返回false；如果scope参数等于local或者injvm参数等于true或者本地指定服务暴漏，返回true；其他情况返回false
                设置isJvmRefer变量为true
            否则
                设置isJvmRefer变量为false
        否则
            设置isJvmRefer变量为injvm属性
        如果isJvmRefer变量等于true
            // 如：injvm://127.0.0.1/com.alibaba.dubbo.demo.DemoService?application=demo-consumer&dubbo=2.0.0&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&owner=danielli&pid=23627&side=consumer&timestamp=1457206162748
            //      protocol属性等于injvm
            //      host属性等于127.0.0.1
            //      port属性等于0
            //      path属性等于com.alibaba.dubbo.demo.DemoService
            //      其他是parameters属性
            创建new URL("injvm", "127.0.0.1", 0, interfaceClass.getName()).addParameters(map)实例设置给url变量
            调用ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension().refer(interfaceClass, url)返回给invoker属性
        否则
            如果url属性不等于null
                使用;分隔url属性成数组，如果数组不等于null并且个数大于0，进行遍历
                    执行URL.valueOf(u)设置给url变量
                    如果url变量的path属性等于null，设置url变量的path属性等于interfaceName属性
                    如果url变量的protocol属性等于registry，添加refer和StringUtils.toQueryString(map)到url变量的参数中，并添加url变量到urls属性中
                    否则
                        执行ClusterUtils.mergeUrl(url, map)设置给url变量，并添加url变量到urls属性中          // 此处生成的URL为消费者URL
            否则
                执行loadRegistries(false)设置给us变量，如果us变量不等于null并且个数大于0，进行遍历              // 返回的是注册中心URL
                    调用loadMonitor(registryURL)返回monitorUrl变量，如果monitorUrl变量不等于null，添加monitor和monitorUrl变量的字符串形式到map变量中
                    // 如：registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=32717&refer=application%3Ddemo-consumer%26dubbo%3D2.0.0%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D32717%26side%3Dconsumer%26timestamp%3D1457249154026&registry=zookeeper&timestamp=1457249156471
                    //      protocol属性等于registry
                    //      host属性等于127.0.0.1
                    //      port属性等于2181
                    //      path属性等于com.alibaba.dubbo.registry.RegistryService
                    //      其他是parameters属性
                    添加refer和StringUtils.toQueryString(map)到u变量的参数中，并添加u变量到urls属性中         // 可以假设为注册中心url内嵌消费者url，这里的消费者url实际为消费者的参数列表
                如果urls属性等于null或者个数等于0
                    抛异常IllegalStateException(No such any registry to reference...)
            // 下边的一个注册中心URL的服务，可能包含多个provider URL
            如果urls属性个数等于1       // 单个注册中心
                // 如：registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=34193&refer=application%3Ddemo-consumer%26dubbo%3D2.0.0%26interface%3Dcom.alibaba.dubbo.demo.DemoService%26methods%3DsayHello%26owner%3Ddanielli%26pid%3D34193%26side%3Dconsumer%26timestamp%3D1457270729249&registry=zookeeper&timestamp=1457270769633
                //      protocol属性等于registry
                //      host属性等于127.0.0.1
                //      port属性等于2181
                //      path属性等于com.alibaba.dubbo.registry.RegistryService
                //      其他是parameters属性
                调用ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension().refer(interfaceClass, urls.get(0))返回给invoker属性          // 单个场景
            否则                      // 多注册中心
                创建new ArrayList<Invoker<?>>()实例设置给invokers变量
                设置registryURL变量等于null
                遍历urls
                    调用ExtensionLoader.getExtensionLoader(Protocol.class).getAdaptiveExtension().refer(interfaceClass, url)返回添加到invokers变量中
                    如果url变量的protocol属性等于registry，设置registryURL变量等于url
                如果registryURL变量不等于null
                    添加cluster和available到registryURL变量的参数中           // 多注册中心，使用cluster=available标示
                    执行ExtensionLoader.getExtensionLoader(Cluster.class).getAdaptiveExtension().join(new StaticDirectory(registryUrl, invokers))给invoker属性   // 多个且存在注册中心
                否则
                    执行ExtensionLoader.getExtensionLoader(Cluster.class).getAdaptiveExtension().join(new StaticDirectory(invokers))给invoker属性                // 多个且不存在注册中心
            如果check属性等于null，设置check属性等于consumer.isCheck()
            如果check属性等于null
                设置check属性等于true
            如果check属性等于true，调用invoker.isAvailable()判断是否可用，如果为false，抛异常IllegalStateException(Failed to check the status of the service...)
            返回ExtensionLoader.getExtensionLoader(ProxyFactory.class).getAdaptiveExtension().getProxy(invoker)
    protected List<URL> loadRegistries(boolean provider)
        如果registries属性等于null或者个数等于0
            如果dubbo属性文件中dubbo.registry.address不等于null，创建new ArrayList<RegistryConfig>()实例并设置给registries属性
            使用|分隔dubbo.registry.address属性值进行遍历，创建new RegistryConfig()实例并设置address属性等于属性值，添加实例到registries属性中
        如果registries属性等于null或者个数等于0，抛异常IllegalStateException(No such any registry to...)
        遍历registries属性
            遍历registryConfig变量的所有setter方法，通过系统属性进行填充，填充规则为：
                dubbo.registry. + id + property
                dubbo.registry. + property
                如果存在getter方法，并且对应getter方法返回的是null
                    根据系统属性dubbo.properties.file、环境变量dubbo.properties.file得到文件名，如果不存在，使用默认名称dubbo.properties，
                    读取文件作为Properties，按照dubbo.registry. + id + property、dubbo.registry. + property规则调用setter方法
        创建new ArrayList<URL>()实例，设置给registryList变量
        如果registries不等于null并且个数大于0，进行遍历
            设置address变量等于registryConfig.getAddress()
            如果address变量等于null
                设置address变量等于0.0.0.0
            如果系统属性dubbo.registry.address存在，设置address变量等于其值
            如果address不等于null并且不等于N/A
                创建new HashMap<String, String>()实例，设置给map变量
                将application属性和registryConfig变量中，获取标注了@Parameter注解的getter方法进行遍历，添加其field和value到map变量中
                添加path和RegistryService.class到map变量中
                添加version和Version类对应的jar版本到map变量中
                添加timestamp和当前系统时间戳到map变量中
                如果可以获取到系统pid且大于0
                    添加pid和系统pid到map变量中
                如果map变量中不包含key名称等于protocol的元素
                    如果ExtensionLoader.getExtensionLoader(RegistryFactory.class).hasExtension("remote")等于true    // 默认没有
                        添加protocol和remote到map变量中
                    否则
                        添加protocol和dubbo到map变量中     // 如果registryConfig.protocol没设置，默认值用dubbo，最终address设置了，还是要参考address
                // 如：zookeeper://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=23031&timestamp=1457196712687
                //      protocol属性等于zookeeper
                //      host属性等于127.0.0.1
                //      port属性等于2181
                //      path属性等于com.alibaba.dubbo.registry.RegistryService
                //      其他是parameters属性
                使用;分隔address，调用UrlUtils.parseURL(address, map)方法返回URL实例，添加到urls变量中
                遍历urls
                    添加registry和url变量的protocol属性到url变量参数中
                    // 如：registry://127.0.0.1:2181/com.alibaba.dubbo.registry.RegistryService?application=demo-consumer&dubbo=2.0.0&owner=danielli&pid=23031&registry=zookeeper&timestamp=1457196712687
                    设置url变量的protocol属性等于属性等于registry
                    如果provider参数等于true并且url的register参数等于true（默认为true）或者provider参数等于false并且url的subscribe参数等于true（默认为true）
                        添加url到registryList变量中
        返回registryList变量
    public synchronized void destroy()
        如果ref属性等于null，退出
        如果destroyed属性等于true，退出
        设置destroyed属性等于true
        执行invoker.destroy()
        设置invoker属性和ref属性等于null
