# JSP
HTML 코드에 자바 코드를 삽입하여 웹 서버에서 동적으로 웹 페이지를 생성하여, 웹 브라우저에 돌려주는 서버 사이드 스크립트 언어

JSP 내부 동작 과정
=

>1.JSP가 실행되면 WAS는 내부적으로 JSP파일을 JAVA Servlet으로 변환   
>2.WAS는 변환된 Servlet을 동작하여 필요 기능을 수행      
>>● Servlet 동작
>>1. WAS는 사용자 요청에 맞는 적절한 Servlet 파일을 컴파일      
>>2. class 파일을 메모리에 올려 Servlet 객체를 생성   
>>3. 메모리에 로드될 때, Servlet 객체를 초기화하는 init() 메서드가 실행   
>>4. WAS는 Request가 올 때마다 thread를 생성하여 처리   
>>5. 각 thread는 Servlet의 단일 객체에 대한 service() 메서드를 실행   
>>6. service() 메서드는 요청에 맞는 적절한 메서드를 호출   
>3. 수행 완료 후 생성된 데이터를 웹페이지와 함께 클라이언트로 응답   

#JSP 특징
=
스크립트 언어이기 때문에 자바 기능을 그대로 사용 가능   
이미 정의되어 있는 객체(WAS에서 제공하는 객체)를 사용   
사용자 정의 태그를 사용하여 효율적으로 웹 사이트 구성 가능   
HTML 코드 안에 자바 코드가 있기 때문에 HTML 코드 작성이 쉬움   
Servlet과 다르게 JSP는 수정된 경우 재배포 할 필요가 없이 WAS가 알아서 처리   

#JSP 문법
=
JSP Expression
-
<%= expression %>   
JSP Expression element는 String으로 변환되어 Servlet의 출력에 삽입   
동적인 페이지 생성, 끝에 세미콜론(;)을 붙이지 않음   
 
```
ex)
<li> DATE:<%= new java.util.Date()%>
 -> DATE:(date 결과가 출력이 됨)
```

JSP Scriptlet
-
<% code fragment %>   
간단한 값이 아닌 복잡한 것을 수행할때 JSP Scriptlet을 사용
임의의 java 코드를 삽입, JSP Scriptlet Tag는 메서드가 아닌 변수만 선언 가능
```
ex) 
<%
java.util.Date date = new java.util.Date();    
System.out.println(date);
%>
-> console에 date 출력

<% if(Math.random()<0.5){%>
<H1>Loss!</H1>
<% else { %>
<H1>Win!</H1>
<% } %>
-> 랜덤 결과 0.5 이상이면 Win! 이하이면 Loss!를 출력
```  

JSP Declaration
-
<%! declaration %>   
JSP Declaration을 사용하면 Servlet 클래스에 삽입되는 메서드나 필드를 정의
JSP Scriptlet Tag와 달리 JSP Declaration Tag는 메서드와 변수 모드 선언이 가능
```
ex)
<%! private int accessCount = 0;%>
-> 변수 accessCount 선언
```

JSP Comment
-
<%-- comment --%>   
주석 설정

JSP Directive
-
<%@ directive %>   
JSP 페이지 전체 구조에 대해 WAS에 지시를 내림
지시 내리는 부분 : Page, include, taglib   
```
ex)
<%@ page import = "java.util.Date" %>
-> Date 페이지 바인딩
<%@ include file = "next.jsp" %>
-> next.jsp 파일을 가져옴
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
-> 태그 라이브러리의 태그를 사용
```

JSP Action
-
```
1. <jsp:forward> action   
   다른 리소스(JSP,html,Servlet과 같은 정적 페이지)로 요청을 전달하는데 사용 

2. <jsp:include> action   
   현재 JSP 페이지에 다른 리소스를 포함시키는데 사용

3. <jsp:useBean> action   
   해당하는 Bean(자바 객체)이 존재하는지 확인, 존재하지 않으면 지정된 객체를 생성

4. <jsp:setProperty> action   
   Bean(자바 객체)의 속성을 설정

5. <jsp:getProperty> action   
   주어진 속성값을 가져오는데 사용, 이를 문자열로 변환하고 동적인 웹 페이지를 생성하는데 사용 가능
```

용도
1)동적으로 파일을 삽입, 2)JavaBeans 구성 요소를 재사용, 3)사용자를 다른 페이지로 전달 가능

JSP에서 동적인 코드를 호출하는 6가지
=
1. Call Java code directly   
java 코드를 직접 호출   
모든 java 코드를 JSP 페이지에 넣음
아주 적은 양의 코드에만 적합   

2. Call java code indirectly
java 코드를 간접적으로 호출   
별도의 utility class(Java Class)를 작성   
utility class를 호출하는데 필요한 java 코드만 jsp에 넣음   

3. Use beans   
beans로 구조화된 별도의 utility class(Java Class)를 작성   
jsp:useBean, jsp:getProperty, jsp:setProperty를 사용하여 utility class를 호출   

4. Use the MVC architecture   
MVC 아키텍쳐를 사용   
Servlet이 요청에 응답하고 적절한 데이터를 검색하여 결과를 Beans에 저장, 결과를 JSP 페이지(View)에 전달하여 결과 표시   
JSP 페이지는 beans를 사용   

5. Use the JSP expression language   
shorthand syntax를 이용하여 간단하게 객체 속성에 접근하고 출력   
jsp:useBean, jsp:getProperty, jsp:setProperty를 expression language으로 간단하게 표현   
일반적으로 beans, MVC 패턴을 함께 사용   

6. Use custom tags   
tag handler class를 만듦   
xml과 같은 사용자 정의 태그를 사용하여 태그 핸들러를 호출   
