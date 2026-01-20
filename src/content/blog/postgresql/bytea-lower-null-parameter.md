---
title: "bytea lower 오류 (null 파라미터)"
description: "null 파라미터가 bytea로 추론되면서 발생하는 lower 오류 해결"
pubDate: "Jan 19 2026"
heroImage: "../../../assets/postgresql-logo.png"
---

JPA `@Query`에서 `null` 파라미터가 `bytea`로 추론되어 `lower` 호출이 실패하는 문제와 해결 방법을 정리했다.

<br />

### 현상

JPA `@Query`에서 파라미터가 `null`일 때 아래 오류가 발생했다.

```
ERROR: function lower(bytea) does not exist
Hint: No function matches the given name and argument types. You might need to add explicit type casts.
```

<br />

### 원인

PostgreSQL은 `null` 파라미터의 타입을 추론해야 하는데, Hibernate가 `null` 값을 바인딩할 때
문맥에 따라 `bytea`로 추론되는 경우가 있다. 이때 `lower(bytea)`가 되어 오류가 발생한다.

<br />

### 문제 발생 쿼리

아래 쿼리는 `:name` 또는 `:email`이 `null`일 때 `bytea` 추론 문제가 발생할 수 있다.

```java
@Query("""
        select c from CompanyEntity c
        where (:name is null or lower(c.name) like lower(concat('%', :name, '%')))
          and (:email is null or lower(c.contactEmail) like lower(concat('%', :email, '%')))
        """)
```

<br />

### 해결

`coalesce`로 파라미터가 항상 문자열로 해석되도록 고정한다.

```java
@Query("""
        select c from CompanyEntity c
        where (
            coalesce(:keyword, '') = ''
            or lower(c.name) like lower(concat('%', :keyword, '%'))
            or lower(c.contactEmail) like lower(concat('%', :keyword, '%'))
        )
        """)
```

`coalesce(:keyword, '')`에서 `''`가 문자열 리터럴이므로 `:keyword`도 문자열로 타입이 고정된다.
이로 인해 `null`이어도 `lower`가 문자열을 대상으로 동작한다.
