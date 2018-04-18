---
title: Cassandra
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### NoSQL(NoSQL = Not Only SQL )
非关系型的数据库。用于超大规模数据的存储。这些类型的数据存储不需要固定的模式，无需多余操作就可以横向扩展。
### Cassandra
需要能够记录一系列关系型数据库所无法快速处理的大量数据。
#### Key-Value数据库
Key-Value数据库会以键值对的方式来对数据进行存储。其内部常常通过哈希表这种结构来记录数据。在使用时，用户只需要通过Key来读取或写入相应的数据即可。因此其在对单条数据进行CRUD操作时速度非常快。而其缺陷也一样明显：我们只能通过键来访问数据。除此之外，数据库并不知道有关数据的其它信息。因此如果我们需要根据特定模式对数据进行筛选，那么Key-Value数据库的运行效率将非常低下，这是因为此时Key-Value数据库常常需要扫描所有存在于Key-Value数据库中的数据。
Key-Value数据库常常用来作为服务端缓存使用，以记录一系列经由较为耗时的复杂计算所得到的计算结果。最著名的就是Redis。比如shiro当中把Redis作为session的缓存。
#### Document-based数据库
存储的数据将不再是一些字符串，而是具有特定格式的文档，如XML或JSON等。支持索引，提高数据查询和筛选的效率。
#### Column-based数据库
![enter description here](https://images2015.cnblogs.com/blog/126867/201603/126867-20160320200929131-309381625.png)
按照列来在数据文件中记录数据，以获得更好的请求及遍历效率。
![enter description here](https://images2015.cnblogs.com/blog/126867/201603/126867-20160320201122365-651136759.png)
Column-based数据库的核心思想：按照列来在数据文件中记录数据，以获得更好的请求及遍历效率。这里有两点需要注意：首先，Column-based数据库并不表示会将所有的数据按列进行组织，也没有那个必要。对某些需要执行请求的数据进行按列存储即可。另外一点则是，Cassandra对Query的支持实际上是与其所使用的数据模型关联在一起的。也就是说，对Query的支持很有限。
#### column superColumn columnFamily
column是一组key-value，superColumn内的数据可以一个column，也可以是一组columns，columnFamily相当于关系型数据库中的表，里面一个key-value形式，value是一组columns。
![enter description here](http://img.my.csdn.net/uploads/201203/6/0_133099905646lp.gif)
#### Partition Key，Clustering Key，Primary Key以及Composite Key

``` sql
1 create table sample {
2     key_one text,
3     key_two text,
4     data text,
5     PRIMARY KEY(key_one, key_two)
6 };
```

创建的Primary Key就是一个由两个列key_one和key_two组成的Composite Key。其中该Composite Key的第一个组成被称为是Partition Key，而后面的各组成则被称为是Clustering Key。**Partition Key用来决定Cassandra会使用集群中的哪个结点来记录该数据(hash计算)**，每个Partition Key对应着一个特定的Partition。**而Clustering Key则用来在Partition内部排序**。

在一个CQL语句中，**WHERE等子句所标示的条件只能使用在Primary Key中所使用的列**。

一个好的Partition Key设计常常会大幅提高程序的运行性能。首先，由于Partition Key用来控制哪个结点记录数据，因此Partition Key可以决定是否数据能够较为均匀地分布在Cassandra的各个结点上，以充分利用这些结点。同时在Partition Key的帮助下，您的读请求应尽量使用较少数量的结点。这是因为在执行读请求时，Cassandra需要协调处理从各个结点中所得到的数据集。因此在响应一个读操作时，较少的结点能够提供较高的性能。因此在模型设计中，如何根据所需要运行的各个请求指定模型的Partition Key是整个设计过程中的一个关键。一个取值均匀分布的，却常常在请求中作为输入条件的域，常常是一个可以考虑的Partition Key。

除此之外，我们也应该好好地考虑如何设置模型的Clustering Key。由于Clustering Key可以用来在Partition内部排序，因此其对于包含范围筛选的各种请求的支持较好。
#### v3 流程
创建一个cassandra.properties文件，配置数据库连接属性。

``` xml
cassandra.contactPoints=xxx.xxx.xxx.xxx
cassandra.port=9042
cassandra.userName=username
cassandra.password=password
cassandra.keyspaceName=mydefault
```
配置cassandra上下文，因为要使用注解获取CassandraTemplate

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:cassandra="http://www.springframework.org/schema/data/cassandra"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
    http://www.springframework.org/schema/data/cassandra
    http://www.springframework.org/schema/data/cassandra/spring-cassandra.xsd
    http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<!-- 引入cassandra配置 -->
 	<context:property-placeholder location="classpath:cassandra.properties" ignore-unresolvable="true"/>

	<!-- REQUIRED: The Cassandra Cluster -->
	<cassandra:cluster contact-points="${cassandra.contactPoints}" port="${cassandra.port}" 
	   username="${cassandra.userName}" password="${cassandra.password}" />

	<!-- REQUIRED: The Cassandra Session, built from the Cluster, and attaching to a keyspace -->
	<cassandra:session keyspace-name="${cassandra.keyspaceName}" />

	<!-- REQUIRED: The Default Cassandra Mapping Context used by CassandraConverter -->
	<cassandra:mapping entity-base-packages="com.sherry.db.entity.cassandra">
	   <cassandra:user-type-resolver keyspace-name="${cassandra.keyspaceName}" />
	</cassandra:mapping>

	<!-- REQUIRED: The Default Cassandra Converter used by CassandraTemplate -->
	<cassandra:converter />

	<!-- REQUIRED: The Cassandra Template is the building block of all Spring Data Cassandra -->
	<cassandra:template id="cassandraTemplate" />

	<!-- 自动扫描(自动注入)  DAO层位置-->
    <context:component-scan base-package="com.sherry.db.operation.cassandra" />
</beans>
```
在entity目录下声明keyspace对象

``` java
@Table
public class Student {
    @PrimaryKey
    private int id;
    private String name; 
    // get set toStirng省略 
}
```
创建dao层

``` java
public class BaseCassandraDao {
 
    protected final Logger logger = LoggerFactory.getLogger(this.getClass());
 
    @Autowired
    private CassandraTemplate template;
 
    protected CassandraTemplate getTemplate() {
        return template;
    }
	protected CqlOperations getCqlOperations() {
		return template.getCqlOperations();
	}
}
@Repository
public class StudentDao extends BaseCassandraDao {
    public void insert() {
        logger.info("##########insert start");
        boolean result = getCqlOperations().execute("insert into mycas.student(id, name, course) values(2,'Ben',{courseid:2,coursename:'literature'})");
        logger.info("##########insert end, result=" + result);
    }......
}
```
由于登陆之后才能确定keyspace，所以使用cql语句执行数据库操作或者使用CassandraTemplate。

