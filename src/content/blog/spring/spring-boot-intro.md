---
title: 'Spring Boot 시작하기'
description: 'Spring Boot로 첫 웹 애플리케이션을 만들어봅시다'
pubDate: 'Jan 08 2026'
heroImage: '../../../assets/blog-placeholder-1.jpg'
---

Spring Boot는 Spring 프레임워크를 기반으로 한 강력한 웹 애플리케이션 개발 도구입니다.

## Spring Boot의 장점

1. **자동 설정(Auto Configuration)**: 복잡한 설정 없이 바로 개발 시작
2. **내장 서버**: Tomcat이나 Jetty가 내장되어 있어 별도 설치 불필요
3. **의존성 관리**: Starter 패키지로 간편한 의존성 관리
4. **프로덕션 준비**: Actuator로 모니터링과 관리 기능 제공

## 첫 프로젝트 생성

Spring Initializr를 사용하면 간단하게 프로젝트를 생성할 수 있습니다:

```bash
# Maven 프로젝트 생성
mvn archetype:generate -DgroupId=com.example -DartifactId=demo
```

## Hello World 컨트롤러

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

간단한 어노테이션만으로 REST API를 만들 수 있습니다.

## 실행하기

```bash
./mvnw spring-boot:run
```

이제 `http://localhost:8080/hello`에 접속하면 "Hello, Spring Boot!"를 확인할 수 있습니다.

## 다음 단계

- Spring Data JPA로 데이터베이스 연동
- Spring Security로 인증/인가 구현
- REST API 설계 패턴
