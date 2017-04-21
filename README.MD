# 简介
这是一个基于Zookeeper与Netty开发的一个Rpc远程调用服务框架。框架中使用了Zookeeper的服务注册与发现功能，Netty作为一个http容器向外提供服务。

## 生产者与消费者
生产者与消费者是RPC设计的两个核心角色。生产者（ProviderProxyFactory.class）提供服务。消费者调用服务（ProviderProxyFactory.class）。数据封装在一个请求对象中（Request.class）对象中序列化之后用于网络层传输。

## 生产者客户端配置

```xml
<bean class="com.rpc.proxy.ProviderProxyFactory">
    <constructor-arg name="providers">
        <map key-type="java.lang.Class" value-type="java.lang.Object">
            <entry key="com.rpc.SpeakInterface" value-ref="speakInterface"/>
        </map>
    </constructor-arg>
    <constructor-arg name="providerConfig">
        <bean id="providerConfig" class="com.rpc.invoke.ProviderConfig">
            <property name="port" value="8888"/>
            <property name="target" value="/invoke"/>
        </bean>
    </constructor-arg>
</bean>

```

## 消费者客户端配置

```xml
  <bean id="consumerConfig" class="com.rpc.invoke.ConsumerConfig">
      <property name="url" value="http://127.0.0.1:8888/invoke" />
  </bean>

  <bean id="speakInterfaceInvoker" class="com.rpc.proxy.ConsumerProxyFactory">
      <property name="consumerConfig" ref="consumerConfig"/>
      <property name="clazz" value="com.rpc.SpeakInterface"/>
  </bean>
  <bean id="speakInterface" factory-bean="speakInterfaceInvoker" factory-method="create"/>
```
