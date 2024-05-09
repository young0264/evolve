# Methos of Making External API Calls

* HTTP Client는 HTTP를 사용하여 통신하는 범용 라이브러리
* RestTemplate은 HTTPClient를 추상화(HttpEntity의 json, xml)해서 제공함. RestTemplateConfig 클래스를 정의하고 있음. HTTP 통신을 쉽게 수행할 수 있는 Spring 클래스.



*   **RestTemplate**

    * Spring 3부터 지원, REST API호출 이후 응답을 받을 때까지 기다리는 동기 방식
    * http 통신에 사용할 수 있는 spring에서 제공하는 템플릿
    * HTTP 서버와의 통신을 단순화하고 RESTful원칙을 지킴 (json, xml등 응답을 받을 수 있음)

    ```java
    RestTemplate restTemplate = new RestTemplate();
    HttpHeaders httpHeaders = new HttpHeaders();
    httpHeaders.setContentType(MediaType.APPLICATION_JSON);
    HttpEntity<SpecificClientsSSEDto> entity = new HttpEntity<>(sseDto, httpHeaders);

    Object obj = restTemplate.postForObject("<http://localhost:8086/sse/send>", entity, Object.class);
    ```



* **AsyncRestTemplate**
  * Spring4에 추가된 비동기 RestTemplate



* **WebClient**
  * Spring5에 추가된 non-block, reactive 웹 클라이언트로 동기, 비동기 HTTP통신을 위한 클라이언트.
  * Non-Blocking I/O 모델을 기반으로 하며, 비동기 및 리액티브 스트림 처리를 지원함. WebClient는 RestTemplate의 대안으로 제공되며, 더 나은 확장성과 성능을 제공함.
  * 비동기 작업을 수행하며, 리액티브 타입인 Flux와 Mono를 반환하여 비동기 결과 처리를 용이하게 함. 따라서 대용량 요청을 처리할 때 성능과 확장성 면에서 이점을 가짐.
  * 다만 러닝커브가 있으며 Webflux 모듈을 통채로 추가해야 사용 가능.



*   **OpenFeign**

    * Spring Cloud프로젝트의 일부로, 선언적인 REST 클라이언트로써 서비스 간의 HTTP 통신을 자동화하는데에 사용됩니다. OpenFeign은 애플리케이션 간의 서비스 호출을 단순화하고 클라이언트 코드를 간결하게 유지할 수 있도록 지원함.
    * 선언적 API 정의 : OpenFeign은 인터페이스 기반으로 RESTful API를 정의함. 애플리 케이션에서 사용하는 외부 서비스의 API를 인터페이스로 작성하고, 각 메서드에는 요청 URL, HTTP 메서드, 요청/응답 형식 등을 어노테이션으로 지정함.
    * 자동화된 요청 처리 : OpenFeign은 정의된 인터페이스를 기반으로 실제 HTTP 요청을 자동으로 생성함. 이를 통해 개발자는 HTTP 요청을 작성하거나 URL, 헤더, 쿼리 파라미터 등을 관리할 필요가 없음. OpenFeign이 요청 처리를 대신 처리함
    * 부가적인 기능 지원 : 요청/응답에 대한 부가적인 기능을 제공함. 요청과 응답 로깅, 헤더 설정, 에러 핸들링 등의 기능을 제공함. 커스터 마이징 가능.
    *   **ex**) 커스터 마이징 예시 코드

        ```java
        @Configuration
        public class FeignConfig {

            // 요청 및 응답 로깅 설정
            @Bean
            Logger.Level feignLoggerLevel() {
                return Logger.Level.FULL; // 로깅 레벨 설정 (NONE, BASIC, HEADERS, FULL)
            }

            // 헤더 설정을 위한 인터셉터 설정
            @Bean
            public RequestInterceptor requestInterceptor() {
                return requestTemplate -> {
                    requestTemplate.header("Authorization", "Bearer YOUR_ACCESS_TOKEN");
                    // 다른 헤더 설정 가능
                };
            }

            // 에러 핸들링을 위한 ErrorDecoder 설정
            @Bean
            public ErrorDecoder errorDecoder() {
                return new CustomErrorDecoder();
            }

            // 사용자 정의 ErrorDecoder 구현
            public class CustomErrorDecoder implements ErrorDecoder {
                @Override
                public Exception decode(String methodKey, Response response) {
                    // 여기서 에러 응답을 분석하고 사용자 정의 예외를 생성하여 반환
                    // 예외 처리 로직 작성
                    return new CustomFeignException("Feign Error: " + response.status());
                }
            }

            // 사용자 정의 예외 클래스
            public class CustomFeignException extends RuntimeException {
                public CustomFeignException(String message) {
                    super(message);
                }
            }
        }
        ```
    * 선언적 방식으로 RESTful API를 정의하고 자동화된 HTTP 요청 처리를 지원하여 서비스 간 통신을 단순화하고 유연성을 제공하는 강력한 도구. Spring Cloud와 함께 사용하면 마이크로 서비스 아키텍처에서의 개발과 통신을 간편하게 처리할 수 있음.
    *   인터페이스/어노테이션 기반으로 코드 작성이 쉽고 설정이 간편

        → 비즈니스 로직에 집중, 다른 spring cloud 기술과 통합이 쉬움.
    * spring cloud 모듈을 추가해야 하는 단점(?) 및 http2를 지원하지 않음.(추가 설정이 필요)

    ```java
    @Configuration
    public class FeignConfig {
        @Bean
        Logger.Level feignLoggerLevel() {
            return Logger.Level.FULL;
        }

        @Bean
        public RestTemplate restTemplate() {
            return new RestTemplate();
        }
    }
    ```

    ```java
    @FeignClient(name="tbTpsCm037-feign", url="<http://localhost:8082>", configuration = FeignConfig.class)
    public interface TbTpsCm037FeignClientApi {
        @GetMapping("/common/api/code/v1/common/select/list")
        void test();
    }
    ```

    ```java
    @EnableFeignClients (basePackages = "org.okestro.tps.api.infrastructure.config.feign")// spring will read @FeignClient annotation
    @Configuration
    @RequiredArgsConstructor
    public class TbTpsCm037FeignClientConfig {
    }
    ```
