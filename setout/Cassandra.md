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


