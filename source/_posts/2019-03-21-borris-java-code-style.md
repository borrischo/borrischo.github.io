---
layout: post
title: "Java Develop Style"
date: 2019-03-21 116:41:00 +0900
comments: true
tags : ["java"]
categories : ["java"]
sitemap :
  changefreq : daily
  priority : 1.0
---

## 개요

특수한 상황을 제외한 혼자서 프로그램들을 만들고 유지보수를 하는 상황은 거의 존재하지 않습니다.
여러 사람들끼리 같이 프로그램을 만들게 되면 규칙을 정하고 개발을 해야만 유지보수를 좀더 쉽게 할 수 있습니다.
이글은 Java개발에 있어서 아주 기초적인 규칙에 대해서 설명을 했으며, Google Java Style Guide와 거의 비슷합니다.

## 코딩 우선 항목

코딩 우선 항목이란 개발하는데 있어서 꼭 생각해봐야 할 항목들을 의미합니다. 개발하는데 있어서 해당 항목을 참고로 개발을 진행 하셨으면 합니다.

### 정확도

코드가 올바르게 동작하는지에 대한 항목입니다. 당연하게도 코드는 원하는 서비스를 이상없이 동작하는게 목적이지만 그러지 않은 경우가 있습니다.

### 크기

코드의 라인수를 의미하는게 아니라, 컴파일된 .Class파일의 사이즈를 의미합니다.

### 속도

CPU 사용치 기준으로 측정된 실행 속도와 사용자의 응답속도를 모두 포함합니다. 가이드상 충분히 빠른 코드를 만드는 것을 권고하지만 필요 이상으로 시간을 낭비할 필요는 없습니다.
아무튼 예를 들어서 5개 레코드를 정렬 한다면 Bubble Sort를 사용하고 백만건 이상의 레코드를 처리하려면 Quick Sort를 사용해야 합니다.

### 견고성

잘못된 입력이나 조건에도 코드는 이상이 없어야 합니다. Exception Handler등을 이용해서 적절히 오류를 처리해야 하는 코드를 작성해야 합니다.

### 안전성

버그를 발생시키지 않는 코드를 작성해야 합니다.

### 시험가능성

Test하기 좋은 코드로 개발을 해야만 합니다. TDD방법론에 따라서 개발을 하며 만약 긴급을 요하는 상황이라면 넘어가셔도 됩니다.

### 유지보수성

유지보수가 쉬운 코드를 만들어야 합니다. 유지보수가 쉬운 코드는 다음과 같은 특징을 지니고 있습니다.

1. 이해하기 쉬운 코드로 작성 (이해라는것은 개발하는 사람의 수준에 따라서 차이가 납니다.)
2. 캡슐화 (본연의 해야할 기능을 충실히 실행하며 의도한것 외에 따른 곳에 영향을 주지 않는 코드를 만들어야 합니다.)
3. 코드에 대한 문서화 (코드에 주석을 포함.)

### 단순성

설명이 없어도 이 코드가 무엇인지 알아볼 수 있어야 합니다. (수준에 따라서 차이가 있습니다.)

### 재사용성

현재 프로젝트가 아닌 다른 프로젝트에서도 코드가 이상 없이 동작할 수 있도록 설계를 해야 합니다.
이렇게 설계된 코드들은 일반적으로 50% 이상의 개발 속도를 제공합니다.

## 프로그램 구성

### 코드 구성항목 순서

코드 구성항목 순서란 하나의 Java파일안에 존재하는 구성항목들의 순서를 의미합니다. 소스 파일은 하나의 public class이 존재해야 하고 추가적으로 non-public class들을 포함할 수 있습니다.
구성항목들은 다음과 같습니다.

1. Package(패키지명)
2. Import절
3. Class정의
    - Class header
    - Constants (final class variables) : public, protected, private 
    - Public static inner classes
    - Protected inner classes, static 또는 반대
    - Private inner classes, static 또는 반대
    - Class variables (private only)
    - Fields (instance variables) (private only)
    - Constructors
    - 기타 methods : method의 순서를 정할 때 (public, protected, private)는 무시하고 아래 가이드라인을 준수해야 합니다.
        - 관련된 메소들을 함께 묶어서 작성.
        - 부모 클래스 함수를 Overriding할 때는 부모 클래스와 같은 순서.

### Import절

소스는 Package구역과 Import구역 순서로 되어 있습니다.
Import된 항목들은 아스타(*)를 사용해서 해당 패키지의 모든 항목들을 선언할 수 있지만 아스타(*)를 사용하는 것은 옳지 않습니다.
import로 선언된 항목들은 명시적으로 나열해야 합니다.

#### 나쁜예

```java
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
```

#### 좋은예

```java
import lombok.extern.slf4j.*;
import org.springframework.*;
```

참고로 내가 쓰지 않는 항목들이 import되어 있을 경우도 있는데 그럴 경우는 툴에서 단축키를 제공해준다.
Intellij같은 경우 Ctrl + Alt + o사용해서 코드 정렬을 도와줍니다.

### 코드 배치

소스는 코드들의 집합입니다. 이러한 소스는 누구든지 보기 편해야 하고 이해가 쉬워야 합니다.
여기에서는 몇몇의 규칙을 통해서 보기 편하고 이해가 쉬운 코드를 만드는 법을 설명합니다.

#### 클래스 선언

- 클래스의 헤더를 가능한 한 줄에 작성해야 합니다.

 ```java
public class ExceptionControllerAdvice {
    ...
}
```

- 한 줄이 넘으면, extends와 implements 앞에서 줄바꾸기를 하고 이어지는 코드들은 들여쓰기를 해야합니다.

 ```java
public class AlbumRepositoryImpl 
        extends QuerydslRepositorySupport 
        implements AlbumRepositoryCustom {
        ...
}
```

- class 헤더의 끝에 중괄호({)를 작성합니다. 중괄호는 한칸 띄고 작성합니다.

 ```java
public class ExceptionControllerAdvice {
    ...
}
```

#### 메소드 헤더

- 가능하면 메소드 헤더를 한 줄에 작성해야 합니다.
- 한줄이 넘으면 파라메터 인자를 기준으로 줄 바꿈하고 그래도 공간이 모자르면 throws영역도 줄바꿈 합니다.
- 메소드 헤더의 끝의 중괄호({)는 한칸 띄고 작성합니다.

#### 들여쓰기(Indentation)

- 들여쓰기는 4 Space로 설정 합니다.
- 들여쓰기는 Tab을 사용해야 합니다. 들여쓰기 이후 공백은 Space로 합니다.

#### 공백(Space)을 주는 항목

#### 공백(Space)을 주면 안되는 항목