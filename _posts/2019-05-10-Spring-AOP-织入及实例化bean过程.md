---
layout: bootstrap
title: 'Spring-AOP-织入及实例化bean过程'
description: 'Spring-AOP-织入及实例化bean过程'
date: '2019-05-10 16:54:00'
author: Jast
---
[How can I make the aspect work on the method AService.methodB()?](https://stackoverflow.com/questions/56073810/how-can-i-make-the-aspect-work-on-the-method-aservice-methodb)

一个Spring-AOP-织入及实例化bean过程的示例：
```
2019-05-10_14:00:52.437 [restartedMain] DEBUG o.s.a.a.a.AnnotationAwareAspectJAutoProxyCreator - Creating implicit proxy for bean 'priceRuleDeviceService' with 0 common interceptors and 2 specific interceptors
2019-05-10_14:00:52.437 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Creating CGLIB proxy: target source is SingletonTargetSource for target object [cn.ubox.mbss.device.server.service.PriceRuleDeviceService@197f7137]
2019-05-10_14:00:52.438 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public cn.ubox.mbss.common.result.IResult cn.ubox.mbss.device.server.service.PriceRuleDeviceService.changePriceRule(int,int,int)
2019-05-10_14:00:52.438 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public cn.ubox.mbss.common.result.IResult cn.ubox.mbss.device.server.service.PriceRuleDeviceService.batchSave(java.util.List)
2019-05-10_14:00:52.438 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public cn.ubox.mbss.common.result.IResult cn.ubox.mbss.device.server.service.PriceRuleDeviceService.savePriceRuleDevice(cn.ubox.mbss.device.entity.PriceRuleDevice)
2019-05-10_14:00:52.439 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public cn.ubox.mbss.common.result.IResult cn.ubox.mbss.device.server.service.PriceRuleDeviceService.updatePriceRuleDevice(cn.ubox.mbss.device.entity.PriceRuleDevice)
2019-05-10_14:00:52.439 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public cn.ubox.mbss.common.result.IResult cn.ubox.mbss.device.server.service.PriceRuleDeviceService.deletePriceRuleDevice(int)
2019-05-10_14:00:52.439 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public cn.ubox.mbss.device.entity.PriceRuleDevice cn.ubox.mbss.device.server.service.PriceRuleDeviceService.queryPriceRuleDeviceById(int)
2019-05-10_14:00:52.439 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public java.util.List cn.ubox.mbss.device.server.service.PriceRuleDeviceService.queryPriceRuleDeviceByPriceRuleId(int)
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public boolean cn.ubox.mbss.device.server.service.PriceRuleDeviceService.delPriceRuleDevice(int,int)
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public boolean cn.ubox.mbss.device.server.service.PriceRuleDeviceService.delPriceRuleDeviceByDeviceId(java.util.List)
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Found 'equals' method: public boolean java.lang.Object.equals(java.lang.Object)
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: public java.lang.String java.lang.Object.toString()
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Found 'hashCode' method: public native int java.lang.Object.hashCode()
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Unable to apply any optimizations to advised method: protected native java.lang.Object java.lang.Object.clone() throws java.lang.CloneNotSupportedException
2019-05-10_14:00:52.440 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract org.springframework.aop.TargetSource org.springframework.aop.framework.Advised.getTargetSource()
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract java.lang.Class[] org.springframework.aop.framework.Advised.getProxiedInterfaces()
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.isInterfaceProxied(java.lang.Class)
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract org.springframework.aop.Advisor[] org.springframework.aop.framework.Advised.getAdvisors()
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.setTargetSource(org.springframework.aop.TargetSource)
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.setExposeProxy(boolean)
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.isProxyTargetClass()
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.isExposeProxy()
2019-05-10_14:00:52.441 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.removeAdvisor(int) throws org.springframework.aop.framework.AopConfigException
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.removeAdvisor(org.springframework.aop.Advisor)
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.replaceAdvisor(org.springframework.aop.Advisor,org.springframework.aop.Advisor) throws org.springframework.aop.framework.AopConfigException
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.addAdvice(int,org.aopalliance.aop.Advice) throws org.springframework.aop.framework.AopConfigException
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.addAdvice(org.aopalliance.aop.Advice) throws org.springframework.aop.framework.AopConfigException
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.removeAdvice(org.aopalliance.aop.Advice)
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract java.lang.String org.springframework.aop.framework.Advised.toProxyConfigString()
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.setPreFiltered(boolean)
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.isPreFiltered()
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.addAdvisor(org.springframework.aop.Advisor) throws org.springframework.aop.framework.AopConfigException
2019-05-10_14:00:52.442 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract void org.springframework.aop.framework.Advised.addAdvisor(int,org.springframework.aop.Advisor) throws org.springframework.aop.framework.AopConfigException
2019-05-10_14:00:52.443 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract int org.springframework.aop.framework.Advised.indexOf(org.aopalliance.aop.Advice)
2019-05-10_14:00:52.443 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract int org.springframework.aop.framework.Advised.indexOf(org.springframework.aop.Advisor)
2019-05-10_14:00:52.443 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract boolean org.springframework.aop.framework.Advised.isFrozen()
2019-05-10_14:00:52.443 [restartedMain] DEBUG o.s.a.f.CglibAopProxy - Method is declared on Advised interface: public abstract java.lang.Class org.springframework.aop.TargetClassAware.getTargetClass()
2019-05-10_14:00:52.562 [restartedMain] DEBUG o.s.b.f.s.DefaultListableBeanFactory - Finished creating instance of bean 'priceRuleDeviceService'
```
