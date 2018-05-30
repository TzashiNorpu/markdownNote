#### 注解的作用:
- 简洁:减少配置和一些固定的逻辑代码
#### 注解由Java 1.5种引入，提供源程序中的元素关联任何信息和元数据的途径和方法
***
#### 常见注解
- JDK自带注解
    - @Override:(方法注解)重写父类的同名方法，便于编译器检查(子类可以不用该注解重写父类的方法，但是编译器不会对子类中的同名方法是否覆盖父类中的方法做检查，这个时候可能存在在子类中拼写方法名错误的情况)
    - @Deprecated:(方法注解)标记父类中该方法已经过时，此时在子类中仍可以使用，但是编译器会出现经过
    - @Supervisewarnings:(方法注解)该注解使得编译器忽略某种警告,警告类型在参数中给出--忽略对@Deprecated注解的方法警告示例:在子类中将@Supervisewarnings("deprecation")注解在调用父类中被@Deprecated注解的方法的方法上
- 常见第三方注解
    - Spring常见注解:
        - @Autowired
        - @Service
        - @Repository
    - Mybatis常见注解:
        - @InsertProvider
        - @UpdateProvider
        - @Options
***
#### 注解分类
- 按照运行机制(生命周期)分:
    - 源码注解:注解只在源码中存在，编译成.class之后就不存在了
    - 编译时注解:在源码和.class文件中都存在，JDK自带的注解都属于编译时注解
    - 运行时注解:在运行阶段还起作用，甚至会影响运行逻辑的注解，如@Autowired注解-在运行时将成员变量自动注入
- 按来源:
    - JDK注解
    - 第三方注解
    - 自定义注解
***
#### 自定义注解
- 语法
    - 注解也是一种类,包含成员
    - 使用@interface关键字定义
    - 成员以无参无异常方式声明
    - 可以用default为成员指定一个默认值
    - 成员的类型限制:包括原始数据类型和String,Class,Annotation和Enumeration
    - 注解只有一个成员则必须取名为value(),使用时可以忽略成员名和赋值符号(=).实际使用中在定义注解时可以不遵循该规则:
        - 假设下例中的@Description注解在定义时只声明了String Desc()成员,之后在使用@Description注解时需要显示的传入参数,如:@Description(desc="Hello World"),而用value()声明时,可以用@Description("Hello World")的形式进行调用
    - 注解类可以没有成员，没有成员的注解称为标识注解
- 元注解
    - 注解的注解:注解在普通注解上,声明普通注解的一些属性,如:
        - @Target({ElementType.xxx})--作用域元注解,声明该普通注解可以注解在哪些元素上
            - CONSTRUCTER:构造方法声明
            - FIELD:字段声明
            - LOCAL_VARIABLE:局部变量声明
            - METHOD:方法声明
            - PACKAGE:包声明
            - PARAMETER:参数声明
            - TYPE:类\接口声明
        - @Retention(RetentionPolicy.xxx)--生命周期元注解,声明该普通注解的运行生命周期
            - SOURCE:源码注解,编译时放弃
            - CLASS:编译时注解,编译时从源代码记录到.class文件中,运行时忽略
            - RUNTIME:运行时注解,运行时存在,通过反射读取
        - @Inherited--该普通注解允许子类注解的继承
            ```
            该注解作用于class类型的父类型时,子类只能继承得到父类型注解在类上的注解;作用于interface类型上的父类时,子类无法继承得到父类上的所有注解
            ```
        - @Documented--生成JavaDoc时会包含该普通注解的信息
- 自定义注解的定义示例
    - 依赖的包
    ```
    import java.lang.annotation.Target;
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Inherited;
    import java.lang.annotation.Documented;
    ```
    - 该注解为*运行时方法/类注解*:
    ```
    @Target({ElementType.METHOD,ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Inherited
    @Documented
    public @interface Description {
        String desc();
        String author();
        int age() default 18;
    }
    ```
- 使用注解的语法
    - @<注解名>(<成员名1>=<成员值1>,<成员名2>=<成员值2>,...)
    - 使用*方法注解*示例:
        ```
        @Description(desc="I am eyeColor",author="Mooc boy",age=18)
        public String eyeColor(){
            ...
        }
        ```
- 解析
    - 解析注解是指通过反射获取类、函数或成员上的**运行时**注解信息,从而实现动态控制程序运行的逻辑--`因为解析是运行时解析的,因此只能解析到运行时注解`
    - 解析常用的方法和类
        ```
        Class.forName("com.ann.test.Child");
        aClass.isAnnotationPresent(Description.class);
        aClass.getAnnotation(Description.class);
        aClass.getMethods();
        method.getAnnotation(Description.class);
        method.getAnnotations();
        ```
***