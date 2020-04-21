## Spring

#### 创建bean的三种方法

```
第一种方法：使用默认构造器创建bean    
    在spring的配置文件中使用bean标签，配以id和class属性之后，且没用其他属性和标签时，    
    采用的就是默认构造函数创建bean对象，如果此时没有默认构造函数，则对象无法创建
第二种方法：使用普通工厂方式创建对象，（使用某个类中的方法创建对象，并存人到spring容器中）
第三种方法：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入spring容器中）
```

#### bean的作用范围调整

```
bean标签的Scope属性：
    作用：用于指定bean的作用范围
    取值：（常用：singleton , prototype）
    	singleton 单例（默认的）
    	prototype 多例
    	request 作用于web应用的请求范围
    	session 作用与web应用的会话范围
    	global-session 作用于集群环境的会话范围（全局会话范围），当不是集群环境时，就是session
```

#### bean的生命周期

```
单例：单例模式的生命周期和容器相同
	出生：当容器创建时出生
	活着：当容器存在时活着
	死亡：当容器销毁时死亡
多例：
	出生：当我们使用对象时，spring框架为我们创建的
	活着：对象在使用过程中就一直活着
	死亡：当对象长时间不用，且没有其他对象引用时，由java垃圾回收机制回收
```

#### spring中的依赖注入

```
依赖注入：Dependency Injection
	IOC的作用：
		降低程序之间的耦合（依赖关系）
	依赖关系的管理：
		以后都交给了spring来维护,在当前类中需要用到其他类的对象时，由spring为我们提供，
		我们只需要在配置文件中说明,依赖关系的维护，我们就称之为依赖注入
	依赖注入：
		能注入的数据有三类：
			基本类型和String
			其他bean类型（在配置文件中配置过的bean）
			复杂类型/集合类型
		注入的方式有三种：
			第一种：使用构造函数
			第二种：使用set方法提供
			第三种：使用注解提供
```

```
 构造函数注入
 	使用的标签：constructor-arg
 		标签出现的位置：bean内部
 		标签中的属性：
 			type:指定要注入的数据类型,该数据类型也是构造函数中的某个或某些参数的类型
 			index:用于指定要注入的数据在构造函数中的指定索引位置的参数赋值，索引位置是从0开始的
             name:用于指定给构造函数中指定的名称赋值（常用）
  ======================以上三个都是给构造函数中的参数赋值========================
             value:用于提供基本数据类型和String类型的数据
             ref:用于指定其他bean数据类型，就是指在spring的IOC容器中配置过的bean的对象
        优势：在获取bean对象时，注入的数据是必须操作，否则对象无法创建
        弊端：改变了bean对象的实例方法，使我们在创建对象时，如果用不到这些数据，也必须提供
```

```
 set方法注入      相较于构造函数更加常用
 	涉及的标签：property
	出现的位置：bean标签的内部
		标签的属性
			name:用于指定注入时所调用的set方法的名称
             value:用于提供基本数据类型和String类型的数据
             ref:用于指定其他bean数据类型，就是指在spring的IOC容器中配置过的bean的对象
	优势：创建对象时没有明确的限制，可以直接使用默认构造函数
	弊端：如果某个成员必须有值，则获取对象时set方法没有执行
```

```
复杂类型的注入/集合类型的注入
        用于给list结构集合注入的标签：
            list,array,set
        用于给Map集合注入的标签：
            map,props
        结构相同，标签可以互换
```

####Spring常用的四大类注解（共11个）

```
用于创建对象的：同在xml文件中配置一个<bean>
	Component 
		作用：用于把当前类对象存入Spring容器中                
		属性： value:用于指定bean的id，如果我们不写则就是当前类名，但首字母小写
    Controller  一般用在表现层
    Service 一般用在业务层
    Repository  一般用在持久层
    以上三种注解和Component的作用和属性是一样的,是spring为我们明确的三层的使用
```

```
用于注入数据的：同在xml文件中的<bean>标签中写一个<property>标签      
    Autowired
        作用:只要容器中有唯一一个bean的对象类型和要注入的变量类型匹配，就可以注入成功               		   如果IOC容器中没有任何bean的类型和要注入的变量类型匹配则报错                
            如果IOC容器中有多个类型匹配时                
        出现位置：方法上，变量上*                
        细节：在使用注解注入时，Set方法就不是必须的了
    Qualifier
    	作用：在按照类中注入的基础之上在按照名称注入，它在给类成员注入时不能单独使用，在给方法注入				时可以                  
    	属性：value:用于指定注入bean的id
    Resource
    	作用：之间按照bean的id注入。他可以独立使用
    	属性：name用于指定bean的id
    ============以上三种方法都只能注入其他bean类型的数据==================  
    value
        作用：用于注入基本数据类型和String类型的数据
    	属性：value:用于指定数据的值，可以使用Spring中的spEl（就是spring中的el表达式）
    	spEl的写法：${表达式} 
```

```
用于改变作用范围的：同在xml文件中使用scope属性实现的功能是一样的      
	Scope
		作用：用于指定bean的作用范围
    	属性：value:指定范围的取值。常用取值：singleton、prototype
```

```
和生命周期有关：同在xml文件中使用init_methode和destory_methode的作用是一样的     
    PreDestroy
    	作用：用于指定销毁方法      
    PostConstructor
    	作用：用于指定初始化方法
```

####定义一个配置类（等同于xml文件)

```
Spring中的新注解
	@Configuration
		作用：指定当前类是一个配置类
		细节：当配置类作为AnnotationConfigApplicationContext对象创建的参数时，该注解可用					不写
	@ComponentScan
		作用：用于通过注解指定spring要扫描的包
		属性：value:他和basePackages的作用是一样的。都是指定创建容器时要扫描的包       
		我们使用此注解就等同于在xml中配置了：
		<context:component-scan base-package="org.lgl">/context:component-scan>
	@Bean
		作用：用于把当前方法的返回值作为bean对象存入spring的IOC容器中
		属性：
			name:用于指定当前bean的id，当不写时，默认值就是当前方法的名称
		细节：当我们使用注解配置方式时，如果方法有参数，spring框架回去容器中查找有没有可用			的bean对象，查找的方式和Autowired注解的作用是一样的
	@Import
		作用：用于导入其他配置类
		属性：
			value:用于指定其他配置类的字节码
			当我们使用Import的注解之后，有Import注解的类就父配置类，而导入的就都是子配置类
	@PeopertySource:
		作用：用于指定properties文件的位置
		属性：
			value:指定文件的名称和路径
			关键字：classpath，表示类路径下
```

#### 使用junit单元测试：测试我们的配置 

```
Spring整合junit的配置
	1.导入spring整合junit的jar包（坐标）
	2.使用junit提供的一个注解把原有的main方法替换了，替换成了spring提供的
    	@Runwith 
    3.告知spring的运行器，spring的IOC是基于xml还是基于注解的，并说明位置
    	@ContextConfiguration 
    		locations:指定xml文件的位置，加上classpath关键字，表示在类路径下
    		classes:指定注解类所在的位置 
加上以下配置
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)  or 
@ContextConfiguration(locations = "classpath:bean.xml")
```

####Spring中基于XML的AOP配置步骤

```
Spring中基于XML的AOP配置步骤
	1.把通知Bean也交给spring来管理
	2.使用aop:config标签开始AOP的配置
	3.使用aop:aspect标签表面配置切面
		id属性：是给切面提供一个唯一标识
		ref属性：是指定通知类bean的id
	4.在aop:aspect标签的内部使用对应的标签来配置通知的类型
		我们现在示例是让printLog方法在切入点方法执行之前：所以是前置通知
		aop：before:表示配置前置通知
			method属性：用于指导Logger类中的那个方法是前置通知
			pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层的中的哪些方法增强
		切入点表达式的写法：
			关键字：execution(表达式)
			表达式：访问修饰符 返回值 包名.包名....包名.类名.方法名（参数列表）
			标准的表达式写法：
				public void org.lgl.service.impl.AccountServiceImpl.saveAccount()
			访问修饰符可以省略
				void org.lgl.service.impl.AccountServiceImpl.saveAccount()
			返回值可以使用通配符，表示任意返回值
				* org.lgl.service.impl.AccountServiceImpl.saveAccount()
			包名可以使用通配符，表示任意包，但是有几个包，就要写几个
				* *.*.*.*.AccountServiceImpl.saveAccount()
			包名可以使用..表示当前包以及子包
				* *..AccountServiceImpl.saveAccount()
			包名和方法名都可以使用*来实现通配
				* *..*.*()
		参数列表：
			可以直接写数据类型：
				基本数据类型直接写名称 int
				引用类型写包名.类名的方式 java.lang.String
			可以使用通配符表示任意参数类型，但是必须要有参数
			可以使用..表示有无参数均可，有参数可以是任意类型
		全通配写法：
			* *..*.*(..)
			
	实际开发中切入点表达式的通常写法：
		切到业务层实现类下的所有方法
			* org.lgl.service.impl.*.*(..)
```

####Aop在xml中的配置

```
<aop:config>
	<aop:aspect id="命名" ref="通知类">
    	配置通知的类型，并且j建立通知方法和切入点方法的关联
    	前置通知:在切入点方法执行之前执行
    	<aop:before method="通知类中的方法名" pointcut-ref="pt1"></aop:before>
    	后置通知：在切入点方法正常执行之后执行，和异常通知不可能同时出现
    	<aop:after-returning method="通知类中的方法名" pointcut-ref="pt1"></aop:after-returning>
    	异常通知：在切入点方法执行产生异常之后执行
    	<aop:after-throwing method="通知类中的方法名" pointcut-ref="pt1"></aop:after-throwing>
    	最终通知：无论切入点方法释放执行成功，它都会在其后面执行
    	<aop:after method="通知类中的方法名" pointcut-ref="pt1"></aop:after>
    	配置环绕通知
    	<aop:around method="通知类中的方法名" pointcut-ref="pt1"></aop:around>
    </aop:aspect>
</aop:config>
配置切面
	配置切入点表达式 id属性用于指定唯一表达式的唯一标识
		此标签写在aop:aspect标签内部时只能当前切面使用
		当写在aop:aspect标签外部时此时就变成了所有切面可用
		<aop:pointcut id="pt1" expression="execution(* *..*.*(..))"></aop:pointcut>
```

##### 基于注解的AOP配置

```
在xml中配置spring开启AOP注解的支持：
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

```
切入点：@Pointcut("execution(* org.lgl.service.impl.*.*(..)))")
	private void pt1(){}

前置：@Before("pt1()")
后置：@AfterReturning("pt1()")
异常：@AfterThrowing("pt1()")
最终：@After("pt1()")
=========但是以上会导致最终通知和后置通知/异常通知出现顺序问题===========
环绕：@Around("pt1()")
```

##### 环绕通知方法

```
环绕通知
	问题：当我们配置环绕通知后，切入点方法没有执行，而通知方法执行了
	分析：通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，
		而我们的代码中没有
	解决：Spring框架为我们提供了一个接口：ProceedingJoinPoint.该方法有一个方法product(),
		此方法就相当于明确调用切入点方法，该接口可以作为环绕通知的参数，
		在程序执行时,spring框架会提供该接口的实现类供我们使用
	spring的环绕通知：
		他是spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方法
```

```
public Object 通知类中的方法名(ProceedingJoinPoint pjp){
    Object rtValue = null;
    try {
        //获取方法执行所需的参数
        Object[] args = pjp.getArgs();
        //明确调用业务层方法（切入点方法）
        System.out.println("前：方法开始记录日志了");
        rtValue = pjp.proceed(args);
        System.out.println("后：方法开始记录日志了");
        return rtValue;
    } catch (Throwable throwable) {
        System.out.println("异常：方法开始记录日志了");
        throw new RuntimeException(throwable);
    }finally {
        System.out.println("最终：方法开始记录日志了");
    }
}
```

##### 不使用xml配置日志

```
在配置类上加上@EnableAspectJAutoProxy
```

##### Spring中基于xml的声明式事务的控制配置步骤    

```
Spring中基于xml的声明式事务的控制配置步骤
	1.配置事务管理器    
	2.配置事务的通知       
    	此时我们需要导入事务的约束 tx的名称空间和约束，同时也需要aop的        
    	此时tx:advice标签配置事务通知            
    		属性：                
    		id：给事务通知起一个唯一标志                
    		transaction-manager:给事务通知提供一个事务管理器引用    
    3.配置Aop中的通用切入点表达式    
    4.建立事务通知和切入点表达式的对应关系    
    5.配置事务的属性        
    	是在事务的通知tx:advice标签的内部
```

```
1、配置事务管理器:
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
```

```
2、配置事务的通知:
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--5、配置事务的属性-->
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED" read-only="false"/>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
        </tx:attributes>
    </tx:advice>
```

```
配置Aop
    <aop:config>
        <!--3、配置切入点表达式-->
        <aop:pointcut id="pt1" expression="execution(* org.lgl.service.impl.*.*(..))"></aop:pointcut>
        <!--4、建立切入点表达式和事务通知的对应关系-->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"></aop:advisor>
    </aop:config>
```

```
5、配置事务的属性：
	isolation：用于指定事务的隔离级别。默认值为:DEFAULT，表示使用数据库的默认隔离级别
	propagation：用于指定事务的传播行为。默认值是Required，表示一定会有事务，
		增删改的选择，查询方法可以选择Supports
	read-only：用于指定事务是否只读，只有查询方法才能设置为true。
		默认值是false，表示读写
	timeout：用于指定事务的超时时间，默认值为-1，表示永不超时，如果指定了数值，以秒为单位
 	rollback-for：用于指定一个异常，当产生该异常时，事务回滚。
 		产生其他异常时，事务不回滚。没有默认值。表示任何异常都回滚
 	no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚。
 		产生其他异常时，事务回滚。没有默认值。表示任何异常都回滚
```

##### Spring中基于注解的声明式事务的控制配置步骤 

```
Spring中基于注解的声明式事务的控制配置步骤
	1.配置事务管理器 
	2.开启spring对注解事务的支持    
	3.在需要事务支持的地方使用@Transactional注解
```

```
1、配置事务管理器:
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
```

```
2、开启spring对注解事务的支持
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
```

```
3、@Transactional
	//配置的是读写型事务的配置
    @Transactional(propagation = Propagation.REQUIRED,readOnly = false)
```

##### 不使用xml配置事务

```
在配置类上加上@EnableTransactionManagement
```

