# SpringBoot入门和进阶Note
#### mvn依赖包的重新下载
- 到工程根目录下运行mvn命令
  ```
  mvn dependency:purge-local-repository
  mvn clean verify
  ```
#### mvn启动项目的三种方法：
- 进入到工程根目录下运行：mvn spring-boot:run
- 进入到工程根目录下运行：mvn install--编译生成jar/war包，该包将自动生成至到target目录下；进入到该tar包文件路径，运行：java -jar jar包名
- IDE启动
#### 同时启动应用不同yml配置的工程：
- 实现方式：IDE启动+mvn启动+java启动--这三种启动方式带不同的配置文件启动可实现多种环境的同时启动
  - mvn install方式命令行启动工程时带配置文件参数的命令格式：java -jar 工程相对路径/jar包名 --spring.profile.active=prod
#### yml工程配置文件（和properties配置文件格式不一样）：
- yml文件格式
  ```
  server:
    port:(此处空一格）8082
    context:(此处空一格）/girl
  ```
- 使用场景：一个应用可以配置一个根application.yml配置文件，再在该文件中配置启用形如application-dev.yml、application-prod.yml对应到测试或生成环境的配置文件（这些文件的参数需要单独配置）
- yml配置文件的读取：
  - 用@Value注解取出到变量中
  - 自定义的多层级的配置参数可以通过对象属性的方式进行读取 
#### 当前项目依赖包的仓库位置修改：
- File--Settings--Build...--Build Tools--Maven ...
#### JPA是标准--Hibernate实现了该标准--Spring-Data-Jpa是Spring对Hibernate的整合
#### mvn打包--进入到项目根目录下运行：
- mvn clean package--此时的打包会运行单元测试的代码
- mvn clean package -Dmaven.test.skip=true--此时的打包不运行测试代码
#### IntelliJ IDEA类注释的自动生成配置和方法注释的LiveTemplate配置：
- 类注释的自动生成：
  - File....
- 方法注释的LiveTeamplate：
#### SpringBoot的常用注解
-
