#<center>学习日记
###1.初始化和销毁bean
>在bean中使用init-method="方法名" destroy-method="方法名"

    <?xml version="1.0" encoding="utf-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://springframework.org/schema/beans/spring-beans-3.0.xsd">
        <bean id="auditorium" class="p215.Auditorium" 
            init-method="turnOnLights"
            destroy-method="turnOffLights"/>
    </beans>

>在beans中使用default-init-method="方法名" default-destroy-method="方法名"


    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"
        default-init-method="turnOnLights"
        default-destroy-method="turnOffLights">
    </bean>
>>销毁方法必须是使用close()方法之后,才能被调用.如下代码

        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("p215/auditorium.xml");
        System.out.println("测试初始化和销毁方法");
        applicationContext.close();


###2.注入bean的属性
####2.1:注入简单的bean
>通过property的name与value形式注入简单的值

    <bean id="kenny" class="p22.Instrumentalist">
        <property name="song" value="ka ka ka"></property>
    </bean>

####2.2:引入其他bean
>通过property 的name与ref形式引入其他的bean
    
    <bean id="kenny" class="p22.Instrumentalist">
        <property name="instrument" ref="sax"></property>
    </bean>
    
    <bean id="sax" class="p22.Saxophone">

####2.3:注入内部类
>通过property中写入bean.注入内部类

    <bean id="kenny_inside" class="p22.Instrumentalist">
        <property name="song" value="ka ka ka-inside"></property>
        <property name="instrument">
            <bean class="p22.Saxophone"></bean>
        </property>
    </bean>
####2.4:通过constructor-arg注册constructor(构造器)

    <bean id="kenny_inside" class="p22.Instrumentalist">
        <property name="song" value="ka ka ka-inside"></property>
        <property name="instrument">
            <bean class="p22.Saxophone"></bean>
        </property>
        <constructor-arg value="15"></constructor-arg>
    </bean>
####2.5:通过命名空间进行注册bean
>通过p的命名空间命名(xmlns:p="http://www.springframework.org/schema/p")在中同p:bean

    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
        <bean id = "saxophone" class="p223.Saxophone"/>
        <bean id="kenny" class="p223.Instrumentalist" p:song="Jingle Bells" p:instrument-ref="saxophone"/>
    </beans>

####2.6:注册(装配)集合
>2.6.1:list集合
>>装配list类型的值, 允许集合
>>装配类型有list类型和 String[]类型.

    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    
    <bean id="guitar" class="p224.Guitar"></bean>
    <bean id="harmonica" class="p224.Harmonica"></bean>
    <bean id="saxophone" class="p224.Saxophone"></bean>
    
    <bean id="hank" class="p224.OneManBand">
        <property name="instrumetns">
            <list>
                <ref bean="guitar"/>
                <ref bean="harmonica"/>
                <ref bean="saxophone"/>
                <ref bean="saxophone"/>
            </list>
        </property>
    </bean>
    </beans>
>2.6.2:set
>>装配set类型的值,不允许重复


    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="hank" class="p224.OneManBand">
        <property name="instrumetns">
            <set>
                <ref bean="guitar"/>
                <ref bean="cybal"/>
                <ref bean="harmonica"/>
                <ref bean="harmonica"/>
            </set>
        </property>
    </bean>
    
    <bean id="guitar" class="p224.Guitar">
    </bean>
    <bean id="cybal" class="p224.Saxophone">
    </bean>
    <bean id="harmonica" class="p224.Harmonica">
    </bean>
    </beans>

>2.6.3:map
>>装配map类型的值.

    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
        <bean id="hank" class="p224.OneManBand4Map">
            <property name="instruments">
                <map>
                    <entry key="guitar" value-ref="guitar"></entry>
                    <entry key="cymbal" value-ref="cybal"></entry>
                    <entry key="harmonica" value-ref="harmonica"></entry>
                </map>
            </property>
        </bean>
        
        <bean id="guitar" class="p224.Guitar"/>
        <bean id="cybal" class="p224.Saxophone"/>
        <bean id="harmonica" class="p224.Harmonica"/>
    </beans>
>2.6.4property
>>装配properties类型的值. 名称和值必须是string类型.

    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
        <bean id="hank" class="p224.OneManBandProperties">
            <property name="instruments">
                <props>
                    <prop key="guitar">guitar.......</prop>
                    <prop key="cymbal">cymbal........</prop>
                    <prop key="harmonica">harmonica.............</prop>
                </props>
            </property>    
        </bean>
    </beans>
