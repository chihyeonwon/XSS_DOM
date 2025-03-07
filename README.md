## DOM 기반 Cross Site Scripting
이 도구를 이용하여 허용받지 않은 서비스 대상으로 해킹을 시도하는 행위는 범죄 행위 입니다.   
해킹을 시도할 때에 발생하는 법적인 책임은 그것을 행한 사용자에게 있다는 것을 명심하시기 바랍니다.    
XSS(Cross-site Scripting)는 웹 상에서 가장 기초적인 취약점 공격 방법 입니다.   
크로스 사이트 스크립팅(XSS)은 애플리케이션에서 브라우저로 전송하는 페이지에서 사용자가 입력하는 데이터를 검증하기 않거나, 출력 시 위험 데이터를 무효화 시키지 않을 때 발생하게 됩니다.   
공격자는 의도적으로 브라우저에서 실행될 수 있는 악성 스크립트를 웹 서버에 입력 또는 출력 될수 있게 공격을 할수 있습니다.    
XSS 취약점을 이용한 공격 방법은 크게 3가지로 분류됩니다.      
- 1. XSS(Stored) - 저장 XSS공격- 접속자가 많은 웹 사이트를 대상으로 공격자가 XSS 취약점이 있는 웹 서버에 공격용 스크립트를 입력시켜 놓으면, 방문자가 악성 스크립트가 삽입되어 있는 페이지를 읽는 순간 방문자의 브라우저를 공격하는 방식   
- 2. 2. XSS(Reflected) - 반사 XSS공격- 악성 스크립트가 포함된 URL을 사용자가 클릭하도록 유도하여 URL을 클릭하면 클라이언트를 공격하는 공격 방식      
- 3. 3. XSS(DOM) - DOM 기반 XSS 공격- DOM 환경에서 악성 URL을 통해 사용자의 브라우저를 공격하는 방식     
해당화면은 DOM 기반의 XSS 취약점 진단을 할수 있는 화면 입니다.
DOM(Document Object Model)은 W3C 표준으로 HTML 및 XML 문서에 접근방법을 표준으로 정의하는 문서객체 모델입니다.
W3C에서는 '프로그램 및 스크립트가 문서의 컨텐츠, 구조 및 형식을 동적으로 접근 및 업데이트할 수 있도록 하는 언어 중립적인 인터페이스' 라고 정의 되어 있습니다.
XSS 및 반사 XSS 공격의 악성 페이로드가 서버측 애플리케이션 취약점으로 인해, 응답 페이지에 악성 스크립트가 포함되어 브라우저로 전달되면서 공격하는 것인 반면,
DOM 기반 XSS는 서버와 관계없이 브라우저에서 발생하는 것이 차이점 입니다.

#### 공격방식- 공격자 -> 조작된 URL 링크를 전송 -> 공격 성공

일반적으로 DOM 기반 XSS 취약점 있는 브라우저를 대상으로 조작된 URL을 이메일을 통해 사용자에게 전송하면, 피해자는 이 URL 링크를 클릭하는 순간 공격 피해를 입을 수 있습니다.

해당 화면의 웹 페이지의 소스코드이고 DOM요소를 통하여 접근을 하고 있는 것을 확인 할수 있습니다. (document. )

[Select] 버튼을 누르면 소스코드상에서 확인한 default 변수에 아래와 같이 변수가 입력이 되면서 정상적으로 동작하는 것을 확인할 수 있습니다.     
http://자신의 IP/vulnerabilities/xss_d/?default=English스크립트 구문을 넣어보도록 하겠습니다.    

스크립트 구문은 다음과 같이 입력하여 줍니다 (cookie 정보를 가져오는것)http://자신의 IP/vulnerabilities/xss_d/?default=<script>alert(document.cookie)</script>XSS      
공격 우회방법 '#' 문자 뒤에 있는 값을 서버로 전송하지 않는 것을 이용하지만, 브라우저에서는 동일하게 실행이 됩니다.    
http://자신의 IP/vulnerabilities/xss_d/?default=English#<script>alert(document.cookie)</script>자신의 원하는 문구를 팝업창으로 띄움     
http://자신의 IP/vulnerabilities/xss_d/?default=English#<script>alert("XSS TEST")</script>테스트가 되지 않으시는 분들은 이렇게 설정을 변경후 테스트를 해보시기 바랍니다.     
      
[인터넷 옵션] -> [보안] -> [사용자 지정수준] -> [보안 설정]Medium 레벨 소스 코드를 확인해 보시면 <script> 부분을 필터링을 하고 있는 것을 알수 있습니다.        
?default=test</option></select><img src="" onerror="alert()">High레벨 에서는 switch 구문을 이용하여 값을 지정해 놓은 것을 확인할 수 있습니다.     
default=English#<script>alert("")</script>해당 공격구문을 참조하여 풀어 보시길 바랍니다.       
English 값 수동변경여기서 테스트한 스크립트값은 단순히 쿠키 값을 출력하는 스크립트이지만,      
해당 스크리트값 대신 다양한 공격용 코드로 대체할 경우 쿠키 정보 및 세션 정보획득, 클라이언트 프로그램 해킹 등 다양한 방법으로 공격이 이루어 질수 있습니다.     




## More Information
https://owasp.org/www-community/attacks/xss/
https://owasp.org/www-community/attacks/DOM_Based_XSS
https://www.acunetix.com/blog/articles/dom-xss-explained/
