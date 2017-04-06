# ExtensionLoader（ExtensionFactory）
    读取META-INF/dubbo/internal/com.alibaba.dubbo.common.extension.ExtensionFactory、META-INF/dubbo/com.alibaba.dubbo.common.extension.ExtensionFactory、META-INF/services/com.alibaba.dubbo.common.extension.ExtensionFactory文件
        选定包含Adaptive注解的类作为cachedAdaptiveClass               // AdaptiveExtensionFactory
        选定包含Activate注解的类和name，加入到cachedActivates中        //
        将实际类名和name，添加到cachedNames中                         // spi=SpiExtensionFactory  spring=SpringExtensionFactory
        将name和实际类名，添加到cachedClasses                         // SpiExtensionFactory=spi  SpringExtensionFactory=spring
    创建cachedAdaptiveClass对应实例，设置给cachedAdaptiveInstance     // AdaptiveExtensionFactory

    AdaptiveExtensionFactory            // 聚合SpiExtensionFactory和SpringExtensionFactory
    SpiExtensionFactory
        public <T> T getExtension(Class<T> type, String name)
            如果type是接口，且包含SPI注解
                调用ExtensionLoader.getExtensionLoader(type)返回对应的ExtensionLoader
                如果loader的getSupportedExtensions()个数大于0
                    调用getAdaptiveExtension()并返回
            返回null
    SpringExtensionFactory              // 聚合所有ApplicationContext
        public <T> T getExtension(Class<T> type, String name)
            返回所有ApplicationContext中，beanname等于name，且是type类型的实例

# ExtensionLoader（非ExtensionFactory）
    private ExtensionLoader(Class<?> type)
        type不能为null、必须有SPI、必须是接口
        type=type
        objectFactory=AdaptiveExtensionFactory
    public Set<String> getSupportedExtensions()
        如果SPI的value长度大于0，设置cachedDefaultName等于value.split(",")[0]
        读取META-INF/dubbo/internal/serviceName、META-INF/dubbo/serviceName、META-INF/services/serviceName文件
            每行使用=分隔成name和class
            如果包含Adaptive注解
                验证所有实现中，只能有一个包含Adaptive注解，并将选定类作为cachedAdaptiveClass
            否则
                如果包含构造参数为type的构造器
                    将class加入到cachedWrapperClasses中
                如果包含默认构造器
                    如果不包含name，根据Extension注解或命名方式截取(substring(0, actualClass.length - type.length))，生成name
                    选定包含Activate注解的类和name，加入到cachedActivates中
                    将实际类名和name，添加到cachedNames中
                    将name和实际类名，添加到cachedClasses
        返回cachedClasses的keySet
    public List<T> getActivateExtension(URL url, String key, String group)
        获取url的key参数值，如果不等于null并且长度大于0，使用,分隔，调用getActivateExtension(url, values, group)
    public List<T> getActivateExtension(URL url, String[] values, String group)
        如果values不包含-default
            遍历所有cachedActivates
                如果group等于null或者长度等于0，或者entry.value.group()中包含group
                    通过entry.key获取对应的扩展实例，如果values不包含entry.key并且values不包含-entry.key并且@Activate注解的values()中的某个key存在于url.getParameters()中
                        添加values中
            排序values
        遍历values
            如果value不以-开头并且values中不包含-value
                获取对应实例，添加到list中                         // 跟排序规则有关，不是简单获取实例
        返回list

    public T getAdaptiveExtension()
        检测cachedAdaptiveInstance是否存在，存在直接返回
        执行getSupportedExtensions()确保所有实现已加载
        如果cachedAdaptiveClass不等于null，创建其实例，否则创建其Adapter类对应的实例
            检测所有方法，如果全部没有Adaptive注解，抛异常
            Adapter实现
                package typePackage;
                import com.alibaba.dubbo.common.extension.ExtensionLoader;
                public class typeClassName$Adpative implements typeClassName {

                    // 没有Adaptive注解的处理
                    public returnTypeName methodName(param1Type arg1, param2Type arg2...) throws Exception1, Exception2... {
                        throw new UnsupportedOperationException("method" + methodName + " of interface " + typeClassName + " is not adaptive method!")
                    }

                    // 有Adaptive注解的处理
                    public returnTypeName methodName(param1Type arg1, param2Type arg2...) throws Exception1, Exception2... {
                        // 找到URL类型的参数，此处假设参数4
                            if (arg4 == null)
                                throw new IllegalArgumentException("url == null");
                            URL url = arg4;
                        // 找到URL类型的参数，此处假设没有，遍历方法参数的所有非静态公共get方法，且返回类型是URL，找不到，则生成失败，直接抛异常，找到后就不在继续找了，假设参数4的get方法存在URL实例
                            if (arg4 == null)
                                throw new IllegalArgumentException("param4Type argument == null");
                            if (arg4.xxxMethod() == null)
                                throw new IllegalArgumentException("param4Type argument xxxMethod == null");
                            URL url = arg4.xxxMethod();

                        // 如果参数中存在com.alibaba.dubbo.rpc.Invocation类型，此处假设参数5有
                            if (arg5 == null)
                                throw new IllegalArgumentException("invocation == null");
                            String methodName = arg5.getMethodName();

                        // 根据Adaptive注解的value方法，如果长度为0，将typeClassName处理成value（大写替换成'.' + 小写）
                        // 获取过程用代码描述过于麻烦，简述，就是从url中获取指定value的值，对于protocol，是直接调用url.getProtocol()，如果value存在多个，尽可能用前面的，如果值都为null，用cachedDefaultName
                            String extName = XXX;
                            if(extName == null)
                                throw new IllegalStateException("Fail to get extension(typeClassName) name from url(" + url.toString() + ") use keys(value)");
                            typeClassName extension = (typeClassName) ExtensionLoader.getExtensionLoader(typeClassName).getExtension(extName);
                            return extension.methodName(arg1, arg2...);
                    }
                }
        遍历所有方法，获取setter方法对应的property，调用objectFactory.getExtension(method.getParameterTypes()[0], property)返回实例，如果存在，赋值
        将实例赋值给cachedAdaptiveInstance
    public T getExtension(String name)
        如果name的长度不大于0，抛异常
        如果name等于true
            如果cachedDefaultName长度不大于0或者等于true
                返回null
            返回getExtension(cachedDefaultName)
        判断cachedInstances中是否存在对应name的实例，如果存在，直接返回
        获取cachedClasses中指定name对应的class，如果不存在，抛异常
        创建对应实例，遍历所有方法，获取setter方法对应的property，调用objectFactory.getExtension(method.getParameterTypes()[0], property)返回实例，如果存在，赋值
        遍历所有cachedWrapperClasses
            通过现有实例调用wrapperClass有参构造函数，重新生成实例，并遍历所有方法，获取setter方法对应的property，调用objectFactory.getExtension(method.getParameterTypes()[0], property)返回实例，如果存在，赋值
        将name和实例添加到cachedInstances中并返回实例

# Wrapper
    public static Wrapper getWrapper(Class<?> c)
        如果当前类是动态类，获取父类，继续判断，知道找到顶级类            // 动态类根据是否是ClassGenerator.DC.class子类决定
        如果最终找到的类是Object类
            返回实现常量Wrapper                                    // 原理一样，不写了
        判断WRAPPER_MAP中是否存在c类型的实际，如果不存在，调用makeWrapper(c)方法返回Wrapper类，并添加到WRAPPER_MAP中，返回Wrapper类
    private static Wrapper makeWrapper(Class<?> c)
        生成原理
            public class Wrapper1 extends Wrapper {         // 1是累加数字
            	// pts的keySet
            	public static String[] pns;
            	// 公有属性、getter方法的属性名和返回类型、setter方法的属性名和第一个参数类型
            	public static Map<String, Class<?>> pts;
            	// 公有方法名称列表
            	public static String[] mns;
            	// 当前类定义的公有方法名称列表（非继承方法）
            	public static String[] dmns;

            	public static Class[] mts1;			// methodName1的参数类型列表
            	public static Class[] mts2;			// methodName2的参数类型列表

            	public String[] getPropertyNames(){
            		return pns;
            	}

            	public boolean hasProperty(String n) {
            		return pts.containsKey(n);
            	}

            	public Class getPropertyType(String n) {
            		return (Class) pts.get(n);
            	}

            	public String[] getMethodNames() {
            		return mns;
            	}

            	public String[] getDeclaredMethodNames() {
            		return dmns;
            	}

            	public void setPropertyValue(Object o, String n, Object v){
            		ClassName w;
            		try{
            			w = ((ClassName) o);
            		} catch (Throwable e){
            			throw new IllegalArgumentException(e);
            		}
            		if(n.equals("fieldName1")){
            			w.fieldName1 = (fieldType1) v;
            			return;
            		}
            		if(n.equals("fieldName2")){
            			w.fieldName2 = (fieldType2) v;
            			return;
            		}
            		throw new NoSuchPropertyException("Not found property " + n + " filed or setter method in class " + ClassName)
            	}

            	public Object getPropertyValue(Object o, String n){
            		ClassName w;
            		try{
            			w = ((ClassName) o);
            		} catch (Throwable e){
            			throw new IllegalArgumentException(e);
            		}
            		if(n.equals("fieldName1")){
            			return (Object) w.fieldName1;
            		}
            		if(n.equals("fieldName2")){
            			return (Object) w.fieldName2;
            		}
            		throw new NoSuchPropertyException("Not found property " + n + " filed or setter method in class " + ClassName)
            	}

            	public Object invokeMethod(Object o, String n, Class[] p, Object[] v) throws InvocationTargetException {
            		ClassName w;
            		try{
            			w = ((ClassName) o);
            		} catch (Throwable e){
            			throw new IllegalArgumentException(e);
            		}
            		try {
            			if (methodName1.equals(n) && p.length == methodName1ParameterTypeLength && p[0].getName().equals(methodName1ParameterType1)) {
            				return (Object) w.methodName1(v[0], v[1]...);
            			}
            		}  catch(Throwable e) {
            			throw new java.lang.reflect.InvocationTargetException(e);
            		}
            		try {
            			if (methodName2.equals(n) && p.length == methodName2ParameterTypeLength && p[0].getName().equals(methodName2ParameterType1)) {
            				return (Object) w.methodName2(v[0], v[1]...);
            			}
            		}  catch(Throwable e) {
            			throw new java.lang.reflect.InvocationTargetException(e);
            		}
            		throw new NoSuchMethodException("Not found method " + n + " in class " + ClassName)
            	}
            }
        