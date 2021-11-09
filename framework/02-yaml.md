# YAML 사용 

application.properties 파일을 application.yml로 변경한다. 

## application.yaml 설정

yaml 파일에 다음을 추가

```jsx
demo:
  datasource:
    database: mysql
    user: root
    password: 1234
```

## Controller 생성

`com.sogood.demo.controller` 에 YamlTestController.java를 생성한다.

### ApplicationContext 사용

applicatoinContext를 사용하여 프러퍼티를 읽는 방법을 살펴본다. 

```java
package com.sogood.demo.controller;

import com.sogood.core.exception.BusinessException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/** 예외처리 테스트 컨트롤러 */
@RestController
@RequestMapping("/demo/yaml")
public class YamlTestController {
    @Autowired
    private ApplicationContext applicationContext;

    /** ConfigurationProperties Test */
    @GetMapping("/yaml-context")
    public ResponseEntity<String> invokeBizError(HttpServletRequest request, HttpServletResponse response) {
        System.out.println(">>>>>>>>>>>>>>>>>>>>>>");
        System.out.println(applicationContext.getEnvironment().getProperty("demo.datasource.database"));
        System.out.println(applicationContext.getEnvironment().getProperty("demo.datasource.user"));
        System.out.println(applicationContext.getEnvironment().getProperty("demo.datasource.password"));
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.TEXT_PLAIN);  // MediaType을 설정해야 한다.

        return new ResponseEntity<String>("success", headers,  HttpStatus.OK);
    }//:

}///~
```

설정값은 applicationContext.getEnvironment().getProperty("속성명")으로 꺼낼 수 있다. 

브라우저에서 다음 주소를 입력한다. 

```java
http://localhost/demo/yaml/yaml-context
```

## ConfigurationProperties 어노테이션 사용

## Bean 생성

com.sogood.demo.config 패키지에 DemoYamlConfig.java를 생성한다.  yaml 설정 파일의 프러퍼티가 계층 구조일 때는 prefix 속성을 사용할 수 있다. yaml에 spring.freemarker.suffix를 사용해보자.

```java
package com.sogood.demo.config;

import lombok.Getter;
import lombok.Setter;
import org.springframework.boot.context.properties.ConfigurationProperties;

/** YAML 설정을 테스트하기 위한 Bean */
@Getter
@Setter
@ConfigurationProperties(prefix="demo.datasource")
@Component
public class DemoYamlConfig {
    private String database;
    private String user;
    private String password;
}com.sogood.demo.controller

```

**Prefix 사용** 

yaml 설정 파일의 프러퍼티가 계층 구조일 때는 prefix 속성을 사용할 수 있다.

> property가 template-loader-path이면 Camel Notation 표기 방식으로 멤버 변수를 선언한다. 즉, templateLoaderPath와 같이 필드명을 설정한다.
>