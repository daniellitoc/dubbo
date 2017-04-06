# xml配置
## dubbo:application
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ApplicationConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用ApplicationConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历ApplicationConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                从当前元素中获取对应的property属性值value，如果不等于null
                    如果property等于registry并且value等于N/A
                        创建
                            RegistryConfig registryConfig = new RegistryConfig();
                            registryConfig.setAddress(RegistryConfig.NO_AVAILABLE);
                        添加registry属性和registryConfig实例到beanDefinition的propertyValues中
                    如果property等于registry，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的registries属性为ManagedList
                    如果参数类型是基本数据类型，property等于version且value为0.0.0
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于monitor，并且Spring的Registry中不存在对应value的Bean或者对应value的Bean不等于MonitorConfig
                        兼容旧版本，实现忽略
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue

## dubbo:module
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ModuleConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用ModuleConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历ModuleConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                从当前元素中获取对应的property属性值value，如果不等于null
                    如果property等于registry并且value等于N/A
                        创建
                            RegistryConfig registryConfig = new RegistryConfig();
                            registryConfig.setAddress(RegistryConfig.NO_AVAILABLE);
                        添加registry属性和registryConfig实例到beanDefinition的propertyValues中
                    如果property等于registry，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的registries属性为ManagedList
                    如果参数类型是基本数据类型，property等于version且value为0.0.0
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于monitor，并且Spring的Registry中不存在对应value的Bean或者对应value的Bean不等于MonitorConfig
                        兼容旧版本，实现忽略
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue

## dubbo:registry
    创建new RootBeanDefinition()实例，设置其beanClass属性等于RegistryConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用RegistryConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历RegistryConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                从当前元素中获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    如果property等于protocol，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的protocols属性为ManagedList
                    如果参数类型是基本数据类型，property等于timeout且value为0
                        设置BeanDefinition的propertyValues中的timeout属性为null
                    如果参数类型是基本数据类型，property等于version且value为0.0.0
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于protocol，并且SPI实现中有对应的value，同时Spring的Registry中不包含此value对应的BeanDefinition
                        创建ProtocolConfig实例，设置其name属性等于value
                        设置BeanDefinition的propertyValues中的protocol属性为ProtocolConfig
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:monitor
    创建new RootBeanDefinition()实例，设置其beanClass属性等于MonitorConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用MonitorConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历MonitorConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    如果property等于protocol，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的protocols属性为ManagedList
                    如果参数类型是基本数据类型，property等于version且value为0.0.0
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于protocol，并且SPI实现中有对应的value，同时Spring的Registry中不包含此value对应的BeanDefinition
                        创建ProtocolConfig实例，设置其name属性等于value
                        设置BeanDefinition的propertyValues中的protocol属性为ProtocolConfig
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:provider
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ProviderConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用ProviderConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    获取所有子元素并遍历
        如果元素名称等于service
            判断第一个service元素的default属性是否存在，如果属性值等于null，设置BeanDefinition的propertyValues中的default属性为false
            将当前元素按照dubbo:service解析成新的BeanDefinition
            如果新的BeanDefinition不等于null
                设置新的BeanDefinition的propertyValues中的provider属性为new RuntimeBeanReference(id)实例
    遍历ProviderConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    如果property等于registry并且value等于N/A
                        创建
                            RegistryConfig registryConfig = new RegistryConfig();
                            registryConfig.setAddress(RegistryConfig.NO_AVAILABLE);
                        添加registry属性和registryConfig实例到beanDefinition的propertyValues中
                    如果property等于registry，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的registries属性为ManagedList
                    如果property等于protocol，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的protocols属性为ManagedList
                    如果参数类型是基本数据类型，property等于async且value为false
                        设置BeanDefinition的propertyValues中的async属性为null
                    如果参数类型是基本数据类型，property等于timeout且value为0
                        设置BeanDefinition的propertyValues中的timeout属性为null
                    如果参数类型是基本数据类型，property等于delay且value为false
                        设置BeanDefinition的propertyValues中的delay属性为null
                    如果参数类型是基本数据类型，property等于version且value为0.0.0
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于protocol，并且SPI实现中有对应的value，同时Spring的Registry中不包含此value对应的BeanDefinition
                        创建ProtocolConfig实例，设置其name属性等于value
                        设置BeanDefinition的propertyValues中的protocol属性为ProtocolConfig
                    如果property等于monitor，并且Spring的Registry中不存在对应value的Bean或者对应value的Bean不等于MonitorConfig
                        兼容旧版本，实现忽略
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:consumer
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ConsumerConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用ConsumerConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    获取所有子元素遍历
        如果元素名称等于reference
            判断第一个reference元素的default属性是否存在，如果不存在或为空，设置BeanDefinition的propertyValues中的default属性为false
            将当前元素按照dubbo:reference解析成新的BeanDefinition
            如果新的BeanDefinition不等于null
                设置新的BeanDefinition的propertyValues中的consumer属性为new RuntimeBeanReference(id)实例
    遍历ConsumerConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)到ManagedMap中
                    如果property等于registry并且value等于N/A
                        创建
                            RegistryConfig registryConfig = new RegistryConfig();
                            registryConfig.setAddress(RegistryConfig.NO_AVAILABLE);
                        添加registry属性和registryConfig实例到beanDefinition的propertyValues中
                    如果property等于registry，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的registries属性为ManagedList
                    如果参数类型是基本数据类型，property等于async且value为false
                        设置BeanDefinition的propertyValues中的async属性为null
                    如果参数类型是基本数据类型，property等于timeout且value为0
                        设置BeanDefinition的propertyValues中的timeout属性为null
                    如果参数类型是基本数据类型，property等于version且value为0.0.0
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于monitor，并且Spring的Registry中不存在对应value的Bean或者对应value的Bean不等于MonitorConfig
                        兼容旧版本，实现忽略
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:protocol
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ProtocolConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用dubbo作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历Spring的Registry中所有的BeanDefinition                                                         // 根据元素位置决定，无法保证全部转换，可能先执行dubbo:protocol元素，然后执行初始化其他元素
        如果BeanDefinition中protocol的value不等于空且value是ProtocolConfig类型，同时value等于当前id变量
            设置对应beanDefinition中的protocol属性为new RuntimeBeanReference(id)实例
    遍历ProtocolConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:service
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ServiceBean，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用interface属性作为beanName变量
        如果beanName变量等于null，使用ServiceBean作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    如果class属性存在
        创建new RootBeanDefinition()实例，设置其beanClass属性等于元素的class属性，设置其lazyInit属性等于false
        如果包含子元素，遍历所有子元素
            如果元素名称等于property
                获取元素name属性，如果不等于null
                    获取value属性和ref属性
                    如果value属性存在，设置classDefinition的propertyValues中的name等于value
                    如果ref属性存在，设置classDefinition的propertyValues中的name等于new RuntimeBeanReference(ref)实例
                    否则
                        抛出异常，异常信息为<property name="name"> sub tag, Only supported <property name="name" ref="" /> or <property name="name" value="" />"
        设置BeanDefinition的propertyValues中的ref属性为new BeanDefinitionHolder(classDefinition, id + "Impl")
    遍历ServiceBean的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    如果property等于methods
                        创建ManagedList类型的局部变量methods
                        遍历当前元素的所有子元素
                            如果元素名称等于method
                                如果不存在name属性，抛出new IllegalStateException("<dubbo:method> name attribute == null")，否则获取作为methodName
                                如果methods等于null，赋值为new ManagedList()
                                解析当前元素作为新的BeanDefinition实例
                                创建new BeanDefinitionHolder(methodBeanDefinition, id + methodName)实例，添加到methods中
                        如果methods不等于null
                            设置BeanDefinition的propertyValues中的methods属性为methods
                    如果property等于registry并且value等于N/A
                        创建
                            RegistryConfig registryConfig = new RegistryConfig();
                            registryConfig.setAddress(RegistryConfig.NO_AVAILABLE);
                        添加registry属性和registryConfig实例到beanDefinition的propertyValues中
                    如果property等于registry，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的registries属性为ManagedList
                    如果property等于provider，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的providers属性为ManagedList
                    如果property等于protocol，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的protocols属性为ManagedList
                    如果参数类型是基本数据类型，property等于async且value为false
                        设置BeanDefinition的propertyValues中的async属性为null
                    如果参数类型是基本数据类型，property等于timeout且value为0
                        设置BeanDefinition的propertyValues中的timeout属性为null
                    如果参数类型是基本数据类型，property等于delay且value为false
                        设置BeanDefinition的propertyValues中的delay属性为null
                    如果参数类型是基本数据类型，property等于version且value为false
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于protocol，并且SPI实现中有对应的value，同时Spring的Registry中不包含此value对应的BeanDefinition
                        创建ProtocolConfig实例，设置其name属性等于value
                        设置BeanDefinition的propertyValues中的protocol属性为ProtocolConfig
                    如果property等于monitor，并且Spring的Registry中不存在对应value的Bean或者对应value的Bean不等于MonitorConfig
                        兼容旧版本，实现忽略
                    如果property等于ref，并且并且Spring的Registry中存在对应value的Bean
                        获取对应的BeanDefinition，判断是否是单例类型，如果不是
                            抛出异常，异常信息为：The exported service ref 'value' must be singleton! Please set the 'value' bean scope to singleton, eg: <bean id="value" scope="singleton" />
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的ref属性为beanReference
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:method
    创建new RootBeanDefinition()实例，设置其beanClass属性等于MethodConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用MethodConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历MethodConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    如果property等于arguments
                        创建ManagedList类型的局部变量arguments
                        遍历当前元素的所有子元素
                            如果元素名称等于argument
                                获取index属性值
                                如果arguments等于null，赋值为new ManagedList()
                                解析当前元素作为新的BeanDefinition实例
                                创建new BeanDefinitionHolder(argumentBeanDefinition, id + index)实例，添加到arguments中
                        如果arguments不等于null
                            设置BeanDefinition的propertyValues中的arguments属性为arguments
                    如果参数类型是基本数据类型，property等于async且value为false
                        设置BeanDefinition的propertyValues中的async属性为null
                    如果参数类型是基本数据类型，property等于timeout且value为0
                        设置BeanDefinition的propertyValues中的timeout属性为null
                    如果参数类型是基本数据类型，property等于stat且value为-1
                        设置BeanDefinition的propertyValues中的stat属性为null
                    如果参数类型是基本数据类型，property等于reliable且value为false
                        设置BeanDefinition的propertyValues中的reliable属性为null
                    如果property等于onreturn
                        获取最后一个'.'标示符之前的部分作为returnRef，之后的部分作为returnMethod
                        设置BeanDefinition的propertyValues中的onreturnMethod属性为returnMethod
                        设置BeanDefinition的propertyValues中的onreturn属性为new RuntimeBeanReference(returnRef)实例
                    如果property等于onthrow
                        获取最后一个'.'标示符之前的部分作为throwRef，之后的部分作为throwMethod
                        设置BeanDefinition的propertyValues中的onthrowMethod属性为throwMethod
                        设置BeanDefinition的propertyValues中的onthrow属性为new RuntimeBeanReference(throwRef)实例
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue

## dubbo:argument
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ArgumentConfig，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用ArgumentConfig作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历ArgumentConfig的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue

## dubbo:reference
    创建new RootBeanDefinition()实例，设置其beanClass属性等于ReferenceBean，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用interface属性作为beanName变量
        如果beanName变量等于null，使用ReferenceBean作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历ReferenceBean的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    如果property等于parameters
                        遍历当前元素的所有子元素
                            如果元素名称等于parameter
                                获取key，value属性值，如果hide属性为true，设置key等于'.' + key
                                添加key和new TypedStringValue(value, String.class)实例到ManagedMap中
                    如果property等于methods
                        创建ManagedList类型的局部变量methods
                        遍历当前元素的所有子元素
                            如果元素名称等于method
                                如果不存在name属性，抛出new IllegalStateException("<dubbo:method> name attribute == null")，否则获取作为methodName
                                如果methods等于null，赋值为new ManagedList()
                                解析当前元素作为新的BeanDefinition实例
                                创建new BeanDefinitionHolder(methodBeanDefinition, id + methodName)实例，添加到methods中
                        如果methods不等于null
                            设置BeanDefinition的propertyValues中的methods属性为methods
                    如果property等于registry并且value等于N/A
                        创建
                            RegistryConfig registryConfig = new RegistryConfig();
                            registryConfig.setAddress(RegistryConfig.NO_AVAILABLE);
                        添加registry属性和registryConfig实例到beanDefinition的propertyValues中
                    如果property等于registry，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的registries属性为ManagedList
                    如果property等于protocol，并且value包含,
                        按,分隔，创建RuntimeBeanReference实例到ManagedList中
                        设置BeanDefinition的propertyValues中的protocols属性为ManagedList
                    如果参数类型是基本数据类型，property等于async且value为false
                        设置BeanDefinition的propertyValues中的async属性为null
                    如果参数类型是基本数据类型，property等于timeout且value为0
                        设置BeanDefinition的propertyValues中的timeout属性为null
                    如果参数类型是基本数据类型，property等于version且value为false
                        设置BeanDefinition的propertyValues中的version属性为null
                    如果property等于protocol，并且SPI实现中有对应的value，同时Spring的Registry中不包含此value对应的BeanDefinition
                        创建ProtocolConfig实例，设置其name属性等于value
                        设置BeanDefinition的propertyValues中的protocol属性为ProtocolConfig
                    如果property等于monitor，并且Spring的Registry中不存在对应value的Bean或者对应value的Bean不等于MonitorConfig
                        兼容旧版本，实现忽略
                    否则
                        创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

## dubbo:annotation
    创建new RootBeanDefinition()实例，设置其beanClass属性等于AnnotationBean，设置其lazyInit属性等于false
    获取当前元素的id属性作为id变量
    如果id变量等于null，获取name属性作为beanName变量
        如果beanName变量等于null，使用AnnotationBean作为beanName变量
        设置id变量为beanName
        判断如果Spring的Registry中存在此id，则设置 id = id + count++ 来保证唯一
    如果id变量不等于null
        如果Spring的Registry中存在此id，抛异常IllegalStateException(Duplicate spring bean id + id)
        否则
            注册当前id和RootBeanDefinition实例到Spring的Registry中，设置RootBeanDefinition的propertyValues中的id属性为id变量
    遍历AnnotationBean的方法
        如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
            根据方法名求出属性property
            判断get方法是否存在，如果存在，并且访问修饰符是public，同时return类型等于set方法的参数类型
                获取对应的property属性值value，如果不等于null
                    创建对应的RuntimeBeanReference实例，设置BeanDefinition的propertyValues中的property属性为beanReference
            否则
                continue
    遍历元素的所有属性，如果属性不属于set类属性，添加到ManagedMap中，最后设置BeanDefinition的propertyValues中的parameters属性为ManagedMap

# 启动
## ServiceBean：实现了InitializingBean、DisposableBean、ApplicationContextAware、ApplicationListener、BeanNameAware
    public void setApplicationContext(ApplicationContext applicationContext)
        设置上下文
        SpringExtensionFactory.addApplicationContext(applicationContext);
        其他情况为兼容代码，可以不考虑，Spring 3.0源码自动将ApplicationListener加入到ApplicationContext中
    public void afterPropertiesSet()
        如果provider属性等于null
            从ApplicationContext中获取所有类型为ProviderConfig类型的实例
            如果存在且个数大于0
                验证所有ProviderConfig中，只能有一个ProviderConfig实例的isDefault等于null或等于true
                    如果存在，调用setProvider(providerConfig)
        如果application属性等于null，并且provider属性等于null或provider属性的application属性等于null
            从ApplicationContext中获取所有类型为ApplicationConfig类型的实例
            如果存在且个数大于0
                验证所有ApplicationConfig中，只能有一个ApplicationConfig实例的isDefault等于null或等于true
                    如果存在，调用setApplication(applicationConfig)
        如果module属性等于null，并且provider属性等于null或provider属性的module属性等于null
            从ApplicationContext中获取所有类型为ModuleConfig类型的实例
            如果存在且个数大于0
                验证所有ModuleConfig中，只能有一个ModuleConfig实例的isDefault等于null或等于true
                    如果存在，调用setModule(moduleConfig)
        如果registries属性等于null或者个数等于0，并且provider属性等于null或provider属性的registries属性等于null或者个数等于0，并且application属性等于null或application的registries属性等于null或者个数等于0
            从ApplicationContext中获取所有类型为RegistryConfig类型的实例
                如果存在且个数大于0
                    收集所有RegistryConfig中，isDefault等于null或等于true的实例，如果存在，最后调用setRegistries(registryConfigs)
        如果monitor属性等于null，并且provider属性等于null或provider属性的monitor属性为null，并且application属性等于null或application的monitor属性等于null
            从ApplicationContext中获取所有类型为MonitorConfig类型的实例
                如果存在且个数大于0
                    验证所有MonitorConfig中，只能有一个MonitorConfig实例的isDefault等于null或等于true
                        如果存在，调用setMonitor(monitorConfig)
        如果protocols属性等于null或者个数等于0，并且provider属性等于null或provider属性的protocols属性等于null或者个数等于0，并且application属性等于null或application的protocols属性等于null或者个数等于0
            从ApplicationContext中获取所有类型为ProtocolConfig类型的实例
                如果存在且个数大于0
                    收集所有ProtocolConfig中，isDefault等于null或等于true的实例，如果存在，最后调用setProtocols(protocolConfigs)
        如果path等于null或长度为0
            如果interfaceName属性不等于空，并且beanName按照interfaceName开头，调用setPath(beanName)
        如果isDelay()等于false
            调用export()
    public void destroy()
        调用unexport()
    public void onApplicationEvent(ApplicationEvent event)
        如果event的类名等于ContextRefreshedEvent
        如果isDelay()等于true，并且exported属性等于false，并且unexported属性等于false
            调用export()
    private boolean isDelay()
        如果depay属性等于null，获取provider的delay属性作为临时变量
        如果临时变量delay不等于null并且不等于-1，返回true，否则返回false

## ReferenceBean：实现了ApplicationContextAware、InitializingBean、DisposableBean、FactoryBean
    public void setApplicationContext(ApplicationContext applicationContext)
        设置上下文
        SpringExtensionFactory.addApplicationContext(applicationContext);
    public void afterPropertiesSet()
        如果consumer属性等于null
            从ApplicationContext中获取所有类型为ConsumerConfig类型的实例
            如果存在且个数大于0
                验证所有ConsumerConfig中，只能有一个ConsumerConfig实例的isDefault等于null或等于true
                    如果存在，setConsumer(consumerConfig)
        如果application属性等于null，并且consumer属性等于null或consumer属性的application属性等于null
            从ApplicationContext中获取所有类型为ApplicationConfig类型的实例
            如果存在且个数大于0
                验证所有ApplicationConfig中，只能有一个ApplicationConfig实例的isDefault等于null或等于true
                    如果存在，调用setApplication(applicationConfig)
        如果module属性等于null，并且consumer属性等于null或consumer属性的module属性等于null
            从ApplicationContext中获取所有类型为ModuleConfig类型的实例
            如果存在且个数大于0
                验证所有ModuleConfig中，只能有一个ModuleConfig实例的isDefault等于null或等于true
                    如果存在，调用setModule(moduleConfig)
        如果registries属性等于null或者个数等于0，并且consumer属性等于null或consumer属性的registries属性等于null或者个数等于0，并且application属性等于null或application的registries属性等于null或者个数等于0
            从ApplicationContext中获取所有类型为RegistryConfig类型的实例
                如果存在且个数大于0
                    收集所有RegistryConfig中，isDefault等于null或等于true的实例，如果存在，最后调用setRegistries(registryConfigs)
        如果monitor属性等于null，并且consumer属性等于null或consumer属性的monitor属性为null，并且application属性等于null或application的monitor属性等于null
            从ApplicationContext中获取所有类型为MonitorConfig类型的实例
                如果存在且个数大于0
                    验证所有MonitorConfig中，只能有一个MonitorConfig实例的isDefault等于null或等于true
                        如果存在，调用setMonitor(monitorConfig)
        如果init属性等于空，获取consumer的init属性作为临时变量
        如果临时变量init等于true，调用getObject()方法
    public Class<?> getObjectType()
        返回getInterfaceClass()
    public Object getObject()
        返回get()

## AnnotationBean：实现DisposableBean、BeanFactoryPostProcessor、BeanPostProcessor、ApplicationContextAware
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory)
        创建new ClassPathBeanDefinitionScanner(beanFactory, true)实例，添加Filter（只对@Service标示的Bean生效）
            对annotationPackage使用,分隔，调用scan方法
    public Object postProcessBeforeInitialization(Object bean, String beanName)
        如果bean参数的class属性不以指定的package开头，返回bean
        遍历bean参数的方法
            如果方法大于3个字符，并且以set开头，并且访问修饰符是public，并且只有一个参数
                如果方法上标识了Reference注解，调用refer(Reference, method.getParameterTypes()[0])返回实例，如果实例不等于null，调用set方法
        遍历bean参数的所有field
            如果field上标识了Reference注解，调用refer(Reference, field.getType())返回实例，如果实例不等于null，调用set方法
    public Object postProcessAfterInitialization(Object bean, String beanName)
        如果bean参数的class属性不以指定的package开头，返回bean
        如果类名上标识了Service注解
            创建new ServiceBean<Object>(service)实例（构造方法包含参数）
            如果service注解的interfaceClass属性是void，并且interfaceName属性等于""
                如果当前bean参数的class的实现接口个数大于0，获取第0个，设置serviceConfig.setInterface(bean.getClass().getInterfaces()[0])
                否则抛出异常
                    Failed to export remote service class " + bean.getClass().getName() + ", cause: The @Service undefined interfaceClass or interfaceName, and the service class unimplemented any interfaces.
            设置serviceConfig.setApplicationContext(applicationContext)
            如果service.registry个数大于0
                遍历从applicationContext中获取对应的RegistryConfig实例，最后调用serviceConfig.setRegistries(registryConfigs)
            如果service.provider不等于空
                从applicationContext中获取对应的ProviderConfig实例，最后调用serviceConfig.setProvider(providerConfig)
            如果service.monitor不等于空
                从applicationContext中获取对应的MonitorConfig实例，最后调用serviceConfig.setMonitor(monitorConfig)
            如果service.application不等于空
                从applicationContext中获取对应的ApplicationConfig实例，最后调用serviceConfig.setApplication(applicationConfig)
            如果service.module不等于空
                从applicationContext中获取对应的ModuleConfig实例，最后调用serviceConfig.setModule(moduleConfig)
            如果service.provider不等于空
                从applicationContext中获取对应的ProviderConfig实例，最后调用serviceConfig.setProvider(providerConfig)
            如果service.protocol个数大于0
                遍历从applicationContext中获取对应的ProtocolConfig实例，最后调用serviceConfig.setProtocols(protocolConfigs)
            调用serviceConfig.afterPropertiesSet()
            调用serviceConfig.setRef(bean)
            添加serviceConfig到serviceConfigs中
            调用serviceConfig.export()
        返回bean
    private Object refer(Reference reference, Class<?> referenceClass)
        如果reference参数的interfaceName属性不等于"",设置给临时变量interfaceName
        否则判断reference参数的interfaceClass属性是否不等于void，如果是，设置给临时变量interfaceName
        否则判断referenceClass参数是否是interface，如果是，设置给临时变量interfaceName
        否则抛出异常
            The @Reference undefined interfaceClass or interfaceName, and the property type " + referenceClass.getName() + " is not a interface.
        使用临时变量interfaceName、reference参数的group、version生成key，判断referenceConfigs属性中是否存在，如果存在，直接返回
        创建new ReferenceBean<Object>(reference)实例（构造方法包含参数）
        如果reference参数的interfaceClass属性等于void，并且interfaceName属性等于""，并且referenceClass参数是interface
            执行referenceConfig.setInterface(referenceClass)
        设置referenceConfig.setApplicationContext(applicationContext)
        如果reference.registry个数大于0
            遍历从applicationContext中获取对应的RegistryConfig实例，最后调用referenceConfig.setRegistries(registryConfigs)
        如果reference.consumer不等于空
            从applicationContext中获取对应的ConsumerConfig实例，最后调用referenceConfig.setConsumer(consumerConfig)
        如果reference.monitor不等于空
            从applicationContext中获取对应的MonitorConfig实例，最后调用referenceConfig.setMonitor(monitorConfig)
        如果reference.application不等于空
            从applicationContext中获取对应的ApplicationConfig实例，最后调用referenceConfig.setApplication(applicationConfig)
        如果reference.module不等于空
            从applicationContext中获取对应的ModuleConfig实例，最后调用referenceConfig.setModule(moduleConfig)
        如果reference.consumer不等于空                                                                                // 重复代码
            从applicationContext中获取对应的ConsumerConfig实例，最后调用referenceConfig.setConsumer(consumerConfig)
        调用referenceConfig.afterPropertiesSet()
        添加key和referenceConfig到referenceConfigs属性中
        返回referenceConfigs属性中，指定key的实例
    public void destroy()
        遍历serviceConfigs属性，调用unexport()方法
        遍历referenceConfigs属性，调用destroy()方法
            