# 코드 관리 
DB 테이블 코드 값을 저장하는 컬럼의 코드 관리는 enum을 사용한다. 코드 값으로 Enum 인스턴스를 생성하기 위해 find() 메서드를 구현한다. 

```java
package com.sogood.biz.blog.code;

import lombok.AllArgsConstructor;
import lombok.Getter;
import org.springframework.http.HttpStatus;

  
 @Getter
 @AllArgsConstructor
 public enum BlogCode {
     // 형식 [테이블명]_[컬럼명]_[코드값] 
     /** 블로그 사용 여부 N: 아니오 */ 
     BLOG_USE_YN_NO("N", "Not Used"),
     /** 블로그 사용 여부 Y: 예 */ 
     BLOG_USE_YN_YES("Y", "Used");
 
     private String code; 
     private String desc; 

     
    private static Map<String, BlogCode> mapToFind = new HashMap<String, BlogCode>();
    static { 
      for (BlogCode myVar : BlogCode.values()) {
        mapToFind.put(myVar.getCode(), myVar);
      }
    }
    public static BlogCode find(String code) {
      return mapToFind.containsKey(code)? mapToFind.get(code): null;
    }
 }///~

```
## Enum 사용
if 문과 switch 문에서 Enum을 사용한다. 
```java
   Blog blogBean = new Blog();
   // 코드값 설정 
   blogBean.setUserYn(BlogCode.BLOG_USE_YN_YES.getCode());
   // DB Insert
   // blogService.insertBlog(blogBean);

   // 클라이언트의 요청 파라미터 
   String paramBlogUseYn = "Y";
   if(paramBlogUseYn.equals(BlogCode.BLOG_USE_YN_YES.getCode())) {
     // do something
     System.out.println("Y");
   }else {
     // do something
     System.out.println("N");
   }

   // 
   BlogCode paramCode = BlogCode.find(paramBlogUseYn);
   switch(paramCode) {
     case BLOG_USE_YN_NO:
       // do Something
     break;
     case BLOG_USE_YN_YES:
       // do Something 
       System.out.println("Y");
     break;
     default:
      // do Something 
     break; 
   }
 }
```


