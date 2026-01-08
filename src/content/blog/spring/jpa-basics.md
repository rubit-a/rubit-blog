---
title: 'Spring Data JPA 기초'
description: 'JPA를 사용한 데이터베이스 연동'
pubDate: 'Jan 07 2026'
heroImage: '../../../assets/blog-placeholder-2.jpg'
---

Spring Data JPA는 JPA(Java Persistence API)를 Spring에서 쉽게 사용할 수 있도록 도와주는 라이브러리입니다.

## JPA란?

JPA는 자바 객체와 데이터베이스 테이블을 매핑해주는 ORM(Object-Relational Mapping) 기술입니다.

## Entity 정의

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String username;

    @Column(nullable = false)
    private String email;

    // getters and setters
}
```

## Repository 생성

```java
public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByUsername(String username);

    List<User> findByEmailContaining(String email);
}
```

Spring Data JPA가 자동으로 구현체를 만들어줍니다!

## 서비스 레이어

```java
@Service
@Transactional
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User createUser(String username, String email) {
        User user = new User();
        user.setUsername(username);
        user.setEmail(email);
        return userRepository.save(user);
    }

    public Optional<User> findByUsername(String username) {
        return userRepository.findByUsername(username);
    }
}
```

## 주요 특징

- **자동 쿼리 생성**: 메서드 이름으로 쿼리 자동 생성
- **페이징과 정렬**: 기본 제공
- **트랜잭션 관리**: `@Transactional`로 간편하게 관리
- **Auditing**: 생성/수정 시간 자동 기록

## 다음 학습 주제

- QueryDSL을 사용한 동적 쿼리
- N+1 문제 해결 방법
- 성능 최적화 전략
