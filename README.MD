# ���
����һ������Zookeeper��Netty������һ��RpcԶ�̵��÷����ܡ������ʹ����Zookeeper�ķ���ע���뷢�ֹ��ܣ�Netty��Ϊһ��http���������ṩ����

## ��������������
����������������RPC��Ƶ��������Ľ�ɫ�������ߣ�ProviderProxyFactory.class���ṩ���������ߵ��÷���ProviderProxyFactory.class�������ݷ�װ��һ����������У�Request.class�����������л�֮����������㴫�䡣

## �����߿ͻ�������

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

## �����߿ͻ�������

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