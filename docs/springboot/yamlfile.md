# springboot 配置文件优先级

配置文件优先级
1. 命令行参数；
2. java:comp/env的JNDI属性(当前J2EE应用的环境)；
3. JAVA系统的环境属性；
4. 操作系统的环境变量；
5. JAR包外部的application-xxx.properties或application-xxx.yml配置文件；
6. JAR包内部的application-xxx.properties或application-xxx.yml配置文件；
7. JAR包外部的application.properties或application.yml配置文件；
8. JAR包内部的application.properties或application.yml配置文件；
9. @Configuration注解类上的@PropertySource指定的配置文件；
10. 通过SpringApplication.setDefaultProperties 指定的默认属性；