---
layout: bootstrap
title: 'Cglib 动态代理使用及深入'
description: 'Cglib 动态代理使用及深入'
date: '2019-8-2 14:26:00'
author: Jast
---
## 编码步骤
### 定义被代理类及接口
```java
package cn.jastz.proxy.cglib;

/**
 * @author zhiwen
 */
public class HelloService {

    public void hello() {
        System.out.println("hello world");
    }
}

```
### 实现MethodInterceptor
```java
package cn.jastz.proxy.cglib;

import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * @author zhiwen
 */
public class ExecuteTimeMethodInterceptor implements MethodInterceptor {

    /**
     *
     * @param obj
     * @param method
     * @param args
     * @param proxy
     * @return
     * @throws Throwable
     */
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        long startTime = System.currentTimeMillis();
        System.out.println("start");
        Object result = proxy.invokeSuper(obj, args);
        System.out.println("end");
        long endTime = System.currentTimeMillis();
        System.out.println("execute time in millisecond is:" + (endTime - startTime));
        return result;
    }
}
```

### Cglib创建代理对象
```java
package cn.jastz.proxy.cglib;

import org.springframework.cglib.proxy.Enhancer;

/**
 * @author zhiwen
 */
public class HelloServiceClient {

    public static void main(String[] args) {
        Enhancer e = new Enhancer();
        //生成代理类继承的类
        e.setSuperclass(HelloService.class);
        //生成代理类实现的接口
        e.setInterfaces(new Class[]{});
        e.setCallback(new ExecuteTimeMethodInterceptor());
        HelloService helloService = (HelloService) e.create();
        helloService.hello();
    }
}

```

## 源码分析
1. `org.springframework.cglib.proxy.Enhancer#create()`
```java
public Object create() {
        classOnly = false;
        argumentTypes = null;
        return createHelper();
    }
```
2. `org.springframework.cglib.proxy.Enhancer#createHelper`
```java
private Object createHelper() {
        preValidate();
        Object key = KEY_FACTORY.newInstance((superclass != null) ? superclass.getName() : null,
                ReflectUtils.getNames(interfaces),
                filter == ALL_ZERO ? null : new WeakCacheKey<CallbackFilter>(filter),
                callbackTypes,
                useFactory,
                interceptDuringConstruction,
                serialVersionUID);
        this.currentKey = key;
        Object result = super.create(key);
        return result;
    }
```
3. `org.springframework.cglib.core.AbstractClassGenerator#create`
```java
protected Object create(Object key) {
        try {
            ClassLoader loader = getClassLoader();
            Map<ClassLoader, ClassLoaderData> cache = CACHE;
            ClassLoaderData data = cache.get(loader);
            if (data == null) {
                synchronized (AbstractClassGenerator.class) {
                    cache = CACHE;
                    data = cache.get(loader);
                    if (data == null) {
                        Map<ClassLoader, ClassLoaderData> newCache = new WeakHashMap<ClassLoader, ClassLoaderData>(cache);
                        data = new ClassLoaderData(loader);
                        newCache.put(loader, data);
                        CACHE = newCache;
                    }
                }
            }
            this.key = key;
            Object obj = data.get(this, getUseCache());
            if (obj instanceof Class) {
                return firstInstance((Class) obj);
            }
            return nextInstance(obj);
        }
        catch (RuntimeException | Error ex) {
            throw ex;
        }
        catch (Exception ex) {
            throw new CodeGenerationException(ex);
        }
    }
```
3.1. `org.springframework.cglib.core.AbstractClassGenerator.ClassLoaderData#ClassLoaderData`
创建ClassLoaderData
```java
public ClassLoaderData(ClassLoader classLoader) {
            if (classLoader == null) {
                throw new IllegalArgumentException("classLoader == null is not yet supported");
            }
            this.classLoader = new WeakReference<ClassLoader>(classLoader);
            Function<AbstractClassGenerator, Object> load =
                    new Function<AbstractClassGenerator, Object>() {
                        public Object apply(AbstractClassGenerator gen) {
                            Class klass = gen.generate(ClassLoaderData.this);
                            return gen.wrapCachedClass(klass);
                        }
                    };
            generatedClasses = new LoadingCache<AbstractClassGenerator, Object, Object>(GET_KEY, load);
        }
```

3.2. `org.springframework.cglib.core.AbstractClassGenerator#generate`
```java
protected Class generate(ClassLoaderData data) {
        Class gen;
        Object save = CURRENT.get();
        CURRENT.set(this);
        try {
            ClassLoader classLoader = data.getClassLoader();
            if (classLoader == null) {
                throw new IllegalStateException("ClassLoader is null while trying to define class " +
                        getClassName() + ". It seems that the loader has been expired from a weak reference somehow. " +
                        "Please file an issue at cglib's issue tracker.");
            }
            synchronized (classLoader) {
                String name = generateClassName(data.getUniqueNamePredicate());
                data.reserveName(name);
                this.setClassName(name);
            }
            if (attemptLoad) {
                try {
                    gen = classLoader.loadClass(getClassName());
                    return gen;
                }
                catch (ClassNotFoundException e) {
                    // ignore
                }
            }
            byte[] b = strategy.generate(this);
            String className = ClassNameReader.getClassName(new ClassReader(b));
            ProtectionDomain protectionDomain = getProtectionDomain();
            synchronized (classLoader) { // just in case
                // SPRING PATCH BEGIN
                gen = ReflectUtils.defineClass(className, b, classLoader, protectionDomain, contextClass);
                // SPRING PATCH END
            }
            return gen;
        }
        catch (RuntimeException | Error ex) {
            throw ex;
        }
        catch (Exception ex) {
            throw new CodeGenerationException(ex);
        }
        finally {
            CURRENT.set(save);
        }
    }
```
3.3 `org.springframework.cglib.core.DefaultGeneratorStrategy#generate`
```java
public byte[] generate(ClassGenerator cg) throws Exception {
        DebuggingClassWriter cw = this.getClassVisitor();
        this.transform(cg).generateClass(cw);
        return this.transform(cw.toByteArray());
    }
```
3.4 `org.springframework.cglib.proxy.Enhancer#generateClass`
```java
public void generateClass(ClassVisitor v) throws Exception {
        Class sc = (superclass == null) ? Object.class : superclass;

        if (TypeUtils.isFinal(sc.getModifiers()))
            throw new IllegalArgumentException("Cannot subclass final class " + sc.getName());
        List constructors = new ArrayList(Arrays.asList(sc.getDeclaredConstructors()));
        filterConstructors(sc, constructors);

        // Order is very important: must add superclass, then
        // its superclass chain, then each interface and
        // its superinterfaces.
        List actualMethods = new ArrayList();
        List interfaceMethods = new ArrayList();
        final Set forcePublic = new HashSet();
        getMethods(sc, interfaces, actualMethods, interfaceMethods, forcePublic);

        List methods = CollectionUtils.transform(actualMethods, new Transformer() {
            public Object transform(Object value) {
                Method method = (Method) value;
                int modifiers = Constants.ACC_FINAL
                        | (method.getModifiers()
                        & ~Constants.ACC_ABSTRACT
                        & ~Constants.ACC_NATIVE
                        & ~Constants.ACC_SYNCHRONIZED);
                if (forcePublic.contains(MethodWrapper.create(method))) {
                    modifiers = (modifiers & ~Constants.ACC_PROTECTED) | Constants.ACC_PUBLIC;
                }
                return ReflectUtils.getMethodInfo(method, modifiers);
            }
        });

        ClassEmitter e = new ClassEmitter(v);
        if (currentData == null) {
            e.begin_class(Constants.V1_2,
                    Constants.ACC_PUBLIC,
                    getClassName(),
                    Type.getType(sc),
                    (useFactory ?
                            TypeUtils.add(TypeUtils.getTypes(interfaces), FACTORY) :
                            TypeUtils.getTypes(interfaces)),
                    Constants.SOURCE_FILE);
        }
        else {
            e.begin_class(Constants.V1_2,
                    Constants.ACC_PUBLIC,
                    getClassName(),
                    null,
                    new Type[]{FACTORY},
                    Constants.SOURCE_FILE);
        }
        List constructorInfo = CollectionUtils.transform(constructors, MethodInfoTransformer.getInstance());

        e.declare_field(Constants.ACC_PRIVATE, BOUND_FIELD, Type.BOOLEAN_TYPE, null);
        e.declare_field(Constants.ACC_PUBLIC | Constants.ACC_STATIC, FACTORY_DATA_FIELD, OBJECT_TYPE, null);
        if (!interceptDuringConstruction) {
            e.declare_field(Constants.ACC_PRIVATE, CONSTRUCTED_FIELD, Type.BOOLEAN_TYPE, null);
        }
        e.declare_field(Constants.PRIVATE_FINAL_STATIC, THREAD_CALLBACKS_FIELD, THREAD_LOCAL, null);
        e.declare_field(Constants.PRIVATE_FINAL_STATIC, STATIC_CALLBACKS_FIELD, CALLBACK_ARRAY, null);
        if (serialVersionUID != null) {
            e.declare_field(Constants.PRIVATE_FINAL_STATIC, Constants.SUID_FIELD_NAME, Type.LONG_TYPE, serialVersionUID);
        }

        for (int i = 0; i < callbackTypes.length; i++) {
            e.declare_field(Constants.ACC_PRIVATE, getCallbackField(i), callbackTypes[i], null);
        }
        // This is declared private to avoid "public field" pollution
        e.declare_field(Constants.ACC_PRIVATE | Constants.ACC_STATIC, CALLBACK_FILTER_FIELD, OBJECT_TYPE, null);

        if (currentData == null) {
            emitMethods(e, methods, actualMethods);
            emitConstructors(e, constructorInfo);
        }
        else {
            emitDefaultConstructor(e);
        }
        emitSetThreadCallbacks(e);
        emitSetStaticCallbacks(e);
        emitBindCallbacks(e);

        if (useFactory || currentData != null) {
            int[] keys = getCallbackKeys();
            emitNewInstanceCallbacks(e);
            emitNewInstanceCallback(e);
            emitNewInstanceMultiarg(e, constructorInfo);
            emitGetCallback(e, keys);
            emitSetCallback(e, keys);
            emitGetCallbacks(e);
            emitSetCallbacks(e);
        }

        e.end_class();
    }
```
4. `org.springframework.cglib.proxy.Enhancer#nextInstance`
```java
protected Object nextInstance(Object instance) {
        EnhancerFactoryData data = (EnhancerFactoryData) instance;

        if (classOnly) {
            return data.generatedClass;
        }

        Class[] argumentTypes = this.argumentTypes;
        Object[] arguments = this.arguments;
        if (argumentTypes == null) {
            argumentTypes = Constants.EMPTY_CLASS_ARRAY;
            arguments = null;
        }
        return data.newInstance(argumentTypes, arguments, callbacks);
    }
```
5. `org.springframework.cglib.proxy.Enhancer.EnhancerFactoryData#newInstance`
```java
public Object newInstance(Class[] argumentTypes, Object[] arguments, Callback[] callbacks) {
            setThreadCallbacks(callbacks);
            try {
                // Explicit reference equality is added here just in case Arrays.equals does not have one
                if (primaryConstructorArgTypes == argumentTypes ||
                        Arrays.equals(primaryConstructorArgTypes, argumentTypes)) {
                    // If we have relevant Constructor instance at hand, just call it
                    // This skips "get constructors" machinery
                    return ReflectUtils.newInstance(primaryConstructor, arguments);
                }
                // Take a slow path if observing unexpected argument types
                return ReflectUtils.newInstance(generatedClass, argumentTypes, arguments);
            }
            finally {
                // clear thread callbacks to allow them to be gc'd
                setThreadCallbacks(null);
            }

        }
```
6. `org.springframework.cglib.core.ReflectUtils#newInstance(java.lang.Class, java.lang.Class[], java.lang.Object[])`

实际调用的是`java.lang.reflect.Constructor`来创建对象的

```java
@SuppressWarnings("deprecation")  // on JDK 9
    public static Object newInstance(final Constructor cstruct, final Object[] args) {
        boolean flag = cstruct.isAccessible();
        try {
            if (!flag) {
                cstruct.setAccessible(true);
            }
            Object result = cstruct.newInstance(args);
            return result;
        }
        catch (InstantiationException e) {
            throw new CodeGenerationException(e);
        }
        catch (IllegalAccessException e) {
            throw new CodeGenerationException(e);
        }
        catch (InvocationTargetException e) {
            throw new CodeGenerationException(e.getTargetException());
        }
        finally {
            if (!flag) {
                cstruct.setAccessible(flag);
            }
        }
    }
```