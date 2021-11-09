# Logging 처리 



# Default Logging

**Framework의 Spring Boot 로깅 처리를 복사할 것** 

아무것도 설정하지 않은 상태에서의 로깅

```java
.   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.5.0)

2021-10-27 19:04:02.216  INFO 2736 --- [  restartedMain] com.sogood.SogoodApplication             : Starting SogoodApplication using Java 12.0.2 on hyeon with PID 2736 (G:\github\sogood-core\target\classes started by latte in g:\github\sogood-core)
2021-10-27 19:04:02.221  INFO 2736 --- [  restartedMain] com.sogood.SogoodApplication             : The following profiles are active: custom-properties.yml,prod,common,proddb,sqllog
2021-10-27 19:04:02.287  INFO 2736 --- [  restartedMain] o.s.b.devtools.restart.ChangeableUrls    : The Class-Path manifest attribute in d:\mavenrepository\com\oracle\database\jdbc\ojdbc8\21.1.0.0\ojdbc8-21.1.0.0.jar referenced one or more files that do not exist: file:/d:/mavenrepository/com/oracle/database/jdbc/ojdbc8/21.1.0.0/oraclepki.jar
2021-10-27 19:04:02.287  INFO 2736 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disable
2021-10-27 19:04:02.288  INFO 2736 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2021-10-27 19:04:03.143  INFO 2736 --- [  restartedMain] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JDBC repositories in DEFAULT mode.
2021-10-27 19:04:03.177  INFO 2736 --- [  restartedMain] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 29 ms. Found 0 JDBC repository interfaces.
2021-10-27 19:04:03.328  WARN 2736 --- [  restartedMain] o.m.s.mapper.ClassPathMapperScanner      : Cannot use both: sqlSessionTemplate and sqlSessionFactory together. sqlSessionFactory is ignored.
4
2021-10-27 19:04:03.329  WARN 2736 --- [  restartedMain] o.m.s.mapper.ClassPathMapperScanner      : Cannot use both: sqlSessionTemplate and sqlSessionFactory together. sqlSessionFactory is ignored.
2021-10-27 19:04:04.098  INFO 2736 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 80 (http)
2021-10-27 19:04:04.117  INFO 2736 --- [  restartedMain] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2021-10-27 19:04:04.118  INFO 2736 --- [  restartedMain] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.46]
2021-10-27 19:04:04.254  INFO 2736 --- [  restartedMain] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationCo
```