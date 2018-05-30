####  Class类
```
class Foo{	
    void print(){
        System.out.println("foo");
    }
}
```
- 类是对象，是java.lang.Class类的实例对象。设有一个类的定义如上，则**Foo这个类也是Class类的一个实例对象**
- 类是对象的概念和Foo foo = new Foo()创建的foo是Foo的实例对象一致

- 任何一个类都是Class的实例对象，这个实例对象有三种表示方式：
    ```
    Class c1 = Foo.class;//即任何一个类都有一个隐含的静态成员变量class
    Class c2 = foo1.getClass();//经由该类对象的getClass方法
    Class c3 = Class.forName("com.imooc.reflect.Foo");//经由类的路径名
    ```
- c1、c2、c3表示了Foo类的**类类型**
- c1、c2、c3都代表了Foo类的类类型，一个类只可能是Class类的一个实例对象(即c1=c2=c3)
- 可以通过类的类类型创建该类的对象实例
    ```
    Foo foo1 = (Foo)c1.newInstance();//需要有无参数的构造方法
    Foo foo2 = (Foo)c2.newInstance();//需要有无参数的构造方法
    Foo foo3 = (Foo)c3.newInstance();//需要有无参数的构造方法
    ```
***
#### 静态类和动态类
- 编译时刻加载的类是静态加载类、运行时刻加载的类是动态加载类
- 静态加载代码示例：通过new创建的对象都是静态加载的。主程序中依赖的类必须在主程序编译之前存在(.class文件),即这些依赖的类必须编译通过
    - Office.java:
        ```
        class Office{
            public void main(String[] args){
                if("Word".equals(args[0])){
                    Word w = new Word();
                    w.start();
                }
                if("Excel".equals(args[0])){
                    Excel e = new Excel();
                    e.start();
                }
            }
        }
        ```
    - Word.java:
        ```
        class Word{
            public void start(){
                System.out.println("word start....");
            }
        }
        ```
    - Excel.java:
        ```
        class Excel{
            public void start(){
                System.out.println("Excel start....");
            }
        }
        ```
- 动态加载代码示例：主程序编译一次即可，抽象出了实现OfficeAble接口的对象,每次新增了实现OfficeAble接口的对象时只需要单独编译该对象文件即可--且不依赖于new创建对象
    - Office.java:
        ```
        class Office{
            public void main(String[] args){
                try{
                    //动态加载：在运行时加载类
                    Class c = Class.forName(args[0]);
                    //通过类类型创建对象
                    OfficeAble oa = (OfficeAble)c.newInstance();
                    oa.start();
                }
                catch(Exception e){
                    e.printStackTrace();
                }
            }
        }
        ```
    - OfficeAble.java
        ```
        interface OfficeAble{
            public void start();
        }
        ```
    - Word.java:
        ```
        class Word implements OfficeAble{
            public void start(){
                System.out.println("word start....");
            }
        }
        ```
    - Excel.java:
        ```
        class Excel implements OfficeAble{
            public void start(){
                System.out.println("Excel start....");
            }
        }
        ```
***
#### 类方法的获取
- 根据类的类类型的getMethods方法获取
    ```
    public static void printClassMethodMessage(Object obj){
		//要获取类的信息  首先要获取类的类类型
		Class c = obj.getClass();//传递的是哪个子类的对象  c就是该子类的类类型
		//获取类的名称
		System.out.println("类的名称是:"+c.getName());
		/*
		 * Method类，方法对象
		 * 一个成员方法就是一个Method对象
		 * getMethods()方法获取的是所有的public的函数，包括父类继承而来的
		 * getDeclaredMethods()获取的是所有该类自己声明的方法，不问访问权限
		 */
		Method[] ms = c.getMethods();//c.getDeclaredMethods()
		for(int i = 0; i < ms.length;i++){
			//得到方法的返回值类型的类类型
			Class returnType = ms[i].getReturnType();
			System.out.print(returnType.getName()+" ");
			//得到方法的名称
			System.out.print(ms[i].getName()+"(");
			//获取参数类型--->得到的是参数列表的类型的类类型
			Class[] paramTypes = ms[i].getParameterTypes();
			for (Class class1 : paramTypes) {
				System.out.print(class1.getName()+",");
			}
			System.out.println(")");
		}
	}
    ```
***
#### 类成员变量的获取
- 根据类的类类型的getFields方法获取
    ```
    public static void printFieldMessage(Object obj) {
		Class c = obj.getClass();
		/*
		 * 成员变量也是对象
		 * java.lang.reflect.Field
		 * Field类封装了关于成员变量的操作
		 * getFields()方法获取的是所有的public的成员变量的信息
		 * getDeclaredFields获取的是该类自己声明的成员变量的信息
		 */
		//Field[] fs = c.getFields();
		Field[] fs = c.getDeclaredFields();
		for (Field field : fs) {
			//得到成员变量的类型的类类型
			Class fieldType = field.getType();
			String typeName = fieldType.getName();
			//得到成员变量的名称
			String fieldName = field.getName();
			System.out.println(typeName+" "+fieldName);
		}
	}
    ```
***
#### 构造方法的获取
- 根据类的类类型的getConstructors方法获取
    ```
    public static void printConMessage(Object obj){
		Class c = obj.getClass();
		/*
		 * 构造函数也是对象
		 * java.lang. Constructor中封装了构造函数的信息
		 * getConstructors获取所有的public的构造函数
		 * getDeclaredConstructors得到所有的构造函数
		 */
		//Constructor[] cs = c.getConstructors();
		Constructor[] cs = c.getDeclaredConstructors();
		for (Constructor constructor : cs) {
			System.out.print(constructor.getName()+"(");
			//获取构造函数的参数列表--->得到的是参数列表的类类型
			Class[] paramTypes = constructor.getParameterTypes();
			for (Class class1 : paramTypes) {
				System.out.print(class1.getName()+",");
			}
			System.out.println(")");
		}
	}
    ```
*** 
#### 方法的反射操作
- 获取某个方法:方法的名称和参数列表才能唯一决定一个方法
- 反射的操作--用对象的类类型获取具体的方法对象，再用方法对象的invoke方法实现方法的反射：methodObj.invoke(对象,参数列表)
    ```
    public class MethodDemo1 {
        public static void main(String[] args) {
        //要获取print(int ,int )方法  1.要获取一个方法就是获取类的信息，获取类的信息首先要获取类的类类型
            A a1 = new A();
            Class c = a1.getClass();
            /*
            * 2.获取方法 名称和参数列表来决定  
            * getMethod获取的是public的方法
            * getDelcaredMethod自己声明的方法
            */
            try {
                //Method m =  c.getMethod("print", new Class[]{int.class,int.class});
                Method m = c.getMethod("print", int.class,int.class);
                
                //方法的反射操作  
                //a1.print(10, 20);方法的反射操作是用m对象来进行方法调用 和a1.print调用的效果完全相同
                //方法如果没有返回值返回null,有返回值返回具体的返回值
                //Object o = m.invoke(a1,new Object[]{10,20});
                Object o = m.invoke(a1, 10,20);
                System.out.println("==================");
                //获取方法print(String,String)
                Method m1 = c.getMethod("print",String.class,String.class);
                //用方法进行反射操作
                //a1.print("hello", "WORLD");
                o = m1.invoke(a1, "hello","WORLD");
                System.out.println("===================");
            //  Method m2 = c.getMethod("print", new Class[]{});
                    Method m2 = c.getMethod("print");
                // m2.invoke(a1, new Object[]{});
                    m2.invoke(a1);
            } catch (Exception e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } 
        
        }
    }
    class A{
        public void print(){
            System.out.println("helloworld");
        }
        public void print(int a,int b){
            System.out.println(a+b);
        }
        public void print(String a,String b){
            System.out.println(a.toUpperCase()+","+b.toLowerCase());
        }
    }
    ```
- **反射的作用：**
1. 简化代码：
    - UserService.java:
        ```
        public class UserService {
            public void delete(){
                System.out.println("删除用户");
            }
            public void update(){
                System.out.println("修改用户");
            }
            public void find(){
                System.out.println("查找用户");
            }
        }
        ```
    - MethodDemo2.java:
        ```
        public class MethodDemo2 {
            public static void main(String[] args) {
                UserService us = new UserService();
                /*
                * 通过键盘输入命令执行操作
                * 输入update命令就调用update方法
                * 输入delete命令就调用delete方法
                * ...
                */
                try {
                    BufferedReader br = new BufferedReader(
                            new InputStreamReader(System.in));
                    System.out.println("请输入命令:");
                    String action = br.readLine();
                    /*if("update".equals(action)){
                        us.update();
                    }
                    if("delete".equals(action)){
                        us.delete();
                    }
                    if("find".equals(action)){
                        us.find();
                    }*/
                    /*
                    * action就是方法名称， 都没有参数--->通过方法的反射操作就会简单很多
                    * 通过方法对象然后进行反射操作
                    */
                    Class c = us.getClass();
                    Method m = c.getMethod(action);
                    m.invoke(us);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
        ```
2. 根据标准javaBean对象的属性名得到该属性相应的getter/setter，在根据该方法获取属性的属性值：
    - User.java:
        ``` 
        //标准的JavaBean类有私有属性都对应有get/set方法，有无参数的构造方法
        public class User {
            private String username;
            private String userpass;
            private int age;
            public User(){}
            public User(String username, String userpass, int age) {
                super();
                this.username = username;
                this.userpass = userpass;
                this.age = age;
            }
            public String getUsername() {
                return username;
            }
            public void setUsername(String username) {
                this.username = username;
            }
            public String getUserpass() {
                return userpass;
            }
            public void setUserpass(String userpass) {
                this.userpass = userpass;
            }
            public int getAge() {
                return age;
            }
            public void setAge(int age) {
                this.age = age;
            }
        }
        ```
    - BeanUtil.java:
        ```
        public class BeanUtil {
            /**
            * 根据标准javaBean对象的属性名获取其属性值
            * 
            * @param obj
            * @param propertyName
            * @return
            */
            public static Object getValueByPropertyName(Object obj, String propertyName) {
                // 1.根据属性名称就可以获取其get方法
                String getMethodName = "get"
                        + propertyName.substring(0, 1).toUpperCase()
                        + propertyName.substring(1);
                //2.获取方法对象
                Class c = obj.getClass();
                try {
                    //get方法都是public的且无参数
                    Method m= c.getMethod(getMethodName);
                    //3 通过方法的反射操作方法
                    Object value = m.invoke(obj);
                    return value;
                } catch (Exception e) {
                    e.printStackTrace();
                    return null;
                }
            }
        }
        ```
    - MethodDemo3.java:
    ```
    public class MethodDemo3 {
        /**
        * @param args
        */
        public static void main(String[] args) {
            // TODO Auto-generated method stub
            User u1 = new User("zhangsan", "123456", 30);
            System.out.println(BeanUtil.getValueByPropertyName(u1, "username"));
        System.out.println(BeanUtil.getValueByPropertyName(u1, "userpass"));
        }
    }
    ```
#### 集合泛型的本质
- 集合的泛型是在编译阶段用于检查输入错误的，编译之后集合的泛型是去泛型化的，即编译后集合就没有泛型的概念了。且反射的操作都是编译之后,即运行时的操作因此c1==c2为True
- 可以通过方法的反射(编译之后)操作集合的泛型，从而绕过集合的泛型在编译阶段的检查
- for循环遍历元素：当内部元素类型与循环内指定元素类型不一致时，会发生内部元素的类型转换--int-->String的报错
    ```
    public class MethodDemo4 {
        public static void main(String[] args) {
            ArrayList list = new ArrayList();
            
            ArrayList<String> list1 = new ArrayList<String>();
            list1.add("hello");
            //list1.add(20);错误的
            Class c1 = list.getClass();
            Class c2 = list1.getClass();
            System.out.println(c1 == c2);
            //反射的操作都是编译之后的操作
            
            /*
            * c1==c2结果返回true说明编译之后集合的泛型是去泛型化的
            * Java中集合的泛型，是防止错误输入的，只在编译阶段有效，
            * 绕过编译就无效了
            * 验证：我们可以通过方法的反射来操作，绕过编译
            */
            try {
                Method m = c2.getMethod("add", Object.class);
                m.invoke(list1, 20);//绕过编译操作就绕过了泛型
                System.out.println(list1.size());
                System.out.println(list1);
                /*for (String string : list1) {
                    System.out.println(string);
                }*///现在不能这样遍历
            } catch (Exception e) {
            e.printStackTrace();
            }
        }
    }
    ```
