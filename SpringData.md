#### 用Maven创建项目
- File-Project-选中Maven-勾选Create from archetype--选择maven-archetype-quickstart
- 填写相应的项目信息内容
#### dependency添加依赖
- mvnrepository.com搜索相应的包名，如mysql，选择相应的包版本，然后复制对应的Maven依赖内容
    ```
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.38</version>
    </dependency>
    ```
#### 数据库准备
- 库准备
    ```
    create database spring_data;
    ```
- 表准备
    ```
    use spring_data;
    create table student(
            id int not null auto_increment,
            name varchar(20) not null,
            age int not null,
            primary key(id)
        ) ENGINE = 'InnoDB';
    ```
- 数据准备
    ```
    insert into student (name, age) values ("test1",20);
    insert into student (name, age) values ("test2",21);
    insert into student (name, age) values ("test3",22);
    insert into student (name, age) values ("test4",20);
    insert into student (name, age) values ("zhangsan",21);
    insert into student (name, age) values ("lisi",22);
    insert into student (name, age) values ("wangwu",22);
    ```
#### 为什么spring-resource目录不支持yml格式文件
#### domain目录存放实体类：
#### DAO和Service都是开发一个接口再开发多个实现类
- DAO示例：
    - 首先定义接口，接口中定义实现类中需要实现的方法：
    ```
    public interface StudentDAO {
        public List<Student> query();
        public void save(Student student);
    }
    ```
    - 定义实现类，在实现类中实现DAO接口中定义的方法：
    ```
    public class StudentDAOImpl implements  StudentDAO{
        @Override
        public List<Student> query() {
            ...
        }
        @Override
        public void save(Student student) {
            ...
        }
    }
    ```
    - 同理可以定义不同的实现类实现该DAO的接口：
    ```
    public class StudentDAOSpringJdbcImpl implements  StudentDAO{
        private JdbcTemplate jdbcTemplate;
        public JdbcTemplate getJdbcTemplate() {
            return jdbcTemplate;
        }
        public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
            this.jdbcTemplate = jdbcTemplate;
        }
        @Override
        public List<Student> query() {
        ...
        }
        @Override
        public void save(Student student) {
            ...
        }
    }
    ```
#### 使用JDBC操作数据库需要使用依赖注入--在Spring中通过配置XML文件实现
- beans.xml中配置了"id=studentDAO"的bean指向StudentDAOSpringJdbcImpl类，因此在StudentDAOSpringJdbcImplTestl类中通过容器上下文ctx.getBean()的方法直接使用,不需要通过new进行实例化后再使用
#### 使用JPA
- 需要的依赖
    - spring-data-jpa
    - hibernate-entitymanager
- 配置了两个bean在beans-new.xml中
    - entityManagerFactory
    - dataSource
    - transactionManager
- SpringDataTest.testEntityManagerFactory()测试方法测试是否能生成对应的数据表
- domain实体类Employee中添加@Entity注解
    - Spring JPA中字段类型建议使用封装类型，如不是int而是Integer
- repository包

#### 看不懂的代码
- JDBCUtil.getConnection():
    ```    
    JDBCUtil.class.getClassLoader().getResourceAsStream("db.properties");

    Properties properties = new Properties();
    properties.load(inputStream);

    Class.forName(driverClass);
    ```
- DataSourceTest类中:
    ```
    ApplicationContext对象和ClassPathXmlApplicationContext对象
    ```