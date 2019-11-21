# spring-security-proxy

## migration

- org.springframework.cloud maven 세팅 변경
  - 예제의 Dalston은 Spring boot 1.5 환경용임.
  - Greenwich로 변경(2.1용이지만 2.2(Hoxton)용은 RC버전이기 때문에 Greenwich로 사용)

    ~~~xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    ~~~

- Zuul maven 세팅 변경
  - AS IS

    ~~~xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-zuul</artifactId>
    </dependency>
    ~~~

  - TO BE

    ~~~xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
    </dependency>
    ~~~

## 내용

- API 게이트웨이를 사용
- 클라이언트는 한 서버의 URL만 알면되고 백엔드는 변경없이 자유롭게 리팩토링 가능.
- CORS 필터가 필요하지 않음.(클라이언트가 단일서버로만 접근하니까)
- resource 서버에 address 세팅 127.0.0.1.(리소스 서버에 보안이 안되어 있으므로 localhost 안에서만 호출하기 위한 조치)
- security 설정에 `http.httpBasic().disable();`을 사용하여 브라우저가 인증 대화 상자를 팝업하지 못하게 설정.
- Spring boot 2.2.0 이상부터 Actuator HTTP는 기본적으로 비활성화.(trace -> actuator/httptrace)
  - 사용하려면 bean 구현 필요