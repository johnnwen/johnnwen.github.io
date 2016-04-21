---
layout:     post
title:      "使用Hibernate框架开发步骤 "
subtitle:   " "
date:       2016-04-21 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kuaidi.jpg"
catalog:    true
tags:
    - Hibernate
    - Java
    - 后台开发框架
    
---

### 使用Hibernate框架开发步骤

##### 1. 创建Hibernate配置文件

##### 2. 创建持久化类

##### 3. 创建对象－关系映射文件

##### 4. 通过HibernateAPI编写访问数据库的代码

### 具体实现代码

##### 1. 创建Hibernate配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
		"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
		"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
    <!-- 配置连接数据库的基本信息 -->
    <property name="connection.username">root</property>
    <property name="connection.password">root</property>
    <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
    <property name="connection.url">jdbc:mysql:///hibernate</property>

	<!-- 配置 hibernate 的基本信息 -->
	<!-- hibernate所使用的数据库方言-->
	
	<property name="dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
	
	<!-- 执行操作时是否在控制台打印SQL -->
	<property name="show_sql">true</property>
	
	<!-- 是否对SQL进行格式化 -->
	<property name="format_sql">true</property>
	
	<!-- 指定自动生成数据表的策略 -->
	<property name="hbm2ddl.auto">update</property>
	
	<!-- 指定关联的.hbm.xml文件 -->
	<mapping resource="com/wyw/hibernate/helloworld/News.hbm.xml"/>
    
    </session-factory>
</hibernate-configuration>
```

##### 2. 创建持久化类

```
public class News {
	private Integer id;
	private String title;
	private String author;
	private Date date;

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getAuthor() {
		return author;
	}

	public void setAuthor(String author) {
		this.author = author;
	}

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	public News(String title, String author, Date date) {
		super();
		this.title = title;
		this.author = author;
		this.date = date;
	}

	public News() {
		// TODO Auto-generated constructor stub
	}

	@Override
	public String toString() {
		return "News [id=" + id + ", title=" + title + ", author=" + author + ", date=" + date + "]";
	}

}
```

##### 3. 创建对象－关系映射文件


```
<hibernate-mapping>
    <class name="com.wyw.hibernate.helloworld.News" table="NEWS">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="native" />
        </id>
        <property name="title" type="java.lang.String">
            <column name="TITLE" />
        </property>
        <property name="author" type="java.lang.String">
            <column name="AUTHOR" />
        </property>
        <property name="date" type="java.sql.Date">
            <column name="DATE" />
        </property>
    </class>
</hibernate-mapping>

```

##### 4. 通过HibernateAPI编写访问数据库的代码

```
public class HibernateTest {

	@Test
	public void test() {

		// 1.创建一个SessionFactory对象

		SessionFactory sessionFactory = null;

		// 1).创建Configuration对象：对应hibernate的基本配置信息和关系映射信息
		Configuration configuration = new Configuration().configure();

		// 4.0之前创建
		// sessionFactory = configuration.buildSessionFactory();

		// 2)创建一个ServiceRegistry对象：hibernate4.x新添加的对象
		// hinbernate的任何配置和服务都需要在该对象中注册后才能有效

		ServiceRegistry serviceregistry = 
				new ServiceRegistryBuilder().applySettings(configuration.getProperties())
				.buildServiceRegistry();

		sessionFactory = configuration.buildSessionFactory(serviceregistry);

		// 2.创建一个Session对象
		Session session = sessionFactory.openSession();
		
		// 3.开启事务
		Transaction transaction = session.beginTransaction();

		// 4.执行保存操作
		News news = new News("Java","wyw",new Date(new java.util.Date().getTime()));
		session.save(news);

		// 5.提交事务
		transaction.commit();

		// 6.关闭Session
		session.close();

		// 7.关闭SessionFactory对象
		sessionFactory.close();

	}

}

```
