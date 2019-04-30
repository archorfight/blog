# Mybatis使用篇

### 一  概念

> http://www.mybatis.org/mybatis-3/   官方文档

- **what:**MyBatis is a first class persistence framework with support for custom SQL, stored procedures and advanced mappings. 
- **why**:MyBatis eliminates almost all of the JDBC code and manual setting of parameters and retrieval of results. 
- **how**:MyBatis can use simple XML or Annotations for configuration and map primitives, Map interfaces and Java POJOs (Plain Old Java Objects) to database records. 

### 二 使用

- 使用方式

  - 编程式
  - 集成式 managed集成到spring
  - 工作当中的使用方式  1：generator生成xml和dao； 2mybatis-plus 生成代码

- 作用域scope生命周期

  | class                    | scope                            |
  | ------------------------ | -------------------------------- |
  | SqlSessionFactoryBuilder | method                           |
  | SqlSessionFactory        | application                      |
  | SqlSession               | request/method(可认为是线程级别) |
  | Mapper                   | method                           |

- Mapper的xml和annotation形式

  - 兼容形式->互补

  - 优缺点

    - xml 跟接口分离，统一管理，复杂sql可读性好   缺点xml文件多
    - 注解  接口就能看到sql语句，简单语句可读性好  缺点：复杂的sql不好维护，可读性差

  - config部分文件解读   

    >http://www.mybatis.org/mybatis-3/configuration.html 

    - Environment
    - TypeHandler(java和表字段类型的转换实现)
      - 定义 `` com.gupao.dal.config.MybatisConfig#localSessionFactoryBean``
      - 注册 ``com.gupao.dal.config.MybatisConfig#localSessionFa ctoryBean ``
      - 注册到 `xml` `resultMap` 里面的`result`标签 里面的`typeHandler`属性里面
    - Plugins
      - 拦截范围
        - Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
        - ParameterHandler (getParameterObject, setParameters)
        - ResultSetHandler (handleResultSets, handleOutputParameters)
        - StatementHandler (prepare, parameterize, batch, update, query)
      - 定义`com.gupao.dal.plugins.TestPlugin`
      - 注册`com.gupao.dal.config.MybatisConfig#localSessionFa ctoryBean` 
      - 使用

    

    

### 三 配置文件解读

- mapper文件解读

  - namespace  关联到接口方法，区分类似package的作用

  - resultap/resultType

    |            | 优点                                   | 缺点           |
    | ---------- | -------------------------------------- | -------------- |
    | reaultType | 多表关联字段是清楚知道的，性能调优直观 | 创建很多实体类 |
    | resultMap  | 不需要写join语句                       | N+1问题        |

  - sql

  - select insert update delete CRUD

  - 动态SQL

    > http://www.mybatis.org/mybatis-3/dynamic-sql.html

  - 缓存

    - 一级缓存
    - 二级缓存

- 1.分页

  - 逻辑分页 `org.apache.ibatis.executor.resultset.DefaultResultSetHandler #handleRowValuesForSimpleResultMap ` 内存里分页

  - 物理分页

    - select ... limit 0,10;
    - 分页插件   ：`PageHelper`

  - 批量操作Batch

    - for 循环  一个一个的插入， 每一次插入都有数据库IO 性能不好

    - foreach 拼sql 性能最好  只有几个IO  缺陷  有SQL长度限制，需要定好List大小 

      ```
      show variables like '%net_buffer%';
      show variables like '%packet%';
      ```

    - ExecutorType.BATCH   `com.gupao.test.dal.TestMapperTest#insertBatchExType `

  - 联合查询

    - 嵌套结果

    - 嵌套查询

      - Lazy loading
      - `com.gupao.dal.config.MybatisConfig#localSessio nFactoryBean `

      ```
      SqlSessionFactory factory =sqlSeesionFactoryBean.getObject();
        factory.getConfiguration().setLazyLoadingEnabled(true);
        ...                       .setAggressiveLazyLoading(false);
        ...                       .setProxyFactory(new CglibProxyFactory());
      ```

      